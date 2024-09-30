### 용어정리

> URI
> 
> 
> URI는 Uniform Resource Identifier의 약자로, 인터넷 상의 자원을 고유하게 식별하고 위치를 지정하는 문자열
> 

> URL
> 
> 
> URL은 Uniform Resource Locator의 약자로, 웹에서 특정 리소스의 위치를 나타내는 주소. URI의 한 형태로, 인터넷 상에서 리소스를 찾는 데 사용되는 표준화된 주소 체계입니다. 치와 취득 방법(HTTP,FTP 등)을 포함함


<br>

### 01 REST
---

1. 정의
    - Representational State Transfer
   
    - ⭐ 자원의 표현에 의한 상태 전달
      
        - 자원의 표현 = 이름
          
            - 자원 = 해당 소프트웨어가 관리하는 모든 것
              
        - 상태(정보) 전달
          
            - JSON or XML 형식으로 주고 받는 것이 일반적
              
2. 개념
    - HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시
      
    - HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것
3. 특징
    - ⭐ Server-Client 구조
      
        - REST Server
          
            - 자원이 있는 쪽
              
            - 비즈니스 로직 처리
              
        - Client : 자원 요청하는 쪽
          
            - 사용자인증, context(세션, 로그인정보) 직접 관리
              
    - Stateless(무상태)
      
        - Client의 context를 server에 저장 ❌
          
        - Server는 Client의 요청을 완전히 별개의 것으로 인식 및 처리
          
    - Cacheable

    - Layered System
      
    - Code-On-Demand
      
    - Uniform Interface
   

<br>

### 02 REST API
---

1. 정의
    - REST 기반으로 서비스 API 구현

2. 특징
    - ⭐ 확장성과 재사용성 높음

    - REST는 HTTP 표준을 기반으로 구현, HTTP를 지원하는 프로그램 언어로 Client, Server 구현 가능
      
3. 설계 규칙
    1. URI는 정보의 자원을 표현해야 한다. 

        - resource는 명사, 소문자 사용

        - resource의 도큐먼트 이름 : 단수명사

        - resource의 컬렉션, 스토어(집합) 이름 : 복수명사

    2. ⭐ 자원에 대한 행위는 HTTP Method(GET, PUT, POST, DELETE 등)로 표현한다.  
        - URI에 HTTP Method가 들어가면 안된다. 

        - 경로 중 변하는 부분은 유일한 값으로 대체

    3. 슬래시 구분자(/) 는 계층 관계를 나타내는데 사용

    4. URI 마지막 문자로 / 포함 ❌

    5. -(하이픈)은 URI 가독성을 높이는데 사용

    6. _(밑줄) 사용 ❌

    7. URI 경로에는 소문자 사용

    8. 파일 확장자 포함 ❌

    9. 연관관계 표현

        - /리소스명/리소스ID/관계가 있는 다른 리소스명


4. 설계 예시
    
    
    | CRUD | HTTP verbs | Route |
    | --- | --- | --- |
    | resource 목록 표시 | GET | /members |
    | resource 하나의 내용 표시 | GET | /members/:id |
    | resource 생성 | POST | /members |
    | resource 수정 | PUT | /members/:id |
    | resource 삭제  | DELETE | /members/:id |

<br>

### 03 RESTful
---

1. ⭐ 정의
   - REST 원리를 따르는 시스템

3. 목적
    - 일관적인 convention을 통한 API 이해도 및 호환성 향상
