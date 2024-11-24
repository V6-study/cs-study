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

### **Conclusion**

- String Class는 JDK 1.5버전 이전에 문자열연산('+', concat)을 할 때에는 조합된 문자열을 새로운 메모리에 할당하여 참조함으로 인해서 성능상의 이슈 존재
- 그러나 JDK1.5 버전 이후에는 컴파일 단계에서 String 객체를 사용하더라도 StringBuilder로 컴파일 되도록 변경 → 성능상 이슈 해결
- 하지만 반복 루프에서 String Class 사용시, 문자열을 더할 때에는 객체를 계속 추가하기 때문에 성능상 이슈 여전히 존재
- String Class를 쓰는 대신 권장하는 방법
    - Thread와 관련이 있으면 StringBuffer 사용
    - Thread 안전 여부와 상관이 없으면 StringBuilder를 사용
