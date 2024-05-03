### Spring 설치

- [Start.Spring.io 이동](https://start.spring.io/)
    - 해당 수업은 `War` 선택
    - Dependencies는 `Spring Web` `Lombok` 추가

<br><br>

### 내가 경험한 에러

- JDK 환경변수 설정했는데 `error : invalid source release: 21`
    - `File | Settings | Build, Execution, Deployment | Build Tools | Gradle`
    - `Gradle JVM:` 버전 맞는거 선택

<br><br>

### 기본 세팅

- __Application.java에 `@ServletComponentScan` 어노테이션 추가
    - `src - main - java - 설정한 그룹`
    - 해당 어노테이션은 주로 메인 또는 설정 클래스에 작성
    - SpringBoot Application 시작 시 지정된 패키지를 스캔 -> 웹 컴포넌트를 자동으로 등록
        - Annotation : 소스 코드에 추가하여 사용할 수 있는 메타데이터의 일종

- Annotation Porcessors 활성화
    - `File | Settings | Build, Execution, Deployment | Compiler | Annotation Processors`
    - 활성화시 IntelliJ가 컴파일 시간에 어노테이션을 처리하여 필요한 코드를 자동으로 생성함

<br><br>

### 자바 클래스 웹 서블릿 설정

- 새로운 자바 클래스 생성
    - HttpServlet 상속받기
    - `@WebServlet()` 어노테이션 추가
        - 특정 HTTP 요청을 처리하는 서블릿을 선언할 때 사용
        - web.xml 파일에 서블릿 매핑 설정하지 않고도 서블릿 구성 됨
        - `(name = "서블릿이름")`
        - `(urlPatterns = "/URL패턴)`
        - 그 외 <br> `loadOnStartup(서블릿 로드 순서)` `initParmas(초기화 파라미터 설정)` `description(설명 제공)`

```java
package group.name.directory

import ...;

//localhost:8080/connection-hello
@WebServlet(name = "helloTemp", urlPatterns = "/connection-hello")
public class HelloTemp extends HttpServlet {

    //pvs 치면 나옴
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        // ... 내부 코드
    }
}
```

<br><br>

### 데이터 저장 클래스 with Lombok

```java
import lombok.Getter;
import lombok.Setter;

@Getter @Setter
public class 클래스명 {
    private 데이터타입 필드명;
    ...
}
```
```java
//JSON데이터 클래스에 저장
...
ServletInputStream inputStream = req.getInputStream();
String msg = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
...
클래스명 객체 = new ObjectMapper().readValue(msg, 클래스명.class);
```

<br><br>

### 내부 코드

```java
@Override
protected void service(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
    //콘솔에 내용 출력
    System.out.println()

    //전체 파라미터 조회
    req.getParameterNames().asIterator().forEachRemaining(paraName -> {
        System.out.println(paraName + " = " + res.getParameter(paraName));
    });

    //특정 파라미터 값 받기
    req.getParameter("KeyName")

    //동일한 키값 파라미터 여러개 받기
    req.getParameterValues("KeyName");  //배열에 저장해야함

    //request 정보 메소드   // (예시)
    req.getMethod();        // GET      (넘어오는 방식)
    req.getProtocol();      // HTTP/1.1 (프로토콜 타입)
    req.getScheme();        // http     (현재 요청이 사용하는 프로토콜의 이름)
    req.getRequestURL();    // http://localhost:8080/req-header
    req.getRequestURI();    // /req-header 
    req.getQueryString();   // null     (파라미터)
    req.isSecure();         // false    (https보안연결 여부)

    //헤더 정보 
    req.getHeaderNames().asIterator().forEachRemaining(headerName -> {
        System.out.println(headerName + " : " + request.getHeader(headerName))
    });

    // 
    //res.setContentType("text/html; charset=utf-8");
    res.setContentType("text/plain");
    res.setCharacterEncoding("utf-8");
    res.getWriter().write("페이지에 보여줄 내용");

    //JSON 데이터 받기
    ServletInputStream inputStream = request.getInputStream();
    String msg = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
}
```
