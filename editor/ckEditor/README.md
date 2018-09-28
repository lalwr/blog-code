# 1. 기본 셋팅

1. ##### 사이트

   - 다운로드(v4.10.1) : https://ckeditor.com/ckeditor-4/download/

   - 문서: https://ckeditor.com/docs/ckeditor4/latest/index.html

2. ##### 설치

   - 다운로드 사이트에 접속해서 Full Package 를 다운받는다.
   - 압축을 해제한다.
   - 압축해제한 폴더를 webapp 폴더에 resources폴더를 만들고 복사한다.

3. ##### 리소스 폴더 매핑

   - ckeditor폴더의 리소스를 매핑하는 부분을 설정한다.

```xml
<mvc:resources mapping="/resources/**" location="/resources/" />
```

4. ##### 기본 에디터 셋팅

```html
<!DOCTYPE html>
<html lang="ko">
<head>
	<script src="/resources/ckeditor/ckeditor.js"></script>
    <script>
    window.onload = function(){
       ck = CKEDITOR.replace("editor");
    };
    </script>
</head>
<body>
    <form>
        <textarea name="editor1" id="editor1" rows="10" cols="80">
            This is my textarea to be replaced with CKEditor.
        </textarea>
    </form>
</body>
</html>
```

# 2. 이미지 기본 업로드

1. ##### ckEditor config 설정을 변경한다.

```javascript
<script src="/resources/components/ckeditor/ckeditor.js"></script>
<script>
    var editorConfig = {
        filebrowserUploadUrl : "/ckEditor/imgUpload", //이미지 업로드
    };

    CKEDITOR.on('dialogDefinition', function( ev ){
        var dialogName = ev.data.name;
        var dialogDefinition = ev.data.definition;

        switch (dialogName) {
            case 'image': //Image Properties dialog
            //dialogDefinition.removeContents('info');
            dialogDefinition.removeContents('Link');
            dialogDefinition.removeContents('advanced');
            break;
        }
    });
	window.onload = function(){
  	   ck = CKEDITOR.replace("editor", editorConfig);
	};
</script>
```

2. ##### 리소스 폴더 매핑

   파일 저장을 아래와 같이 설정 했을 경우

```java
//파일 기본경로
String defaultPath = request.getSession().getServletContext().getRealPath("/");
//파일 기본경로, 상세경로
String filePath = defaultPath + "resources" + File.separator + "ckEditorImg" + File.separator;
```

파일을 저장한 위치와 요청한 url을 매칭시키는 설정을 추가한다.

```java
<mvc:resources mapping="/ckEditorImg/**" location="/resources/ckEditorImg/" />
```

3. ##### 서버에서 아래의 형식에 맞게 JSON으로 응답한다.

   - uploaded : 이미지 파일이 업로드 완료되었는지 여부. 성공은 1, 실패는 0으로 설정
   - fileName : 표시할 파일이름
   - url : 이미지가 들어가 있는 경로
   - 문서 : https://ckeditor.com/docs/ckeditor4/latest/guide/dev_file_upload.html#response-file-uploaded-successfully

# 3. 드래그 이미지 업로드

1. ##### add-on 설치

   upload image 애드온은 '드래그앤드롭'을 가능하게 해주고, 나머지는 종속되는 프로그램

   - [Upload image](http://ckeditor.com/addon/uploadimage)
   - [Upload widget](http://ckeditor.com/addon/uploadwidget)
   - [Notification aggregator](http://ckeditor.com/addon/notificationaggregator)
   - [file tools](http://ckeditor.com/addon/filetools)
   - [notification](http://ckeditor.com/addon/notification)

   프로그램을 다운받아서 `ckeditor/plugins`  디렉토리에 넣는다

2. ##### ckEditor config 설정을 변경

```java
<script src="/resources/components/ckeditor/ckeditor.js"></script>
<script>
    var editorConfig = {
         filebrowserUploadUrl : "/ckEditor/imgUpload?type=image", //이미지만 업로드
         extraPlugins : 'uploadimage'
    };
 
 
    CKEDITOR.on('dialogDefinition', function( ev ){
        var dialogName = ev.data.name;
        var dialogDefinition = ev.data.definition;
 
        switch (dialogName) {
            case 'image': //Image Properties dialog
            //dialogDefinition.removeContents('info');
            dialogDefinition.removeContents('Link');
            dialogDefinition.removeContents('advanced');
            break;
        }
    });
 
    window.onload = function(){
         ck = CKEDITOR.replace("editor", editorConfig);
    };
</script>
```

