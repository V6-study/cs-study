## TCP

인터넷 프로토콜 스위트의 핵심 프로토콜 중 하나로, 어플리케이션 간 reliable, ordered and error-checked 데이터의 전송을 보장한다.

- 연결 지향적: 통신 전에 연결을 설정하고 유지한다.
- 신뢰성: 데이터 전송의 신뢰성을 보장한다. 손실된 패킷은 재전송한다.
- 순서 보장: 데이터가 보낸 순서대로 수신된다.
- 양방향 통신: 양방향으로 동시에 데이터를 주고받을 수 있다.
- 바이트 스트림: 데이터를 연속된 바이트 스트림으로 처리한다.

### TCP 작동 방식 간단히

- sender, receiver 간 3-way handshake를 통해 연결을 설정한다.
- 데이터를 패킷 단위로 쪼갠다.
- 패킷에 순서를 부여한다.
- 오류 없이 패킷이 전달되도록 한다. 손실된 패킷은 재전송한다.
- 흐름제어를 통해 데이터 전송 속도를 조절한다.

### 흐름제어

발신자가 수신자의 처리 능력에 맞춰 데이터 전송 속도를 조절하는 것이다. 발신자와 수신자 두 노드 간 개념이라고 생각하면 된다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9887a34e-d6f9-43f1-a7b8-f3f3bef7edf3/5ea36ecd-f2f8-417e-bcfe-43286e81cfdb/image.png)

1. send buffer, receive buffer
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9887a34e-d6f9-43f1-a7b8-f3f3bef7edf3/0b5c70ec-f854-4df1-835b-ffa45122c5aa/image.png)
    
    - TCP는 보낼 데이터는 send buffer에, 받은 데이터는 receive buffer에 저장한다.
    - Application이 준비됐을때 receive buffer로부터 데이터를 읽는다.
    - 따라서 흐름제어는 receive buffer가 받지 못할만큼의 데이터를 전송하지 않도록 데이터 흐름을 관리하는 것이다.
2. Receive Window
    
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9887a34e-d6f9-43f1-a7b8-f3f3bef7edf3/b31dd3e9-9aba-4e3e-992b-991cea399817/image.png)
    
    - 이때, 수신자가 receive buffer에 남은 자리를 Receive Window라는 개념으로 관리한다.
    - 그래서 TCP는 패킷을 전송 받을 때마다, 이 정보를 ack 메세지에 담아 발신자에게 보낸다.
    - Stop and Wait: 이를 응용한 것으로, 매번 전송한 패킷에 대한 확인 응답을 받아야 그 다음 패킷을 전송할 수 있는 방식이다.
        
        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9887a34e-d6f9-43f1-a7b8-f3f3bef7edf3/73935332-a70f-4243-a17f-0946a5a573ad/image.png)
        
        - 오버플로우가 일어날 가능성은 없지만, 너무 느리다는 단점이 있다. 당연히 요즘에는 잘 사용하지 않는다.
3. Sliding Window
    - 하나를 보내고 기다리는게 아니라, 여러개를 동시에 보낼 수 있는 TCP 프로토콜이다. 이때 동시에 보낼 수 있는 사이즈가 정해져 있고, 이를 윈도우라고 한다.
        
        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9887a34e-d6f9-43f1-a7b8-f3f3bef7edf3/216ee68e-15c8-46fa-a6a3-cc0631997f47/image.png)
        
        - e.g. 위 그림은 윈도우 사이즈가 7이며, 7개 이하 데이터를 한번에 보낼 수 있는 경우이다.
        - 0, 1을 보내면 윈도우 사이즈가 줄어든 것을 알 수 있다.
        - 0과 1을 잘 받았다고 ACK2를 보내면 다시 윈도우가 2칸 늘어난다.
        - 만약 윈도우 사이즈가 0이 되면, TCP는 데이터 전송을 멈추고 persist timer를 시작한다. 타이머가 시작되고 WindowProbe라는 작은 패킷을 계속 전송한다. receiver가 윈도우 사이즈에 여유가 생겼음을 알릴 수 있도록.
        - $LastByteSent - LastByteAcked ≤ ReceiveWindowAdvertised$
            
            ![1_KvfIrP_Iwq40uVdRZYGnQg.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9887a34e-d6f9-43f1-a7b8-f3f3bef7edf3/17b78609-7240-4736-aea7-d5f4ff0a82b9/1_KvfIrP_Iwq40uVdRZYGnQg.png)
            
    - ACK 메세지도 데이터와 마찬가지로 한꺼번에 처리된다. (e.g. 데이터 3, 4, 5마다 ACK 4, 5, 6을 보내는 것이 아니라 6만 보낸다)
    - 데이터와 ACK는 모두 양방향으로 전송되어야 한다.
        - Two separate circuits: 둘이 전송되는 통로가 따로 존재하는 방식
        - Piggybacking: 같이 보내는 방식
    - 여전히 ACK를 받을 때까지 기다려야 한다는 점에서 비효율적이다. 그렇다고 윈도우를 무분별하게 늘리면 다시 흐름제어에 실패한다. 하지만 제일 범용적으로 사용되는 흐름제어 방식이다.
