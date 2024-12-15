### **01 더티 체킹**

- 트랜잭션 안에서 엔티티의 변경이 일어난 경우, **변경된 엔티티의 모든 내용**을 자동으로 DB에 반영하는 것
    - 변경 감지 기준은 최초의 조회 상태
    - 명시적인 update 쿼리 작성 없이도 데이터베이스의 데이터가 갱신
- JPA에서 자동으로 처리되는 기능
- 더티(Dirty): 상태의 변화가 생김
- 체킹(Checking): 검사

### **02 주의점**

- **영속성 컨텍스트**가 관리하는 엔티티 대상으로만 이루어짐
    - 영속성 컨텍스트 : JPA가 엔티티 객체를 관리하는 작업을 수행하는 환경, 엔티티 객체들이 **영속 상태**로 관리되는 메모리 공간
- 변경된 엔티티의 모든 내용을 update 쿼리로 만들어 전달하는데, 이때 필드가 많아지면 전체 필드를 update하는게 비효율적일 수도 있음
    - **@DynamicUpdate**를 해당 Entity에 선언하여 변경 필드만 반영시키도록 만들어줄 수 있음
        
        ```
        @Getter
        @NoArgsConstructor
        @Entity
        @DynamicUpdate
        public class Order {
        
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            private Long id;
            private String product;
        ```
