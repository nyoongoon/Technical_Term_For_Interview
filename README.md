# Technical_Term_For_Interview
면접을 위한 기술 용어를 정리합니다.
<br>
이해를 돕기 위해 개인적인 방식으로 풀어서 서술합니다.

<br>

## Buffer

- Java에서 버퍼를 비워줘야 하는 경우
```java
//숫자 입력 후 바로 문자를 입력 받는 경우 버퍼에 있는 데이터를 가져오게 된다.
sc.nextLine(); //\n을 비워준다
```

## CGI(Common Gateway Interface)
- 웹서버와 프로그램 사이의 데이터를 주고 받는 규칙.

## Class\<T\>
```java
public final class Class<T> extends Object implements Serializable, GenericDeclaration, Type, AnnotatedElement
```
- 주요 메소드
 ```java
 static Class<?> forName(String className) {
 //인자값으로 클래스 이름을 넘기면 해당 클래스를 찾아 로딩한다. 클래스 이름은 반드시 패키지 이름을 포함해야한다.
 //로딩된 클래스는 자신의 static 블록을 실행시킨다.
 }
 ```
## Context Init Parameter(컨텍스트 초기화 매개변수)
 - 같은 웹 애플리케이션에 소속된 서블릿들이 공유하는 매개변수.
 - 1. DD파일에 설정하거나, 애노테이션으로 설정 가능
 ```html
<comtext-param>
    <param-name>매개변수 이름</param-name>
    <param-value>매개변수 값</param-value>
</comtext-param>
```
- 서블릿 코드에서 초기화 매개변수 꺼내기 --> 컨텍스트 초기화 매개변수를 얻기위해 ServletContext객체가 필요함.<br> HttpServlet으로부터 상속받은 getServletContext()를 호출하여 얻을 수 있다.
```java
ServletContect sc = this.getServletContext();
sc.getInitParameter("args");
```

## DAO(Data Access Object)
- 데이터베이스와 연동하여 데이터를 처리하는 모델 컴포넌트.
- 보통 하나의 DB테이블이나 DB뷰에 대응한다.

         웹 브라우저 - 서블릿(컨트롤러) - DAO(모델) - DBMS
                         |       \     |
                       JSP(뷰)   -  값 객체(VO)
                       
## DataSource
- javax.sql 확장 패키지 
         
         javax.sql 패키지는 java.sql 패키지의 기능을 보조하기 위해 만든 확장 패키지로 제공하는 주요 기능은 다음과 같다.
         1. DriverManager를 대체할 수 있는 DataSource 인터페이스 제공
         2. Connection 밑 Statement 객체의 풀링
         3. 분산 트랜잭션 처리
         4. Rowsets의 지원

**DataSource 의 장점**
- 1. DataSource는 서버에서 관리하기 때문에 데이터베이스나 JDBC 드라이버가 변경되더라도 애플리케이션을 바꿀 필요가 없다.

         ----------------                        ------------------              
         | (DataSource)-|--> DBMS                | (   WebApp     )|        ->  DriverManager를 사용하는 경우 웹 애플리케이션에서 관리하기 때문에
         |  =========== |                        | (              )|           데이터베이스의 주소가 바뀐다거나, JDBC드라이버가 변경될 경우 웹 애플리케이션의 코드도 변경해야함!
         | (WebApp)     |                        | (DriverManager-)|--> DBMS
         ---------------                         ------------------
            톰캣 서버                                     톰캣 서버       
- 2. DataSource는 자체적으로 커넥션 풀 기능을 구현하기 때문에 웹 애플리케이션에서 따로 작업할 것이 없음.  
  
## DB Connection Pool (DB 커넥션 풀)
- DB커넥션 객체를 여러개 생성하여 풀(Pool)에 담아 놓고 필요할 때 꺼내 쓰는 방식.
- cf) 자주 쓰는 객체를 미리 만들어두고, 필요할 때마다 빌리고 사용한 다음 반납하는 방식을 풀링(pooling)이라고 함.
- _**싱글 커넥션 사용의 문제점**_ :
        
                    Connection
               /         |        \
          Statement   Statement  Statement  
              |          |          |
             DAO1       DAO2       DAO3 --> 예외 발생 --> 롤백 --> 커넥션 자체를 롤백해야함!!
             
