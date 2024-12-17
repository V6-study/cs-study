#### Spring Security란?

**스프링 시큐리티**는 스프링 기반의 애플리케이션에서 보안을 관리하는데 사용되는 하위 프레임워크이다

스프링 시큐리티는 Filter 형태로 동작한다

Dispatcher Servlet이 어플리케이션으로 요청을 핸들링 하기 전 필터를 통해 여러 검증을 진행하는데

이 과정에서 **인증, 인가** 및 보호 기능을 제공하여 **보안 관련 작업**을 쉽게 **수행**할 수 있도록 도와준다

(HTTP 요청 → WAS → 필터1 → 필터2 → ...  →  서블릿 → 컨트롤러)

![image](https://github.com/user-attachments/assets/1681d682-a1a9-4862-9f7c-e75b95b54a27)


필터의 내부 동작은 다음과 같이 진행된다

![image](https://github.com/user-attachments/assets/c7de7b0f-da6b-4239-abcc-48a221897b85)


동작 흐름

1.  클라이언트 요청
    -   사용자가 로그인 요청을 보냄
2.  DelegatingFilterProxy  
    -   요청을 FilterChainProxy에 위임
3.  SecurityContextPersistenceFilter
    -   SecurityContext를 세션에서 가져오거나 생성
4.  UsernamePasswordAuthenticationFilter
    -   UsernamePasswordAuthenticationToken을 생성하여 AuthenticationManager에게 전달
    -   UsernamePasswordAuthenticationToken →  AuthenticationManager
5.  AuthenticationManager (ProviderManager)
    -   해당 토큰을 처리할 수 있는 AuthenticationManager를 탐색
6.  AuthenticationProvider  
    -   UserDetailsService를 통해 사용자 정보를 가져옴
    -   PasswordEncoder를 사용하여 비밀번호를 비교
    -   인증에 성공하면 Authentication 객체를 생성하여 반환
7.  SecurityContext
    -   인증된 Authentication 객체를 SecurityContext에 저장
8.  FilterSecurityInterceptor
    -   사용자의 권한을 기반으로 리소스 접근을 결정
    -   필터 체인 중 맨 마지막에 위치한 필터로 인가 여부 확인
    -   실제 인가 처리는 AccessDecisionManager에게 위임
    -   AccessDecisionManager
        1.  AccessDecisionVoter들에게 인증 정보, 요청 정보, 권한 정보를 넘겨주어 인가 여부 결정
        2.  각 AccessDecisionVoter는 인가 여부를 반환하고 Manager가 최종 인가 여부를 결정
        3.  권한이 있다면 접근 허용, 없다면 AccessDeniedException 발생

접근 제한이 필요한 API 혹은 요청에는 보안 토큰의 정보를 통해 유저가 권한이 있는지 여부를 체크하고 리소스를 요청할 수 있도록 구성할 수 있다

인증 및 인가 과정을 처리하기 위해 SecurityFilterChain을 빈으로 등록하여야 한다

@Configuration 애노테이션이 적용되어 있는 WebSecurityConfig 내부의 코드

```
@Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

        http
                .cors((cors) -> cors.configurationSource(configurationSource()))
                .csrf((csrf) -> csrf.disable())
                .sessionManagement((sessionManagement) ->
                        sessionManagement.sessionCreationPolicy(SessionCreationPolicy.STATELESS)) // JWT 사용을 위한 STATELESS
                .authorizeHttpRequests((authorizeHttpRequests) -> // URL 별 접근 권한  설정
                                authorizeHttpRequests
                                        .requestMatchers(PathRequest.toStaticResources().atCommonLocations()).permitAll()
                                        .requestMatchers("**").permitAll()
                                        .requestMatchers(HttpMethod.GET).permitAll()
                                        .requestMatchers("/members/logout").permitAll()
                                        .anyRequest().authenticated() // 나머지 접근은 인증 필요
                )
                .addFilterBefore(authenticationFilter(), AuthenticationFilter.class) // JWT 인증  필터 추가
                .addFilterBefore(authorizationFilter(), AuthorizationFilter.class) // 인가 필터 추가
        ;
        return http.build();
    }
```

-   서블릿 컨테이너(tomcat)에서 생성된 DelegatingFilterPorxy가 요청을 가로챔
-   해당 프록시는 스프링의 ApplicationContext에서 FilterChainProxy 빈을 찾음
-   실제 보안 처리를 위해 요청을 FilterChainProxy로 위임
-   SeucrityFilterChain 빈의 설정을 기반으로 실제 보안 필터들의 체인을 구성
-   설정한 순서대로 필터들을 실행

HTTP 요청

→ DelegatingFilterProxy

→ FilterChainProxy

→ SecurityFilterChain

→ 설정한 필터들 (authenticationFilter, authorizationFilter 등)

→ Dispatcher Servlet

더보기

DelegatingFilterProxy가 필요한 이유

서블릿 컨테이너(Tomcat)와 스프링 컨테이너의 라이프사이클 차이때문

![image](https://github.com/user-attachments/assets/f4d6ece5-b62d-40eb-b33c-f8867f7a07d7)


1\. 서블릿 필터의 등록 시점

-   서블릿 컨테이너는 웹 애플리케이션 시작 시 필터들을 먼저 등록하고 초기화 함
-   이 시점에서 아직 스프링 컨테이너가 완전히 초기화되지 않았을 수 있음
-   따라서 스프링의 빈으로 등록된 필터들을 직접 사용할 수 없음

2\. DelegatingFilterProxy의 역할

-   서블릿 컨테이너와 스프링 컨테이너 사이의 다리 역할
-   서블릿 필터 체인에 등록되어 있다가, 실제 요청이 왔을 때 스프링 컨테이너에서 관리하는 필터 빈을 찾아서 위임

**서로 다른 생명주기를 가진 두 컨테이너 사이를 연결해주는 요소**

출처

[https://mangkyu.tistory.com/18](https://mangkyu.tistory.com/18)

[https://devwithpug.github.io/spring/spring-security-1/](https://devwithpug.github.io/spring/spring-security-1/)

[https://docs.spring.io/spring-security/reference/servlet/architecture.html](https://docs.spring.io/spring-security/reference/servlet/architecture.html)

[https://velog.io/@dlthgml0108/Spring-Security-%EC%9D%B8%EC%A6%9D%EA%B3%BC-%EC%9D%B8%EA%B0%80](https://velog.io/@dlthgml0108/Spring-Security-%EC%9D%B8%EC%A6%9D%EA%B3%BC-%EC%9D%B8%EA%B0%80)
