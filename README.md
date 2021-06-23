# Technical_Term_For_Interview
면접을 위한 기술 용어를 정리합니다.
<br>
이해를 돕기 위해 개인적인 방식으로 풀어서 서술합니다.

<br>

## CGI(Common Gateway Interface)
- 웹서버와 프로그램 사이의 데이터를 주고 받는 규칙.

## Class\<T\>
```java
public final class Class<T> extends Object implements Serializable, GenericDeclaration, Type, AnnotatedElement
```
- 주요 메소드
 
       - forName(String className) 
         인자값으로 클래스 이름을 넘기면 해당 클래스를 찾아 로딩한다. 클래스 이름은 반드시 패키지 이름을 포함해야한다.
         로딩된 클래스는 자신의 static 블록을 실행시킨다.
## Context Parameter(컨텍스트 초기화 매개변수)

## DD(Depolyment Desciptor)
- 웹 애플리케이션의 배치 정보를 담고 있는 파일.
- 서블릿에서는 web.xml이 DD파일이다. 
- web.xml의 <welcome-file-list>태그를 이용하여 웰컴 파일을 설정할 수 있다. 
- 웹 서버에게 요청할 때, 서블릿 이름을 생략하고 디렉터리 위치까지만 지정한다면, 웹서버는 해당 디렉터리에서 웰컴 파일을 찾아 보내준다.
- 웰컴 파일을 설정해놓지 않으면 디렉터리 위치까지만 요청했을 경우, 404오류 페이지를 출력한다.

## GET Request(HTTP Request Method)
  1. 웹 브라우저 주소창에 URL을 입력하는 경우
  2. 링크를 클릭하는 경우
  3. <form>태그의 method 속성값이 get인 경우<br>
  
          URL에 데이터를 포함하고 있어 데이터 조회에 적합
          바이너리 및 대용량 데이터 전송 불가
  
## HttpServlet
  - HttpServlet(abstrac class) --> GenericServlet(abstrac class) --> Servlet(Interface)
          
          - doGet(HttpServletRequest response, HttpServletResponse response), doPost(HttpServletRequest request, HttpServletResponse response)
          - HttpServletRequest 
            주요 메소드 : setCharacterEncoding("UTF-8")->doPost메소드에서 쓰인다. doGet인 경우 server.xml에서 설정, getParameter(String name) ...

          - HttpServletResponse
            주요 메소드 : setContentType("text/html;charset=UTF-8"), getWriter() ...
  
## JDBC(Java Database Connectivity)
  - 자바에서 데이터베이스에 접속할 수 있게 도와주는 자바 API. == 데이터베이스에 접속할 수 있게 해주는 인터페이스.
  - 이 인터페이스를 통해서 자바 어플리케이션과 DBMS의 통신을 가능하게 해준다. 
  - <자바 앱> ~ <JDBC(Interface)> ~ (DBMS protocol) ~ <데이터베이스>
        
          WEB-INF/lib/mysql-connector-java-version.bin.jar) <-에 포함된 com.mysql.jdbc.Driver 클래스를
          자바 정적 클래스 DriverManager의 registerDriver()를 이용하여 구현하거나 정적 클래스 Class의 forName()을 이용하여 구현.
          -> 위와 같이 드라이버를 구현한 다음, DriverManager의 getConnection()을 호출하여 DB서버에 연결.
          -> 연결이 성공하면, DB 접속 정보인 java.sql.Connection 인터페이스 구현체를 반환 받는다.
          -> Connection의 createStatement()등의 메소드를 호출하여 SQL문을 담아낼 java.sql.Statement 인터페이스 구현체를 반환시킬 수 있다.
          -> SQL문을 담아낸 다음, Statement의 executeQuery()를 통해 DB서버에 SQL문을 보낸 뒤, 서버의 질의결과를 java.sql.ResultSet 인터페이스 구현체로 반환받는다. 
                                                          
  
## POST Request(HTTP Request Method)
  -  <form>태그의 method 속성값이 post인 경우<br>
  
          URL에 데이터가 포함X
          메시지 본문에 데이터를 포함 -> 실행 결과 공유X
          바이너리 및 대용량 데이터 전송 가능
  
## Refresh
  - 일정 시간이 지나고 자동으로 서버에 요청을 보내는 방법.
  - 응답 헤더를 이용하여 리프래시 정보를 보낼 수 있다.
  
  ``` java
  response.addHeader("Refresh", "1;url=list");
  ```
  
  - HTML 본문의 meta 태그를 이용하여 리프래시 정보를 보낼 수 있다.
  
  ``` html
  <meta http-equiv='Refresh' content='1; url=list'>
  ```
  
## Redirect
  - 즉시 다른 페이지로 이동.
   ``` java
  response.sendRedirect("URL");
  ```
  
  
## Servlet(서블릿)
- 서버와 프로그램이 데이터를 주고 받을 수 있도록 HTTP를 따르는 자바로 작성된 프로그램. <br>CGI(Common Gateway Interface) 프로그램 중 자바로 작성된 프로그램이다. 

## Servlet Container(서블릿 컨테이너)
- 서브릿을 대신하여 CGI(Common Gateway Interface)규칙에 따라 웹 서버와 데이터를 주고 받는 프로그램. <br> 덕분에 서블릿을 개발할 때 CGI규칙을 신경 쓸 필요가 없어졌다. 
- 서블릿 컨테이너는 클라이언트로 요청을 받으면 해당 서블릿을 찾아본다. 만약 없다면 해당 서블릿의 인스턴스를 생성한다. <br>한 번 서블릿 객체가 생성되면 웹 애플리케이션이 종료될 때까지 유지한다.
- 대표적인 프로그램으로 "톰캣"이 있다.
  
## Servlet Init Parameter(서블릿 초기화 매개변수)
  - 서블릿을 생성하고 초기화 할 때, 즉 서블릿 컨테이너가 'init()을 호출할 때 전달하는 데이터'.
  - 보통 데이터베이서 연결 정보, 시스템 환경 정보 같은 정적인 데이터를 서블릿에 전달할 때 사용.
  - DD 파일의 서블릿 배치 정보에 설정하거나, 서블릿 소스코드에서 애노테이션을 사용하여 설정한다.
 ```html
 <init-param>
     <param-name>매개변수 이름</param-name>
     <param-value>매개변수 값</param-value>
 </init-param>
  - 서블릿 코드에서 초기화 매개변수 꺼내기 ->
 ```java
 this.getInitParameter("매개변수 이름"); //반환하는 값은 문자열임.
 ```

  
