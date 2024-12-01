> has-a 관계
> 
> - **컴포지션**이나 **집합 관계(Aggregation)**로 구현
> - X는 Y를 가지고 있다(X has a Y) 라는 문장이 성립할 때 적합

> is-a 관계
> 
> - **상속**을 사용하여 구현
> - X는 Y다(X is a Y) 라는 문장이 성립할 때 적합

### 1 컴포지션이란?

- 객체를 포함(조립)하여 다른 객체를 구성하는 방식
- has-a 관계 표현

### 2 **상속과의 차이**

- 상속
    - **부모-자식 관계**를 형성
    - is-a 관계가 명확할 때 사용
    - 부모 클래스의 특성과 동작을 자식 클래스가 물려받음
- 컴포지션
    - 한 클래스가 다른 클래스의 **인스턴스를 멤버 변수**로 포함하여 동작을 확장

### 3 구조

- private 인스턴스 형태로 객체 포함
- 인스턴스 객체의 메서드나 속성 활용
    
    ```java
    class Engine {
        void start() {
            System.out.println("Engine starts...");
        }
    }
    
    class Car {
        private Engine engine;  // Car has an Engine (컴포지션)
    
        Car(Engine engine) {
            this.engine = engine;
        }
    
        void startCar() {
            engine.start();
            System.out.println("Car starts...");
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            Engine engine = new Engine();
            Car car = new Car(engine);
            car.startCar();
        }
    }
    
    ```
    

### 4 장단점

- 장점
    - 클래스 간 상속의 대안 또는 보완책으로 자주 사용
        - 상속에 비해 약한 결합, 높은 유연성과 확장성
    - 다중 상속과 유사한 기능 구현 가능
- 단점
    - **구현 복잡성 증가**
    - **설계 의도 전달 어려움**
        - has-a 관계는 is-a 관계만큼 직관적이지 않음
