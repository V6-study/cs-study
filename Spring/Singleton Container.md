## 싱글톤(Singletone)
### 싱글톤이란? 
싱글톤(Singletone)은 디자인 패턴 중 하나로, 애플리케이션 내에서 **하나의 인스턴스만 생성되도록 보장하는 패턴**이다.<br/>
즉, 어떤 클래스가 싱글톤으로 정의되면, 해당 클래스의 인스턴스는 애플리케이션 내에서 오직 **하나만 존재**하게 된다.<br/>
<br/>

### 싱글톤이 필요한 이유
1. 자원절약 : 객체를 여러번 생성하지 않고 하나의 인스턴스만 사용. 메모리 자원 절약.
2. 글로벌 접근 : 애플리케이션 내 어디에서든 동일한 인스턴스를 접근할 수 있음.
3. 제어된 인스턴스 생명 주기 : 인스턴스 생성/소멸을 제어 -> 애플리케이션의 안정적인 관리.



<br/><br/><br/>


## 스프링을 사용하지 않는 DI 컨테이너의 문제점
> DI(Dependency Injection) 컨테이너 : 의존성을 관리하고 주입해주는 역할을 함.

스프링을 사용하지 않고 DI(Dependency Injection) 컨테이너를 사용한 예시를 살펴보자.<br/>
AppConfig.class
```Java
public class AppConfig {
    /* 의존성 주입 */
    public MemberService memberService(){
        return new MemberServiceImpl(getMemberRepository());
    }
    public OrderService orderService(){
        return new OrderServiceImpl(getMemberRepository(), getDiscountPolicy());
    }


    /* 구현체 정의해주기 */
    public MemberRepository getMemberRepository() {
        return new MemoryMemberRepository();
    }
    public DiscountPolicy getDiscountPolicy() {
//        return new FixDiscountPolicy();
        return new RateDiscountPolicy();
    }
}
```

```Java
        AppConfig appConfig = new AppConfig();
        // 1. 조회: 호출할 때마다 객체를 생성
        MemberService memberService1  = appConfig.memberService();
        // 2. 조회: 호출할 때마다 객체를 생성
        MemberService memberService2  = appConfig.memberService();

        // 두 객체가 참조값이 다름
        System.out.println("memberService1 : " + memberService1);
        System.out.println("memberService2 : " + memberService2);
```
![image](https://github.com/user-attachments/assets/6003dd9b-03da-4192-8033-c7c24845ff9a)

두 객체의 참조값이 다른 것을 확인할 수 있다.<br/>
예를 들어, 고객 트래피이 초당 100이 나오면, 초당 100개의 객체가 생성되고 소멸된다.<br/>
근데 이 객체가 모두 동일하게 사용되는 객체라면, 심각한 메모리 낭비를 초래한다.<br/>

<br/><br/><br/>

## 스프링 컨테이너
스프링 프레임워크는 기본적으로 싱글톤 스코프를 가지는 빈을 생성하여 관리한다.<br/>
즉, 스프링 컨테이너(ApplicationContext) 내에 하나의 빈 정의에 대해 하나의 인스턴스만 생성한다.<br/>
`@Configuration` 어노테이션과 `Bean`어노테이션을 이용하여 스프링 컨테이너에 빈을 등록할 수 있다.<br/>
AppConfig.class
```Java
@Configuration
public class AppConfig {
    /* 의존성 주입 */
    @Bean
    public MemberService memberService(){
        return new MemberServiceImpl(getMemberRepository());
    }
    @Bean
    public OrderService orderService(){
        return new OrderServiceImpl(getMemberRepository(), getDiscountPolicy());
    }


    /* 구현체 정의해주기 */
    @Bean
    public MemberRepository getMemberRepository() {
        return new MemoryMemberRepository();
    }
    @Bean
    public DiscountPolicy getDiscountPolicy() {
//        return new FixDiscountPolicy();
        return new RateDiscountPolicy();
    }
}
```
```Java
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

        MemberService memberService1  = ac.getBean("memberService", MemberService.class);
        MemberService memberService2  = ac.getBean("memberService", MemberService.class);

        // 두 객체가 참조값이 같은 것을 확인
        System.out.println("memberService1 : " + memberService1);
        System.out.println("memberService2 : " + memberService2);

        assertThat(memberService1).isSameAs(memberService2);
```
![image](https://github.com/user-attachments/assets/5e37c51f-95f3-49da-958d-4d82901da3fe)


<br/><br/><br/>


## CGLIB 프록시
위의 `AppConfig.class` 예시에서 `getMemberRepository`에 대해 코드 상으로는<br/>
해당 메서드는 Bean에 등록될 때 말고도 `memberService`, `orderService` 메서드를 Bean 등록할 때 호출되며,<br/>
새로운 `MemoryMemberRepository`를 생성한다.<br/>
하지만 실제 세 메서드에서 사용되는 `MemoryMemberRepository`는 모두 같은 참조 값을 갖는다.<br/>

**CGLIB 프록시**
<br/>
스프링에서 @Configuration 어노테이션이 붙은 클래스는 특별한 처리 과정을 거친다.<br/>
이 클래스 내에서 정의된 @Bean 메소드는 싱글톤 빈을 생성하고 반환하는데, 이는 스프링의 **CGLIB 프록시**를 통해 구현된다.<br/>
<br/>
 
1. 스프링은 @Configuration 클래스를 상속받아 프록시 클래스를 생성하고, 이 프록시 클래스가 실제 빈을 관리한다.
2. @Bean 메소드가 호출될 때마다 새로운 인스턴스를 생성하는 것이 아니라, 이미 생성된 빈을 반환하게 된다.
3. 프록시 메커니즘: 실제 빈의 메소드를 호출하기 전에, 프록시가 캐시된 빈 인스턴스를 반환한다.


<br/><br/><br/>



## 싱글톤 주의점
하나의 객체 인스턴스를 생성하여 공유하므로 상태를 유지(stateful)하게 설계하면 안된다.<br/>
즉, 무상태(stateless)로 설계해야한다.<br/>
❗️ Spring Bean에 공유 값을 설정하면 큰 장애가 발생할 수 있음<br/>
- 특정 클라이언트에 의존적인 필드가 있으면 안된다.
- 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다.
- 가급적 읽기만 가능해야 한다.
- 필드 대신 자바에서 공유되지 않는 지역변수/파라미터/ThreadLocal 등을 사용해야한다.



