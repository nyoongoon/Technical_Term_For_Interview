# Technical_Term_For_Interview
면접을 위한 기술 용어를 정리합니다.
<br>
이해를 돕기 위해 개인적인 방식으로 풀어서 서술합니다.

<br>

## Ajax
### (회원가입시) Ajax를 사용하는 이유 1.
 1. 요청에 대한 응답을 html이 아닌 Data(Json)를 받기 위해서

- 기존의 C/S

 		     (회원가입 )요청
 		브라우저 	-->   서버
 			 <--
 			html로 응답 
 			(메인화면)

- 클라이언트가 꼭 브라우저이기만 하진 않음!
- ex) 안드로이드 앱  -> html 파일을 응답할 수 없음. -> 서버 각각 필요

**-> 서버가 데이터(JSON)만 리턴하게 된다면 여러 클라이언트에서 동일 서버 사용 가능**

- Ajax
 			   		
 		브라우저             --->   	서버
			1. (회원가입 요청)
				  <---
			2. data(JSON)로 응답 (정상)
				  --->
			3. 메인페이지 요청
				  <---
			4. html로 응답	  


		브라우저 	      --->   		서버
 			1. (회원가입 요청)
 				<---
 			2. data(JSON)로 응답 (정상)
 			<---	
 	    3. 앱 내부에서 화면이동

### (회원가입시) Ajax를 사용하는 이유 2.
2. 비동기 통신을 하기 위해 


``` javascript
//ajax호출시 default가 비동기 호출 
$.ajax({
	type:"POST", //방식
	url:"/api/user/join", //어느주소?
	//JSON<-자바스크립트오브젝튼
	data: JSON.stringify(data), //http body데이터
	contentType:"application/json; charset=utf-8", //body데이터가 어떤 타입인지(MIME)
	dataType:"json"//응답이 왔을 때 (기본적으로 모드것은 문자열로 옴(버퍼라서))-> 생긴게 JSON이라면 자바스크립트 오브젝트로 바꿔줌.
	
}).done(function(res){
	alert("회원가입이 완료되었습니다.");
}).fail(function(err){
	alert(JSON.stringify(err));
}); //ajax통신을 이용해서 3개의 데이터를 json으로 변경하여 insert 요청
```

- cf) 자바스크립트 오브젝트 vs JSON
```jsp
<script>
	let data = {
		username : "ssar",
		password : "1234",
		email : "ssar@nate.com"
	};
console.log(data); // 자바스크립트 오브젝트
console.log(JSON.stringigy(data)); //JSON 문자열
</script>
```
### (생활코딩 - jQuery Ajax)
- jQuery.ajax([settings])
- settings엔 Ajax 통신을 위한 옵션을 담고 있는 객체가 들어간다. 주요한 옵션을 열거 해보면 다음과 같다.
- data : 서버로 데이터를 전송할 때 이 옵션을 사용
- dateType : 서버측에서 전송한 데이터를 어떤 형식의 데이터로 해석 할 것인가를 지정. 값으로 올 수 있는 것은 xml, json, script, html. 형식을 지정하지 않으면 jQuery가 알아서 판단
- success 성공했을 때 호출할 콜백을 지정 
- type 데이터를 전송하는 방법을 지정. get/post

<br/><br/>

## Annotation(애노테이션)
- 컴파일이나 배포, 실행할 때 참조할 수 있는 주석. 애노테이션을 사용하여 클래스나, 필드, 메서드에 대해 부가 정보를 등록할 수 있다. 

- 애노테이션 유지 정책
```java
@Retention(RetentionPolicy.RUNTIME) // RetentionPolicy.SOURCE : 소스 파일에서만 유지. 컴파일할 때 제거됨. 클래스 파일에 애노테이션 정보 남아있지 않음.
			            // RetentionPolicy.CLASS : 클래스 파일에 기록됨. 실행 시에는 유지되지 않음. 실행 중에는 애노테이션 값을 꺼낼 수 없음(기본정책)
	 		            // RetentionPolicy.RUNTIME : 클래스 파일에 기록됨. 실행 시에도 유지됨. 즉, 실행 중에 클래스에 기록된 애노테이션 값 참조 가능
```

