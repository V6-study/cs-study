각자 PC OS마다 서로 다른 가상 메모리 주소 공간을 갖기 때문에 참조형 데이터들은 인스턴스를 직접 전달할 수 없다  
참조형 데이터를 외부로 전달하기 위해서는 직렬화 과정을 거쳐야한다  
   
직렬화란?  
   
자바 직렬화(Serialization)는 객체 인스턴스의 데이터를 I/O 스트림에 적합한 일련의 데이터로 변환하는 과정이다  
구체적으로, 자바 시스템 내부에서 사용되는 **객체 또는 데이터를** 외부의 자바 시스템에서도 사용할 수 있도록 **바이트(byte) 형태로 데이터를 변환**하는 기술이다  
   
이 과정을 통해 객체를 **파일로 저장**하거나, **네트워크를 통해 전송**하거나, **다른 컴퓨터 환경에서 객체를 재구성**할 수 있다  
직렬화된 데이터는 나중에 역직렬화(Deserialization) 과정을 통해 다시 객체로 변환할 수 있다

![image](https://github.com/user-attachments/assets/3498a45c-c667-4f71-80e7-d4b4d6b0b8b2)

   
자바에서 **직렬화**를 구현하기 위해서는 **Serializable 인터페이스를 구현**해야 한다  
이 인터페이스는 직렬화 가능한 의미를 나타내는 **마커 인터페이스**로, 실제 메서드는 포함하고 있지 않다

![image](https://github.com/user-attachments/assets/9c072d71-3480-4f17-bd6b-8c2289e61fcd)

   
Serializable 인터페이스를 구현한 User 클래스

```
public class User implements Serializable {
    private String id;
    private String pw;
    private String email;
    // ...
}
```

이 경우에 클래스의 멤버변수 모두가 직렬화 대상이 된다  
   
하지만 보안상이나 기타 이유로 일부 멤버변수를 제외하고 싶다면 transient 키워드를 적용하여 직렬화 대상에서 제외할 수 있다

```
public class User implements Serializable {
    private String id;
    private transient String pw; // 직렬화에서 제외되는 필드
    private String email;
    // ...
}
```

   
**transient** 키워드가 포함되어 있는 멤버변수는 **역직렬화** 과정에서 **기본값으로 초기화** 된다는 점을 알아두자  
   
다른 객체를 멤버변수로 갖고 있다면 직렬화는 어떻게 진행될까??

```
public class User implements Serializable {
    private String id;
    private transient String pw;
    private String email;
    private Card card;
    // ...
}
```

   
   
카드를 멤버 변수로 갖고 있을 때  
   
카드 클래스가 Serializable 인터페이스를 구현하고 있지 않다면 InvalidClassException이 발생한다  
   
따라서 직렬화를 진행하기 위해서는 기본 자료형을 제외한 **참조 자료형의 필드 변수가**  
   
모두 **Serializable 인터페이스를 구현하고 있어야 한다**  
   
그렇다면 직렬화 시점 이후에 클래스의 구조가 변경이 되면 역직렬화 과정에서 데이터를 그대로 읽어올 수 있을까??  
   
먼저 직렬화 가능한 객체는 serialVersionUID를 통해서 버전이 관리된다  
   
앞서 작성했던 예제들은 serialVersionUID를 선언하지 않았다  
   
이러한 경우에는 JVM이 런타임 시점에 클래스 구조를 기반으로 해시값을 자동으로 생성하여 serialVersionUID로 사용한다  
   
직렬화 시점의 serialVersionUID와 역직렬화 시점의 serialVersionUID가 다르다면  
   
역직렬화 시점에서 InvalidClassException이 발생한다  
   
serialVersionUID를 명시적으로 관리하여도 직렬화 시점과 역직렬화 시점의 serialVersionUID가 다르다면 이 역시 InvalidClassException이 발생하게 된다  
   
그렇다면 어째서 serialVersionUID를 관리해야 할까??  
   
다음의 예제를 통해서 알아보자

```
public class User implements Serializable {
    private static final long serialVersionUID = 1L;
    private String id;
    private transient String pw;
    private String email;
    private Card card;
}
```

   
기존 User 클래스에 serialVersionUID를 명시하여 버전 관리를 진행하기로 하였으며 해당 객체를 직렬화하여 외부로 내보냈다  
   
이후 User 클래스에 phoneNumber 라는 필드를 추가하고 serialVersionUID를 갱신하였다

```
public class User implements Serializable {
    private static final long serialVersionUID = 2L; // 갱신된 버전
    private String id;
    private transient String pw;
    private String email;
    private Card card;
    private String phoneNumber; // 추가된 필드
}
```

   
이후 역직렬화를 통해 방금 전 내보냈던 객체를 불러오려 했지만 서로 버전이 다르기에 InvalidClassException이 발생한다  
   
그렇다면 **필드는 추가된 상태**로 **직렬화 시점의 버전으로 변경** 후 역직렬화를 진행하면 어떻게 될까?

```
public class User implements Serializable {
    private static final long serialVersionUID = 1L; // 직렬화 시점의 버전으로 변경
    private String id;
    private transient String pw;
    private String email;
    private Card card;
    private String phoneNumber; // 추가된 필드
}
```

   
이후 역직렬화 과정을 진행하니 기존 직렬화 데이터를 성공적으로 읽어올 수 있었다  
   
새로 추가된 필드는 기본값으로 처리되었으며 클래스의 변경이 일어났지만 호환성을 유지하며 데이터를 불러올 수 있었다  
   
즉 명시적으로 serialVersionUID를 관리함으로  
   
**개발자가 의도적으로 호환성을 제어**하고  
**일부 필드 변경에 대해 유연한 대응**을 할 수 있었다  
   
큰 구조 변경 시에는 의도적으로 값을 변경하여 비호환성을 표시하는 등의 방법으로 사용할 수 있다  
   
마지막으로 Serializable 인터페이스를 구현한 객체의 직렬화 및 역직렬화 과정은 다음과 같다  
 

```
// 직렬화 과정

User user = new User();

byte[] serializedUser;
try (ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
    // 객체 직렬화를 위한 ObjectOutputStream 생성
    // try-with-resources를 사용하여 자동으로 스트림 닫기
    try (ObjectOutputStream oos = new ObjectOutputStream(baos)) {
        
        // 객체를 바이트 배열로 직렬화
        // 이 과정에서 객체의 상태를 연속된 바이트로 변환
        oos.writeObject(user);
        
        // 직렬화된 바이트 배열을 변수에 저장
        serializedUser = baos.toByteArray();
    }
}
// 예외 처리는 try-with-resources에 의해 암시적으로 처리됨
```

```
// 역직렬화 과정

// 직렬화된 바이트 배열로부터 입력 스트림 생성
// ByteArrayInputStream은 바이트 배열을 입력 스트림으로 변환
try (ByteArrayInputStream bais = new ByteArrayInputStream(serializedUser)) {
    
    // 객체 역직렬화를 위한 ObjectInputStream 생성
    try (ObjectInputStream ois = new ObjectInputStream(bais)) {
        // readObject()로 직렬화된 데이터를 객체로 읽어들임
        Object objectPost = ois.readObject();
        User user = (User) objectPost;
    } catch (ClassNotFoundException e) {
        throw new RuntimeException(e);
    }
}
// try-with-resources로 자동으로 입력 스트림 리소스 해제
```

   
   
출처  
[https://velog.io/@whitebear/%EC%9E%90%EB%B0%94-%EC%A7%81%EB%A0%AC%ED%99%94-%ED%99%95%EC%8B%A4%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0](https://velog.io/@whitebear/%EC%9E%90%EB%B0%94-%EC%A7%81%EB%A0%AC%ED%99%94-%ED%99%95%EC%8B%A4%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0)  
[https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%A7%81%EB%A0%AC%ED%99%94Serializable-%EC%99%84%EB%B2%BD-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%A7%81%EB%A0%AC%ED%99%94Serializable-%EC%99%84%EB%B2%BD-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0)  
[https://hjjungdev.tistory.com/187](https://hjjungdev.tistory.com/187)  
[https://m.blog.naver.com/writer0713/220922099055](https://m.blog.naver.com/writer0713/220922099055)
