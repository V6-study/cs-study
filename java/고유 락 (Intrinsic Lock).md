자바의 모든 객체는 락(lock)을 갖고 있다

모든 객체가 갖고 있으므로 고유 락(intrinsic lock)이라고 하며, 모니터처럼 동작한다고 하여 모니터 락(monitor lock), 혹은 그냥 모니터(monitor)라고 한다

예제를 통해 고유 락을 다루는 synchronized를 확인해보자

### 고유 락 예제

```
import java.time.LocalDateTime;
import java.util.concurrent.TimeUnit;

public class IntrinsicLock {
    public static void main(String[] args) {
        IntrinsicLock intrinsicLock = new IntrinsicLock();

        Thread thread1 = new Thread(() -> {
            System.out.println("thread1 before call " + LocalDateTime.now());
            intrinsicLock.syncMethod1("from thread1");
            System.out.println("thread1 after call " + LocalDateTime.now());
        });

        Thread thread2 = new Thread(() -> {
            System.out.println("thread2 before call " + LocalDateTime.now());
            intrinsicLock.syncMethod2("from thread2");
            System.out.println("thread2 after call " + LocalDateTime.now());
        });

        thread1.start();
        thread2.start();
    }

    private synchronized void syncMethod1(String msg){
        System.out.println("in the sync method1" + msg + " " + LocalDateTime.now());
        try {
            TimeUnit.SECONDS.sleep(5);
        }catch (InterruptedException e){
            e.printStackTrace();
        }
    }

    private synchronized void syncMethod2(String msg){
        System.out.println("in the sync method2" + msg + " " + LocalDateTime.now());
        try {
            TimeUnit.SECONDS.sleep(5);
        }catch (InterruptedException e){
            e.printStackTrace();
        }
    }
}
```

해당 코드에서의 고유 락은 IntrinsicLock 객체 자체에 연결된 잠금 메커니즘이다

syncMethod1과 syncMethod2는 둘 다 synchronized 키워드로 선언되어 있기에 객체의 고유 락을 사용한다

-   한 스레드가 synchronized 메서드에 들어가면, 해당 객체의 고유락을 획득
-   **고유 락을 획득한 스레드가 메서드를 실행하는 동안**, 다른 스레드들은 **같은 객체의 다른 synchronized 메서드에 진입할 수 없음**

#### 결과
![image](https://github.com/user-attachments/assets/a1fe431c-966a-4d00-b7fb-9aa4b92b9266)

#### 실행 과정

1.  thread1이 syncMethod1 에 먼저 진입
2.  thread1은 5초 동안 sleep 상태
3.  thread2는 thread1이 락을 해제할 때 까지 대기
4.  5초 후 thread1가이메서드를 완료하면 thread2가 자신의 synchronized 메서드를 실행

실행 시간 (총 10초)

메서드에 synchronized 키워드를 선언하지 않았을 때

![image](https://github.com/user-attachments/assets/b9ca49d0-8407-4d1b-853c-2c9c1eadf41e)


스레드가 동시에 작업을 진행하기에 총 실행 시간은 5초

synchronized 메서드를 선언 시 **상호 배제**(mutual exclusion)를 통해 스레드 간의 안전한 동기화를 보장한다는  
사실을 알 수 있다

### 고유 락 재진입 (Reentrancy)

고유 락 재진입이란 **이미 lock을 획득한 스레드는 추가 lock 획득 없이  
다른 synchronized 메서드를 호출**할 수 있는 것이다

```
public class Reentrancy {
    public synchronized void a(){
        System.out.println("a");
        b();
    }

    public synchronized void b(){
        System.out.println("b");
    }

    public static void main(String[] args) {
        new Reentrancy().a();
    }
}
```

1.  메서드 a가 객체의 고유 락을 획득한다
2.  a 내부에서 b를 호출할 때 같은 스레드는 이미 락을 보유하고 있으므로 바로 b 메서드를 실행할 수 있다

### Structured Lock vs Reentrant Lock

Structured Lock이란 고유 락을 이용한 동기화 구조이다

Structured Lock은 블록 단위로 lock의 획득 및 해제가 일어나므로

A획득 -> B획득 -> B해제 \-> A해제는 가능하지만

A획득 -> B획득 -> A해제 -> B해제 는 불가능하다

이를 가능하게 하기 위해서는 Reentrant Lock을 사용해야 한다

#### Structured Lock 예제

```
public class StructuredLockDemo {
    private final Object lockA = new Object();
    private final Object lockB = new Object();

    public void demonstrateStructuredLock() {
        synchronized (lockA) {
            System.out.println("Lock A acquired");
            
            synchronized (lockB) {
                System.out.println("Lock B acquired");
                // 여기서 lockB를 먼저 해제해야 함
            } // Lock B 해제
            
            // Lock A는 블록의 마지막에 자동으로 해제됨
        } // Lock A 해제
    }
}
```

#### Reentrant Lock 예제

```
import java.util.concurrent.locks.ReentrantLock;

public class ReentrantLockDemo {
    private final ReentrantLock lockA = new ReentrantLock();
    private final ReentrantLock lockB = new ReentrantLock();

    public void demonstrateReentrantLock() {
        lockA.lock();
        try {
            System.out.println("Lock A acquired");

            lockB.lock();
            try {
                System.out.println("Lock B acquired");
            } finally {
                lockA.unlock(); // 먼저 획득한 A 락 먼저 해제
            }
        } finally {
            lockB.unlock(); // 나중에 획득한 B 락 나중에 해제
        }
    }
}
```

### 가시성(Visibility)

값을 사용한 다음 블록을 빠져나가고 나면 다른 쓰레드가 변경된 값을 즉시 사용할 수 있게 해야한다는 의미

자바에서는 스레드가 락을 획득하는 경우 그 이전에 쓰였던 값들의 가시성을 보장한다

구조적 락과 명시적 락은 모두 가시성을 보장한다

출처

[https://velog.io/@tkdtkd97/Java-%EA%B3%A0%EC%9C%A0-%EB%9D%BD-Intrinsic-Lock](https://velog.io/@tkdtkd97/Java-%EA%B3%A0%EC%9C%A0-%EB%9D%BD-Intrinsic-Lock)

[https://brunch.co.kr/@kd4/156](https://brunch.co.kr/@kd4/156)