- 주의할 점 : DataSource와 같은 톰캣 서버가 제공하는 객체에는 애노테이션을 적용할 수 없다.
<br/><br/>

## AOP(관점지향 프로그래밍)
- 하나의 프로그램 방법론
- AOP란?
: Aspect Oriented Programming
: 개발, 운영할 때 활용하는 코드가 필요한 경우가 있다. (개발/운영 관점)
: Primary(Core) Concern(주업무)과 Cross-cutting Concern(부업무) 
-> 로그처리, 보안처리, 트랜잭션처리 
 
=> 여러 오브젝트에 나타나는 공통적인 부가 기능을 모듈화하여 재사용하는 기법.

-> 어떤 부가기능을? 언제? -> Advice
-> 어디에? -> Joinpoint, Pointcut

### Advice 종류
- 어떤 부가기능을 언제 사용할지에 대한 정의
- 메서드가 실행되기 전 -> @Before
- 메서드 정상실행 -> @AfterReturning
- 메서드 예외 -> @AfterThrowing
- 메서드 정상실행||예외 -> @After
- 메서드 실행 전후 -> @Around

### JoinPoint
- 어드바이스가 적용될 수 있는 위치를 나타냄
- 메서드 호출할 때 -> 스프링 aop는 이때만 어드바이스 적용
cf) 변수에 접근할 때, 객체를 초기화할 때, 객체에 접근할 때 -> 스프링X

### Pointcut
- Advicd를 적용할 Joinpoint를 선별하는 작업

### Target
: 부가 기능이 적용될 대상

### AOP는 어디에 사용될까?
- 성능 검사
- 트랜잭션 처리
- 로깅 

<br/><br/>

## API (Application Programming Interface)

- 응용 프로그램에서 운영 체제나 프로그래밍 언어가 **제공하는 기능을 제어**할 수 있게 만든 인터페이스

### API의 특징
- 구현과는 독립적으로 사양만 정의되어 있다.
- API에 따라 접근 권한이 필요할 수 있다.
ex) Java API, Open source API

### API vs Libaray 
- 라이브러리는 API들을 종류나 목적에 따라 나누어 정의한 API 묶음.

<br/><br/>


##  ApplicationContext (컨테이너 설정)

- IoC -> 미리 컨테이너에 띄워 놓기
- 두 가지 ApplicatinoContext
1. root-applicationContext.xml  (ContextLoaderListener가 실행)
: 어노테이션 Service, Repository 등을 스캔(메모리에 로딩)하고 DB관련 객체를 생성
2. servlet-applicationContext.xml (DispatcherServlet이 실행) 
: ViewResolver, Interceptor, MultipartResolver 객체를 생성, 웹과 관련된 어노테이션 Controller, RestController 를 스캔

<br/><br/>


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
## Encription(암호화)
	
### 대칭키 방식

		     (대칭)키 (양쪽에 똑같은 키를 사용해서 암호화, 복호화)
		    ↙
	  정보 -> 암호화(encription) -> 암호
	      <- 복호화(decription) <- 
		↖
		  (대칭)키


: 보편적으로 많이 사용하는 대칭키 방식 -> AES

### 비대칭키 방식 
- 암호화와 복호화 할때 사용하는 키가 다름!


		      키1
		    ↙
	  정보 -> 암호화(encription) -> 암호
	      <- 복호화(decription) <-
		    ↖
		     키2 

-> 비대칭키, 공개키, RSA 

: 공개키와 비공개키를 사용 
: 수신자는 공개키와 비공개키를 가짐
: 수신자는 공개키를 (인터넷에) 오픈
: 송신자는 받은 공개키를 가지고 암호화를 함.
: 수신자는 비밀키를 통해서 복호화를 할 수 있음

-> 공개키로 암호화를 하고 비공개키로 복호화를 한다. -> 비대칭키, 공개키 방식 -> RSA 
	
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
 <br/><br/>
	
## FrontController / RequestDispatcher / DispatcherServlet

------>  WebServer   -->
request  (요청이 URL, 혹은 자바파일 )

