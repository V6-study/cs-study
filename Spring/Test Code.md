### **Test Code****의 중요성**

-   개발 초기에 문제를 발견할 수 있음.
-   코드 리팩토링 시 기존 기능을 검증을 통해 리팩토링 안정성을 보장.
-   개발 프로세스 안정성 증가. 신뢰성 향상.
-   개발 시간 감소.

테스트 코드는 초기 작성에 시간이 걸릴 수 있지만, 장기적으로는 개발 속도를 향상시킬 수 있다.

테스트 코드를 한번 작성하면 반복적인 테스트 작업이 감소하고 자동화된 테스트로 인해 시간이 절약된다. 

새로운 기능이 추가되었을 때도 테스트 코드를 통해 기존의 코드에 영향이 갔다면, 어떤 부분을 수정해야 하는 지 알 수 있는 장점도 존재한다.

<br/><br/><br/>


### **Test Code 종류**

**단위 테스트(Unit Test)**: 개별 클래스나 메서드의 기능을 테스트

**통합 테스트(Integration Test)**: 여러 컴포넌트가 함께 동작하는 것을 테스트

**기능 테스트(Function Test)**: 전체 시스템의 기능을 테스트

**엔드 투 엔드 테스트(End-to-End Test)**: 사용자 관점에서 애플리케이션 흐름을 테스트

<br/><br/><br/>

### **Test Code 예제**

#### **단위테스트**

JUnit이나 Mockito를 사용하여 작성할 수 있다. 아래는 JUnit5을 사용한 예제이다.

```
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;

@ExtendWith(SpringExtension.class) // JUnit 5에서 Spring 테스트 확장을 사용
@WebMvcTest(controllers = HomeController.class) // HomeController만 테스트
public class HomeControllerTest {

    @Autowired
    private MockMvc mvc; // MockMvc를 주입 받아서 웹 요청을 테스트

    @Test
    public void home_return() throws Exception {
        // when
        String home = "home"; // 기대하는 응답 내용

        // then
        mvc.perform(get("/home")) // /home에 GET 요청을 보냄
                .andExpect(status().isOk()) // 응답 상태가 200 OK인지 확인
                .andExpect(content().string(home)); // 응답 내용이 "home"인지 확인
    }
}
```

HomeController만을 테스트하고 다른 계층은 모킹하고나 로드하지 않기 때문에 컨트롤러의 동작만 검증하는 단위테스트이다.

\* \`@ExtendWith(SpringExtension.class)\`

Spring의 테스트 확장을 Junit5에 연결하는 역할. 스프링 부트 테스트와 JUnit 사이의 연결자 역할을 한다.

JUnit4에서는 \`@RunWith(SpringRunner.class)\`를 사용한다.

JUnit5에서는 @SpringBootTest 만으로 Spring 테스트 환경이 자동으로 설정되기 때문이다.

\* \`@WebMvcTest\`

컨트롤러만 사용할 때 선언. Spring MVC에 집중할 수 있게 함.

해당 어노테이션을 통해 MockMvc를 자동으로 구성.

\* \`MockMvc\`

\`@Autowired\`를 통해 Spring 의존성 주입을 사용하여 MockMvc 객체를 주입받는다.

웹 API를 테스트할 때 사용한다. 이를 통해 HTTP GET, POST, DELETE 등에 대한 API 테스트가 가능하다.

\* \`@Test\` 어노테이션은 해당 메서드가 테스트 메서드임을 나타낸다.

<br/><br/>

#### **통합테스트**

통합테스트는 Spring Boot의 \`@SpringBootTest\` 어노테이션을 사용하여 작성할 수 있다.

```
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.test.web.servlet.MockMvc;
import org.junit.jupiter.api.Test;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

// Spring Boot 통합 테스트 클래스
@SpringBootTest // Spring Boot 애플리케이션 컨텍스트를 로드하여 통합 테스트를 수행
@AutoConfigureMockMvc // MockMvc를 자동 구성
public class MyControllerTest {

    // MockMvc를 주입받아 사용
    @Autowired
    private MockMvc mockMvc;

    // /api/my-endpoint에 대한 GET 요청을 테스트하는 메서드
    @Test
    public void testGetEndpoint() throws Exception {
        // MockMvc를 사용하여 /api/my-endpoint에 대한 GET 요청을 수행하고, 응답 상태가 200 OK인지 확인
        mockMvc.perform(get("/api/my-endpoint"))
               .andExpect(status().isOk());
    }
}
```

전체 애플리케이션 컨텍스트를 로드하여 다양한 컴포넌트가 함께 동작하는지 확인할 수 있기 때문에 통합 테스트이다.

\* \`@SpringBootTest\`  
Spring Boot 애플리케이션 컨텍스트를 로드한다. 전체 애플리케이션을 시작하고 모든 빈을 로드한다.

\* \`@AutoConfigureMocMvc\`

MockMvc를 자동으로 구성하여 테스트에서 사용할 수 있게 한다.