- 싱글 커넥션을 사용하면 하나의 DAO가 롤백 기능을 호출할 경우, Statement엔 롤백 기능이 없기 때문에, 그 커넥션을 통해 이뤄지는 다른 모든 작업도 롤백이 됨.
- DB커넥션 풀을 사용하면, 각 요청에 대해 별도의 커넥션 객체를 사용하기 때문에 다른 작업에 영향을 주지 않는다.
- 또한, DB커넥션 객체는 버리지 않고 풀에 보관해두었다가 재사용하기 때문에, 가비지가 생성되지 않고 속도도 빨라진다. 
- Java DB커넥션 풀 만들기 코드
  https://blog.naver.com/nyoongoon/222414537038

## DD(Depolyment Desciptor)
- 웹 애플리케이션의 배치 정보를 담고 있는 파일.
- 서블릿에서는 web.xml이 DD파일이다. 
- web.xml의 <welcome-file-list>태그를 이용하여 웰컴 파일을 설정할 수 있다. 
- 웹 서버에게 요청할 때, 서블릿 이름을 생략하고 디렉터리 위치까지만 지정한다면, 웹서버는 해당 디렉터리에서 웰컴 파일을 찾아 보내준다.
- 웰컴 파일을 설정해놓지 않으면 디렉터리 위치까지만 요청했을 경우, 404오류 페이지를 출력한다.
- \<load-on-startup\> : 클라이언트 요청이 없어나 오기 전에 미리 배치할 서블릿을 생성할 때 사용한다.
 
 ``` html
 <load-on-startup>숫자(생성순서)</load-on-startup>
 ```
 
## EL(Expression Language)
 - 콤마와 대괄호를 사룔하여 자바 빈(인스턴스)의 프로퍼티나, 맵, 리스트, 배열의 값을 쉽게 꺼낼 수 있게 해주는 기술. static으로 선언된 메소드를 호출할 수도 있다.
 - JSP에선 주로 보관소(내장 객체)에 들어있는 값을 꺼낼 때 사용함. cf)\<jsp:useBean\>과 다른 점은 EL로는 객체 생성 불가하다는 점.
 - ${}은 즉시 적용(immediate evaluate) : JSP가 실행될 때 페이지에 즉시 반영됨.
 - #{}은 지연 적용(deferred evaluation) : 시스템이 필요하다고 판단될 때 사용. --> JSF에서 UI만들 때 주로 사용하기 때문에 자주 사용 X
 ``` java
 ${member.no} //또는
 ${member["no"]}
 // 위를 자바 코드로 나타낸다면
 Member obj = (Member)pageContext.findAttribute("member");
 int value = obj.getNo(); //내장객체 PageContext의 findAttribute()는 작은 순서에 따라 보관소를 뒤져서 객체를 찾는다. 마지막까지 없다면 null반환.
 ${requestScopte.member.no} //ServletRequest 보관소에서 값 꺼내기.
 ```
 
## Filter(javax.servlet.Filter_서블릿 필터) 
 - 필터는 서블릿 실행 전후에 어떤 작업을 하고자 할 때 사용하는 기술. 암호 해제, 자원 준비, 로그 남김 같은 작업들을 필터를 통해 처리할 수 있음.
 - 필터의 배치 방법 : 1.DD파일에 배치 정보 설정. 2. 애노테이션으로 기술하여 설정.
 ```html
 <filter>
     <filter-name>필터 이름</filter-name>
     <filter-class>필터 위치</filter-class>
     <init-param>
          <param-name>이름</param-name>
          <param-value>값</param-value>
     </init-param>
 </filter>
 <filter-mapping>
     <filter-name>이름</filter-name>
     <url-pattern>필터가 적용되어야하는 URL(/*는 모든 요청에 대해 이 필터를 적용하라는 뜻)</url-pattern>
 </filter-mapping>
 ```
 - 주요 메소드
 ```java
 void init(FilterConfig config)throws ServletException{
  //필터 객체가 생성되고 나서 준비작업을 위해 한 번 호출됨.
  //FilterConfig 객체를 통하여 필터 초기화 매개변수의 값을 꺼낼 수 있음.
 }
 void doFilter(ServletRequest request, ServletResponse response, FilterChain nextFilter) throws IOException, ServletException{
  //서블릿이 실행되기 전에 해야할 작업
  //다음 작업을 호출. 더이상 필터가 없다면 서블릿의 service()호출
  nextFilter.doFilter(request, response);
  //서블릿을 실행한 후, 클라이언트에게 응답하기 전 해야할 작업.
 }
 ```
 
