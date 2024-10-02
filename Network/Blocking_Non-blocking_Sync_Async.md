Block과 Non-block에 대해서는 해석이 다양하지 않아 명확하다. 하지만 Sync, Async는 그 의미가 사용되는 맥락에 따라 조금씩 다른 것 같아 이해되는 대로 정리했다.

> **Blocking** / **Non-blocking**
> 
> 
> 기다리는지. 다른 작업의 실행이 현재 작업의 실행을 막는지. 제어권을 넘겨주는지
> 
- **Blocking**: 기다림
    - 다른 작업이 현재 작업을 막는다.
    - 호출된 함수가 작업을 완료하기 전까지 호출한 함수에게 제어권을 넘겨주지 않고 기다리게 만든다.
- **Non-blocking**: 기다리지 않음
    - 다른 작업이 현재 작업을 막지 않는다.
    - 호출된 함수가 (작업이 완료되지 않았아도) 바로 리턴한다. 제어권을 넘겨줬으므로 호출한 함수는 기다리며 다른 일을 할 수 있다.


> **Synchronous** / **Asynchronous** (동기/비동기)
> 
- **프로그래밍** 관점
    - **Synchronous programming**: 여러 작업을 순차적으로 실행함.
        
        ![img](https://github.com/user-attachments/assets/35499040-6aa8-41fa-b768-e1ce1129a379)
        
    - **Asynchronous programming**: 여러 작업을 동시에 실행함. 독립적으로 실행함.
        - 참고로 **asynchronous programming**을 가능하게 하는 2가지 방법으로 multithreading과 non-block IO가 있다.
        - **multithread**
            
            ![img](https://github.com/user-attachments/assets/809d7cf2-c6f3-4853-9192-33ce63c45870)
            
            따지고 보면 위 그림에선 multithread, non-block I/O 모두 적용된 것이다.
            
        - **non-block I/O**
            
            싱글 스레드가 작업을 처리하는데, I/O 작업을 non-blocking 방식으로 수행하는 것
            
            ![img](https://github.com/user-attachments/assets/acc9b5c6-e76d-4464-9c34-8b38020e3de6)
            
            CPU 작업과 I/O 작업은 동시에 할 수 있기 때문에, I/O 작업을 non-block으로 하면 적은 쓰레드 개수로도 여러 일을 동시에 할 수 있는 것이다.

            
- **I/O** 관점 (2가지)
    1. **sync I/O = block I/O. async I/O = non-block I/O**
    2. I/O 작업 완료를 내가 신경 쓰는지
    - **sync I/O:** 내가(요청자가) I/O 완료를 직접 챙겨야 함.
        - 호출한 함수가 호출된 함수의 작업 완료 여부를 계속 확인한다.
            
            호출된 함수의 리턴을 기다리거나, 리턴을 받더라도 작업 완료 여부를 스스로 확인한다.
            
    - **async I/O:** 요청자가 완료를 직접 챙지 않아도 됨.
        - 다른 누군가가 완료를 알려주거나 함수 **callback**으로 처리한다.
        - 호출한 함수는 호출된 함수에게 callback을 전달한다.
            
            호출된 함수가 작업이 완료되면 전달 받은 callback을 실행하고, 호출한 함수는 작업 완료 여부를 신경쓰지 않고 다른 일을 할 수 있다.
            
   
- **백엔드 아키텍처** 관점
    - **Synchronous communication**
        
        ![img](https://github.com/user-attachments/assets/2a5fbdda-ba95-4039-9a8d-81a2dc91b377)
        
        - A, B, C는 모두 micro-service라고 가정한다.
        - C에서 응답 불가능한 상태가 발생하면 서비스 A, B까지 모두 응답 불능이 될 수 있다.
    - **Asynchronous communication**
        
        ![img](https://github.com/user-attachments/assets/706c9c56-fab7-4ef9-b5b4-6cd163adbe1b)
        
        - 서비스 A에서 서비스 B가 관심 있을 법한 이벤트가 발생하면, Message Q에 넣음. 그리고 A는 자기 할일을 계속 함. B는 Message Q에 새로운 메세지가 없나 항상 기다림. 새로운 메세지 있으면 consume해서 작업 진행함.
        - 물론 A가 B에서 제공하는 데이터를 필요로 하는 경우처럼 API를 통해서 작업을 처리해야 경우도 존재한다. 모든 상황에 쓰이진 않는다.
        - C에서 문제가 생겨도 다른 서비스에 미치는 영향을 줄일 수 있음.
- **보편적인 설명**
    - **Synchronous**: 작업들이 같이 시작해, 같이 끝난다. 작업 중 다른 작업이 끼어들지 못한다.
        - 동시 실행, 종료를 보장하고 다른 작업이 끼어들지 못하기 위해 사용한다.
        
        ![img](https://github.com/user-attachments/assets/01808acd-f3f7-4c5f-85a7-f9e6a94cbc72)
        
    - **Asynchronous**: 작업들의 시작과 끝이 다르다. 작업 중에 끼어들어도 괜찮다.
        - 시작, 종료 시기를 신경쓰지 않기 위해 사용한다.
        
        ![img](https://github.com/user-attachments/assets/c95a413d-c911-4000-ad49-f19175006c5a)
        

> Block, Non-block과 Sync, Async를 함께 적용하면?
> 
1. Blocking Sync
    - 프로그램은 작업을 순차적으로 실행한다.
    - I/O 작업이 시작되면, 스레드는 완료될 떄까지 기다린다.
    - 이해와 구현이 쉽지만, I/O 작업량이 많은 경우 비효율적이다.
    - e.g. 내가 복합기의 스캔 시작 버튼을 누른다. 그 사이 다른 일은 할 수 없다(Block). 완료되면 결과물을 갖고 내 자리로 돌아간다.

2. NonBlocking Sync
    - I/O 작업을 시작하고도 스레드는 계속 작업을 실행한다.
    - I/O 작업 완료 여부를 주기적으로 확인한다.
    - Sync-Blocking보다 효율적이지만, busy-waiting이 걸릴 수 있다.
    - e.g. 내가 복합기의 스캔 시작 버튼을 누르고, 그 사이 다른 일을 한다(Non-block). 물론 틈틈이 스캔이 완료됐는지 확인해줘야 한다(Sync). 하지만 다른 일을 할 수 있다는 점에서 다르다.
    
3. Blocking Async
    - 프로그램은 여러 개의 I/O 작업을 동시에 진행할 수 있다.
    - I/O 작업 시작마다 해당 스레드는 블락된다.
    - OS에서 callback 과 같은 방법을 통해 작업 완료를 notice 해준다.
    - e.g. 심부름꾼이 등장한다. 나는 심부름꾼에게 스캔을 시킨다. 심부름꾼이 복합기로 가서 스캔을 시작한다. 스캔이 완료됐는지 직접 확인하진 않지만(Async), 스캔이 완료될 때까지 기다리긴 마찬가지다(Blocking). 완료되면 심부름꾼이 알려준다.
    
4. Non-blocking Async
    - 프로그램은 여러 개의 I/O 작업을 동시에 진행할 수 있고, OS에서 작업 완료를 notice 해준다.
    - 프로그램은 I/O 작업을 기다리지 않고 실행한다.
    - e.g. 나는 내 자리에서 할일을 하고, 심부름꾼에게 스캔을 시킨다. 심부름꾼은 복합기에 가서 시작 버튼을 누른다. 심부름꾼은 스캔을 기다리면서 다른 일을 할 수 있다(non-blocking). 틈틈이 스캔이 완료됐는지 확인하면서. 완료되면 내 자리로 돌아와 알려준다(callback).

> Non-blocking Async가 성능이 가장 좋을까? (처리량/시간)
> 
- Blocking Sync와 멀티쓰레딩으로도 성능을 올릴 수 있다.
    - 하지만 쓰레드 관리 비용이 존재함.
        - 한 작업이 오래 걸린다?
            - 쓰레드 할당 오버헤드가 적다 → 불리하지 않다.
        - 짧게 끝나는 작업이 여러 번이다?
            - 쓰레드 할당 오버헤드가 많다 → Non-blocking이 유리하다.
- 개발자의 인지 비용 등도 고려해야 한다.
    - Blocking Sync는 직관적이다.
    - Non-blocking은 실행 순서, 순차 처리를 신경 써야 하는 비용이 있다.


---
참고자료
- https://musma.github.io/2019/04/17/blocking-and-synchronous.html
- https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/
- https://haneepark.github.io/2021/07/18/blocking-nonblocking-sync-async/
- https://www.youtube.com/watch?v=EJNBLD3X2yg
