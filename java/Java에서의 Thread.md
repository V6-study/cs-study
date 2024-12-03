쓰레드는 하나의 프로세스 내에서 독립적으로 실행되는 작업 단위로

프로그램 실행의 가장 작은 단위로 볼 수 있으며 이는 동시성 프로그래밍의 핵심 요소이다

일반적인 자바 애플리케이션은 하나의 메인 쓰레드로 시작하지만, 여러 작업을 동시에 처리하기 위해서는 추가적인 쓰레드가 필요하다

### 스레드

#### 1\. Thread 클래스 상속

```
public class MyThread extends Thread{
    @Override
    public void run(){
        // 작업 내용
    }
}
```

Thread 클래스를 상속받고 run 메서드를 재정의하여 구현한다

구현 예제

```
public class MyThread extends Thread{
    @Override
    public void run(){
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + ": " + i);
            try {
                Thread.sleep(500);
            }catch (InterruptedException e){
                System.out.println("Thread interrupted");
            }
        }
    }
}
```

실행 결과

```
public class test {
    public static void main(String[] args) {
        Thread thread1 = new MyThread();
        Thread thread2 = new MyThread();

        thread1.start();
        thread2.start();
    }
}
```

<div align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdIrUNq%2FbtsKZL337Zn%2FKvnr1lrylmu8Lh8JoPvQ3K%2Fimg.png" alt="" width="500"/>
</div>

#### 2\. Runnable 인터페이스 구현

```
public class MyThread implements Runnable{
    @Override
    public void run(){
        // 작업 내용
    }
}
```

```
public class MyThread implements Runnable{
    @Override
    public void run(){
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + ": " + i);
            try {
                Thread.sleep(500);
            }catch (InterruptedException e){
                System.out.println("Thread interrupted");
            }
        }
    }
}
```

```
public class test {
    public static void main(String[] args) {
        Thread thread1 = new Thread(new MyThread(),"myThread");
        
        // Runnable 인터페이스는 함수형 인터페이스로 익명구현객체를 통해 thread를 생성할 수도 있다
        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println(Thread.currentThread().getName() + ": " + i);
                try {
                    Thread.sleep(500);
                }catch (InterruptedException e){
                    System.out.println("Thread interrupted");
                }
            }
        },"myThread");

        thread1.start();
        thread2.start();
    }
}
```

#### 실행 결과

<div align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd12bsr%2FbtsKYxTgEB3%2FZZGrBrRp09Tecj7uXoFJP0%2Fimg.png" alt="" width="500"/>
</div>

스레드의 실행 순서는 스케줄러에 의해 결정되며 보장되지 않는 특징을 갖고 있다.

JVM의 스레드 스케줄링에 따라 각 스레드의 실행 순서가 달라질 수 있다

두 예제에서는 run 메서드를 재정의 했지만,

**Thread의 start 메서드를 통해 실행**했다

이유를 살펴보기 전에 먼저 구현했던 코드를 **run 메서드로 다시 실행**해보자

```
thread1.run();
thread2.run();
```

#### 결과

<div align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0kpLu%2FbtsKYK56wjo%2FxCoAPleKpW8PPGOwnQ0amK%2Fimg.png" alt="" width="500"/>
</div>


왼쪽의 그림은 기존 start 메서드를 통해 실행한 결과

오른쪽의 그림은 run 메서드를 통해 실행한 결과이다

우리가 재정의 했던 메서드의 출력 부분은 다음과 같다 

```
System.out.println(Thread.currentThread().getName() + ": " + i);
```

현재 작업이 **진행되고 있는 스레드의 이름**을 호출하도록 구현하였는데

main이라는 이름의 스레드가 실행되고 있는 것을 확인할 수 있다

main 스레드는 기본적으로 main 메서드를 실행하는 첫 번째 스레드이다

run 메서드를 사용하면 코드는 구현 코드는 정상적으로 동작하지만

현재 스레드의 **호출 스택**에서 순차적으로 실행되며

첫번째 스레드의 작업이 끝나야 두번째 스레드의 작업이 시작된다

