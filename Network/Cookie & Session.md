쿠키와 세션은 HTTP 프로토콜의 **~Connectionless~,** Stateless 특성을 보완하기 위해 사용되는 기술

-   Stateless: 서버가 클라이언트의 이전 상태를 보존하지 않음
-   Stateless 환경을 보완하지 않은 상태로 서비스를 운영하게 되면??
    -   로그인이 필요한 모든 페이지로 이동할 때마다 새로 로그인을 하게 되는 상황 발생

## 쿠키

웹사이트가 사용자의 브라우저에 저장하는 작은 데이터 파일로, 사용자의 상태 정보를 유지하고 웹사이트의 기능을 향상시키기 위해 사용

-   사용자의 컴퓨터나 모바일 기기의 로컬 저장소에 저장
    -   브라우저는 이 로컬 저장소에 접근하여 쿠키를 읽고 사용
-   클라이언트에서 먼저 설정할 수도 있고 서버에서 먼저 설정할 수 있으나, 보통은 서버에서 먼저 설정해서 쿠키를 만들어줌
-   로그인, 장바구니, 사용자 커스터마이징, 사용자 행동분석 등에 사용

### 쿠키의 종류

| 종류 | 특징 |
| --- | --- |
| Session Cookie | Expires 또는 Max-Age 속성을 지정하지 않은 쿠키로, 브라우저 종료 시 쿠키가 삭제됨 |
| Persistent Cookie | 장기간 유지되는 쿠키로, 파일로 저장되어 브라우저가 종료되어도 다시 사용할 수 있음 |
| Secure Cookie | HTTPS 프로토콜에서만 사용하며, 쿠키 정보가 암호화되어 전송됨 |
| Third-Party Cookie | 방문한 도메인과 다른 도메인의 쿠키, 일반적으로 광고 배너 등을 관리할 때 유입 경로를 추적하기 위해 사용 |

### 쿠키의 구성 요소

-   name (필수 요소)
    -   쿠키를 식별하는 키(key)
-   value (필수 요소)
    -   쿠키에 저장되는 실제 데이터

---

-   expires 또는 max-age
    -   쿠키의 유효 기간 설정
    -   생략 시 세션 쿠키로 취급 (브라우저 종료 시 삭제)
-   domian
    -   쿠키가 전송될 도메인을 지정
    -   생략 시 쿠키를 생성한 도메인으로 설정
-   path
    -   쿠키가 유효한 경로를 지정
    -   생략 시 현재 페이지의 경로로 설정
-   secure
    -   HTTPS 연결에서만 쿠키 전송을 허용
-   HttpOnly
    -   JavaScript를 통한 쿠키 접근을 방지
-   SameSite
    -   크로스 사이트 요청에 대한 쿠키 전송 제어

### 쿠키의 프로세스

1.  클라이언트가 서버로 요청을 보냄
2.  서버에서 쿠키 값을 할당
3.  클라이언트는 이후 서버에 request할 때 전달받은 쿠키를 자동으로 요청헤더에 추가하여 요청
    -   요청헤더에 추가하는 과정은 브라우저에서 처리

![Description](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbYC1xV%2FbtsJQ4LyTdt%2FRjJFB9kwA6evfGvJN77D30%2Fimg.png)

만약 쿠키의 기한이 정해져 있지 않고 명시적으로 지우지 않는다면 쿠키는 반 영구적으로 존재하게 됨

**쿠키로 로그인 등의 구현이 되지만 어째서 세션을 사용해야 할까??**

-   보안성
    -   쿠키는 클라이언트 측에 저장되어 있어 변조나 탈취의 위험이 존재
-   데이터 용량
    -   쿠키의 용량 제한은 4kb로 저장할 수 있는 정보량이 제한적
-   서버 제어
    -   세션은 서버에서 직접 관리하므로 세션 만료, 강제 로그아웃 등의 기능을 쉽게 구현 가능

## 세션

웹 서버와 클라이언트 간의 상태를 유지하기 위한 메커니즘으로, 일정 시간 동안 같은 사용자로부터 들어오는 일련의 요청을 하나의 상태로 보고, 그 상태를 유지하는 기술

-   세션은 쿠키를 기반으로 하지만, 사용자 정보 파일을 서버 측에서 관리
-   서버에서는 클라이언트를 구분하기 위해 세션 ID를 부여하며, 웹 브라우저가 서버에 접속해서 브라우저를 종료할 때까지 인증상태를 유지

### 세션의 특징

-   각 클라이언트에게 고유 ID를 부여
-   세션 ID로 클라이언트를 구분해서 클라이언트의 요구에 맞는 서비스 제공
-   쿠키보다 우수한 **보안**
-   사용자에 대한 정보가 서버에 많이 유지될수록 서버 메모리를 많이 차지하게 됨
-   DB중 RDBMS에 저장한다면 직렬화 및 역직렬화에 관한 오버헤드가 발생함
    -   세션 데이터는 주로 객체 형태로 존재
    -   RDBMS는 객체를 직접 저장할 수 없기에 직렬화 역직렬화 과정이 필요
    -   이러한 과정에서 CPU와 메모리 사용량이 증가하여 성능 저하 발생

### 세션 기반 로그인 프로세스

![Description](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F1SvaD%2FbtsJSJZ9ju1%2FlFkQnftevfEYJ1OVTbGQOK%2Fimg.png)

  
  

1.  로그인에 성공하면 세션ID를 생성, 서버에서 세션ID를 쿠키로 설정해서 클라이언트에게 전달
2.  클라이언트가 서버에 요청을 보낼 때 해당 세션ID를 쿠키로 담아 전에 로그인했던 아이디인지 확인
3.  로그인 유지

쿠키는 클라이언트 측에 데이터를 저장해 빠르고 간편한 방식으로 상태를 유지하지만 보안적인 측면에서 제약이 존재

반면, 세션은 서버 측에서 데이터를 관리하며 더 안전한 방식으로 사용자 인증을 처리

---

참고 자료

[https://www.geeksforgeeks.org/difference-between-session-and-cookies/](https://www.geeksforgeeks.org/difference-between-session-and-cookies/)

[https://www.tutorialspoint.com/what-is-the-difference-between-session-and-cookies](https://www.tutorialspoint.com/what-is-the-difference-between-session-and-cookies)

[https://interconnection.tistory.com/74](https://interconnection.tistory.com/74)

[https://velog.io/@octo\_\_/쿠키Cookie-세션Session](https://velog.io/@octo__/%EC%BF%A0%ED%82%A4Cookie-%EC%84%B8%EC%85%98Session)

[https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)
