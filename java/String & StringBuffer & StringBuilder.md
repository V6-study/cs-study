| 분류 | String | StringBuffer | StringBuilder |
| --- | --- | --- | --- |
| 변경 | Immutable | Mutable | Mutable |
| 동기화 |  | Synchronized 가능 (Thread-safe) | Synchronized 불가능. |

**1. String 특징**

```java
String str = "Hello";
str = "World";
```

- new 연산을 통해 생성된 인스턴스의 메모리 공간은 변하지 않음 (Immutable)
    - 기존의 "Hello"를 수정한 것이 아니라 새로운 "World"라는 String 객체를 생성하고, str이 이를 참조하도록 변경
    - "Hello"는 여전히 메모리에 존재
- Garbage Collector로 제거되어야 함.
    - "Hello"라는 문자열 객체는 더 이상 참조되지 않을 경우 Garbage Collector(GC)가 메모리에서 제거할 수 있는 상태가 된다
- 문자열 연산시 새로 객체를 만드는 Overhead 발생
- 객체가 불변하므로, Multithread에서 동기화를 신경 쓸 필요가 없음. (조회 연산에 매우 큰 장점)

```bash
String 클래스
문자열 연산이 적고, 조회가 많은 멀티쓰레드 환경에서 좋음
```

**2. StringBuffer, StringBuilder 특징**

- 공통점
    - new 연산으로 클래스를 한 번만 만듬 (Mutable)
    - 문자열 연산시 새로 객체를 만들지 않고, 크기를 변경시킴
    - StringBuffer와 StringBuilder 클래스의 메서드가 동일함.
- 차이점
    - StringBuffer는 Thread-Safe함 / StringBuilder는 Thread-safe하지 않음 (불가능)
    
    ```bash
    StringBuffer 클래스
    - 문자열 연산이 많은 Multi-Thread 환경
    
    StringBuilder 클래스
    - 문자열 연산이 많은 Single-Thread 또는 Thread 신경 안쓰는 환경
    ```