이처럼 구현한 run 메서드를 직접 실행하면 멀티스레딩 형식으로 작업이 이루어지지 않는다

그렇다면 start 메서드는 어떻게 동작하길래 멀티스레딩 형식으로 작업이 이루어지는 것일까 ??

**start 메서드** 가장 큰 특징은 **각 스레드별 독립적인 호출 스택을 생성**한다는 것이다

<div align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FccjGL4%2FbtsKY1GzZkA%2FcoerIqPeFoL0J6GICj5aHk%2Fimg.png" alt="" width="500"/>
</div>

start 메서드를 호출하면 JVM은 스레드를 위한 호출 스택을 새로 만들어주고

스레드의 각 호출 스택 안에는 run 메서드가 호출된다 

그렇기에 실제 멀티 스레딩 형식으로 작업을 진행하기 위해서는 start 메서드를 통해서 스레드를 실행시켜야 한다

### 실행 제어

<div align="center">
  <img src="https://blog.kakaocdn.net/dn/bZ299f/btsKX3SHinb/K7ypMWWSn34iKVpqz5J2k0/img.gif" alt="" width="500"/>
</div>


스레드의 상태를 제어해야할 때 제어를 하기 전 해당 스레드의 상태를 알아야 한다

이 때 스레드의 getState 메서드를 통해 상태를 확인할 수 있다

| 상태 | 상수 | 설명 |
| --- | --- | --- |
| 객체 생성 | NEW | 스레드 객체는 생성되고 start() 메서드가 호출되지 않은 상태 |
| 실행 대기 | RUNNABLE | 실행 중 또는 실행 가능 상태 |
|   | WATING | 실행가능하지 않은 일시정지 상태 |
| 일시 정지 | TIME\_WATING | 주어진 시간 동안 기다리는 상태 |
|   | BLOCKED | 동기화 블럭에 의해 일시정지된 상태(사용하고자 하는 객체의 lock이 풀릴 때까지 기다리는 상태) |
| 종료 | TERMINATED | 실행을 마친 상태 |

상태를 통해 동기화와 스케줄링을 관리할 수 있다

동기화 관리

-   wait, notify, notifyAll 메서드를 통해 스레드 간 협업을 조절할 수 있다
    -   wait
        -   스레드가 lock을 가지고 있는 상태에서 호출 (lock == 임계 영역에 대한 접근 권한)
        -   해당 스레드는 lock을 반납하고 대기 상태로 전환
        -   다른 스레드가 notify 혹은 notifyAll을 호출할 때까지 대기
    -   notify
        -   대기 상태에 있는 스레드 중 하나를 깨움
        -   깨어난 스레드에게 lock을 얻을 수 있는 기회를 제공
        -   깨어난 스레드는 현재 실행 중인 스레드가 lock을 놓을 때까지 기다린 후, 다른 스레드들과 lock을 획득하기 위해 경쟁
-   synchronized 키워드를 사용하여 임계 영역에 대한 접근을 제어할 수 있다
    -   한 번에 하나의 스레드만 접근할 수 있는 임계 영역을 생성

스케줄링 관리

-   sleep 메서드로 스레드를 일시 정지 상태로 만들어 다른 스레드에게 실행 기회를 줄 수 있다
-   yield 메서드를 사용하여 다른 스레드에게 실행을 양보할 수 있다
-   join 메서드로 특정 스레드의 작업이 끝날 때까지 대기할 수 있다

출처

[https://javatrainingschool.com/multithreading/start-and-run-method/](https://javatrainingschool.com/multithreading/start-and-run-method/)

[https://www.cs.fsu.edu/~jtbauer/cis3931/tutorial/essential/threads/lifecycle.html](https://www.cs.fsu.edu/~jtbauer/cis3931/tutorial/essential/threads/lifecycle.html)

[https://velog.io/@jsj3282/Thread%EC%9D%98-%EC%83%81%ED%83%9C](https://velog.io/@jsj3282/Thread%EC%9D%98-%EC%83%81%ED%83%9C)
