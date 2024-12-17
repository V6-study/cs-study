### Spring MVC란?

Spring MVC는 웹 애플리케이션 개발을 위한 Java 기반의 프레임워크이다. MVC 패턴을 기반으로 하여 웹 애플리케이션을 구조적으로 개발할 수 있게 해준다.

#### 주요 특징

-   유연하고 확장 가능한 웹 애플리케이션 개발 가능
-   DI(Dependency Injection)와 IoC(Inversion of Control) 지원
-   RESTful 웹 서비스 구현 용이
-   다양한 뷰 기술 지원 (JSP, Thymeleaf 등)

---

### 핵심 컴포넌트 (DispatchServlet, Model, View, Controller)

**1\. DispatchServlet**

-   프론트 컨트롤러 역할
-   모든 웹 요청의 진입점
-   요청을 적절한 핸들러에게 전달하고 응답을 관리

**2\. Controller**

-   비즈니스 로직을 처리
-   요청을 처리하고 모델을 업데이트
-   적절한 뷰를 선택하여 반환

**3\. Model**

-   데이터와 비즈니스 로직을 포함
-   컨트롤러와 뷰 사이의 데이터 전달 역할

**4\. View**

-   모델 데이터를 표시
-   다양한 뷰 기술 활용 가능 (JSP, Thymeleaf 등)

---

### 동작 방식

1.  **클라이언트 요청**
    -   클라이언트가 서버에 HTTP 요청을 보냄
    -   모든 요청은 DispatcherServlet이 먼저 받음
2.  **HandlerMapping 검색**
    -   DispatcherServlet이 HandlerMapping에게 요청을 처리할 Controller 검색 요청
    -   URL과 매핑된 Controller 정보를 찾음
3.  **Controller 선택**
    -   HandlerMapping이 해당 요청을 처리할 적절한 Controller 반환
4.  **Controller 실행**
    -   DispatcherServlet이 선택된 Controller에게 요청 처리를 위임
    -   Controller는 비즈니스 로직 처리를 위해 Service 호출
5.  **비즈니스 로직 처리**
    -   Service 계층에서 필요한 비즈니스 로직 처리
    -   필요한 경우 Repository를 통해 데이터베이스 작업 수행
6.  **Model 데이터 생성**
    -   처리 결과를 Model 객체에 담음
    -   Controller가 View 이름과 함께 DispatcherServlet에 반환
7.  **ViewResolver 동작**
    -   DispatcherServlet이 ViewResolver를 통해 실제 View 검색
    -   View 이름을 실제 View 객체로 변환
8.  **View 렌더링**
    -   View 객체가 Model 데이터를 사용하여 클라이언트에게 보낼 응답 생성
    -   HTML, JSON 등 다양한 형식의 응답 생성 가능
9.  **응답 반환**
    -   최종 생성된 응답을 DispatcherServlet이 클라이언트에게 반환

---

### 주요 애노테이션

**컨트롤러 관련**

-    \` @Controller \` : 클래스를 컨트롤러로 지정
-    \` @RestController \` : REST API용 컨트롤러
-    \` @RequestMapping \` : URL 매핑 설정
-    \` @GetMapping \` , \` @PostMapping \` 등: HTTP 메소드별 매핑

**요청 처리 관련**

-    \` @RequestParam \` : 요청 파라미터 바인딩
-    \` @PathVariable \` : URL 경로 변수 바인딩
-    \` @RequestBody \` : HTTP 요청 본문 바인딩
-    \` @ResponseBody \` : HTTP 응답 본문 생성
