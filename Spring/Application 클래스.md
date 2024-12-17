### **Application 클래스의 역할**

Application 클래스는 Spring Boot 애플리케이션의 **"진입점"**이다.

메인 메서드를 포함한다. \`SpringApplication.run()\`메서드를 통해 애플리케이션을 시작한다.

Spring context을 초기화하고 필요한 빈(beans)을 로드하는 역할을 한다.

<br/><br/><br/>


### **정의 및 사용 방법**

Application 클래스를 정의할 때는 \`@SpringBootApplication\`  어노테이션을 사용하여 정의한다.

이는 다음의 여러 어노테이션을 결합한 것이다.

**\`SpringBootConfiguration\`** : Spring Boot 설정 클래스로 지정하겠다.

**\`EnableAutoConfiguration\`** : Spring Boot의 자동 설정을 활성화하겠다.

**\`ComponentScan\`** : 지정된 패키지에서 Spring 컴포넌트를 스캔기능을 활성화 하겠다.

<br/><br/>

#### **클래스 정의 예시**

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

<br/><br/>

#### **추가 작성 내용**

-   **\`CommandLineRunner\` 인터페이스**  
    애플리케이션 시작 시 실행할 코드를 작성할 수 있다.
-   **\`@Configuration\` 클래스**  
    추가 설정을 위해 빈을 등록할 수 있다.
-   **\`@Profile\` 어노테이션**  
    특정 프로파일에서만 활성화되는 빈을 정의할 수 있다.
-   **\`@EventListener\`**  
    애플리케이션 이벤트를 처리할 수 있다.

<br/><br/>

**예시)**

```
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.event.EventListener;
import org.springframework.context.annotation.Profile;
import org.springframework.boot.context.event.ApplicationReadyEvent;

@SpringBootApplication
public class MyApplication implements CommandLineRunner {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }

    @Override
    public void run(String... args) throws Exception {
        // 애플리케이션 초기화 코드
        System.out.println("애플리케이션 시작!");
    }

    @EventListener(ApplicationReadyEvent.class)
    public void onApplicationReady() {
        // 애플리케이션 준비 완료 후 실행할 코드
        System.out.println("애플리케이션 준비 완료!");
    }

    @Configuration
    public static class AppConfig {
    
        @Bean
        public MyService myService() {
            return new MyServiceImpl();
        }

        @Bean
        @Profile("dev")
        public MyService devMyService() {
            return new DevMyServiceImpl();
        }
    }
}

// MyService 인터페이스 정의
interface MyService {
    void performTask();
}

// MyService 구현체
class MyServiceImpl implements MyService {
    @Override
    public void performTask() {
        System.out.println("MyServiceImpl 실행 중...");
    }
}

// DevMyService 구현체
class DevMyServiceImpl implements MyService {
    @Override
    public void performTask() {
        System.out.println("DevMyServiceImpl 실행 중...");
    }
}
```

<br/><br/><br/>

### **동작 과정**

\`SpringApplication.run()\` 메서드 호출 시 동작 과정을 알아보자.

1.  \`SpringApplication\` 인스턴스 생성
2.  \`run()\` 메서드를 통해 어플리케이션 실행
    -   환경 설정 및 준비
    -   애플리케이션 컨텍스트 생성 및 설정
    -   애플리케이션 컨텍스트 시작
3.  \`CommandLineRunner\` 및 \`ApplicationRunner\` 구현 되어있다면 이를 실행
4.  애플리케이션 실행 완료

> **💡 Spring Boot의 내장 WAS**  
>   
> WAS는 톰캣(Tomcat), 제티(Jetty), 언더토우(Undertow)와 같은 웹 애플리케이션 서버를 말한다.  
> Spring Boot는 기본적으로 WAS를 애플리케이션에 내장할 수 있는 기능을 제공한다.  
> 이를 통해 개발자는 별도의 WAS를 설치/설정 할 필요 없이 애플리케이션을 실행할 수 있다.  
>   
> \`SpringApplication.run()\`메서드 호출 시 Spring Boot는 이 내장 WAS를 실행한다.

<br/><br/><br/>

### **주의 사항**

-   루트 패키지에 위치해야한다.  
    ㄴ \`@SpringBootApplication\`의 하위 속성인\`@ComponentScan\`이 모든 하위 패키지를 자동으로 스캔함.  
     
-   메인 클래스에 \`@SpringBootApplication\` 어노테이션을 사용해야 한다.  
    ㄴ main 메서드를 포함 시켜야 한다. (어플리케이션의 진입점 역할을 하기 때문에)  
     
-   메인 클래스를 디폴트 패키지(패키지 선언 없음)에 두지 않는다.  
    ㄴ 컴포넌트와 엔티티 스캔 시 문제가 발생할 수 있음.  
     


<br/><br/>


참고
[tistory - wordbe](https://wordbe.tistory.com/161)
[github - gyoogle](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Web/Spring/%5BSpring%20Boot%5D%20SpringApplication.md)
