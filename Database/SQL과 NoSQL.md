#### SQL

**관계형 데이터베이스** 관리 시스템(RDBMS)의 데이터를 관리하기 위해 설계된 프로그래밍 언어

-   장점
    -   데이터는 정해진 데이터 **스키마에 따라 테이블에 저장 → 무결성 보장**
        -   스키마: 테이블, 열, 관계, 제약 조건 등의 정보를 포함
    -   데이터는 **관계를 통해 여러 테이블에 분산**
        -   정규화 과정 (데이터의 중복을 피하고 일관성을 유지하기 위한 원칙)
    -   ACID 원칙을 준수하여 트랜잭션의 안정성 보장
-   단점
    -   스키마 변경이 어려워 **유연성이 떨어짐**
    -   복잡한 조인 쿼리로 인한 **성능 저하** 가능성
    -   **대체로 수직적 확장**만 가능  
        -   수평적 확장이 어려운 이유
            -   테이블 간 관계로 데이터 분산이 복잡함
            -   스키마 변경 시 모든 서버에 영향
            -   ACID 준수를 위한 오버헤드 발생
            -   JOIN 연산이 서버 간에 이루어져야 하므로 성능 저하

-   사용 사례
    -   금융, 결제 등 데이터 정합성이 중요한 서비스
    -   관계가 복잡한 데이터를 다루는 경우
    -   데이터 구조가 명확하고 변경이 적은 경우

#### NoSQL

NoSQL은 Not Only SQL이라고 불림

이유는 SQL이 아닌 쿼리 언어를 사용할 수 있지만,

일부 NoSQL 데이터베이스에서는 SQL과 유사한 쿼리를 지원하기 때문

이를 통해 확장성, 유연성, 빠른 데이터 액세스가 필요한 상황에서 더 유리하게 활용됨

-   장점
    -   사전에 스키마를 정의하지 않아도 저장할 수 있음 **→ 유연한 관리**
        -   작업을 진행하는 동시에 데이터를 정의하는 방식
    -   단순 쿼리의 **빠른 처리 속도**
        -   **데이터를 하나의 컬렉션에 포함**하여 조인 연산 최소화
            -   다른 구조의 데이터를 같은 컬렉션에 추가할 수 있음
            -   관계 개념이 없으며 관련 데이터를 동일한 컬렉션에 저장함
        -   인메모리 캐싱 적극 활용
        -   분산 처리를 통한 병렬 처리
    -   수직 믹 **수평 확장**에 자유로움
        -   NoSQL은 처음부터 분산 환경을 고려하여 설계됨
            -   데이터를 여러 서버에 분산 저장하는 것이 기본 아키텍처
-   단점
    -   일관성 유지의 어려움
        -   데이터가 여러 컬렉션에 중복되어 있기 때문에 수정 시 모든 컬렉션에서 작업을 수행해야 함  
            
            ```
            // 주문 컬렉션
            orders = {
                order_id: 1,
                user: {
                    name: "김철수",
                    email: "kim@email.com"
                },
                product: "노트북",
                price: 1000000
            }
            
            // 사용자 컬렉션
            users = {
                user_id: 1,
                name: "김철수",
                email: "kim@email.com"
            }
            ```
            
             
        -   이러한 단점을 BASE 원칙의 적용을 통해 최소화
            -   분산 시스템에서 완벽한 데이터 일관성 대신 고가용성과 성능을 우선시하며, 일시적인 데이터 불일치를 허용하되 최종적으로는 일관성을 보장하는 원칙
-   빠른 개발, 대용량 데이터 처리, 유연한 스키마가 필요한 프로젝트에 적합
-   사용 사례
    -   실시간 빅데이터 처리
    -   높은 확장성이 필요한 서비스
    -   SNS 등 비정형 데이터 처리가 많은 경우
    -   캐싱이 필요한 경우

#### NoSQL 데이터베이스 유형

<div align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvyWuR%2FbtsKCQ68LW4%2FIhbcuO2fEGQlGc8XYO2UsK%2Fimg.png" alt=""/>
</div>

-   문서형(Document) - MongoDB
    -   JSON 형태의 문서로 데이터 저장
    -   게시판, 로깅 등 다양한 타입의 데이터 처리에 적합
-   키-값(Key-Value) - Redis
    -   단순한 구조로 빠른 처리 속도
    -   캐싱, 세션 관리 등에 적합
-   컬럼형(Column) - Cassandra
    -   데이터를 컬럼 단위로 저장
    -   로그 데이터 등 시계열 데이터 처리에 적합
-   그래프형(Graph) -Neo4j
    -   노드간의 관계를 저장
    -   SNS, 추천 시스템 등 관계 데이터 처리에 적합

#### SQL vs NoSQL 선택 기준

-   데이터 구조
    -   SQL: 구조화된 데이터, 명확한 스키마
    -   NoSQL: 비구조화된 데이터, 유연한 스키마
-   확장성
    -   SQL: 수직적 확장 중심
    -   NoSQL: 수평적 확장 용이
-   트랜잭션
    -   SQL: ACID 준수, 강한 일관성
    -   NoSQL: BASE 원칙, 최종적 일관성
-   쿼리 성능
    -   SQL: 복잡한 조인에 강점
    -   NoSQL: 단순 쿼리에 강점
-   데이터 크기와 처리량
    -   SQL: 중소규모 데이터, 복잡한 트랜잭션 처리에 적합
    -   NoSQL: 대용량 데이터, 높은 처리량이 필요한 경우 적합
-   개발 및 유지보수
    -   SQL: 명확한 스키마로 인한 안정적인 유지보수
    -   NoSQL: 빠른 개발과 변경이 용이하나 일관성 관리에 주의 필요

출처

[https://seodeveloper.tistory.com/entry/DB-%EC%8A%A4%ED%82%A4%EB%A7%88%EC%99%80-%ED%85%8C%EC%9D%B4%EB%B8%94%EC%9D%80-%EB%8B%A4%EB%A5%B8%EA%B0%80%EC%9A%94](https://seodeveloper.tistory.com/entry/DB-%EC%8A%A4%ED%82%A4%EB%A7%88%EC%99%80-%ED%85%8C%EC%9D%B4%EB%B8%94%EC%9D%80-%EB%8B%A4%EB%A5%B8%EA%B0%80%EC%9A%94)

[https://nice-engineer.tistory.com/entry/SQL-vs-NoSQL](https://nice-engineer.tistory.com/entry/SQL-vs-NoSQL)

[https://www.oracle.com/kr/database/nosql/what-is-nosql/](https://www.oracle.com/kr/database/nosql/what-is-nosql/)

[https://velog.io/@issac/DB-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-Transaction%EC%9D%98-ACID-%EC%86%8D%EC%84%B1%EA%B3%BC-%EB%B6%84%EC%82%B0%EC%8B%9C%EC%8A%A4%ED%85%9C-BASE-%EC%86%8D%EC%84%B1](https://velog.io/@issac/DB-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-Transaction%EC%9D%98-ACID-%EC%86%8D%EC%84%B1%EA%B3%BC-%EB%B6%84%EC%82%B0%EC%8B%9C%EC%8A%A4%ED%85%9C-BASE-%EC%86%8D%EC%84%B1)
