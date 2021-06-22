# Technical_Term_For_Interview
면접을 위한 기술 용어를 정리합니다.
<br>
이해를 돕기 위해 개인적인 방식으로 풀어서 서술합니다.

<br>

## CGI(Common Gateway Interface)
- 웹서버와 프로그램 사이의 데이터를 주고 받는 규칙.

## DDFile(Depolyment Desciptor)
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
  
## Servlet(서블릿)
- 서버와 프로그램이 데이터를 주고 받을 수 있도록 HTTP를 따르는 자바로 작성된 프로그램. <br>CGI(Common Gateway Interface) 프로그램 중 자바로 작성된 프로그램이다. 

## Servlet Container(서블릿 컨테이너)
- 서브릿을 대신하여 CGI(Common Gateway Interface)규칙에 따라 웹 서버와 데이터를 주고 받는 프로그램. <br> 덕분에 서블릿을 개발할 때 CGI규칙을 신경 쓸 필요가 없어졌다. 
- 서블릿 컨테이너는 클라이언트로 요청을 받으면 해당 서블릿을 찾아본다. 만약 없다면 해당 서블릿의 인스턴스를 생성한다. <br>한 번 서블릿 객체가 생성되면 웹 애플리케이션이 종료될 때까지 유지한다.
- 대표적인 프로그램으로 "톰캣"이 있다.

## POST Request(HTTP Request Method)
  -  <form>태그의 method 속성값이 post인 경우<br>
  
          URL에 데이터가 포함X
          메시지 본문에 데이터를 포함 -> 실행 결과 공유X
          바이너리 및 대용량 데이터 전송 가능
