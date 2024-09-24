# [TCP] 3 way handshake & 4 way handshake


### 용어 정리

> **SYN 패킷**
> 
> 
> TCP 연결 설정 과정에서 사용되는 특별한 패킷입니다. 클라이언트가 서버와의 연결을 시작하기 위해 보내는 첫 번째 메시지로, 연결 요청을 나타냅니다.
> 

> **ACK(Acknowledgment)**
> 
> 
> TCP 프로토콜에서 데이터 수신을 확인하는 신호입니다. 3-way handshake 과정에서 서버가 클라이언트의 연결 요청을 받았음을 알리는 데 사용됩니다.
> 

> **FIN 플래그**
> 
> 
> TCP 연결 종료 과정에서 사용되는 특별한 신호입니다. 이는 한 쪽이 연결을 종료하고자 할 때 상대방에게 보내는 메시지로, 더 이상 데이터를 전송하지 않겠다는 의사를 나타냅니다.
> 

<br>

### **3 way handshake - 연결 성립**

> TCP는 정확한 전송을 보장해야 한다. 따라서 통신하기에 앞서, 논리적인 접속을 성립하기 위해 3 way handshake 과정을 진행한다.

![Untitled_-_3-way_handshake](https://github.com/user-attachments/assets/dcb8cfb6-bade-41d1-ae1f-b06374cf016e)

1. 클라이언트가 서버에게 SYN 패킷을 보냄 (sequence : x)
>
2. 서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄 (sequence : y, ACK : x + 1)
>
3. 클라이언트는 서버의 응답은 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄
<br>
이렇게 3번의 통신이 완료되면 연결이 성립된다. 이 후 과정은 클라이언트의 요청으로 시작된다. seq, ACK 값을 주고 받으며 +1 되는 이유는 보안과 관련이 있다. 이 값을 계산해서 통신을 가로채는것을 세션 하이재킹이라고 한다.

<br>

### **4 way handshake - 연결 해제**

> 연결 성립 후, 모든 통신이 끝났다면 해제해야 한다.

![Untitled_-_4-way_handshake_(1)](https://github.com/user-attachments/assets/5dff8da7-dfac-498f-8f46-db7a4f2a6c6e)

1. 클라이언트는 서버에게 연결을 종료한다는 FIN 플래그를 보낸다.

2. 서버는 FIN을 받고, 확인했다는 ACK를 클라이언트에게 보낸다. 
    - 이때 모든 데이터를 보내기 위해 CLOSE_WAIT 상태가 된다)

3. 데이터를 모두 보냈다면, 연결이 종료되었다는 FIN 플래그를 클라이언트에게 보낸다.

4. 클라이언트는 FIN을 받고, 확인했다는 ACK를 서버에게 보낸다. 

    - Q. Server에서 FIN을 전송 전에 보낸 패킷이 FIN패킷보다 늦게 도착한다면?

        - 이 패킷은 Drop, 데이터는 유실

        - 해결 방법 : TIME_WAIT

            - Client는 Server로부터 FIN을 수신하더라도 일정시간(디폴트 240초) 동안 세션을 남겨놓고 잉여 패킷을 기다리는 과정

    - 서버는 ACK를 받은 이후 소켓을 닫는다

    - TIME_WAIT 시간이 끝나면 클라이언트도 닫는다

<br>
이렇게 4번의 통신이 완료되면 연결이 해제된다.
<br><br><br>
[참고자료]

[🔗](https://www.youtube.com/watch?v=Ah4-MWISel8) [따라學IT] 09. 연결지향형 TCP 프로토콜 - TCP 3Way Handshake

[🔗](https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake) [ 네트워크 쉽게 이해하기 22편 ] TCP 3 Way-Handshake & 4 Way-Handshake

[🔗](https://www.youtube.com/watch?v=K9L9YZhEjC0) 이해하면 인생이 바뀌는 TCP 송/수신 원리