## GET Request(HTTP Request Method)
  1. 웹 브라우저 주소창에 URL을 입력하는 경우
  2. 링크를 클릭하는 경우
  3. <form>태그의 method 속성값이 get인 경우<br>
  
          URL에 데이터를 포함하고 있어 데이터 조회에 적합
          바이너리 및 대용량 데이터 전송 불가
  
## HttpServlet
  - HttpServlet(abstrac class) --> GenericServlet(abstrac class) --> Servlet(Interface)
          
          - doGet(HttpServletRequest response, HttpServletResponse response), doPost(HttpServletRequest request, HttpServletResponse response)
## HttpServletRequest 
   - 주요 메소드 
 
``` java
setCharacterEncoding("UTF-8"){ //doPost메소드에서 쓰인다. doGet인 경우 server.xml에서 설정 }
getParameter(String name){}
// RequestDispatcher 객체 생성
// 다른 서블릿이나 JSP로 작업을 위임할때 사용. forward(제어권 돌아오지않음)하거나 include(제어권 돌아옴)한다
 RequestDispatcher rd = request.getRequestDispatcher("URL");
 rd.include(request, response);
 //혹은 jsp태그를 활용
 <jsp:include page="URL">
void setAttribute(String name, Object o){
 // 주로 forward()나 include()안에 담긴 객체를 공유하기 할 때, setAttibute를 사용하여 공유할 객체에 값을 보관할 수 있다.
 } 
 // 세션 내장객체를 가져올 수 있다
 HttpSession session = request.getSession();
 
 
 
```
 
## HttpServletResponse
   - 주요 메소드 
           
           setContentType("text/html;charset=UTF-8"), getWriter() ...
  
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

## JNDI  
  
## JSP(JavaServerPage)                                                          
 - 화면 생성을 쉽게 해주는 기술. 뷰 컴포넌트를 만들 때 사용한다. <br>
 
          1. 개발자는 서버에 JSP 파일을 작성해 둔다. -> 클라이언트가 JSP를 실행해달라고 요청하면 서블릿 컨테이너는 JSP파일에 대응하는 자바 서블릿을 찾아서 실행한다.
          2. JSP에 대응하는 서블릿이 없거나 JSP파일이 변경되었다면 JSP 엔진을 통해 JSP파일을 해석하여 서블릿 자바 소스를 생성.
          3. 서블리 자바 소스는 자바 컴파일러를 통해 서블릿 클래스 파일로 컴파일 됨. JSP파일을 바꿀 때마다 이 과정을 반복.
          4. 컴파일된 서블릿의 service()가 호출되면, 출력 메서드를 총해 서블릿이 생성한 HTML화면을 웹 브라우저로 보냄.
 - JSP엔진은 JSP파일로부터 서블릿 클래스를 생성할 때 HttpJspPage 인터페이스를 구현한 클래스를 만든다.
 
          - HttpJsp(interface) -> JspPage(interface) -> Servlet(interface)
            _jspService()         jspInit(), jspDestory()
              ㄴ> 서블릿컨테이너가      ㄴ> 오버라이딩 주의(Init(),Destory()X)
                  service()호출하면
                  이 메소드가 호출됨
 - _jspService()에 선언된 로컬 변수중에서 다음의 참조 변수는 반드시 이 이름으로 존재해야함. 
          
          - PageContext pageContext, HttpSession session, ServletContext application, ServletConfig config; JspWriter out, Object page
 - 아래의 객체들은 이 메서드가 호출될 때 반드시 준비되는 객체들이기 떄문에, 이 객체들을 가르켜 JSP 내장객체(Implicit Object)라고 한다.
          
          - request, response, pageContext, session, application, config, out, page, exception
 - JSP 전용 태그 : 지시자, 스크립트릿, 선언문, 표현식
          
           지시자
           %@ ... %>는 지시자로 웹 컨테이너가 JSP 페이지를 Servlet 클래스로 변환할 때, 필요한 여러 가지 정보들을 기술하기 위해 사용 됨.
            page 지시자 : <%@page 속성 목록 %>
            include 지시자 : <%@include 속성 목록 %>
            taglib 지시자 : <%@taglib 속성 목록 %>
  
           스크립틀릿(Scriptlet) 
           <% %>
           JSP페이지에서 JAVA언어를 사용하기 위한 요소 중 가장 많이 사용되는 요소.

           선언(Declaration) 
           <%! %>
           JSP페이지 내에서 사용되는 변수 또는 메소드를 선언할 때 사용되며, 선언된 변수 및 메소드는 전역의 의미로 사용. 
           스크립틀릿과의 차이점은 스크립틀릿에서 변수를 선언하면 지역변수로 선언됨. 

           표현식(Expression) 
           <%= %>
           JSP페이지 내에서 사용되는 변수 또는 리턴값이 있는 메소드 결과값을 출력하기 위해 사용. 
           결과값은 String 타입이며, 세미콜론 사용불가
 
