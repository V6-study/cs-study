### **배열(Array)**

배열은 동일한 데이터 타입의 요소들이 연속된 메모리 공간에 저장된 자료 구조이다.

**특징 :** 

-   배열 선언 시 **크기가 고정**된다.
-   한번 결정된 **크기는 변경할 수 없다**.
-   **인덱스를 사용하여 요소에 빠르게 접근**할 수 있으며, 인덱스는 0부터 시작한다.
-   배열 안의 모든 요소가 **동일한 데이터 타입**을 가진다.
-   **연속된 메모리 공간**에 할당 된다.

<br/><br/>

#### **Java에서 배열 다루기**

```Java
// 배열 만들기
int[] arr1;

// 배열 만들면서 크기 지정
int[] arr2 = new int[5];

// 배열 만들면서 크기+값 지정
int[] arr3 = {1, 2, 3, 4, 5};

// arr3 배열에서 index 0인 요소에 접근
System.out.println(arr3[0]); // 1
```

> **Java의 Arrays 클래스**  
> Java에서 배열 자체에는 메서드가 없지만, 배열을 다룰 때 Arrays 클래스가 유용한 메서드들을 제공한다.

```Java
import java.util.Arrays;

int[] array = {5, 2, 8, 1, 3};

// 배열을 문자열로 출력
System.out.println(Arrays.toString(array)); // [5, 2, 8, 1, 3]

// 배열 정렬
Arrays.sort(array);
System.out.println(Arrays.toString(array)); // [1, 2, 3, 5, 8]

// 배열 복사
int[] newArray = Arrays.copyOf(array, array.length);
System.out.println(Arrays.toString(newArray)); // [1, 2, 3, 5, 8]

// 배열의 특정 값으로 채우기
Arrays.fill(array, 10);
System.out.println(Arrays.toString(array)); // [10, 10, 10, 10, 10]

// 배열 이진 탐색 (정렬된 배열에서만 사용 가능)
int index = Arrays.binarySearch(newArray, 5);
System.out.println(index); // 3
```

<br/><br/><br/>

### **리스트(List)**

리스트는 요소의 데이터 제한이 없고, 크기가 가변적인 자료구조이다.

**특징 :** 

-   가변 크기이다. 필요에 따라 크기를 늘리거나 줄일 수 있다.
-   다양한 데이터 타입의 요소들을 포함할 수 있다.
-   (Linked List의 경우) 비연속적 메모리 공간에 할당될 수 있다.
-   배열과 비교했을 때 삽입, 삭제 등의 연산이 배열보다 유연하게 수행될 수 있다.

Java에서 List 인터페이스를 통해 ArrayList, LinkedList 등의 여러 구현체를 만들 수 있다.

이 둘의 공통적 특징은 요소의 순서를 유지하고, 중복을 허용한다는 점이다.

❗️ Python 에서는 리스트 타입이 배열 기능을 제공한다.

<br/><br/><br/>


### **ArrayList**

ArrayList는 **가변 크기**의 **배열 기반** 리스트이다.

**특징 :** 

-   초기 용량을 지정할 수도 있고, 필요에 따라 크기를 자동으로 증감할 수 있다. 즉 **크기가 동적**이다.
-   요소의 **추가, 삭제, 검색, 순회** 등의 작업을 효율적으로 수행할 수 있다.
-   배열과 마찬가지로 **인덱스를 사용해 요소에 접근**한다. 즉, 빠른 접근이 가능하다.
-   배열과 마찬가지로 **연속된 메모리 공간**에 할당된다.

<br/><br/>

#### **Java에서 ArrayList 다루기**

```Java
import java.util.ArrayList;

public class ArrayListExample {
    public static void main(String[] args) {
    	// ArrayList 생성
        ArrayList<String> fruits = new ArrayList<>(); // 기본 생성자 (초기용량 10)
        // 초기 용량을 20으로 정해주고 싶으면 new ArrayList<>(20)으로 해주면 됨.

        // 요소 추가
        fruits.add("Apple"); // 리스트 끝에 추가
        fruits.add("Orange");
        fruits.add(1, "Banana"); // 지정된 index에 요소 삽입

        // 요소 접근
        String fruit = fruits.get(1); // "Banana"

        // 요소 변경
        fruits.set(1, "Pineapple");

        // 요소 제거
        fruits.remove("Apple");

        // 리스트 크기 확인
        int size = fruits.size();

        // 리스트의 모든 요소 출력
        for (String item : fruits) {
            System.out.println("Fruit: " + item);
        }
        
        // 리스트 안에 요소 포함되어 있는지 확인
        fruits.contains("Pineapple"); // true
        
        // 리스트 모든 요소 제거
        fruits.clear();
        
        // 리스트가 비어있는지 확인
        fruits.isEmpty(); // true
        
    }
}
```

** 연속된 메모리 공간을 할당해주는데 어떻게 동적으로 크기 조절을 해주지? **

ArrayList는 초기 용량을 초과하여 요소를 추가할 경우 내부적으로 더 큰 배열을 생성하고 기존 배열의 요소를 복사한다. 이 과정이 자동으로 처리되어 개발자는 배열 크기나 복사 작업에 대해 신경 쓸 필요가 없다.



