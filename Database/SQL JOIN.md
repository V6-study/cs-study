## SQL - JOIN
두 개 이상의 테이블이나 데이터베이스를 연결하여 데이터를 검색하는 방법
<br>

### 종류
- INNER JOIN
  - EQUI JOIN
  - NATURAL JOIN
  - CROSS JOIN
- OUTER JOIN
  - LEFT OUTER JOIN
  - RIGHT OUTER JOIN
  - FULL OUTER JOIN
<br>

### 1. INNER JOIN (내부 조인)
<img src="https://github.com/user-attachments/assets/6e3959e6-4802-475a-9e96-0115f314dc9f"/>
교집합을 나타내는 조인 방식
```sql
SELECT *
FROM employee INNER JOIN department
ON employee.DepartmentID = department.DepartmentID;
```
<br>

#### 1-1. EQUI JOIN (동등 조인)
- 조인 구문에서 동등비교만을 사용한다.
- 다른 비교 연산자를 사용하지 않는다.

#### 1-2. NATURAL JOIN (자연 조인)
- 동일한 이름을 가진 컬럼의 각 쌍에 대한 단 하나의 컬럼만 포함하고 있다.
```sql
SELECT * FROM employee NATURAL JOIN department;
```

#### 1-3. CROSS JOIN (교차 조인)
- 조인되는 두 테이블에서 곱집합을 반환한다.
- 예를 들어, m행을 가진 테이블과 n행을 가진 테이블이 교차 조인되면 m*n 개의 행을 생성한다.
<img src="https://github.com/user-attachments/assets/90aad3e7-44e4-428d-b0ed-872b46ea1a42"/>
<br>

### 2. OUTER JOIN (외부 조인)
특정 테이블의 데이터가 모두 필요한 상황에서 활용한다.

#### 2-1. LEFT OUTER JOIN
<img src="https://github.com/user-attachments/assets/561df5f1-70d4-41c4-b860-f277c75cb8de"/>
기준 테이블 값과 조인 테이블 값의 중복된 값을 보여준다.

```sql
SELECT *
FROM employee LEFT OUTER JOIN department
ON employee.DepartmentID = department.DepartmentID;
```

#### 2-2. RIGHT OUTER JOIN
<img src = "https://github.com/user-attachments/assets/c2b47326-2ed7-4950-8bc1-2e61528a2f59"/>
LEFT OUTER JOIN과 반대로 오른쪽 테이블 기준으로 JOIN한다.
```sql
SELECT *
FROM employee RIGHT OUTER JOIN department
ON employee.DepartmentID = department.DepartmentID;
```

#### 2-3. FULL OUTER JOIN
<img src="https://github.com/user-attachments/assets/6f8f229c-63f5-44cf-afe7-993324eaac7e"/>
합집합을 말한다. 양쪽 테이블의 모든 데이터가 검색된다.
```sql
SELECT *
FROM employee FULL OUTER JOIN department
ON employee.DepartmentID = department.DepartmentID;
```
<br>

### 3. SELF JOIN
- 자기자신을 조인하는 것이다. 자신이 갖고 있는 칼럼을 다양하게 변형시켜 활용할 때 사용한다.