4. Rate Control / Policing
    - ACK 없이 트래픽이 유지되는 흐름제어 방식. 하지만 잘 사용되진 않는다.

### 혼잡제어

네트워크 전체의 혼잡을 조절하는 것이다. 네트워크 내 패킷 수가 과도하게 증가하는 현상을 혼잡이라고 하며, 혼잡제어는 어느 한 노드가 네트워크를 overload하지 않도록 제어하는 것이다.

1. 혼잡 윈도우(Congestion Window)
    
    ![img1.daumcdn.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9887a34e-d6f9-43f1-a7b8-f3f3bef7edf3/7daf3008-76f7-4e0c-a648-af84cbbbeb97/img1.daumcdn.png)
    
    - 발신자는 자신의 윈도우 크기를 정할때, 수신자가 보내준 rwnd 와 자신이 네트워크 상황을 고려해 정한 cwnd 중 더 작은 값을 택한다.
    - 마찬가지로 Sliding Window 방식을 사용한다.
2. 느린 시작 Slow Start
    
    ![img1.daumcdn.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9887a34e-d6f9-43f1-a7b8-f3f3bef7edf3/f99d8901-69c2-4025-9f87-ea030e5e4f13/img1.daumcdn.png)
    
    - $cwnd = 1$
    - For each ACK, $cwnd ← cwnd + 1 [packets]$
    - 발신자는 RTT 동안 cwnd 만큼의 패킷을 전송할 수 있으므로, 전송한 모든 패킷의 ACK를 받으면 cwnd는 RTT 마다 2배씩 증가한다.
    - Slow start는 2배씩 전송을 늘리다가 패킷 드랍으로 인해 RTO가 발생하면 종료된다.
        - 이때 slow start를 종료하는 임계값인 ssthresh가 설정되고, cwnd 값은 다시 1로 리셋된다.
        - For each RTO
            - $ssthresh ← cwnd/2 [packets]$
            - $cwnd ← 1 [packets]$
    - 이후 slow start를 다시 수행하는데, cwnd 값이 ssthresh 값보다 커지면 네트워크 용량에 도달했다고 판단하고 slow start를 종료, congestion avoidance 단계로 넘어간다.
3. Congestion Avoidance (aka AIMD)
    
    ![img1.daumcdn.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9887a34e-d6f9-43f1-a7b8-f3f3bef7edf3/fe68933a-5a4f-4567-ad38-68df85b1ccca/img1.daumcdn.png)
    
    - ACK을 수신하면 cwnd를 1/cwnd 씩 늘린다. 따라서 전송된 모든 패킷의 ACK를 수신하면, cwnd는 RTT마다 1씩 선형적으로 증가한다.
    - 패킷 드랍이 일어나는 경우
        1. RTO
            - RTO가 일어난 경우 slow start로 돌아가며 ssthresh를 cwnd/2로, cwnd를 1로 리셋한다.
        2. 3 duplicate ACK
            - 동일한 sequence number의 ACK을 3번이나 연속적으로 받으면 timeout이 일어나지 않아도 패킷 드랍이 일어났다고 판단하는 것을 의미한다.
            - 수신자 측에서 순서가 맞지 않는 패킷이 도착하면 이전 패킷(먼저 도착해야 할 패킷)의 ACK을 재전송하기 때문이다.
            - 이 경우 발신자는 fast restransmit (재전송)을 수행한다.
            - For each 3 dup-ACK, $cwnd ← cwnd/2$
            - RTO에 비해 심각한 상황이 아니므로 cwnd를 절반으로 줄여 전송량을 유지하고자 한다. 이를 fast recovery 상태에 돌입했다고 한다.

---

참고자료

- https://www.brianstorti.com/tcp-flow-control/
- [https://ai-com.tistory.com/entry/네트워크-TCP-Congestion-Control-1-기본-원리](https://ai-com.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCP-Congestion-Control-1-%EA%B8%B0%EB%B3%B8-%EC%9B%90%EB%A6%AC)
- https://m.blog.naver.com/if_you-/220881729649
- https://www.youtube.com/watch?v=r9kbjAN2788&t=7s
