저장 프로시저는 **SQL 문장들의 집합**으로,  
데이터베이스에 저장되어 필요할 때 호출하여 실행할 수 있는 **프로그램 단위**이다

프로시저의 특징

1.  재사용성
    -   자주 사용되는 쿼리를 모듈화하여 필요할 때마다 호출 가능
2.  성능 향상
    -   최초 실행 시 컴파일되어 캐시에 저장, 이후 실행 시 빠른 속도로 처리
3.  네트워크 트래픽 감소
    -   하나의 요청으로 여러 sql문을 실행할 수 있음
4.  보안 강화
    -   테이블 직접 접근 대신 프로시저 접근 권한 부여로 보안 강화

단점

1.  초기 컴파일 후 변경된 데이터 환경에 대한 최적화 어려움
    -   실행 과정
        1.  최초 실행: 구문 분석, 최적화, 컴파일 과정
        2.  이후 실행: 캐시된 실행 계획을 사용하여 처리
    -   데이터 분포, 데이터의 양, 인덱스 변경, 통계 정보, 하드웨어 리소스  
        등의 변화가 일어나도 **초기에 캐시된 실행 계획을 계속 사용하기 때문**에 발생하는 문제점
2.  복잡한 로직의 경우 디버깅의 어려움

#### 프로시저의 기본 구조

```
-- mysql

DELIMITER //

CREATE PROCEDURE procedure_name(
    -- 매개변수 영역
    IN  input_parameter  INT,     -- 입력 매개변수
    OUT output_parameter VARCHAR(50),  -- 출력 매개변수
    INOUT inout_parameter DECIMAL(10,2)   -- 입출력 매개변수
)
BEGIN
    -- 선언부: 변수 선언
    DECLARE variable1 INT DEFAULT 0;
    DECLARE variable2 VARCHAR(50);
    
    -- 실행부: 비즈니스 로직 구현
    SET variable1 = input_parameter + 100;
    SET variable2 = 'Hello';
    
    -- 결과 반환
    SET output_parameter = variable2;
    SET inout_parameter = variable1;
    
END //

DELIMITER ;

-- -----------------------------------------------------------------------------------

SET @input = 100;           -- IN 파라미터용
SET @output = '';          -- OUT 파라미터용
SET @inout = 50;           -- INOUT 파라미터용

-- 2. 실행 전 값 확인
SELECT @input AS input_before, @output AS output_before, @inout AS inout_before;

-- 3. 프로시저 실행 후 2번 확인
CALL procedure_name(@input, @output, @inout);
```

**자주 사용되는 복잡한 쿼리나 트랜잭션 로직을 캡슐화**한다면

저장 프로시저를 통해 데이터베이스 작업의 **일관성과 효율성을 크게 향상**시킬 수 있다