-> 톰캣 -> web.xml 
	   |
	   "~.do"(특정주소) 로 오면 frontcontroller가 낚아채기

-----> 
request

- 최초 앞단에서 request요청을 받음
- frontcontroller가 낚아채는 등의 새로운 req, res새롭게 new 될 수 있음

	-> 그래서 기존의 req, res를 유지하기 위해 아래의 RequestDispatcher가 필요

### RequestDispatcher(JSP)
- 필요한 클래스 요청이 도달했을 때 FrontController에 도착한 req, res를 그대로 유지시켜준다.

### DispatcherServlet 
- 스프링에서 DispatcherServlet은 FrontController + RequestDispatcher 이다.
- DispatcherServlet이 자동 생성 되어질 때 수 많은 객체가 생성(IoC)된다. 보통 필터들인데 개발자가 직접 등록할 수 도 있고 기본적으로 필요한 필터들은 자동 등록되어진다.	
 
<br/><br/>	
	
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
	 
## IoC 컨테이너
- 역제어(Inversino of Control) : 개발자가 작성한 코드의 흐름에 따라 제어가 이루어지는 것이 아니라, 외부에 의해 코드의 흐름이 바뀌는 것.
- -> DI(의존성 주입)는 IoC의 한 형태. 
- ex) 역제어의 사례 - 1. 이벤트
: 메시지를 작성하는 중이더라도 상대편으로부터 메시지를 받게 되면 즉시 메시지를 출력한다. -> 외부에서 발생한 이벤트에 의해 코드의 흐름이 바뀐 것.
- ex) 역제어의 사례 - 2. 의존성 주입 

```java
//예전 방식
class ProjectListController{
    public void execute(){
	 ProjectDao dao = new ProjectDao(); //의존 객체 직접 생성
	 ArrayList<Project> projects = dao.list();
...	 
```
	 
- 예전 방식은 자신이 사용할 객체(의존객체-Dependencies)를 자신이 직접 만들어서 씀

``` java	 
//의존성 주입
class ProjectListController{
    ProjectDao dao;
    
    public void setProjectDao(ProjectDao dao){ //외부에서 ProjectDao를 매개변수로 받아서 사용
	 this.dao = dao;
    }
	 
    public void execute(){
	 ArrayList<Project> projects = dao.list();
```
- 의존성 주입을 이용한 방식은 외부에서 사용할 객체(의존 객체)를 주입 받는다. 위의 코드에서 셋터 메서드를 통해 외부에서 ProjectDao객체를 주입받고 있다. 
	 
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

 
## jQuery
	 
1. 엘리먼트를 선택하는 강력한 방법
2. 선택된 엘리먼트들을 효율적으로 제어
3. 자바스크립트 라이브러리

### Hello World
1. \<script\>태크에 jQuery url 넣기

``` jsp
<html>
	<body>
		<div class = "welcome"></div>
		<div class = "welcome"></div>
		<script type="text/javascript" src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
		<script type="text/javascript">
			$('.welcome').html('hello world').css('background-color', 'yellow');
		</script>
	</body>
</html>
```
- jQuery 문법 
: $(제어대상).method1().method2();
    주어(->css 셀렉터 문법) //  서술어
cf)css 셀렉터 문법 -> 제어 대상에 명시된 클래스명을 가진 모든 엘리먼트들을 찾음
- jquery가 제공하는 모든 메소드들은 메소드가 리턴될 때, 제어했던 대상을 리턴. (꼬리에 꼬리를 물은 코딩 방식 가능-> 체인)

cf)  
	
	jquery를 이용해 form값을 Controller로 넘겨주도록 프로그래밍하였다. form은 실제론 보이지 않게 하기 위하여 type="hidden"으로 하고 class 이름을 정한다. 
	이 때 class가 아니더라도, id, name을 이용할 수 있다. jquery는 이 form에 값을 넣어주는 부분  $('.class이름').val(데이터); 형식으로 정해준다.

