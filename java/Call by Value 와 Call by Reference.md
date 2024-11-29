### **값에 의한 호출 (Call by Value)**

메서드 호출 시 전달된 인수의 값을 복사하여 메서드에 전달하는 방식이다.

이 경우 원래 변수는 메서드 내부 변화에 영향을 받지 않는다.

**예시)**

```
public class Main {
    public static void modifyValue(int x) {
        x = 10;
    }
    
    
    public static void main(String[] args) {
        int a = 5;
        modifyValue(a);
        System.out.println(a); // 출력: 5
    }
}
```

modifyValue에서 10으로 값 변경하기를 시도했음에도, a는 여전히 5인것을 볼 수있다.

이는 인수로 a가 전달될 때, 변수 x에 a 자체가 아닌 a의 값이 5가 대입되었기 때문이다.

또한, x는 함수 파라미터로 선언되었기 때문에 함수 안에서만 유효하다.

### **참조에 의한 호출(Call by Reference)**

메서드 호출 시 인수로 전달된 변수의 참조(메모리 주소)를 전달하는 방식이다.

메서드 내에서 참조된 변수의 "값"이 변경되면, 원래 변수에도 영향을 미친다.

**예시)**

```
#include <iostream>
using namespace std;

void modifyReference(int &x) {
    x = 10;
}

int main() {
    int a = 5;
    modifyReference(a);
    cout << a << endl; // 출력: 10
    return 0;
}
```

Java는 참조에 의한 호출이 없기 때문에 C++ 코드로 예시를 들겠다.

이 코드에서는 메서드는 파라미터로 int &x를 선언했다. 이는 x가 int 타입 변수의 참조임을 의미한다.

인수로 a를 전달하였을 때 참조 변수 x는 값이 아닌 a의 "참조(메모리 주소)"가 x에 전달된다.

a와 x는 같은 참조(메모리 주소)를 쓰기 때문에 메서드 내부에서 x의 값이 변경되면 a의 값도 변경된다.

---

### **❗️Java의 호출 방식은 '값에 의한 호출(Call by Value)'이다.**

이는 기본 데이터 타입뿐만 아니라 객체 타입에 모두 적용된다.

즉, Java에서 객체 참조(reference)를 전달할 때에도 값에 의한 호출을 사용한다.

즉, **변수의 값을 복사하여 대입**한다.

근데, Java에서도 객체를 메서드의 인수로 넘기고 값을 변경하면 메서드 외부에서 변경된 값을 확인할 수 있지 않나? 

명확히 이해하기 위해서는 Java가 값을 대입하는 방식에 대해 이해할 필요가 있다.

기본형과 참조형에 대한 대입 예시를 살펴보자.

#### **기본형 vs 참조형**

**기본형(Primitive Type)**

-   int , long , double , boolean 처럼 변수에 사용할 값을 직접 넣을 수 있는 데이터 타입.
-   선언과 동시에 크기가 정해진다. 따라서 크기를 동적으로 바꿀수 없다.
-   참조형에 비해 더 빠르고 메모리를 효율적으로 처리한다.

**Primitive Type 대입 예시**

```
int a = 10;	// a에 값 대입
int b = a;	// b에 a 대입
a = 11;		// a 값 변경
System.out.println("a="+a); // a=11
System.out.println("b="+b); // b=10
```

b에 a를 대입한 후 a의 값을 변경하면? a는 변경되지만 b는 변경되지 않는다.

**Java에서는 항상 변수의 값을 복사해서 대입한다.**

변수에 기본형 값(10)이 바로 들어가 있기 때문에, 해당 값만 복사해서 대입한다. 때문에 변수 b의 값은 변경되지 않는 것이다.

**참조형(Reference Type)**

-   int\[\] students, Student student 와 같이 데이터에 접근하기 위한 참조(주소)를 저장하는 데이터 타입.
-   동적으로 메모리 할당이 가능하다.
-   기본형과 달리 더 복잡한 데이터 구조를 만들고 관리할 수 있다.

**Reference Type 대입 예시**

```
int[] arr1 = new int[2];
int[] arr2 = arr1;
arr1[0]=1;

System.out.println("arr1="+ Arrays.toString(arr1)); // arr1=[1,0]
System.out.println("arr2="+ Arrays.toString(arr2)); // arr2=[1,0]
```

위에서 배열 객체를 생성하여 arr1 변수에 넣고, 이를 arr2에 대입하였다. arr1을 변경하고 출력하면, arr2도 변경되어 있음을 알 수 있다. 이는 두개가 동일한 배열을 참조하고 있기 때문이다. 하지만 이것도 "값에 의한 호출"이다.

arr1은 참조형 변수로, 이 변수는 참조값을 갖고 있다.

arr2도 참조형 변수로, arr1의 값을 복사하여 대입한다.

두 변수가 아예 같은 참조였다면, arr2가 변경되면 arr1도 변경될 것이다.

```
arr2 = new int[] {1, 2, 3};

System.out.println("arr1="+ Arrays.toString(arr1)); // arr1=[1,0]
System.out.println("arr2="+ Arrays.toString(arr2)); // arr2=[1,2,3]
```

arr2에 다른 객체를 대입해도 arr1은 변경되지 않는다. 이는 참조 "값"을 복사하여 대입하였기 때문이다.

#### **객체 타입 메서드 호출**

대입 방식을 이해했다면, 객체 인수가 전달될 때 파라미터에 어떻게 저장되는지 이해될 것이다.

아래 예시를 살펴보자.

```
public class Main {
    public static void main(String[] args) {
        MyObject obj = new MyObject(5);
        modifyObject(obj);
        System.out.println(obj.value); // 출력: 10

        resetObject(obj);
        System.out.println(obj.value); // 출력: 10
    }

    public static void modifyObject(MyObject o) {
        o.value = 10; // 객체의 필드 값을 변경
    }

    public static void resetObject(MyObject o) {
        o = new MyObject(20); // 참조 변수를 새로운 객체로 변경
    }
}

class MyObject {
    int value;
    MyObject(int value) {
        this.value = value;
    }
}
```

-   'modifyObject'와 'resetObject'메서드에 전달되는 값은 객체의 참조값(메모리 주소)의 복사된 값이다.
-   'modifyObject'에서의 'o'와 원래 변수(obj)는 같은 참조값을 가지므로,  
    메서드 내부에서 'o' 객체의 상태를 변경하면 원래 객체에도 영향을 미친다.
-   그러나, resetObject 메서드 내에서 'o'를 새로운 객체로 변경하면 새로운 참조 값을 가지게 되고,  
    원래 변수(obj)에는 영향을 미치지 않는다.  
    참조값을 복사한 값을 저장했을 뿐, 같은 참조가 아님을 증명.
