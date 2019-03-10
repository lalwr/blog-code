# Spring 동작 원리

1. 웹 애플리케이션이 실행되면 WAS가 web.xml을 설정을 로드한다.

2. Web.xml에 등록되어 있는 `ContextLoaderListener`가 생성된다. ContextLoaderListener는 `ServletContextListener`를 구현하고 있기 때문에 WAS가 애플리케이션을 로드할때 생성되는 `ServletContext`가 생성될때 호출된다.

3. ContextLoaderListener는 설정되어 정보를 기반으로 로드하며 Root WebApplicationContext가 생성된다. 설정을 하지 않으면 `applicationContext.xml` 이 기본 설정 파일이다. Servlet WebApplicationContext가 Root WebApplicationContext를 참조하며 반대는 불가능하다.

   ```xml
   <listener>
           <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
       </listener>
   
   <context-param>
       <param-name>contextConfigLocation</param-name>
       <param-value>com.config.RootApplicationContextConfig</param-value>
   </context-param>
   ```

   ![https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc](./images/spring.png)

4. 클라이언트로 부터 요청이오면 `DispatcherServlet`이 생성된다. DispatcherServlet은 FrontController 라고도 부른다.  DispatcherServlet은 작업을 분담하여 관리하는 역할을 한다.

   ![https://terasolunaorg.github.io/guideline/1.0.x/en/Overview/SpringMVCOverview.html](./images/dispatcherServlet.png)

5. DispatcherServlet도 설정되어 정보를 기반으로 로드하며 Servlet WebApplicationContext가 생성된다.

   ```xml
   <servlet>
       <servlet-name>mvc</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <init-param>
           <param-name>contextClass</param-name>
           <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
       </init-param>
       <init-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>com.config.WebMvcContextConfiguration</param-value>
       </init-param>
       <load-on-startup>1</load-on-startup>
   </servlet>
   ```