# 목차
- [HTTP(Hypertext Transfer Protocol)](#http-hypertext-transfer-protocol-)
  * [HTTP/1.0](#http-10)
  * [HTTP/1.1](#http-11)
  * [HTTP/2.0](#http-20)
  * [HTTP/3.0](#http-30)
    + [QUIC](#quic)
- [HTTPS(Hypertext Transfer Protocol Secure)](#https-hypertext-transfer-protocol-secure-)
  * [인증서](#---)
  * [CA(Certificate Authority)](#ca-certificate-authority-)
  * [SSL (Secure Sockets Layer)](#ssl--secure-sockets-layer-)
  * [TLS (Transport Layer Security)](#tls--transport-layer-security-)
    + [TLS handshake (RSA)](#tls-handshake--rsa-)
    + [TLS handshake (Diffie-Hellman)](#tls-handshake--diffie-hellman-)
    + [**TLS 1.3 handshake**](#--tls-13-handshake--)
    + [0-RTT ((Zero Round Trip Time)](#0-rtt---zero-round-trip-time-)
<br/><br/><br/><br/>

# HTTP(Hypertext Transfer Protocol)

※ 기본 포트 : 80

인터넷 상에서 클라이언트와 서버가 자원을 주고 받을 때 쓰는 통신 규약.

다음과 같은 단점이 있다.

1.  **텍스트 기반 프로토콜**: HTTP는 텍스트 명령을 사용하여 요청과 응답을 주고 받음.  
    \- 기본 HTTP는 데이터가 평문으로 전송되므로, 중간에 패킷을 가로채기 쉬움
2.  **무상태 프로토콜**: 각 요청과 응답이 독립적이며, 서버는 이전 요청에 대한 정보를 유지하지 않음.  
    \- 웹 애플리케이션이 각 요청에 대해 별도로 인증 및 상태 정보를 전송해야 함을 의미  
    \- **헤더 오버헤드** : 쿠키, 사용자 에이전트, 각종 커스텀 헤더 등이 요청마다 반복적으로 전송 -> 대역폭과 리소스 소모 증가

<br/><br/>
## HTTP/1.0

-   특징 :
    -   수명이 짧은 연결
    -   **요청 당 TCP handshake**가 발생됨. 한 연결 당 하나의 요청 처리.
-   문제점 :
    -   **RTT(Round Trip Time)가 늘어나는 문제점**.
    -   HTTP 1.0 환경에서는 하나의 IP에 여러 개의 도메인을 운영할 수 없음.

RTT 감소를 위해 이미지 스프라이트(image sprite), 코드 압축, base64인코딩 등 시행

<br/><br/>
## HTTP/1.1

-   특징 :
    -   한번 TCP 연결을 해놓고 계속 데이터를 주고 받음. (**keep-alive default**)
    -   서버가 여러 호스트를 가질 수 있다는 전제로 **호스트 헤더를 추가함**.
    -   다운로드 받다가 연결 끊기면 다시 다운로드 받을 수 있도록 함. (**대역폭 최적화**에 대한 헤더를 추가)
-   문제점 :
    -   **HOL(Head Of Line-Blocking)** : 네트워크에서 같은 큐(queue)에서 선두에 있는 작업이 지연되거나 블록되면 뒤에 있는 모든 작업이 지연되는 현상
    -   **무거운 Header** : 수 많은 http 요청이 발생할 것이고, header의 정보는 대부분 동일할 것이다. 즉, 불필요한 데이터를 주고받는데 네트워크 자원이 낭비 됨.

<br/><br/>
## HTTP/2.0

-   특징 :
    -   **바이너리 포맷 계층** 추가 : 파싱, 전송 속도 ⬆️, 오류 발생 가능성 ⬇️
    -   **멀티플렉싱 (I/O Multiplexing)** : 동시에 여러 개의 메세지를 주고 받을 수 있음.  
        또한, 응답은 요청 순서에 상관없이 Stream으로 받기 때문에 HOL(Head Of Line) Blocking 문제 보완.  
        즉 여러 개의 스트림을 사용하여 송수신한다는 것. 이를 통해 특정 스트림의 패킷이 손실되었다고 하더라도 해당 스트림에만 영향을 미치고 나머지 스트림은 멀쩡하게 동작할 수 있음.
    -   **서버 푸시 (Server push)** : 클라이언트가 리소스가 필요할지 알기도 전에 미리 리소스를 로드하여 대기 시간을 줄이는 것. 즉, 서버가 클라이언트의 요청없이 응답을 보내는 방법으로 클라이언트의 요청을 최소화하여 서버가 리소스를 보내주는 방식.
    -   **헤더 압축 (Header Compression)** : 중복되는 헤더는 공통 필드로 헤더 재구성하고, 중복되지 않은 헤더만 허프만 인코딩 압축 방법 등으로 압축시킴.
    -   **우선순위 (Stream Prioritization)** : 소스간 우선 순위를 설정 가능. 응답에 대한 우선순위를 정해 우선순위가 높을수록 응답을 빨리 함.
-   문제점 :
    -   여전히 TCP 초기 연결에 대해 RTT로 인한 지연시간이 존재.
    -   때문에 HOL을 보완하긴 했지만 완전한 해결이 되지 않음.

<br/><br/>
## HTTP/3.0

-   특징
    -   2020년 등장하였으며, TCP 위에서 돌아가는 이전 버전과 달리 HTTP3는 **QUIC이라는 계층** 위에서 돌아가며, TCP 기반이 아닌 **UDP 기반**으로 돌아간다.
    -   http/3.0에서는 **무조건 https를 사용**한다. (네이버는 HTTP3와 HTTP2를 혼용하여 컨텐츠를 서빙하며, 구글은 HTTP3로만 서빙)
    -   HTTP/2.0 에서 장점이었던 멀티플렉싱을 가지고 있으며, "**초기 연결 설정 시 지연 시간 감소"**라는 대표적 특성이 있음.

<br/><br/>
### QUIC

-   HTTP/2.0의 경우 3-RTT가 필요했다면 QUIC은 1-RTT만 필요.
    -   HTTP/2.0 : TCP handshake (1 RTT) + TLS handshake(1~2 RTT)
    -   QUIC : TCP handshake와 TLS handshake를 하나의 단계로 통합 TLS로 암호화통신을 구축할 때의 **단 한번의 handshake**를 활용해 클라이언트와 **서버간의 연결,암호화 통신 모두 다 구축**.
-   **전송 속도 향상** : 첫 연결 설정에서 필요한 정보와 함께 데이터를 전송. 연결 성공 시 설정을 캐싱하여 다음 연결 때 바로 성립 가능
-   **Connection UUID**라는 고유한 식별자로 서버와 연결 : 커넥션 수립 필요가 없음
-   **TLS (전송 계층 보안, Transport Layer Security) 기본 적용**
-   IP Spoofing / Replay Attack을 방지하여 **보안성을 향상**


<br/><br/><br/><br/>
# HTTPS(Hypertext Transfer Protocol Secure)

※ 기본 포트 : 443

인터넷 상에서 정보를 암호화하는 SSL/TLS 프로토콜을 사용해 클라이언트와 서버가 자원을 주고 받을 때 쓰는 통신 규약.

<br/><br/>
## 인증서

인증서는 **주체에 대한 정보, 공개키에 대한 정보를 포함하는 단순한 데이터 파일**이다.

-   주체에 대한 정보 : 인증서 발급한 CA, 도메인, 웹사이트 소유자, 인증서 소유자
-   공개키에 대한 정보 : 공개키, 공개키암호화방법

주체는 클라이언트가 접속한 서버가 클라이언트가 의도한 서버가 맞는지 확인할 때 쓰이고 공개키는 처음 인증작업을 수행할 때 쓰임.

인증서의 역할은 클라이언트가 **접속한 서버가 클라이언트가 의도한 서버가 맞는지를 보장하는 역할**을 한다. 

<br/><br/>
## CA(Certificate Authority)

인증서를 발급하는 기업들을 CA라고 함. 보통은 인증기관인 CA에서 발급한 SSL인증서를 기반으로 인증작업을 수행함.

CA로 부터 인증서 구입 시 서비스의 도메인, 공개키와 같은 정보를 제출해야 함.

CA로부터 인증서 활용 과정

1\. 서버A가 HTTPS를 적용하기 위해 비대칭키(공개키/개인키) 생성

2\. CA 기업에 서비스 도메인, 공개키 제출.

3\. CA 기업은 해당 기업의 이름, A서버 공개키, 공개키 암호화 방법 등을 담은 인증서를 생성.

4\. 인증서를 CA기업의 개인키로 암호화 하여 A서버에게 제공. (CA기업의 공개키는 브라우저가 이미 알고 있다.)

5\. A서버에는 처음 클라이언트에게 이 암호화된 인증서를 건내줌.

6\. 클라이언트는 CA기업의 공개키로 인증서 해독하여 A서버의 공개키를 얻게 됨.

이제 클라이언트는 이 공개키로 세션을 생성해서 암호화 하여 A서버와 통신.

<br/><br/>
## SSL (Secure Sockets Layer)

데이터 **암호화, 인증, 무결성**을 중심 기능으로 하는 프로토콜.

1990년대 넷스케이프(Netscape)에서 처음 개발됨.

SSL 3.0도 여전히 몇몇 보안 문제를 가지고 있음.

이후 TLS 1.0을 지나 1.3버전이 되면서 아예 TLS로 명칭 변경됨.

<br/><br/>
## TLS (Transport Layer Security)

SSL기반의 후속 프로토콜. SSL과 마찬가지로 **암호화, 인증, 무결성**을 중심 기능으로 함. 더 안전하고 효율적.

TLS Handshake : 세션키를 (대칭키로) 생성하는 과정! 

<br/><br/>
### TLS handshake (RSA)

![image](https://github.com/user-attachments/assets/24a88ff7-5f48-4761-88f1-461311802150)


1.  **Client : ClientHello**
    -   클라이언트가 핸드셰이크를 시작. 클라이언트는 ClientHello 메시지를 서버에 보냄  
        (지원하는 암호 스위트 목록, 클라이언트의 랜덤 값, 지원하는 키 교환 방법, 프로토콜 버전이 포함됨)
2.   **Server : ServerHello, Certificate, Done**
    -   ServerHello:  
        서버는 응답으로 ServerHello 메시지를 보냄  
        (선택된 암호 스위트, 서버의 랜덤 값, 그리고 서버가 선택한 키 교환 방법이 포함됨)
    -   Certificate:  
        서버는 자신의 인증서를 클라이언트에 전송. (공개 키 전송)
    -   ServerHelloDone:  
        이후, 서버는 핸드셰이크 준비가 완료되었음을 알리고 클라이언트의 응답을 기다림
3.  **Client : Client Key Exchange, Create Session key, Change Cipher Spec Request, Finished**
    -   Client Key Exchange :  
        **클라이언트는 서버에게 받은 공개키를 이용해서 프리마스터 시크릿을 암호화** 한 후, Client Key Exchange 메시지를 통해 서버에 프리마스터 시크릿을 전송 (프리마스터 시크릿은 TLS 세션에서 생성되는 비밀 값으로, 클라이언트와 서버가 대칭 키를 생성하는 데 사용)
    -   Create Session key:  
        클라이언트는 프리마스터 시크릿, 서버 랜덤값, 클라이언트 랜덤값 등을 사용하여 동일한 **세션키(대칭키)를 만듦.**
    -   Change Cipher Spec Request :  
        클라이언트는 Change Cipher Spec 메시지를 보내, 이후의 통신이 암호화될 것임을 알림
    -   Finished :  
        클라이언트는 Finished 메시지를 보내 핸드셰이크가 완료되었음을 알림
4.  **Server : Create Session key, Change Cipher Spec Respons , **Finished****  
    -   Create Session key:  
        **서버는 암호화된 프리마스터 시크릿을 개인키로 복호화 함**  
        서버는 프리마스터 시크릿, 서버 랜덤값, 클라이언트 랜덤값 등을 사용하여 동일한 **세션키(대칭키)를 만듦**
    -   Change Cipher Spec Respons :  
        서버는 클라이언트와 마찬가지로 Change Cipher Spec 메시지를 보내, 이후의 통신이 암호화될 것임을 알림
    -   Finished :  
        서버는 Finished 메시지를 보내 핸드셰이크가 완료되었음을 알림 (핸드셰이크 메시지의 무결성을 검증하는 정보를 포함)
5.  **안전한 대칭 암호화 성공:** 
    -   핸드셰이크가 완료되고, 세션 키를 이용해 통신이 계속 진행.

<br/><br/>
### TLS handshake (Diffie-Hellman)

위의 방식처럼 RSA의 개인키를 사용하지 않는 대신 Diffie-Hellman 알고리즘(지수 계산)을 이용해 동일한 예비 마스터 암호를 생성함.

서버와 클라이언트가 각자 계산을 위한 매개변수를 제공하고, 각자 서로 다른 계산 결과가 나오지만, 합치면 결과가 동일해짐.

RSA방식과 다른 점은 **클라이언트와 서버가 서로 교환**한 DH 매개변수를 사용하여 일치하는 예비 마스터 암호를 별도로 계산한다는 것.

1.  **Client : ClientHello**
    -   클라이언트가 핸드셰이크를 시작. 클라이언트는 ClientHello 메시지를 서버에 보냄  
        (지원하는 암호 스위트 목록, 클라이언트의 랜덤 값, 지원하는 키 교환 방법, 프로토콜 버전이 포함됨)
2.   **Server : ServerHello, Server **Key Exchange****, Certificate,** Digital signature**
    -   ServerHello:  
        서버는 응답으로 ServerHello 메시지를 보냄  
        (선택된 암호 스위트, 서버의 랜덤 값, 그리고 서버가 선택한 키 교환 방법이 포함됨)
    -   Server Key Exchange:  
        **서버는 DH 매개변수를 통해 계산된 공개키를 전송**
    -   Certificate:  
        서버는 자신의 인증서를 클라이언트에 전송. (공개 키 전송)
    -   Digital signature  
        서버는 이 시점까지의 모든 메시지에 대한 디지털 서명을 계산
3.  **Client : Client Key Exchange, Create Session key, Change Cipher Spec Request, Finished**  
    -   Client Key Exchange :  
        서버의 디지털 서명을 통해 서버의 신뢰성을 확인하면, **클라이언트는 자신의 DH 매개변수를 통해 계산된 공개키를** **Client Key Exchange 메시지를 통해 전송**
    -   Create Session key:  
        **DH 매개변수를 사용하여 서로의 공개 키를 기반으로 동일한 비밀 키를 계산**
    -   Change Cipher Spec Request :  
        클라이언트는 Change Cipher Spec 메시지를 보내, 이후의 통신이 암호화될 것임을 알림
    -   Finished :  
        클라이언트는 Finished 메시지를 보내 핸드셰이크가 완료되었음을 알림
4.  **Server : Create Session key, Change Cipher Spec Respons , **Finished****  
    -   Create Session key:  
        **DH 매개변수를 사용하여 서로의 공개 키를 기반으로 동일한 비밀 키를 계산**  
        
    -   Change Cipher Spec Respons :  
        서버는 클라이언트와 마찬가지로 Change Cipher Spec 메시지를 보내, 이후의 통신이 암호화될 것임을 알림
    -   Finished :  
        서버는 Finished 메시지를 보내 핸드셰이크가 완료되었음을 알림 (핸드셰이크 메시지의 무결성을 검증하는 정보를 포함)
5.  **안전한 대칭 암호화 성공:** 
    -   핸드셰이크가 완료되고, 세션 키를 이용해 통신이 계속 진행.

<br/><br/>
### **TLS 1.3 handshake**

위의 과정들에서 많이 간소화 됨.

1\. Hello 메세지를 보낼 때 DH 매개변수를 만들어 같이 보냄.

2\. Change Cipher Spec이 생략.

3\. Client Key Exchange 과정이 세션 키 생성 과정과 통합.

1.   **ClientHello**
    -   클라이언트가 핸드셰이크를 시작. 클라이언트는 ClientHello 메시지를 서버에 보냄  
        (암호 목록, 클라이언트의 랜덤 값, DH 매개변수, 프로토콜 버전이 포함됨)
2.  **ServerHello, Certificate, Encrypted Extensions, (+ CertificateVerify), Session, Done**  
    -   ServerHello:  
        서버는 응답으로 ServerHello 메시지를 보냄  
        (선택된 암호 스위트, 서버의 랜덤 값, DH 매개변수 등이 포함됨)
    -   Certificate:  
        서버는 자신의 인증서를 클라이언트에 전송. (공개 키 전송)
    -   Encrypted Extensions:  
        추가 확장 정보를 제공. 선택한 암호 스위트에 따라 다를 수 있는 추가 옵션을 포함
    -   클라이언트에게 받은 값으로 
    -   CertificateVerify:  
        클라이언트 인증을 요구하는 경우, 서버는 클라이언트에게 CertificateVerify 메시지를 요청
3.  **Create Session key, Finished**  
    -   Create Session key:  
        서버와 클라이언트는 공유된 DH매개변수를 사용하여 세션키를 생성.
    -   Finished :  
        Finished 메시지를 보내 핸드셰이크가 완료되었음을 알림.
4.  **안전한 대칭 암호화 성공:** 
    -   핸드셰이크가 완료되고, 세션 키를 이용해 통신이 계속 진행.

TLS 1.3 핸드셰이크는 이전 버전과 달리, 핸드셰이크 과정에서 대칭키가 교환되고 나면 모든 후속 핸드셰이크 메시지도 암호화된다.

<br/><br/>
### 0-RTT ((Zero Round Trip Time)

TLS 1.3에서는 0-RTT handshake를 지원한다. 이 기능은 이전에 연결된 적이 있는 클라이언트가 서버와 재연결할 때 사용할 수 있다.

-   **ClientHello with Early Data**
    -   클라이언트는 이전 세션의 PSK(Pre-Session Key)를 사용하여 ClientHello 메시지와 함께 초기 데이터를 서버에 보냅니다.
    -   이 초기 데이터는 암호화되어 전송됩니다.
-   **ServerHello with Early Data**
    -   서버는 클라이언트의 ClientHello 메시지를 수신하고, PSK를 검증합니다.
    -   서버는 ServerHello 메시지와 함께 초기 데이터를 클라이언트에 보냅니다.

 위의 과정이 0-RTT 내에 완료됨.
