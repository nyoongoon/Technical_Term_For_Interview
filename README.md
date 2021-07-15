# Technical_Term_For_Interview
면접을 위한 기술 용어를 정리합니다.
<br>
이해를 돕기 위해 개인적인 방식으로 풀어서 서술합니다.

<br>

## Annotation(애노테이션)
- 컴파일이나 배포, 실행할 때 참조할 수 있는 주석. 애노테이션을 사용하여 클래스나, 필드, 메서드에 대해 부가 정보를 등록할 수 있다. 

- 애노테이션 유지 정책
```java
@Retention(RetentionPolicy.RUNTIME) // RetentionPolicy.SOURCE : 소스 파일에서만 유지. 컴파일할 때 제거됨. 클래스 파일에 애노테이션 정보 남아있지 않음.
			            // RetentionPolicy.CLASS : 클래스 파일에 기록됨. 실행 시에는 유지되지 않음. 실행 중에는 애노테이션 값을 꺼낼 수 없음(기본정책)
	 		            // RetentionPolicy.RUNTIME : 클래스 파일에 기록됨. 실행 시에도 유지됨. 즉, 실행 중에 클래스에 기록된 애노테이션 값 참조 가능
```

- 주의할 점 : DataSource와 같은 톰캣 서버가 제공하는 객체에는 애노테이션을 적용할 수 없다.

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
 
 isInstance(Object obj) : 주어진 객체가 해당 클래스 또는 인터페이스의 인스턴스인지 검사
 
 clazz.getAnnotation(Component.class).value(); // 클래스로부터 애노테이션 추출. 애노테이션에 정의된 메서드를 호출하여 속성값을 꺼냄. @Component의 기본 속성값을 꺼내고 싶으면, value()호출.
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

## DataBinding(interface)
- 입력한 값을 도메인 모델에 맞춰 자동으로 변환 후 할당해주는 **'동적 데이터 변환 인터페이스'** 이다.
- 프론트 컨트롤러가 페이지 컨트롤러를 실행하기 전에, 원하는 데이터가 무엇인지 묻기 때문에 이에 대한 호출 규칙을 정의해 놓아야한다.
- 페이지 컨트롤러 중에서, **'클라이언트가 보낸 데이터가 필요한 경우'**. DataBinding(interface)를 구현한다. 

``` java
public interface DataBinding{
    Object[] getDataBinders();
}
```

- getDataBinders()의 반환값은 필요한 데이터의 이름과 타입의 정보를 담은 Object배열이다. 

``` java
public Object[] getDataBinders(){
    return new Object[]{
        "member", vo.Member.class // 이 페이지 컨트롤러에서 필요한 데이터 이름과, 타입의 정보를 명시해둔다.
    }
```
- 프론트 컨트롤러 입장에서, 이 인터페이스를 구현해놓은 페이지 컨트롤러를 호출할 때만 VO객체를 준비하면 된다. 
- getDataBinders()에서 지정한 대로, 프론트 컨트롤러가 VO객체를 무조건 생성할 것이기 때문에 **객체의 존재 유무로 분기문을 작성하면 안된다.**

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
- BasicDataSource -> implements ->  DataSource
- DataSource 가 만들어 주는 Connection 객체는 커넥션 대행 객체 안에 포장되어 있는 객체이다.
        
        DataSource는 DriverManager가 생성한 커넥션을 리턴하는 것이 아니라, 커넥션 대행 객체(Proxy Object)를 리턴한다.
        아파치 DBCP 컴포넌트의 BasicDataSource를 사용할 경우, PoolableConnection 객체를 반환해준다. 
        이 대행 객체 안에는 진짜 커넥션을 가리키는 참조변수 _conn과 커넥션 풀을 가리키는 _pool이 들어있다.
        따라서 DataSource가 만들어준 커넥션 대행 객체에 대해 close()를 호출하면, 커넥션 대행 객체는 진짜 커넥션 객체를 커넥션 풀에 반납한다.
        
