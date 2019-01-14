# Intellij IDEA	

> Intellij IDEA책을 보면서 정리하면 좋은 내용들을 정리한다



## 단축키(Mac-Window)

##### 퀵 수정 기능

`Option + Enter(Alt + Enter)`

##### 명령어 검색 기능

`Shift`를 2회 누름



## 1장

### 라이선스

- 서브스크립션 방식(2018년 12월 기준)
  - 갱신 2,3년차 이후에는 할인 가격 적용
  - 갱신을 하지 않으면 기본 버전을 영구적으로 사용 할 수 있다.

## 3장 파일 편집

##### 행/선택 항목 복사

`Command + D(Ctrl + D)`

##### 프로젝트 내 찾기

`Command + Shift + F(Ctrl + Shift + F)`

##### 프로젝트 내 변경

`Command + Shift + R(Ctrl + Shift + R)`



### 3.1 HTML 파일 생성과 미리 보기

#### LiveEdit로 미리 보기

Body를 편집하기 전에 HTML 미리 보기 

- 구글 크롬 브라우저에서 `JetBrains IDE Support` 확장 프로그램 설치

`Ctrl + Shift + D` (윈도우에서는 파일 이름을 오른쪽 클릭하고 `Debug 'index.html'`)를 누르면 브라우저가 시작된다.

현재 열린 파일을 디버그 모드로 실행되며 HTML파일을 열었다면 브라우저로 미리 보기를 보여주고 자바스크립트 디버그 상태이다. 파일을 편집하면 실시간으로 렌더링 결과를 확인할 수 있다.

##### 프로젝트 툴 윈도우 활성화/비활성화(Project)

`Command + 1(Alt + 1)`

##### 신규 파일 생성(New..) 

`Command + N(Alt + Insert)`

##### 설정 화면 표시(Settings..)

`Command + ,(Ctrl + Alt + S)`

##### 열린 HTML 파일을 브라우저로 미리 보기(Debug context configuration)

`Ctrl + Shift + D(파일 이름을 오른쪽 클릭하고 Debug 선택)`



### 3.2 편집과 자동 완성 기능

#### 완료 태그의 자동 완성 기능

HTML 시작 태그에 대응하는 종료 태그를 자동으로 입력력된다.

#### 자동 완성 후보

`div`태그를 입력하면 `>`를 입력하기 전에 커서를 이동해 스페이스를 입력하면 지정할 수 있는 속성 후보가 자동으로 나타난다. 자동 완성 후보를 `Tab`으로 확정하면 뒤에 이어지는 스테이트먼트를 교체하는 형태로 자동 완성 되지만 `Enter`로 확정하면 이어지는 스테이트먼트를 그대로 두고 자동 완성 내용이 커서 위치에 삽입된다.

#### Emmnet/Zen Coding

