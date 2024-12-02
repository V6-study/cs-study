## Error & Exception

<br>

#### 1. 기본 계층구조
```
Throwable
├── Error
│   ├── StackOverflowError
│   ├── OutOfMemoryError
│   └── ...
└── Exception
    ├── RuntimeException (Unchecked)
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   └── ...
    └── IOException (Checked)
        ├── FileNotFoundException
        └── ...
```

<br>

### 2. Error vs Exception

- **Error**
    - 시스템 레벨의 심각한 문제
    - 애플리케이션에서 복구가 불가능
    - 예외 처리(try-catch)를 권장하지 않음
    - 예시: OutOfMemoryError, StackOverflowError

<br>

- **Exception**
    - 애플리케이션 레벨의 문제
    - 개발자가 구현한 로직에서 발생, JVM은 정상 동작
    - 예외 처리를 통해 정상적인 프로그램 흐름 유지 가능

<br>

### 3. Checked vs Unchecked Exception
- **Checked Exception**
    - Exception 클래스를 직접 상속
    - 컴파일 시점에 확인
    - 반드시 예외 처리 필요 (try-catch 또는 throws). 처리하지 않으면 컴파일되지 않음
    - JVM 외부와 통신(네트워크, 파일시스템 등)할 때 주로 쓰임
    - 예시: IOException, SQLException
    ```
    // 반드시 예외 처리 필요
    public void readFile() throws IOException {
        FileInputStream file = new FileInputStream("file.txt");
    }
    ```

<br>

- **Unchecked Exception**
    - RuntimeException 클래스를 상속
    - 런타임 시점에 확인
    - 예외 처리가 선택적
    - 예시: NullPointerException, ArrayIndexOutOfBoundsException
    ```
    // 예외 처리가 선택적
    public void divide(int a, int b) {
        System.out.println(a / b);  // ArithmeticException 발생 가능
    }
    ```

<br>

### 4. 예외 처리 방법

##### try-catch-finally

```
// Bad
try {
    // 코드
} catch (Exception e) {  // 너무 광범위한 예외 처리
}

// Good
try {
    // 코드
} catch (SQLException e) {
    // SQL 예외 처리
} catch (IOException e) {
    // IO 예외 처리
}
```


```
// Checked Exception 생성
public class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}

// Unchecked Exception 생성
public class CustomRuntimeException extends RuntimeException {
    public CustomRuntimeException(String message) {
        super(message);
    }
}
```