cf) 
		
	.val()
	.val()은 양식(form)의 값을 가져오거나 값을 설정하는 메소드입니다.
	
	문법 1
	.val()
	선택한 양식의 값을 가져옵니다. 예를 들어
	var jb = $( 'input#jbInput' ).val();
	은 아이디가 jbInput인 input 요소의 값을 변수 jb에 저장합니다.

	문법 2
	.val( value )
	선택한 양식의 값을 설정합니다. 예를 들어
	$( 'input#jbInput' ).val( 'ABCDE' );
	는 아이디가 jbInput인 input 요소의 값을 ABCDE로 정합니다.

	 
	 
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

		   			   build()              openSession()
		   SqlSessionFactoryBuilder ----> SqlSessionFactory ----> SqlSession 
		   		              |
		   	            (설계도 투입)|
		   			      |
		   		  	   mybatis 설정파일 <-- SQL맵퍼파일
							    
- SqlSessionFactoryBuilder : 복잡한 객체는 전문가를 통해 생성하도록 설계 -> 이런 객체 생성 패턴을 **빌더 패턴(Builder Pattern)**이라고 함.					   	
``` java		   
String resource = "dao/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
// build의 매개변수 값으로 mybatis 설정 파일의 입력 스트림을 넘겨줘야 함.
// 입력스트림을 얻기 위해서 mybatis에서 제공하는 Resources 클래스를 용.
// Resourcs의 getResourceAsStream()메서드를 사용하면 자바 클래스 경로에 있는 파일의 입력 스트림을 쉽게 얻을 수 있다.		   
```		   
- SqlSession : SQL을 실행하는 객체. 이 객체가 있어야만 SQL문을 실행할 수 있다. 이 객체는 직접 생성할 수 없고, SqlSessionFactory를 통해서만 얻을 수 있다.
``` java
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

## SQL맵퍼 파일
- #{프로퍼티명} 자리에 객체의 프로퍼티 값이 놓인다. **객체의 프로퍼티란 겟터/셋터를 가리키는 용어** 프로퍼티 이름은 겟터/셋터 메서드의 이름에서 추출.
		   
		   Date getStartDate(){...}
		   void setStartDate(Date d){...}
		   	      ⬇
		   (get/set)StartDate
		   	      ⬇
		   프로퍼티 이름 : startDate
- 객체가 아닌 기본 데이터 형으로 전달할 때 -> 자동 포장(Auto-boxing)되어 전달됨. 
- Integer클래스 같은 랩퍼(wrapper) 클래스에서는 프로퍼티를 의미하는 겟터 메서드가 없기 때문에, 기본 타입의 객체(랩퍼(wrapper) 객체)로부터 값을 꺼낼 때는 아무 이름이나 사용할 수 있음.		   
### * \<mappper\> 루트 엘리먼트 
- \<mapper\> 태그의 namespace 속성은 자바의 패키지처럼 SQL문을 묶는 용도
### * \<select\>, \<insert\>, \<update\>, \<delete\>엘리먼트
- id 속성 : 각각의 sql문을 구문하기 위해 id 속성을 사용
- resultType 속성 : select문을 실행하면 결과가 생성되는데, 이 결과를 담을 객체를 지정하는 속성이 resultType. resultType에는 클래스 이름이 온다. mybatis 설정파일에 그 클래스 이름에 대한 별명이 정의되어 있다면 resultType값으로 별명 사용 가능
- 칼럼과 셋터 메서드 : mybatis는 select결과가 저장된 resultType의 인스턴스를 생성후, 각 칼럼에 대응하는 셋터메서드를 찾아서 호출한다. 이때 셋서 메서드는 대소문자 구분없이 set을 뺸 메서드의 이름과 칼럼 이름이 같으면 된다. 칼럼의 이름과 일치하는 셋터가 없다면 값이 저장되지 않는다. 칼럼의 이름과 셋터가 일치하지 않는다면, select문을 작성할 때 칼럼에 별명을 붙이면 된다. 
### * \<resultMap\> 엘리먼트
- 칼럼에 별명을 붙이는 대신 \<resultMap\>을 이용하면 칼럼 이름과 셋터 이름의 불일치 문제를 해결할 수 있음. \<resultMap\> 태그의 type값은 칼럼 데이터를 저장할 클래스 또는 클래스의 별명.
		   
``` html
<resultMap type="project" id="projectResultMap"\
	<id column="pno" property="no"/> // <-- 칼럼이름과 셋터 메서드가 불일치할 경우 여기서 설정시켜줌!					      
	<result column="PNAME" property="title"/>
	<result column="CONTENT" property="content"/>
	<result column="STA_DATE" property="startDate" javaType="java.sql.Date"/>
