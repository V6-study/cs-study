## **Stream API 란?**

java 8에 도입된 기능으로, **Collection(예: list, set, map 등)을 처리**하는데 사용된다.

데이터의 연속적인 흐름으로, 람다 표현식과 함께 사용하여 매우 **간결하고 효율적**으로 데이터를 처리할 수 있다.

주요 특징은 다음과 같다.

-   **선언적**  
    스트림 API는 데이터를 어떻게 처리할지에 대한 '방법'보다는 '**무엇을 할 것인지**'에 초점을 맞춘다.
    
-   **함수형 프로그래밍**  
    람다 표현식을 사용하여 더 간결하고 읽기 쉬운 코드를 작성할 수 있다.
    
-   **지연 연산**  
    스트림의 중간 연산은 **지연(lazy) 연산**으로, 최종 연산이 호출될 때까지 실제로 수행되지 않는다.  
    최종연산이 호출될 때, 스트림 파이프라인 전체가 실행되며 데이터 처리가 수행된다.
    
-   **병렬 처리**  
    스트림은 쉽게 병렬 처리를 할 수 있는 메서드를 제공한다.  
    ㄴparallelStream()을 사용하여 병렬 스트림을 생성할 수 있다.
    
-   **내부 반복**  
    코드의 일관성과 관결성을 유지할 수 있다.  
    내부 반복은 최적화가 라이브러리 수준에서 이루어지며, 대규모 데이터셋이나 병렬 처리에 대해 더 나은 성능을 제공할 수 있다.


<br/><br/><br/>

## **Stream API 활용 상황**

-   대량의 데이터에 대해 반복적인 작업을 수행할 때.
-   데이터 변환 및 필터링이 필요할 때.
-   데이터 집계나 통계가 필요할 때.
-   데이터의 특정 조건을 검사할 때.

<br/>

**전통적인 loop문(for, while) VS Stream API**

| **비교 항목**          | **Stream API**                                        | **for 문**                                                |
|---------------------|--------------------------------------------------------|----------------------------------------------------------|
| **가독성**            | 선언적 스타일로 코드가 간결하고 명확함                          | 익숙하고 직관적이나, 복잡한 로직에서는 코드가 장황해질 수 있음          |
| **함수형 프로그래밍**    | 함수형 프로그래밍 스타일을 지원, 람다식과 메서드 참조 사용 가능       | 함수형 프로그래밍 스타일 지원 부족                               |
| **병렬 처리**          | 병렬 처리 쉽게 구현 (`parallelStream()` 사용)               | 병렬 처리 구현이 복잡함                                       |
| **지연 연산**          | 지연 연산으로 최적화 가능                                    | 즉시 연산                                                 |
| **디버깅 용이성**       | 디버깅이 상대적으로 어려울 수 있음                             | 각 단계에서 변수 추적이 쉬움                                   |
| **상태 유지**          | 상태 유지가 어렵고 비효율적일 수 있음                           | 반복문 내부에서 상태 유지 및 수정이 용이함                        |
| **제어 구조**          | 제한적 (`forEach` 내부에서 `break`, `continue` 사용 불가)    | 다양한 제어 구조 (`break`, `continue`, `return` 등) 사용 가능  |

<br/>

정리!
- 스트림 API는 가독성, 병렬 처리, 함수형 프로그래밍 스타일, 지연 연산 등의 이점으로 인해 데이터 처리 작업에 매우 유용하다.
- 특히 **복잡한 데이터 변환 및 필터링 작업**에 적합하다
- for 문은 단순한 반복 작업, 상태 유지가 필요한 작업, 디버깅이 중요한 경우, 다양한 제어 구조를 사용해야 하는 경우에 더 적합하다.



<br/><br/><br/>

## **Stream API 기본 사용**

### **Stream 생성**

**예시)**

