## OSI 7 layer의 탄생 배경

**네트워크란?**

-   컴퓨터나 기타 기기들이 리소스를 공유하거나 데이터를 주고 받기 위해 유선 혹은 무선으로 연결된 통신 체계

**네트워크의 역할**

1.  애플리케이션 목적에 맞는 통신 방법 제공
2.  신뢰할 수 있는 데이터 전송 방법 제공
3.  네트워크 간의 최적의 통신 경로 설정
4.  목적지로 데이터 전송
5.  노드 사이의 데이터 전송

**프로토콜이란?**

-   데이터를 송수신 하기 위한 규칙

**위 모든 역할을 하나의 네트워크 프로토콜에서 하게 된다면?**

-   유지 보수 및 기능 추가의 어려움

🚩 **모듈화를 통해 계층별로 구분하자 → OSI 7 layer**

## OSI 7 layer

![image.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGafUH%2FbtsJMa5tbin%2FHXMf5bfJB4TBD1rfNusOIk%2Fimg.png)
1.  **물리 계층**: 비트 단위의 데이터 전송을 담당
    -   bits 단위로 데이터 전송
      
2.  **데이터링크 계층**: 인접 노드 간 데이터 전송, 오류 검출 및 수정 <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcRLo1B%2FbtsJMaqYSkn%2FCFkM6mAPxGP6GzMJbt7n80%2Fimg.png" alt="데이터 링크 계층 이미지" align="right" width="142" height="130">
      
    

-   직접 연결된 노드 간의 통신 담당
-   MAC 주소 기반 통신 (ARP)
    -   ARP(Address Resolution Protocol): IP 주소를 MAC 주소와 매칭시키기 위한  
        프로토콜

3.  **네트워크 계층**: 패킷 라우팅, IP 주소 지정 <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbMQwNy%2FbtsJMhJ8pmU%2FgzuwR9Exrets9nllMS3XD1%2Fimg.png" alt="데이터 링크 계층 이미지" align="right" width="142" height="130">

-   호스트 간의 통신 담당(IP)
-   목적지 호스트로 데이터 전송
-   네트워크 간의 최적의 경로 설정

4.  **전송 계층**: 애플리케이션 간이 통신 담당  
    → 목적지 애플리케이션으로 데이터를 전송함
    -   TCP, UDP
5.  **세션 계층**: 애플리케이션 간의 통신에서 TCP/IP 세션을 관리
    -   RPC (Remote Procedure Call)
6.  **표현 계층**: 애플리케이션 간의 통신에서 메시지 포맷 관리
    -   인코딩 ↔ 디코딩
    -   암호화 ↔ 복호화
    -   압축 ↔ 압축 풀기
7.  **응용 계층**: 애플리케이션 목적에 맞는 통신 방법 제공
    -   HTTP, DNS, SMTP, FTP, …

각 상위 레이어는 하위 레이어의 기능을 활용하여 통신 작업을 처리

ex) Transport layer는 통신 방법(TCP, UDP)만 결정하고 Network layer가 정한 경로를 사용하여 전달하기만 하는 것

### OSI 7 layer 모델의 통신 과정

![image.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcFV5Ez%2FbtsJLV8HGKn%2FosLdkVSKgNkop9mmiUTG9k%2Fimg.png)

애플리케이션 A에서 애플리케이션 B로 데이터를 전송한다고 가정

1.  응용 계층에서 시작하여 각 계층별 캡슐화를 거쳐 물리 계층까지 도달
2.  물리 계층에서 Communication Network의 물리 계층으로 데이터를 전송
3.  Communication Network는 역캡슐화를 진행하며 네트워크 계층까지 검증을 진행
4.  Communication Network의 네트워크 계층에서 목적지를 확인한 후 다시 캡슐화를 통해 물리 계층까지 도달
5.  Communication Network의 물리 계층에서 애플리케이션 B의 물리 계층으로 데이터를 전달
6.  애플리케이션 B의 물리 계층부터 응용 계층까지 역캡슐화를 진행하며 데이터를 전달받음

### 캡슐화/역캡슐화 과정

![image.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPzpVQ%2FbtsJLGqoH7T%2Fzk45ZoK9NHK4ikIgzb0XeK%2Fimg.png)

![image.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fphoou%2FbtsJM71foX6%2Fpn190i5Q41WqGRGafNSZ10%2Fimg.png)

**전송 계층**

응용 계층에서 받은 메시지를 TCP의 헤더를 붙여 캡슐화, 해당 데이터는 세그먼트로 네트워크 계층에 전달

**네트워크 계층**

전송 계층에서 받은 세그먼트에 IP 헤더를 붙여 캡슐화, 해당 데이터는 패킷으로 데이터링크 계층으로 전달

**데이터링크 계층**

네트워크 계층에서 받은 패킷에 Ethernet 헤더를 붙여 캡슐화, 해당 데이터는 프레임으로 물리 계층에 전달(헤더와 동시에 트레일러 또한 페이로드에 붙이게 되며, 트레일러는 데이터 전송 후에 에러가 없었는지 확인하는 용도로 사용됨)

![image.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FOlJUu%2FbtsJLkuvdyW%2F8QRedaSARBVZmapkkVdWV0%2Fimg.png)

**데이터링크 계층**

물리 계층에서 받은 프레임의 헤더와 트레일러를 구분해 나오는 페이로드를 네트워크 계층으로 전달

**네트워크 계층**

데이터링크 계층에서 받은 패킷의 헤더를 구분하여 나오는 페이로드를 전송 계층에 전달

**전송 계층**

네트워크 계층에서 받은 세그먼트의 헤더를 구분하여 나오는 페이로드를 응용 계층에 전달

---

참고자료

[https://www.youtube.com/watch?v=6l7xP7AnB64](https://www.youtube.com/watch?v=6l7xP7AnB64)

[https://hanamon.kr/네트워크-기본-프로토콜이란-osi-7-계층-별-프로토콜/](https://hanamon.kr/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EA%B8%B0%EB%B3%B8-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C%EC%9D%B4%EB%9E%80-osi-7-%EA%B3%84%EC%B8%B5-%EB%B3%84-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C/)

[https://seosh817.tistory.com/31](https://seosh817.tistory.com/31)

[https://aws-hyoh.tistory.com/70](https://aws-hyoh.tistory.com/70)

[https://co-no.tistory.com/entry/통신-RPCRemote-Procedure-Call의-개념-및-특징](https://co-no.tistory.com/entry/%ED%86%B5%EC%8B%A0-RPCRemote-Procedure-Call%EC%9D%98-%EA%B0%9C%EB%85%90-%EB%B0%8F-%ED%8A%B9%EC%A7%95)

[https://blog.naver.com/tmk0429/222294384125](https://blog.naver.com/tmk0429/222294384125)
