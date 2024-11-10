## 용어정리

- **Statement** ⭐
    - 거의 DML(select DQL이라고도 함) 사용
    - SQL 바로 실행
- **PreparedStatement ⭐**
    - 매개변수화된 SQL문 사용가능
    - 미리 SQL준비 후 실행
        - 매개변수화 된 값만 바꾸기 때문에 속도가 압도적으로 빠름
    - 보안에 강함

## **01 Error based SQL Injection**

![image](https://github.com/user-attachments/assets/70133430-c73d-4dd7-bffc-c12e97c6c70b)

- `’ OR 1=1 --`  삽입
    - WHERE 절을 모두 참으로 만들고, -- 를 넣어줌으로 뒤의 구문을 모두 주석 처리
    - Users 테이블에 있는 모든 정보를 조회
    - 가장 먼저 만들어진 계정으로 로그인에 성공

## **02 Union based SQL Injection**

![image](https://github.com/user-attachments/assets/d21e4b9c-07c4-40d1-9e6f-7d80fa903077)

- Union 키워드와 함께 컬럼 수를 맞춰서 SELECT 구문을 넣어줌
- 두 쿼리문이 합쳐서서 하나의 테이블로 보여짐
- 인젝션이 성공하게 되면, 사용자의 개인정보가 게시글과 함께 화면에 보여짐

## **03 Blind SQL Injection**

### **Boolean based SQL**

- 특정한 값이나 데이터를 전달받지 않고, 단순히 참과 거짓만 이용해 정보 획득
- DB의 테이블 정보 등을 추출

![image](https://github.com/user-attachments/assets/ec212f45-8223-4be2-a254-5ce474da4d9c)

- 실제 로그인 가능한 계정 + sql 구문 주입
    - abc123’ and ASCII(SUBSTR(SELECT name From information_schema.tables WHERE table_type=’base table’ limit 0,1)1,1)) > 100 -- 이라는 구문을 주입
- 해당구문은 MySQL 에서 테이블 명을 조회하는 구문
    - 과정
        - limit 키워드를 통해 하나의 테이블만 조회
        - SUBSTR 함수로 첫 글자 확인
        - ASCII 를 통해서 ascii 값으로 변환
    - 결과
        - 만약 조회되는 테이블 명이 Users 라면 ‘U’ 자가 ascii 값으로 조회
        - 뒤의 100 이라는 숫자 값과 비교
        - 거짓 → 로그인 실패 → 참이 될 때까지 뒤의 100이라는 숫자를 변경해 가면서 비교

## **04 대응방안**

1. **특수문자 필터링**
    - 입력 값에서 SQL 쿼리 실행에 위험을 줄 수 있는 특수문자(예: `'`, `"`, `;`, `-`, `#`, `/*`, `/` 등)가 포함되지 않았는지 확인
2. **Prepared Statement 구문사용**
    - 입력값을 쿼리에 직접 포함하지 않고, 파라미터로 전달하여 실행
    - 입력값을 데이터로만 처리하여 쿼리 구문으로 인식하지 않으므로 SQL 인젝션 공격을 효과적으로 차단할 수 있음
3. **Error Message 노출 금지**
    - DB 에러 발생 시 따로 처리를 해주지 않았다면, 에러가 발생한 쿼리문과 함께 에러에 관한 내용을 반환
        
        ⇒ 테이블명, 컬럼명, 쿼리문 등 노출 위험
        
    - DB 오류발생 시 사용자에게 보여줄 수 있는 페이지를 제작 혹은 메시지박스 띄우기

**4. ORM (Object-Relational Mapping) 사용**

- 코드에서 직접 SQL을 작성하지 않고, 객체 지향적으로 데이터베이스와 상호작용
- ORM 라이브러리들은 내부적으로 SQL 인젝션 방어를 위해 파라미터 바인딩을 처리
- e.g.
    - JPA, Hibernate, Django ORM 등
 
## 05 출처

[🔗](https://noirstar.tistory.com/264) **SQL Injection 이란? (SQL 삽입 공격)**
