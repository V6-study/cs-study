
## 스택(Stack)

- 특징
    - LIFO (Last In First Out, 후입선출)
- ADT
    - push()
    - pop()
    - peek()
    - isEmpty()
    - size()
- 구현
    - 단순리스트를 이용
        - 후단에서 push,pop
        
        ![image](https://github.com/user-attachments/assets/7486d876-2b03-4887-b82e-58d97aa74d47)

        
    - 링크드리스트를 이용
        - 전단에서 push,pop
        - push
            
            ![image](https://github.com/user-attachments/assets/4c2e0ca4-40db-4374-b4df-29c8624b7670)

            
        - pop
            
            ![image](https://github.com/user-attachments/assets/92731b22-96b7-40f8-b71a-fc7a04c58242)

            
- 시간 복잡도
    - O(1)
- 용도
    - 되돌리기(Ctrl+Z)
    - 함수 호출
    - 괄호검사
    - 계산기(후위 표기식 계산, 중위 표기식 → 후위표기식)
    - 미로 탐색

## 큐(Queue)

- 특징
    - FIFO(First-In-First-Out, 선입선출)
    - 삽입은 후단, 삭제는 전단에서 이루어짐
- ADT
    - enqueue(x)
    - dequeue()
    - peek()
    - size()
    - isEmpty()
- 구현
    - 선형 큐
        - 비효율적
            
            ![image](https://github.com/user-attachments/assets/d7429f29-874c-4ef3-b209-90e02de4dc21)


            
    - 원형 큐
        - 배열을 원형으로 사용
            
            ![image](https://github.com/user-attachments/assets/340d2b6d-77cb-46b5-a31f-37fe70119aae)

            
        - 전단과 후단을 위한 변수 사용
            - front
                - 첫번째 요소 하나 앞의 인덱스
                    - 한 칸을 비워두는 이유 : 공백상태와 포화상태를 구분하기 위해서
                - 최근 삭제 위치
                - front+1 % MAX_QSIZE
            - rear
                - 최근 삽입 위치
                - rear+1 % MAX_QSIZE
            - 공백상태
                - front == rear
            - 포화상태
                - front == (rear+1) % MAX_QSIZE
        
        ![image](https://github.com/user-attachments/assets/cc7de95f-6534-4f26-b43a-1831177d3a9f)

        - 원형 링크드 리스트를 이용한 구현
            
            ![image](https://github.com/user-attachments/assets/75d1cbba-e617-4ad9-87e4-08873b604e78)

            
            - tail 사용
                - front = tail의 다음 노드
                - rear = tail
                - tail
            - enqueue
                
                ![image](https://github.com/user-attachments/assets/683c7e32-4e37-4eab-8deb-e1a8c145c6a4)

                
                ![image](https://github.com/user-attachments/assets/b2f5aff9-cf4b-4750-945a-03180530da53)

                
            - dequeue
                
                ![image](https://github.com/user-attachments/assets/8e6c87d8-e235-4cb1-abd8-544d94847506)

                ![image](https://github.com/user-attachments/assets/eeeb9767-a6b0-4771-87f7-8a84e0f2490c)