...
</resultMap>									       
```		   
### * \<id\> 엘리먼트
- \<id\>태그에서 지정한 프로퍼티는 객체의 식별자로 사용됨. select를 통해 생성된 결과 객체들은 별도의 보관소에 저장(캐싱, caching)해두고, 다음 select를 실행할 때 재사용. 이때, 보관소에 저장된 객체를 구분하는 값으로 \<id\>에서 지정한 프로퍼티를 사용함.		   
		   
### * \<result\> 엘리먼트  
- \<resultMap\> 태그의 자식 태그로서 칼럼과 셋터 메서드의 연결을 정의함. column속성엔 칼럼이름, property속성엔 객체의 프로퍼티 이름을 지정.
- javaType 속성 : 칼럼의 값을 특정 자바 객체로 변환 가능 --> 속성을 지정하지 않는다면 셋터의 매개변수 타입에 맞춰 칼럼 값이 변환 됨. 		   
		   
### * Mybatis의 Select 결과 캐싱
- 성능 향상을 위해서, 한 번 생성된 객체는 버리지 않고 보관소에 두었다가, 다음 select를 실행할 때 재사용. 
- select 결과에 대해 \<resultMap\>에 정의된 대로 자바 객체를 생성하고 싶다면, \<select\>의 resultMap 속성에 \<resultMap\> id를 지정한다. 
		   
``` html
<select id="selectList" resultMap="projectResultMap"> ... </select>
```		   

### * SQL문의 입력 매개변수 처리
- sql문을 작성할 때 외부에서 값을 주입할수 있도록 입력 매개변수를 지정해야하는 경우가 있다.
- mybatis에서는 입력 매개변수를 '#{프로퍼티명}'으로 표시.
		   
``` html
<insert id="insert" parameterType="project">
	insert into PROJECTS(PNAME, CONTENT, STA_DATE, END_DATE, STATE, CRE_DATE, TAGS) values (#{title}, #{content}, #{startDate}, #{endDate}, 0, now(), #{tags}</insert>
```
		   
- #{프로퍼티 명}이 가리키는 값은 \<insert\>의 parameterType에 지정한 객체의 프로퍼티 값(겟터 메서드의 반환값)임. 즉 #{title} 자리에는 Project객체의 getTitle() 반환값이 놓임. 
- cf) 값 객체가 기본 타입일 경우 -> #{아무 이름 사용 가능} 		   
- 입력 매개변수에 값 공급하기 : SqlSession 메서드를 호출할 때 값 객체를 전달. 		   
``` java
int count = sqlSession.insert("dao.ProjectDao.insert", project);
```		   
- 'dao.ProjectDao' : SQL맵퍼 파일의 네임스페이스 이름
- 'insert'는 SQL아이디
- project는 INSERT문을 실행할 때 입력 매개변수에 값을 공급할 객체.		   
- sqlSession.insert()를 호출하면 SQL맵퍼 파일에서 dao.ProjectDao.insert 네임스페이스이름+아이디를 가진 sql문을 찾아 실행한다.		   

## mybatis 설정 파일
- mybatis에서는 자체 커넥션풀을 구축, 여러개의 데이터베이스 연결 정보 설정 후 원하는 DB 지정 사용, select결과 캐싱, 값 객체에 별명 부여.
- ->이런 동작환경을 설정하기 위한 xml파일.
### * \<configuration\> 루트 엘리먼트 		   
- mybatis 설정 파일의 루트 엘리먼트는 configuration. 
- configuration의 주요 자식 엘리먼트
		   
		properties   : 프로퍼티 파일이 있는 경로 설정. \<property\>를 사용하여 개별 프로퍼티 정의 가능
		settings     : 프레임워크의 실행환경 설정.
		typeAliases  : 자바 클래스 이름(패키지 이름 포함)에 대한 별명 설정
		typeHandlers : 칼럼의 값을 자바 객체로, 자바 객체를 칼럼의 값으로 변환해 주는 클래스를 설정
		environments : 프레임워크에서 사용한 데이터베이스 정보(트랜잭션 관리자, 데이터 소스)를 설정.
		mappers      : SQL 맵퍼 파일들이 있는 경로 설정

- \<properties\> 엘리먼트 
: 데이터베이스 연결 정보처럼 자주 변경될 수 있는 값은 mybatis 설정 파일에 두지 않고, 보통 프로퍼티 파일에 저장. 프로퍼티 파일을 로딩하려면 <properties> 태그를 사용.
: 프로퍼티 파일이 클래스 경로에 있다면 resource 속성을 사용하여 설정. 만약 클래스 경로가 아닌 다른 경로에 있다면 url 속성을 사용. 
: 프로퍼티 파일에 정의된 것 외에 추가로 프로퍼티 정의 가능. \<property\> 태그를 사용하여 프로퍼티 이름과 값을 정의.		   
: 프로퍼티 파일에 저장된 값은 ${프로퍼티 명} 형식으로 참조

- \<typeAliases\> 엘리먼트 		   
: SQL 맵퍼 파일에서 매개변수 타입이나 결과 타입을 지정할 때, 별명 사용 가능.

``` html
<typeAliases>
	<typeAlias type = "vo.Project" alias="project"/>
</typeAliases>	
```		   
		   
- \<environments\> 엘리먼트
: DB 환경정보를 설정할 때 사용하는 태그 -> 여러 개의 DB 접속 정보를 설정할 수 있다. 설정된 DB 정보 중에서 하나를 선택할 때는 default 속성을 사용.

``` html
\<environments default="development"\>
		   \<environment id ="development"\>... \</environment\>		   
```		   
		   
- \<environment\> 엘리먼트		   
: 트랜잭션 관리 및 데이터 소스를 설정하는 태그
: 트랜잭션이란 여러 개의 데이터 변경 작업을 하나의 작업으로 묶은 것. mybatis에선 JDBC, MANAGED 두 가지의 트랜잭션 관리 방식이 있다.		   
: JDBC -> 직접 JDBC의 커밋, 롤백 기능을 사용하여 mybatis 자체에서 트랜잭션을 관리
: MANAGED -> 서버의 트랜잭션 관리 기능을 이용. Java EE 애플리케이션 서버나 서블릿 컨테이너에서 트랜잭션을 관리
		   
- 데이터 소스 설정
: mybatis는 JDBC 표준 인터페이스인 javax.sql.DataSource 구현체를 이용하여 DB 커넥션을 다룸.<br/>
: mybatis에서 사용 가능한 데이터 소스 유형 세 가지
		   
		UNPOLLED : DB 커넥션을 요청할 떄마다 매번 커넥션 객체를 생성. 높은 성능 요구하지 않는 단순 앱에 적합
		POOLED	 : 미리 DB커넥션 객체 생성해두고, 요청하면 즉시 반환. DB에 연결하는 과정, 즉 연결을 초기화하고 사용자를 인증하는 과정이 없기 때문에 속도가 빠르다.
		JNDI     : Java EE 애플리케이션 서버나 서블릿 컨테이너에서 제공하는 데이터 소스를 사용 <-- 속성을 data_source로 지정.
		   
- \<mappers\> 엘리먼트
: SQL맵퍼 파일들의 정보를 설정할 때 사용. 		   
: resource 속성 -> 자바 클래스 경로 사용.
: url 속성 -> 운영체제의 파일 시스템 경로 사용.		
		   
## * mybatis 로그 출력 켜기<br/>
: 로그 출력 기능을 켜면 mybatis에서 실행하는 sql문과 매개변수 값, 실행 결과를 실시간으로 확인할 수 있다.<br/>
: 특히 동적 SQL문이 실행 조건에 따라 어떻게 달라지는지 확인할 수 있어 프로그램을 디버깅할 때 매우 유용하다.
- mybatis 설정 파일에 로그 설정 추가

``` html
<settings>
	<setting name="logImpl" value="LOG4J"/> //로그 출력기를 설정할 때 name 속성은 "logImpl" value 속성은 로그 출력기의 이름.
</settings>	
```		   

### * Log4J 설정 파일 작성 <br/>
: 로그의 수준, 출력 방식, 출력 형식, 로그 대상 등에 대한 정보가 들어감. 이 파일은 자바 클래스 경로에 두어야 함.
- 로그의 출력 등급 설정<br/>
ex) log4j.rootLogger=ERROR, stdout --> 루트 로거의 출력 등급 설정, 출력 담당자 선언.	
: 루트 로거는 최상위 로거를 가리킴. 하위 로거들은 항상 부모의 출력 등급을 상속 받는다.
		   
		   로그 출력 등급
		   FATAL       : 애플리케이션을 중지해야 할 심각한 오류
		   ERROR       : 오류가 발생했지만, 애플리케이션은 계속 실행할 수 있는 상태
		   WARN        : 잠재적인 위험을 안고 있는 상태
		   INFO        : 애플리케이션의 주요 실행 정보
		   DEBUG       : 애플리케이션의 내부 실행 상황을 추적해 볼 수 있는 상세 정보
		   TRACE       : 디버그보다 더 상세한 정보 
		   
- 출력 담당자 선언
: 루트 로거의 출력 등급을 설정하는 값 다음에 출력 담당자(Appender)의 이름이 온다. 위의 stdout
- 출력 담당자의 유형을 결정
: -> 로그를 어디로 출력할지 설정.

``` 
log4j.appender.이름=출력 담장자(패키지명 포함한 클래스명)		   
```  
- 출력 담당자를 지정하는 프로퍼티 이름은 항상 'log4j.appender'로 시작
- 출력 담장자의 클래스
		   
		   org.apache.log4j.ConsoleAppender    : System.out 또는 System.err로 로그를 출력시킨다. 기본은 System.out(모니터)
		   org.apache.log4j.FileAppender       : 파일로 로그를 출력
		   org.apache.log4j.net.SocketAppender : 원격의 로그 서버에 로그 정보를 담은 LoggingEvent 객체를 보낸다.

-로그의 출력 형식 정의

``` 
log4j.appender.이름.layout=출력형식 클래스(패키지명을 포함한 클래스명)
``` 
- 출력 형식 클래스
		   
		   org.apache.log4j.SimpleLayout  : 출력 형식은 '출력 등록 - 메시지'
		   org.apache.log4j.HTMLLayout    : HTML 테이블 형식으로 출력
		   org.apache.log4j.PatternLayout : 변환 패턴의 형식에 따라 출력 ex) %-5p[%t]:%m%n
		   org.apache.log4j.xml.XMLLayout : log4j.dtd 규칙에 따라 XML을 만들어 출력
		   
- 특정 패키지의 클래스에 대해 로그의 출력 등급 설정
: log4j.logeer.패키지이름=출력등급		   

## * 동적 SQL
- mybaits에서 제공하는 동적SQL 기능을 사용하면 하나의 SQL문으로 여러 상황에 대처할 수 있다. 
### * 동적 SQL 엘리먼트

		   - <if test="조건"> SQL 문 </if>
		   : <if>태그는 어떤 값의 상태를 검사하여 참일 경우에만 SQL문을 포함하고 싶을 때 사용한다. 
		     test속성에 지정된 조건이 참이면 <if> 태그의 내용을 반환한다. 
		    
		   
		   - <choose>
		    	<when test="조건1">SQL 문</when>
		   	<when test="조건2">SQL 문</when>
		   	<otherwise> SQL문</otherwise>
		     </choose>
		   : <choose> 태그는 검사할 조건이 여러 개 일 경우에 사용. 
		     test 속성에 지정된 조건이 참이면 <when> 태그의 내용을 반환. 
		     일치하는 조건이 없으면 <otherwise>의 내용을 반환.
		  
		   - <where>
		   	<if test="조건"> SQL문 <when>
		     </where>
		    : <where> 태그는 WHERE절을 반환. 
		      <where>안의 하위 태그를 실행하고 나서 반환값이 있으면 WHERE절을 만들어 반환한다.
		  
		   - <trim prefix="단어" prefixOverride="문자열|문자열">
			<if test="조건"> SQL문 </when>
		     </trim>
		     : <trim> 태그는 특정 단어로 시작하는 SQL문을 반환하고 싶을 때 사용. 
		     prefix는 반환값 앞에 붙일 접두어를 지정. prefixOverrides는 반환할 값에서 제거해야하는 접두어 지정.
		  
		   - <set>
		   	<if test="조건"> SQL문 </when>
		     </set>
		     : <set> 태그는 UPDATE 문의 SET절을 만들 때 사용. 
		       <set> 절의 항목이 여러개 일 경우 자동으로 콤마를 붙임.
		   
		   - <foreach 
			    item="항목"
			    index="인덱스"
			    collection="목록"
			    open="시작문자열"
			    close="종료문자열"
			    separator="구분자">  
		     </foreach>
		    : <foreach> 태그는 목록의 값을 가지고 SQL문을 만들 떄 사용. 특히 IN(값,값,...)조건을 만들 때 좋다.
		      item 속성에는 항목을 가리킬 때 사용할 변수의 이름을 지정.
		      index 속성에는 항목의 인덱스 값을 꺼낼 때 사용할 변수 이름을 지정
		      collection 속성에는 java.util.List 구현체나 배열 객체가 옴
		      open 속성에는 최종 반환값의 접두어를 지정.
		      close 속성에는 최종 반환값의 접미어를 지정
		      separator 속성은 반복으로 생성하는 값을 구분하기 위해 붙이는 문자열을 지정.
		   
		  - <bind name="변수명" value="값"/>
  		    : <bind>태그는 변수를 생성할 때 사용한다. 			  
		   
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
				 
<br>
---
<br>				 
				 
		   
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
<br/><br/>

## Singleton 싱글턴 디자인 패턴

### 싱글턴이란?
: 클래스의 인스턴스를 하나만 생성하고, 어디서든 그 인스턴스를 탐조할 수 있도록 하는 패턴(전역접근 제공)
: 생성자가 여러번 호출 되더라도 실제로 생성되는 객체는 하나

### 싱글턴을 쓰는 이유?
1. 고정된 메모리 영역을 가지고 하나의 인스턴스만 사용하기 때문에 메모리 낭비 방지
2. 싱글턴 클래스의 인스턴스는 전역이기 때문에 다른 클래스의 인스턴스들이 데이터 공유하기가 쉬움
3. DBCP(DataBase Connection Pool)처럼 공통된 객체를 여러 개 생성해야 하는 상황에 많이 사용. 

### 정잭 클래스의 개념
- 모든 메소드가 static 클래스를 지칭. 또는 inner static class를 뜻하기도 함.

-> 쓰는 이유.
1. 상태를 가지고 있지 않고 global access를 제공할 때 유용
2. Static은 컴파일 때 static binding으로 싱글턴보다 좀 더 빠르다.
3. 클래스 자체에 static을 붙여 사용할 수 없다.(inner class일 때만 가능)


					싱글턴 vs 정적 클래스

	원리	    하나의 인스턴스를 생성하여 재사용 | 인스턴스 생성 x
	인터페이스 구현.        		가능 | 불가능 
	Override			  가능  |  불가능
	Load	 	     필요에 따라 lazy 가능 | static binding으로 빠르게 로딩
	OOP			       	  O    |   X  


- 싱글턴이 객체지향에 더 적합 

<br/><br/>

## web.xml
- 관문의 개념
- (문지기가) 하는 일
		
		1. ServletContext의 초기 파라미터
		2. Session의 유효시간 설정
		3. Servlet/JSP에 대한 정의
		4. Servlet/JSP 매핑
		5. Mime Type 매핑 (데이터의 타입)
		6. Welcome File list
		7. Error Pages 처리
		8. 리스너/필터 설정  
		9. 보안
