### 용어정리

> SOP
> 
> - 같은 출처에서만 리소스를 공유할 수 있다 라는 규칙을 가진 정책

<br>

### CORS란?
---

1. 정의
    - Cross-Origin Resoure Sharing

    - 도메인이 다른 서버끼리 리소스를 주고 받을 때 보안을 위해 설정된 정책

2. 등장배경
    - 브라우저는 기본적으로 SOP 정책을 따름

    - 어쩔 수 없이 외부 리소스를 사용하는 경우 존재

    - 예외 조항을 두고 출처가 다르더라도 리소스 요청 허용

        - CORS 정책을 지킨 리소스 요청 → 허용
3. Origin
    - 다른 출처(Cross-Origin) 판단하는 부분

    - 구성요소
        - Scheme(프로토콜)

        - Host(도메인)

        - Port
    - e.g.
        - HTTPS://www.domain.com:3000
4. 검사 위치
    - 브라우저에 출처를 비교하는 로직이 구현됨

    - 서버는 CORS를 위반하더라도 정상적으로 응답

    - 브라우저에서 응답의 파기여부를 결정

### CORS 동작

```java
@CrossOrigin(origins = {"*"})
@RestController
@RequiredArgsConstructor
@Log4j2
public class MemberController {
    private final MemberService memberService;
    private final TokenProvider tokenProvider;

    @PostMapping("/members/signup")
    public ResponseEntity<String>signUp(@RequestBody MemberRequestDTO memberReqeustDto) {
        try {
            memberService.signUp(memberReqeustDto);
            return new ResponseEntity<>("회원가입 성공", HttpStatus.CREATED);
        }catch (IllegalArgumentException e) {
            return new ResponseEntity<>(e.getMessage(), HttpStatus.BAD_REQUEST);
        }
    }
    ...
```

![Untitled - CORS](https://github.com/user-attachments/assets/a6df5737-383e-4d56-b6c8-c006d82d682a)

1. HTTP 프로토콜을 사용하여 요청 헤더 `Origin` 필드에 출처를 함께 담아 보냄

2. 서버가 응답할 때, `Access-Control-Allow-Origin`이라는 값에 “이 리소스를 접근하는 것이 허용된 출처를 담음

3. 브라우저에서 자신이 보낸 `Origin`과 서버가 보내준 응답의 `Access-Control-Allow-Origin` 을 비교해 유효한 응답인지 판단

<br>

[참고자료]<br>
[🔗](https://velog.io/@effirin/CORS%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80) CORS란 무엇인가
