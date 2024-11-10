## 01 B-Tree

- B-Tree는 탐색 성능을 높이기 위해 균형 있게 높이를 유지하는 Balanced Tree의 일종
- 자식 node의 개수가 2개 이상
- node 내의 key가 1개 이상일 수 있음 

### 1.1 B-Tree 조건

![주황색 부분은 '자식 node를 가리키는 포인터'이고, 살구색 부분은 '각 node의 key'이다](https://github.com/user-attachments/assets/ebc85ce1-776d-4eca-9c39-826f5cac7a2c)

주황색 부분은 '자식 node를 가리키는 포인터'이고, 살구색 부분은 '각 node의 key'이다

1. node의 key의 수가 k개라면, 자식 node의 수는 k+1개이다.
2. node의 key는 반드시 정렬된 상태여야 한다.
3. 자식 node들의 key는 현재 node의 key를 기준으로 크기 순으로 나뉘게 된다.
4. root node는 항상 2개 이상의 자식 node를 갖는다. (root node가 leaf node인 경우 제외)
5. M차 트리일 때, root node와 leaf node를 제외한 모든 node는 최소 M/2, 최대 M개의 서브 트리를 갖는다.
6. 모든 leaf node들은 같은 level에 있어야 한다.

### 1.2 검색

- 편향된 트리의 경우 leaf node까지 탐색한다면, 모든  node를 방문하여 O(n) 시간이 걸림
- 편향되지 않고 밸런스 잘 유지하면 최악의 경우에도 O(logN)의 시간이 보장됨

## 02 B+Tree

### 2.1 B-Tree와의 차이점

![image.png](https://github.com/user-attachments/assets/ebc85ce1-776d-4eca-9c39-826f5cac7a2c)
![image](https://github.com/user-attachments/assets/25f246e6-53fe-43eb-a347-36623b88f238)

- B-Tree는 모든 데이터를 순회하려면 트리의 모든 노드를 방문해야 함
    - 이 단점을 개선한 자료구조
- leaf node에만 데이터를 저장
- leaf node가 아닌 node에서는 자식 포인터만 저장
    - B-Tree의 경우 최상의 경우 특정 key를 root node에서 찾을수 있음
    - but, B+Tree의 경우 반드시 특정 key에 접근하기 위해서 leaf node까지 가야
- leaf node끼리는 Linked list로 연결
    - Full scan을 하는 경우 O(1) (선형 시간)이 소모
    - 인덱스 컬럼의 부등호를 이용한 순차 검색 연산에 강함
        - 인덱스 컬럼에서 `WHERE age > 7` 과 같은 조건이 주어지면, B+Tree는 해당 조건을 만족하는 첫 리프 노드까지 탐색한 후, 링크드 리스트를 따라가며 이후의 연속된 데이터를 순차적으로 빠르게 접근
- key 중복 존재

### 2.2 검색

- B-Tree와 동

### 2.3 활용

- 인덱스(Index)
    - DB 테이블에 대한 검색 속도를 향상시켜주는 자료구조

## 03 출처

```bash
검색, 삽입, 삭제의 과정이 궁금하다면…
```

- [🔗](https://rebro.kr/169) B-Tree
- [🔗](https://rebro.kr/167?category=484170) B+Tree