```
import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

public class CollectionStreamExample {
    public static void main(String[] args) {
        // Collection에서 스트림 생성
        List<String> list = Arrays.asList("apple", "banana", "cherry");
        
        Stream<String> stream = list.stream();
        stream.forEach(System.out::println);
        
        
        
        // Array에서 stream 생성
        String[] array = {"apple", "banana", "cherry"};
        
        Stream<String> stream1 = Arrays.stream(array); // Arrays.stream() 사용
        stream1.forEach(System.out::println);

        Stream<String> stream2 = Stream.of(array); // Stream.of() 사용
        stream2.forEach(System.out::println);
        
        
        // Range에서 stream 생성
        IntStream rangeStream = IntStream.range(1, 10);
        rangeStream.forEach(System.out::println);
    
    }
}
```

Collection에서는 `컬렉션변수.stream()` 을 통해 스트림 생성이 가능하다.

Array에서는 `Arrays.stream(배열)`또는 `Stream.of(배열)` 을 통해 생성 가능하다.

<br/><br/>

### **중간 연산**

중간 연산은 스트림을 변환하거나 필터링하는 것이며, 또 다른 스트림을 반환한다.

중간 연산은 지연(lazy) 연산으로 최종 연산이 호출되기 전까지 실제로 수행되지 않는다.

다음은 자주 사용되는 중간 연산들이다.


**filter()**

조건에 맞는 요소만을 포함하는 새로운 스트림을 반환한다.

```
List<Integer> evenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());
```

<br/>

**map()**

각 요소를 주어진 함수에 따라 변환하여 새로운 스트림 반환한다.

```
List<String> upperCaseWords = words.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());
```

<br/>

**distinct()**

중복된 요소를 제거한 새로운 스트림을 반환한다.

```
List<Integer> distinctNumbers = numbers.stream()
    .distinct()
    .collect(Collectors.toList());
```

<br/>

**sorted()**

스트림의 요소를 정렬하여 새로운 스트림을 반환한다.

comparator를 사용하여 커스텀 정렬도 가능하다

```
List<String> sortedWords = words.stream()
    .sorted()
    .collect(Collectors.toList());


// 커스텀 정렬
List<String> sortedWordsByLength = words.stream()
    .sorted(Comparator.comparingInt(String::length))
    .collect(Collectors.toList());
```

<br/>

**limit()**

스트림의 요소를 지정한 개수만큼만 포함하는 새로운 스트림을 반환한다.

```
List<Integer> limitedNumbers = numbers.stream()
    .limit(5)
    .collect(Collectors.toList());
```

<br/>

**skip()**

처음 n개의 요소를 건너뛴 스트림을 반환한다.

```
List<Integer> skippedNumbers = numbers.stream()
    .skip(3)
    .collect(Collectors.toList());
```

<br/><br/>

### **최종 연산**

스트림 요소를 소모하여 결과를 생성하는 연산을 말한다.

최종 연산이 호출된 후 스트림 요소들이 처리되고, 스트림은 더 이상 사용할 수 없다.

아래는 자주 사용되는 최종 연산들이다.

**collect()**

스트림 요소를 수집하여 컬렉션을 다른 형태로 변경한다.

```
List<String> collectedList = words.stream()
    .collect(Collectors.toList());
```

<br/>

**forEach()**

스트림 각 요소에 대해 주어직 작업을 수행한다. 결과를 반환하지는 않는다.

```
words.stream()
    .forEach(System.out::println);
```

<br/>

**reduce()**

스트림 요소들을 결합하여 하나의 값으로 만든다.

```
int sum = numbers.stream()
    .reduce(0, Integer::sum);
```

<br/>

**min(), max()**

스트림의 최소값 또는 최대값을 반환한다. 요소가 없으면 빈 Optional을 반환한다.

```
Optional<Integer> minNumber = numbers.stream()
    .min(Integer::compareTo);

Optional<Integer> maxNumber = numbers.stream()
    .max(Integer::compareTo);
```