## JSP Implicit Object(JSP 내장 객체를 데이터 보관소로 활용하기)
- 데이터 보관소 역할 : 서블릿은 데이터를 공유하기 위해 네가지 종류의 JSP 내장 객체를 활용하여 데이터 보관소로 사용한다.
- 공유 범위 순서 : application(ServletContext) > session(HttpSession) > request(ServletRequest) > pageContext(JspContext)             
          
            1. ServletContext 내장 객체 : 웹 애플리케이션이 시작될 때 생성되어 웹 애플리케이션이 종료될 때까지 유지. ->  참조변수 : application
            
            2. HttpServlet : 클라이언트의 최초 요청시 생성되어 브라우저를 닫을 때까지 (클라이언트 당 한 개 생성)-> :session
            - 주요 메소드
            session.invalidate(); // 세션 객체를 제거.
            
            
            3. ServletRequest : 클라이언트의 요청이 들어올 때 생성되어 클라이언트에게 응답을 보낼 때까지. (포워딩이나 인클루딩을 통해 협업하는 Servlet끼리 데이터 공유 가능)-> : request
            
            4. JspContext : JSP페이지를 실행하는 동안 -> : pageContext
            - JspContext 와 JSP 로컬변수와 차이점 : JspContext 태그 핸들러에게 데이터를 접근하고자 할 때 사용. (태그 핸들러가 로컬변수에는 접근 못함)
            - cf) <jsp:include>같은 액션 태그의 값을 다루는 객체를 '태그 핸들러'라고 한다. -> (커스텀태그를 잘 사용하지 않기 때문에 JspContext도 잘 사용하지 않는다.)
            
            : 참조변수.setAttribute(키, 값);
            : 참조변수.getAttribute(키);
            
## JSTL(JSP Standard Tag Library) 
- JSP 태그의 확장 라이브러리
            
## MVC(model-view-controller)
 - 서비스와 제품의 주기가 짧아짐으로써 코드의 재사용성을 높이기 위해 고안된 아키텍쳐.
 - 모델 : 데이터를 다루는 일.
 - 뷰 : 화면을 만드는 일.
 - 컨트롤러 : 클라이언트의 요청을 받아 모델 컴포넌트를 호출하거나, 전달하기 쉬운 방식으로 데이터를 가공함. <br>
            모델이 수행 작업을 가지고 뷰에게 전달. ;일종의 조정자.
  
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
 ```
  - 서블릿 코드에서 초기화 매개변수 꺼내기 ->
 ```java
 this.getInitParameter("매개변수 이름"); //반환하는 값은 문자열임.
 ```
## ServletContextListener
- 서블릿 컨테이너(톰캣)는 웹 어플리케이션(Servlet/JSP)의 상태를 모니터링 할 수 있도록 시작에서 종료까지 주요한 사건에 대해 알림 기능을 제공.
- 사건(event)이 발생했을 때 알림을 받는 객체를 '리스너(Listener)'라고 하고, DD 파일에 등록하여 사용한다. 
            
            서블릿 컨테이너 -(감시)-> 웹 애플리케이션 -(사건)-> 서블릿 컨테이너 -(리스너의 메소드 호출)-> 리스너
- DAO처럼 여러 서블릿이 사용하는 객체는 공유하는 것이 좋다. -> ServletContextListener  구현 하기
