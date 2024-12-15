### **01 JPA (Java Persistence API)**

- 자바 진영의 ORM 기술 표준
- EJB 엔티티빈 (자바 표준) ~ 하이버네이트 (오픈소스) ~ JPA (자바 표준) 의 과정을 거져 발전

### **02 ORM (Object-Relational Mapping)**

- 객체 관계 매핑
- 객체는 객체대로 설계, RDMS는 RDBMS대로 설계
- ORM 프레임워크가 중간에서 매핑
    - 즉, 객체가 DB테이블이 되도록 만들어줌

### **03 동작 방식**

![image](https://github.com/user-attachments/assets/f17a0d55-1937-4059-8497-44b3a12e122d)

JPA는 애플리케이션과 JDBC 사이에서 동작

### **3.1 저장**

![image](https://github.com/user-attachments/assets/8cf44fbb-be31-4db9-9fb1-1f900940740f)

### **3.2 조회**

![image](https://github.com/user-attachments/assets/4c0bf4aa-2b29-454a-bf8f-4d3af59f7499)

### **04 JPA를 사용해야 하는 이유**

- **객체 중심 개발 가능**
    - SQL 중심 개발 시,
        - 하나의 테이블 생성시, 이에 해당하는 CRUD 모두 만들어야 함
        - 추후에 컬럼 변경 시, SQL 모두 수정 필요
- **생산성 향상**
    - SQL 쿼리를 직접 생성하지 않고, 만들어진 객체에 JPA 메소드를 활용해 데이터베이스를 다룸
- **유지보수**
    - SQL 중심 개발 시, 쿼리 수정이 필요하면 DTO필드도 모두 변경 필요
    - JAP에서는 엔티티 클래스 정보만 변경하면 됨.
- **성능 증가**
    - 1차 캐시와 동일성 보장
        - 같은 트랜잭션 안에서는 같은 엔티티 반환
            
            ```
            String memberId = "100";
            Member m1 = jpa.find(Member.class, memberId);//SQL
            Member m2 = jpa.find(Member.class, memberId);//캐시
            println(m1 == m2)//true
            ```
            
    - 트랜잭션을 지원하는 쓰기 지연
        - 트랜잭션을 commit 할 때까지, INSERT SQL 모음
        - JDBC BATCH SQL 기능을 사용해서 한번에 SQL 전송
        - UPDATE, DELETE로 인한 로우(ROW) 락 시간 최소화
            - 로우 락 : 데이터베이스에서 특정 레코드(row)에 대해 다른 트랜잭션이 동시에 수정할 수 없도록 잠그는 메커니즘
    - 지연로딩과 즉시로딩 설정 가능
        - 지연로딩: 객체가 실제 사용될 때 로딩
        - 즉시 로딩: JOIN SQL로 한번에 연관된 객체까지 미리 조회
