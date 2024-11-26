### Database Anomaly

> 이상: 데이터베이스에서 비정상적이거나 비일관적인 데이터 상태를 의미한다.


#### 종류
##### 1. 삽입 이상 (Insertion Anomaly)
- 불필요한 데이터까지 함께 추가해야 하거나 일부 데이터를 추가할 수 없는 상황.
- 예: 기본키가 {Student ID, Course ID} 인 경우
    - Course를 수강하지 않은 학생의 데이터를 추가할 수 없는 경우 이상 발생.
    - 강의 정보가 필수로 요구되는데 Course ID가 없다. 굳이 삽입하기 위해서는 '미수강'과 같은 Course ID를 만들어야 한다.
##### 2. 삭제 이상 (Deletion Anomaly)
- 의도하지 않은 데이터도 함께 삭제되는 문제.
- 예: 어떤 학생이 수강을 철회하는 경우, {Student ID, Course ID, Department, Course ID, Grade}의 정보 중 Student ID, Department 와 같은 학생에 대한 정보도 함께 삭제되는 문제.
##### 3. 갱신 이상 (Update Anomaly)
- 데이터의 일부만 변경하여 데이터가 불일치하는 모순 발생. (중복된 데이터가 여러 곳에 존재할 때)
- 예: 어떤 학생의 전공 (Department) 이 "컴퓨터에서 음악"으로 바뀌는 경우, 모든 Department를 "음악"으로 바꾸어야 한다. 그러나 일부를 바꾸지 못하는 경우가 발생하는 경우.