젯브레인 IDE는 Emmet(https://emmet.io/)을 표준으로 지원하고 있어 별도 플러그인 설치 없이 Emmet 표기법을 작성하소 `Tab`을 누르는 것만으로 해당 기능을 사용할 수 있다.

|                  생략 표기법                  |                      전개 후                      |
| :-------------------------------------------: | :-----------------------------------------------: |
|                     hello                     |           \<div class="hello"\>\</div\>           |
|              hello{Hello World}               |      <div class="hello"\>Hello World\</div\>      |
|                       a                       |                \<a href="">\</a\>                 |
|                a{Hello World}                 |           \<a href="">Hello World\</a\>           |
| a\[href=http://jetbrains.com\]\[Hello World\] | \<a href="http://jetbrains.com">Hello World\</a\> |
|                  script:src                   |            \<script src="">\</script>             |
|             script[src=hello.js]              |        \<script src="hello.js">\</script>         |
|                   form:post                   |      \<form action="" method="post">\</form>      |

#### Intention Action

정의되지 않은 곳에서 커서를 맞추고 `Option + Enter(Alt + Enter)`를 누르면 Intention Action(상황이나 조건에 맞게 액션을 수행하는) 팝업이 나타난다.

#### Emmet

`script:src` 라고 입력하고 `Tab`을 누르면 `<script src=""\>`로 변환된다. 파일 이름을 입력하고 `Option + Enter(Alt + Enter)`를 누르고 **'Create File 파일이름'** 라는 Intention Action을 선택하면 IDE가 파일을 생성한다.

#### Postfix completion(포스트픽스 자동 완성)

문법 검증 기능을 사용해 우선 생각나는 대로 계산식을 작성하고 **이 계산식을 사용한 if 문을 만들어야지** 같이 문장으로 변환하는 경우 Postfix completion문법을 사용하여 작성 할수 있다.
`arg1>arg2.if` 작성 후 `Tab`을 입력하면 `if (arg1 > arg2) { }`가 작성된다. 
다른 Postfix completion은 Preferences 다이얼로그의 **Editor -> General -> Postfix completion**에서 확인할 수 있다.

##### 자바 스크립트의 주요 Postfix completion

| postfix  |            사용 방법            |
| :------: | :-----------------------------: |
| 수식.log |     식을 console.log로 출력     |
| 수식.var |        식을 변수에 대입         |
| 수식.if  | 식을 조건문으로 한 if 문을 구성 |
| 수식.not |        식의 조건을 반전         |

### Expand Selection

구문제 맞게 선택 범위를 넓힐 수 있는 기능으로 `Option + ↑(Ctrl + W)` 을 사용하여 시작과 종료 위치를 문맥에 따라 선택해준다. 선택 범위가 너무 커져버리면 `Option + ↓(Ctrl + Shift + W)`로 선택 범위를 축소할 수 있다(Shrink Selection).

### 변수 추출

`Option Command + V(Ctrl + + Alt + V)`로 변수를 추출할 수 있다.

### 인라인

변수를 추출하는 것과 반대로 변수를 제거해 인라인으로 리펙토링할 수 있다. 제거하려는 변수에 커서를 두고 `Option Command + N(Ctrl + + Alt + N)`을 누른다.

### 코드 포맷

평집 과정에서 스페이스나 들여쓰기가 많이 맞지 않을 경우  `Option + Command + L(Ctrl + Alt + L)`을 누르면 정리해준다.
Preferences 아이얼로그의 Editor -> Code Style에서 조정할 수 있다. 'Scheme'란에 'Project'를 지정하면 다른 프로젝트에 영향을 미치지 않는 설정이 가능하다.

### 리네임 리팩토링

`Command + Shift + R(Ctrl + Shift + R)`을 통해 안전하게 이름을 변경할 수 있다.



## 4장 실행과 디버깅

##### 프로젝트 툴 윈도우로 포커스 이동

`Command + 1(Ctrl + 1)`



### Inspection

스크롤바에서 노란색 강조는 파일의 정적 분석 결과를 나타내며, 프로그램에 문법적으로 틀리지는 않지만 수정하는 게 바람직한 코드가 있다고 경고하는 표현이다. `Command + F1(Ctrl + F1)`으로 경고 내용을 확인할 수 있다.
불필요한 경고 메시지는 `Option + Enter(Alt + Enter)`을 사용해 경고 메시지를 해제하자.



### Live Template 활용

- `psvm`을 입력하고 `Tab`을 누르면 main 메소드가 생성된다.
- `fori`을 입력하고 `Tab`을 누르면 for문이 생성된다.
- `"".var`을 입력하고 `Tab`을 누르면 문자열을 저장히기 위한 변수를 선언한다.
- `수식.if`을 입력하고 `Tab`을 누르면 if조건문이 생성된다.
- `수식.not을 입력하고 `Tab`을 누르면 if조건문이 반전되어 생성된다.
- `sout`을 입력하고 `Tab`을 누르면 출력하는 문장이 생성된다.

##### 자동 완성 세미콜론

`Command + Shift + Enter(Ctrl + Shift + Enter)`



## ETC

##### 빠른 평가 표현식

`Option + Space(Ctrl + Shift + I)`

##### 편집기에서 데이터 흐름 정보 미리보기

`Control + Shift + P(Ctrl + Shift + P)`

##### 조인 라인 액션

`Control + Shift + J(Ctrl + Shift + J)`

##### 다이어그램 팝업

`(Ctrl + Alt + U)`

