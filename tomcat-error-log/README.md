# Tomcat Error 로그 보기

`WEB-INF/classes`폴더를 만들고`logging.properties` 파일을 생성 하고 아래 내용을 입력한다.

```properties
org.apache.catalina.core.ContainerBase.[Catalina].level=INFO
org.apache.catalina.core.ContainerBase.[Catalina].handlers=java.util.logging.ConsoleHandler
```

서버 구동시 톰캣 에러를 볼수있다.