ex) 초기 용량 10인 ArrayList에 11번째 요소를 추가할 경우

더 큰 배열을 생성 -> 기존 배열 요소들을 복사함 -> ArrayList의 내부 배열 참조를 새로운 배열로 변경

<br/><br/><br/>

### **LinkedList**

List, Deque, Queue 인터페이스를 구현하여 다양한 방식으로 사용될수 있다.


**특징 :**

<img width="945" alt="image" src="https://github.com/user-attachments/assets/c904f209-d2fa-4a8a-8951-125a4d6c05bc">

-   각 요소(Node)는 자신과 뒤의 요소를 가리키는 참조(포인터)를 가지고 있다.
-   요소 **삽입과 삭제**가 배열이나 ArrayList보다 **효율적**이다. 노드 참조만 변경해주면 된다.
-   반면 **요소 접근은 비교적 비효율적**이다. head 노드부터 순차적으로 찾아야하기 때문에.
-   **메모리 할당이 비연속적**이다.

![image](https://github.com/user-attachments/assets/7173ab76-981b-4116-abe3-147c96aab962)


Java에서 LinkedList는 앞/뒤 요소를 가리키는 2개의 포인터를 갖고있다.  
즉, 이중 연결 리스트(doubly linked list)를 기반으로 한다. 이를 통해 양방향으로 리스트 순회가 가능하다.

<br/><br/>

#### **Java에서 LinkedList 다루기**

```Java
import java.util.LinkedList;

public class LinkedListExample {
    public static void main(String[] args) {
        LinkedList<String> fruits = new LinkedList<>();

        // 요소 추가 (리스트 끝에 요소가 추가됨)
        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Orange");

        // 첫 번째 위치에 요소 추가
        fruits.addFirst("Strawberry");

        // 마지막 위치에 요소 추가
        fruits.addLast("Watermelon");

        // 요소 접근
        // get(int index): 지정된 위치의 요소를 반환
        String firstFruit = fruits.getFirst(); // 첫번째 요소 반환 : "Strawberry"
        String lastFruit = fruits.getLast(); // 마지막 요소 반환 : "Watermelon"

        // 요소 변경
        fruits.set(2, "Pineapple"); // Index 2의 요소 Banana를 Pineapple로 변경

        // 요소 제거
        // remove(Object o): 지정된 요소를 리스트에서 제거
        // remove(int index): 지정된 위치의 요소를 제거
        fruits.removeFirst(); // 첫 번째 요소 제거
        fruits.removeLast(); // 마지막 요소 제거

        // 리스트 크기 확인
        int size = fruits.size();
        System.out.println("Size of the list: " + size);

        // 리스트의 모든 요소 출력
        for (String item : fruits) {
            System.out.println("Fruit: " + item);
        }
        
        // 리스트 안에 요소 포함되어 있는지 확인
        fruits.contains("Pineapple"); // true
        
        // 리스트 모든 요소 제거
        fruits.clear();
        
        // 리스트가 비어있는지 확인
        fruits.isEmpty(); // true
        
    }
}
```

<br/><br/><br/>

### **Array vs ArrayList vs LinkedList**

| **특성/기능** | **Array** | **ArrayList** | **LinkedList** |
| --- | --- | --- | --- |
| **기본 구조** | 고정 크기의 배열 | 동적 크기의 배열 기반 리스트 | 연결 리스트 |
| **메모리 할당** | 연속된 메모리 공간 | 연속된 메모리 공간 | 비연속적인 메모리 공간 |
| **초기 크기 지정** | 필요 | 필요 없음(기본 10) | 필요 없음 |
| **크기 조절** | 불가능 | 자동 조절   (새 배열 생성 및 복사)       | 자동 조절 |
| **인덱스를 통한 접근 시간** | O(1) | O(1) | O(n) |
| **삽입/삭제 시간** | O(n)   맨 끝은 O(1) | O(n)   맨 끝은 O(1) | O(1)       |
| **요소 검색 시간** | O(n) | O(n) | O(n) |
| **메모리 사용량** | 요소 크기에 비례 | 요소 크기 + 배열의 여유 공간 | 요소 크기 + 참조 크기 |
| **순차적 데이터 접근** | 빠름 | 빠름 | 느림 |
| **임의 데이터 접근** | 빠름 | 빠름 | 느림 |
| **적용 예** | 고정 크기의 데이터, 빈번한 읽기 | 동적 크기의 데이터, 빈번한 읽기 | 빈번한 삽입/삭제 작업 |




**참고**
배열은 동일한 데이터 타입, 리스트는 다양한 데이터 타입을 넣을 수 있다고 했지만
Java에서 리스트 뿐만 아니라 배열도 요소 타입을 Object로 선언하면 다양한 타입의 데이터 요소를 넣을 수 있다.
그럼에도, 컬렉션의 리스트는 더 높은 유연성과 타입 안전성을 제공한다. 
(배열은  런타임 타입 오류 가능. 리스트는 제네릭을 통해 컴파일 타임 타입 안전성 제공.)
상황에 따라 적절한 자료 구조를 선택하는 것이 중요하다.
