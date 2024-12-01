# **Casting(업캐스팅 & 다운캐스팅)**


## **1 캐스팅이란?**

- 한 데이터 타입에서 다른 데이터 타입으로 변환하는 작업

## **2 캐스팅이 필요한 이유는?**

### **2.1 다형성**

- 객체가 여러 가지 형태를 가질 수 있는 성질
- 주로 **부모 클래스 타입**의 변수로 **자식 클래스 객체**를 참조
    - 오버라이딩된 함수를 분리해서 활용할 수 있다.
- 형변환의 종류
    1. **묵시적 형변환** (업캐스팅) : 캐스팅이 자동으로 발생
        
        ```java
        Parent p = new Child(); // (Parent) new Child()할 필요가 없음
        ```
        
        > Parent를 상속받은 Child는 Parent의 속성을 포함하고 있기 때문
        > 
    2. **명시적 형변환** (다운캐스팅) : 캐스팅할 내용을 적어줘야 하는 경우
        
        ```java
        Parent p = new Child();
        Child c = (Child) p;
        ```
        
        > 다운캐스팅은 업캐스팅이 발생한 이후에 작용한다.
        > 

### **2.2 상속**

- 캐스팅을 통해 범용적인 프로그래밍이 가능

```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}
class Dog extends Animal {
    void sound() {
        System.out.println("Dog barks");
    }
}
class Cat extends Animal {
    void sound() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal[] animals = { new Dog(), new Cat() };  // 업캐스팅
        for (Animal animal : animals) {
            animal.sound();  // 다형성: 실제 객체 타입에 따라 메서드 호출
        }
    }
}

```
