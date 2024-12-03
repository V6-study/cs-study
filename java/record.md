record란

2020년 초에 Java 14 버전에서 프리뷰 기능으로 제공되어

Java 16 버전에서 정식 스펙으로 채택된 새로운 클래스 타입이다

데이터 중심의 클래스를 정의하기 위한 간결한 방법으로 주요 특징은 다음과 같다

1.  간결한 데이터 클래스 정의
2.  자동으로 메서드 생성 (toString(), equals(), hashCode(), getter)
3.  불변성 보장
4.  생성자 및 검증 로직 추가 가능
5.  추가 메서드 정의 가능

record는 DTO, 불변 데이터 모델, 값 객체 등을 만들 때 유용하게 사용할 수 있다

예제 코드를 살펴보자 

```
// 소괄호 안에 필드 변수를 정의하며 자동으로 private final로 선언
public record Student(
    String name,
    int age,
    String major,
    double gpa
) {}
```

기본 구조는 위와 같다

추가적으로 **컴팩트 생성자**를 통한 **추가적인 검증이나 정규화**를 수행할 수 있으며

추가 메서드를 정의할 수 있다 (값 변경은 불가능)

적용 코드

```
public record Student(
    String name,
    int age,
    String major,
    double gpa
) {
    // 컴팩트 생성자 (선택적)
    public Student {
        if (age < 0) {
            throw new IllegalArgumentException("나이는 음수일 수 없습니다.");
        }
        
        // 이름의 앞뒤 공백 제거
        name = name.trim();
    }
    
    // 추가 메서드 정의
    public boolean isHonorStudent() {
        return gpa >= 4.0;
    }
}
```

다음으로 정의한 레코드를 사용하는 예시를 살펴보자

```
public class StudentManagementSystem {
    public static void main(String[] args) {
        // 레코드 인스턴스 생성
        Student student1 = new Student("김철수", 22, "컴퓨터 과학", 4.5);
        Student student2 = new Student("이영희", 21, "경영학", 3.8);

        // 레코드의 자동 생성된 메서드 사용
        System.out.println("학생 이름: " + student1.name());
        System.out.println("전공: " + student1.major());
        
        // 커스텀 메서드 사용
        System.out.println("최우등생인가요? " + student1.isHonorStudent());

        // toString() 메서드 (자동 생성)
        System.out.println(student1);

        // equals() 메서드 (자동 생성)
        Student sameStudent = new Student("김철수", 22, "컴퓨터 과학", 4.5);
        System.out.println("두 학생은 같은가? " + student1.equals(sameStudent));
    }
}
```

위와 같이 사용할 수 있는데

POJO의 경우 필드 이름이 name이라면 getName()으로 메서드를 정의하여 호출하지만

레코드의 경우는 필드 이름을 그대로 메서드 명으로 사용하여 것을 볼 수 있는데 이유는 다음과 같다고 한다

1.  간결성
    -   필드 이름을 그대로 사용함으로써 코드가 더 간결해지고 가독성이 향상된다
2.  직관성
    -   필드 이름과 getter 메서드 이름이 일치하여 더 직관적인 API를 제공한다
3.  불변성 강조
    -   Record는 불변 데이터 객체를 위한 것이므로 get 접두사 없이 필드 이름을 사용하는 것이 이러한 특성을 잘 반영한다
4.  함수형 프로그래밍 스타일
    -   필드 이름을 그대로 사용하는 방식이 함수형 프로그래밍 패러다임과 더 잘 어울린다

자세한 내용은 [https://openjdk.org/jeps/395](https://openjdk.org/jeps/395 "OpenJdk") 문서를 참고해보자