**톰캣 서버에 DataSource 설정하기** (서버에서 관리하는 DataSource사용)
     
        1. context.xml 편집.
        2. 프로젝트의 DD파일에 서버 자원 참조한다는 선언해준다.
        3. InitialContext의 lookup()메소드를 이용하여 JNDI이름으로 등록되어 있는 서버 자원을 찾기. 
        4. context.xml에 자동으로 close하라고 설정을 해두면, 자원을 따로 해제하는 코드를 넣지 않아도 된다.
        
         
 

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
	
## Dependency(의존성)
- 두 모듈간 연결성이 있는 상태를 의존성이라고 한다. 
- 객체지향 언어에는 두 클래스간의 관계. 
		
	 	ex)페이지 컨트롤러가 작업을 수행하려면 데이터베이스로부터 정보를 가져다줄 DAO가 필요. 
			    Controller --(Dependency)--> DAO 

- 이렇게 *특정 작업을 수행할 때 사용하는 객체*를 **의존 객체**라고 하고 이런 관계를 **의존 관계**라고 함.
- **의존성 증가에 따른 문제**
1. 코드의 잦은 변경 : 의존객체를 사용하는 쪽과 의존객체 사이의 결합도가 높아져서 의존객체에 변경이 발생하면 바로 영향을 받게 됨	
2. 대체가 어렵다. : 다른 데이터베이스를 사용한다면 SQL문을 그에 맞게 변경해야한다. 
- **해결책 : 의존 객체를 외부에서 주입 (Dependency Injection)**
- 빈 컨테이너(Java Beans Container)는 객체가 실행 되기 전에 그 객체가 필요로 하는 의존 객체를 주입해 주는 역할을 함.	
- 좀 더 일반적인 말로 역제어(IoC; Inversion Of Control)이라 함.
		
			              빈 컨테이너
				         ↓ 1.주입
		Controller --2.사용-> DAO
	
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
## FrontController(프론트 컨트롤러)
- 컨트롤러에서, 요청 데이터를 처리하는 코드나, 모델과 뷰를 제어하는 코드가 중복되는 경우를 줄이기 위해서 프론트 컨트롤러를 사용한다.
- 프론트 컨트롤러 디자인 패턴에서는 **프론트 컨트롤러**와, **페이지 컨트롤러** 두 개의 컨트롤러를 사용한다. 
- 프론트 컨트롤러에서는 VO 객체의 준비, 뷰 컴포넌트로의 위임, 오류 처리 등과 같은 공통 작업을 담당.
- 페이지 컨트롤러는 이름 그대로 요청한 페이지만을 위한 작업을 수행한다. (DAO를 사용하여 데이터를 준비, ServletRequest 보관소에 데이터 저장, 프론트 컨트롤러에게 뷰 정보(JSP URL)를 반환.

 ``` java
 @WebServlet(*.do)
 public class DispatcherSerlvet extends HttpServlet{
     protected void service(~~){//get, post 뿐만 아니라 다앙햔 요청 방식에 대응.    
         //프론트 컨트롤러의 역할
         //1. 요청 URL에서 서블릿 경로 알아내기
         String servletPath = request.getSetvletPath(); //URL에서 서블릿 경로만 추출하는 메소드.
         //2. 페이지 컨트롤러로 위임
         //3. 요청 매개변수로부터 VO 객체 준비
         //4. JSP로 위임
         //5. 오류처리
         //6. 프론트 컨트롤러의 배치 : @WebServlet(*.do)를 사용하여. 클라이언트 요청 중에서 서블릿 경로 이름이 .do로 끝나는 경우 이곳에서 처리.
      }
 }
 ```
- 페이지 컨트롤러를 일반 클래스로 전환하면 서블릿 기술에 종속되지 않아 재사용성이 높아진다.
- 마찬가지로 이유로, 프런트 컨트롤러와 페이지 컨트롤러 사이에서 데이터를 주고 받을 때, Map객체를 사용하여 서블릿 기술의 종속을 줄인다. 

``` java
public interface Controller {
	String execute(Map<String, Object> model) throws Exception;
	 //execute는 프론트 컨트롤러가 페이지 컨트롤러에게 일을 시키기 위해 호출하는 메서드. 반환값은 화면 출력을 수행하는 JSP의 URL
	 //페이지 컨트롤러는, 프론트 컨트롤러가 담아준 객체를 map으로 꺼내고, DAO에서 작업한 결과를 map에 담아 프론트 컨트롤러에 보낸다
	 //map에 저장된 값은 프론트 컨트롤러가 꺼내서 ServletRequest 보관소에 저장시키고, JSP가 꺼내서 사용한다. 	
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

## JNDI(Java Naming and Directory Interface)  
  - 디렉터리 서비스에 접근하는데 필요한 API이며, 애플리케이션은 이 API를 이용하여 서버의 자원을 찾을 수 있다. 
  - cf) *자원*은 데이터베이스 서버나 메시징 시스템 같이, 다른 시스템과의 연결을 제공하는 객체. 특히, JDBC자원을 데이터 소스라고 부른다.
  - Java EE 애플리케이션 서버에서 자원을 찾을 때 기본 JNDI 이름
           
          java:comp/env               : 응용 프로그램 환경 항목
          java:comp/env/jdbc          : JDBC 데이터 소스
          java:comp/ejb               : EJB 컴포넌트
          java:comp/UserTransaction   : UserTranscation 객체
          java:comp/env/mail          : JavaMail 연결 객체
          java:comp/env/url           : URL 정보
          java:comp/env/jms           : JMS 연결 
  
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
  
## Mybatis
- 퍼시스턴스 프레임워크 : 이전의 저장한 데이터를 다시 불러올 수 있는 기술(퍼시스턴스)을 가진 프레임워크.		   		  
- Mybatis는 SQL문장으로 직접 DB를 다루는 SQL맵퍼.
- Mybatis의 핵심은 개발과 유지보수가 쉽도록 소스 코드에 박혀있는 SQL을 별도의 파일로 분리, 캡슐화 하는 것.
- Mybatis 프레임워크의 핵심 컴포넌트 
		   
		   SqlSession                  : 실제 SQL을 실행하는 객체. 이 객체는 SQL을 처리하기 위해 JDBC드라이버를 사용함.
		   SqlSessionFactory           : SqlSession 객체를 생성함.
		   SqlSessionFactoryBuilder    : mysql 설정 파일의 내용을 토대로 SqlSessionFactory를 생성함.
		   mybatis 설정 파일             : DB연결 정보, 트랜잭션 정보, mybatis제어정보 등의 설정 내용을 포함. SessionFactory를 만들 때 사용 됨.
		   SQL 맵퍼 파일                 : SQL문을 담고 있는 파일. SqlSession 객체가 참조함.

- SqlSessionFactory(의존객체) : SQL를 실행할 때 사용할 도구를 만들어 주는 객체
- SqlSession : SQL을 실행하는 객체. 이 객체가 있어야만 SQL문을 실행할 수 있다. 이 객체는 직접 생성할 수 없고, SqlSessionFactory를 통해서만 얻을 수 있다.
```java
SqlSession sqlSession = sqlSessionFactory.openSession();
	try {
		return sqlSession.selectList("dao.ProjectDao.selectList");
		   			//dao.ProjectDao --> sql맵퍼의 네임스페이스 이름
		   			//selectList --> sql문의 아이디				
	}finally {
		sqlSession.close();
	}		   
```		   
- 사용을 완료한 후, close()를 호출하여 SQL문을 실행할 때 사용한 자원을 해제 해야한다.
- SqlSession의 주요 메소드
		   
		   selectList() 
		   selectOne()
		   insert()
		   update()
		   delete()
- selectList(String sqlId) : 호출할 때 넘기는 매개변수 값은 SQL아이디. **SQL아이디**는 SQL맵퍼에서의 **네임스페이스 이름**과 **SQL문의 아이디**를 결합하여 만든 문자열.
- selectList(String sqlId, Object parameter) --> sql문을 실행하는데 값이 필요하다면 두 번째 매개변수로 값을 담은 객체를 넘기면 됨 
- commit()과 rollback()메소드 : 운영 데이터베이스에 적용/취소.
- 자동커밋 기능 : 편리하지만 트랜잭션을 다를 수 없음.		   
```Java
	SqlSession sqlSession = sqlSessionFactory.openSession(true)
```		   
<br>		   

### * SQL맵퍼
- #{프로퍼티명} 자리에 객체의 프로퍼티 값이 놓인다. **객체의 프로퍼티란 겟터/셋터를 가리키는 용어** 프로퍼티 이름은 겟터/셋터 메서드의 이름에서 추출.
		   
		   Date getStartDate(){...}
		   void setStartDate(Date d){...}
		   	      ⬇
		   (get/set)StartDate
		   	      ⬇
		   프로퍼티 이름 : startDate
- 객체가 아닌 기본 데이터 형으로 전달할 때 -> 자동 포장(Auto-boxing)되어 전달됨. 
- Integer클래스 같은 랩퍼(wrapper) 클래스에서는 프로퍼티를 의미하는 겟터 메서드가 없기 때문에, 기본 타입의 객체(랩퍼(wrapper) 객체)로부터 값을 꺼낼 때는 아무 이름이나 사용할 수 있음.		   

		   
		   
## POST Request(HTTP Request Method)
  -  <form>태그의 method 속성값이 post인 경우<br>
  
          URL에 데이터가 포함X
          메시지 본문에 데이터를 포함 -> 실행 결과 공유X
          바이너리 및 대용량 데이터 전송 가능

## Properties (java.util.Properties)
- 이름=값 형태로 된 파일을 다룰 때 사용하는 클래스. 
		   
			 < Properties 객체 >
	        ex).      키      ||      값
		jndi.dataSource  ||  java:comp/env/jdbc/db    

- 주요 메소드
```java
Properties props = new Properties();
props.load(new FileReader(propertiesPath)); //load()메소드는 FileReader를 통해 읽어들인 프로퍼티 내용을 키-값 형태로 내부 맵에 보관
props.keySet(); // Properties에 저장된 키 목록을 반환.				 
				 
```
				
				 
		   
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
  	   
## Reflection API
- 클래스나 메서드의 내부구조를 들여다 볼 때 사용하는 도구라는 뜻의 API. 
- 리플랙션 API를 활용하여 인스턴스 생성과 메소드 호출을 **자동화**할 수 있음. (<- 생성과 호출하기 위한 데이터 준비를 자동화)
- 주요 API
		   
		   Class.newInstance() : 주어진 클래스의 인스턴스를 생성
		   Class.getName() : 클래스의 이름을 반환
		   Class.getMethods() : 클래스에 선언된 모든 public 메서드의 목록을 배열로 반환
		   Method.invoke() : 해당 메서드를 호출
		   Method.getParameterTypes() : 메서드의 매개변수 목록을 배열로 반환

## Reflections Library
- 오픈소스 라이브러리로 자바에서 제공하는 reflection api 보다 쉽게 클래스를 찾거나 클래스의 정보를 추출할 수 있다.
```java
Refelctions reflector = new Reflections(""); // 매개변수 값은 클래스를 찾을 때 출발하는 패키지. 빈문자열은 classpath에 있는 모든 패키지 검색
Set<Class<?>> reflector.getTypeAnnotatedWith(Component.class); // 애노테이션이 붙은 클래스를 찾을 수 있다. ex)@Component가 붙은 클래스를 찾고 싶으면 앞의 코드처럼 애노테이션 클래스를 지정.
						     
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
