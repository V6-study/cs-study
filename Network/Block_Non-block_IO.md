## IO

input, output. 데이터의 입출력

### IO의 종류

- network(socket)
- file
- pipe (프로세스 간 통신할때 쓰이는 개념)
- device (키보드 등)

## Socket

네트워크 통신은 소켓을 통해 데이터 입출력 된다.

백엔드 서버는 네트워크 상 클라이언트들과 각각 소켓을 열고 통신한다.

## Block, Non-block I/O

### Block I/O

I/O 작업을 요청한 프로세스 또는 스레드는 요청이 완료될 때까지 블락된다.

- OS 레벨에서 block I/O
    
    ![img](https://github.com/user-attachments/assets/789e631e-6548-40c8-98fc-cfdafa66d56b)
    
    1. read system call을 호출하면 호출한 스레드는 블락된다. 
    2. kernel 모드로 컨텍스트 스위칭이 된다.
    3. 커널에서는 read I/O를 실행한다.
    4. 디바이스에서 데이터 응답을 받으면 데이터를 kernel space에서 user space로 옮긴다.
    5. 스레드는 이제서야 준비된 데이터를 읽고 실행한다.
- Socket에서 block I/O
    
    ![img](https://github.com/user-attachments/assets/b1a406a0-29f1-4a80-937e-48b5c0df8aed)
    
    1. 소켓 S에서 소켓 A로 데이터를 보내려고 한다.
    2. 소켓 A는 일단 기다린다.
        - 소켓 A는 read system call을 호출하고 기다린다.
        - 이때 receive buffer에 데이터가 들어올 때까지 read system call을 호출한 스레드는 블락된다.
    3. 소켓 S는 send buffer에 보낼 데이터를 write해야 한다.
        - send buffer가 가득 차면 write system call을 호출한 스레드는 블락된다.

### Non-block I/O

프로세스 또는 스레드를 블락시키지 않고 요청에 대한 현재 상태를 즉시 리턴한다. 따라서 스레드는 다른 작업을 수행할 수 있다.

- OS 레벨에서 Non-block I/O
    
    ![img](https://github.com/user-attachments/assets/10314b14-f6f6-49b6-a132-5c0fdbc2bbf7)
    
    1. read non-blocking system call이 발생하면 kernel mode로 컨택스트 스위칭이 일어난다.
    2. 커널 모드에서 read I/O를 실행하자마자 리턴을 한다. 
        - 아직 데이터가 없으므로 -1 과 에러코드를 리턴한다.
    3. 스레드는 read의 결과를 리턴 받지 않은 채로 다른 일을 수행할 수 있다.
    4. 그 사이 커널에서는 응답이 오면 데이터를 준비한다.
    5. 스레드는 다른 일을 수행하다가 read non-blocking system call을 다시 실행한다.
    6. 다시 컨텍스트 스위칭이 일어나고, 데이터가 존재하면 user space로 이동된다.
- Socket에서 non-block I/O
    
    ![img](https://github.com/user-attachments/assets/c686c6e0-0953-4d58-8f14-f2e0126b18bd)
    
    1. 소켓 A에서 receive buffer에 데이터가 있는지 확인한다.
        - 소켓 A의 스레드에서 read non-blocking system call을 실행한 것이다.
        - 데이터가 없다는 리턴을 받고, read system call 호출은 종료된다. 스레드는 블락되지 않는다.
    2. 소켓 B에서 write non-blocking system call을 실행한다.
        - 만약 send buffer가 가득 찬 경우, 적절한 에러 코드와 함께 wrtie system call은 바로 반환된다. 스레드는 블락되지 않는다.

## Non-block I/O 결과 처리 방식

Non-block I/O에서 I/O 작업 완료를 어떻게 확인하는지?

1. 완료됐는지 반복적으로 확인한다.
    
    단점
    
    - 완료된 시간과 완료를 확인한 시간 사이 갭으로 인해 처리 속도가 느려질 수 있다.
    - 완료됐는지 반복적으로 확인하는 것은 CPU 낭비가 발생한다.
        - 만약 block I/O로 동작하면?
            
            ![img](https://github.com/user-attachments/assets/c686c6e0-0953-4d58-8f14-f2e0126b18bd)
            
            1. 서버의 소켓에서 소켓 A에서 오는 데이터 요청을 read하는 system call을 실행한다.
            2. 서버의 스레드는 블락되어 소켓 A에서 데이터 응답이 있기 전까지 기다린다. 
            3. 나머지 클라이언트는 요청에 대한 응답을 받지 못한다.
    1. I/O multiplexing(다중 입출력) 사용한다.
    - 관심있는 I/O 작업들을 동시에 모니터링하고, 그 중 완료된 작업들을 한 번에 알려준다.
    - 네트워크 통신에 많이 사용된다. 이미 톰캣, nodejs 등에 적용된 것
        
        ![img](https://github.com/user-attachments/assets/e1cfd2a5-8e16-49d3-97d1-bcf79e0c7aa8)
        
    - 종류
        - select
        - poll
        - epoll (리눅스)
        - kqueue (macOS)
        - IOCP(I/O completion port) (윈도우)
    - Linux epoll 예시
        
        ![img](https://github.com/user-attachments/assets/c686c6e0-0953-4d58-8f14-f2e0126b18bd)
        
        1. epoll을 통해 6개의 소켓 중 하나라도 read 이벤트가 발생하면 알려달라고 이벤트를 등록한다.
        2. 소켓 A, B, C에서 요청이 발생한다.
        3. epoll을 호출한 스레드는 깨어남과 동시에 3개의 소켓에 데이터가 있음을 알려준다.
        4. 서버는 3개의 소켓에 대해 한번에 데이터를 읽고 처리를 한다.
            - 이때 스레드 풀에 스레드는 3개 존재한다.
    1. Callback 사용
    - 종류
        - POSIX AIO
        - LINUX AIO
