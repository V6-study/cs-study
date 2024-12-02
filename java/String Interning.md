## String Interning

<br>

String Interning은 Java의 String Pool(또는 String Constant Pool)과 관련된 메모리 최적화 메커니즘이다.

<br>

#### 1. String Pool이란
- JVM의 힙 메모리 내에 존재하는 특별한 메모리 영역
- 동일한 문자열 값을 재사용하기 위한 저장소
- Java 7부터는 Heap 영역에 위치 (이전에는 Permanent Generation에 있었음)

<br>

#### 2. Interning의 동작 방식
```
String str1 = "hello";  // 리터럴로 생성
String str2 = "hello";  // 같은 리터럴
String str3 = new String("hello");  // new 연산자로 생성

System.out.println(str1 == str2);  // true
System.out.println(str1 == str3);  // false
System.out.println(str1 == str3.intern());  // true
```

- String str1을 생성할 때 'hello'는 리터럴 문자열이므 Heap 영역의 String Pool에 위치하게 됨
- str2가 가르키는 'hello'를 String Pool에 넣으려고 보니 이미 존재함. 고로 str2는 str1과 같은 String Pool의 'hello'를 가르키게 됨.
- new 연산자로 생성한 String str3는 일반 힙 메모리에 저장

<br>

> intern()의 사용?
- 문자열 비교에는 equals()을 흔히 사용함. 그러나 == 연산자를 사용해야 할 때가 있음. 그럴 때 사용하는 것이 intern()
- String pool에서 리터럴 문자열이 이미 존재하는지 체크하고 존재하면 해당 문자열을 반환하고, 아니면 리터럴을 String pool에 넣어줍

<br>

#### 3. 주요 특징
- 문자열 리터럴은 자동으로 intern됨
- new 연산자로 생성된 문자열은 자동으로 intern되지 않음
- intern() 메소드를 통해 명시적으로 String Pool에 문자열을 추가할 수 있음
- 동일한 문자열에 대해 하나의 인스턴스만 유지하므로 메모리 사용량 감소

<br>

#### 4. 장단점
- 장점
  - 메모리 사용량 최적화
  - 문자열 비교 시 equals() 대신 == 연산자 사용 가능
  - 문자열 검색 성능 향상
- 단점
  - intern() 메소드 호출 시 오버헤드 발생
  - String Pool이 커지면 가비지 컬렉션 성능에 영향
  - 과도한 사용 시 메모리 부족 발생 가능
