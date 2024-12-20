## 용어정리

> **기본타입(Primitive Type)**
> 
> - 자체를 저장
> - e.g. int, long, float, double, boolean

> **참조 타입(Reference Type)**
> 
> - 힙 메모리에 저장된 객체의 주소를 참조
> - 자바의 모든 **객체**와 **클래스**는 참조 타입으로 관리
> - 컬렉션 프레임워크는 기본적으로 **객체**를 저장하거나 다

> **Wrapper 클래스**
> 
> - 기본 데이터 타입을 **객체**로 다룰 수 있도록 감싸주는 클래스
> - 기본 타입과 객체 사이를 연결해주는 다리 역할
> - 다양한 유용한 메서드와 기능을 제공
> - e.g. Integer, Long, Float, Double, Boolean

---

## 1 박싱과 언박싱

- 박싱
    - 기본 타입 → Wrapper 클래스로 만드는 동작
- 언박싱
    - Wrapper 클래스 → 기본 타입으로 변환
    
    ```java
    // 박싱
    int i = 10;
    Integer num = new Integer(i);
    
    // 언박싱
    Integer num = new Integer(10);
    int i = num.intValue();
    ```
    
    ![image](https://github.com/user-attachments/assets/be225ccb-0c25-4be8-ba77-bd8bafdd9267)

    

## **2 오토 박싱 & 오토 언박싱**

JDK 1.5부터는 자바 컴파일러가 박싱과 언박싱이 필요한 상황에 명시적으로 변환 코드를 작성하지 않아도 되도록 함

```java
// 오토 박싱
int i = 10;
Integer num = i;

// 오토 언박싱
Integer num = new Integer(10);
int i = num;
```

### **2.1 성능**

편의성을 위해 오토 박싱과 언박싱이 제공되고 있지만, 내부적으로 추가 연산 작업이 거치게 된다.

따라서, 오토 박싱&언박싱이 일어나지 않도록 동일한 타입 연산이 이루어지도록 구현하자.

- **오토 박싱 연산**
    
    ```java
    public static void main(String[] args) {
        long t = System.currentTimeMillis();
        Long sum = 0L;
        for (long i = 0; i < 1000000; i++) {
            sum += i;
        }
        System.out.println("실행 시간: " + (System.currentTimeMillis() - t) + " ms");
    }
    
    // 실행 시간 : 19 ms
    ```
    
- **동일 타입 연산**
    
    ```java
    public static void main(String[] args) {
        long t = System.currentTimeMillis();
        long sum = 0L;
        for (long i = 0; i < 1000000; i++) {
            sum += i;
        }
        System.out.println("실행 시간: " + (System.currentTimeMillis() - t) + " ms") ;
    }
    
    // 실행 시간 : 4 ms
    ```
    

📢 100만건 기준으로 약 5배의 성능 차이가 난다. 따라서 서비스를 개발하면서 불필요한 오토 캐스팅이 일어나는 지 확인하는 습관을 가지자.

**[출처]**

- [링크](http://tcpschool.com/java/java_api_wrapper)
- [링크](https://sas-study.tistory.com/407)
