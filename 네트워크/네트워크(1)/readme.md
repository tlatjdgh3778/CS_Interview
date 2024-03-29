# 네트워크 (1)

## 네트워크란?
네트워크는 Net + Work (Network) 의 합성어로써 컴퓨터들이 통신 기술을 이용하여 그물망처럼 연결된 통신 이용 형태를 말한다.

## 1. OSI 7계층
### 1-1. OSI 모형
**OSI 모델**(Open Systems Interconnection Reference Model) 은 국제표준화기구(ISO)에서 개발한 모델로 컴퓨터 네트워크 프로토콜 디자인과 통신을 계층으로 나누어 설명한 것이다.

<p align="center"><img src="https://t1.daumcdn.net/cfile/tistory/99B9DA355C39CC9725" width="70%"/><br><a href="https://reakwon.tistory.com/59">https://reakwon.tistory.com/59</a></p>

이렇게 7계층으로 나눈 모델을 사용하면 특정 네트워킹 시스템에서 일어나는 일을 계층을 활용해서 시각적으로 쉽게 설명할 수 있다. 그리고 네트워크 에러가 발생했을 때 어떤 계층에서 발생했는지도 파악이 가능하다.

데이터를 전송할 때 상위 계층에서 하위 계층으로 데이터를 전달하는데 각 계층을 넘어갈 때 마다 해당 계층의 헤더를 붙여서 전달한다. 헤더는 각 계층별 기능을 수행하기 위한 정보가 들어있다. 이 과정을 **캡슐화(Encapsulation)** 라고 한다. 

수신자는 전달 받은 정보를 하위 계층에서부터 상위 계층으로 전달하면서 헤더의 정보를 확인하면서 헤더를 벗겨내는 과정을 거치는데 이 과정을 **역캡슐화(Decapsulation)** 이라고 한다.

다음은 각 계층에 대해서 알아보자.

### 1-2. OSI 7계층 

**1. 계층 1 물리 계층(Physical Layer)**

물리 계층은 전기적, 기계적, 기능적인 특성을 이용해서 통신 장비로 데이터를 전송한다.

물리 계층에서는 데이터를 전송하는 역할만 하며 전송하거나 전송 받을 때 데이터가 무엇인지, 어떠한 에러가 있는지 등에 대해서는 신경 쓰지 않는다.

* 전송 단위 - Bit

**2. 계층 2 데이터 링크 계층(Data Link Layer)**

직접 연결된 두 개의 노드 사이에서 안전하게 데이터가 전송되도록 하고 물리 계층에서 담당하지 않는 흐름 제어 및 오류 수정의 기능을 담당한다.

물리 계층에서는 데이터의 신뢰성에 대한 특별한 검사를 하지 않기 때문에 데이터 링크 계층에서 데이터의 신뢰성을 보장해주는 것이다.

데이터 링크 계층에서는 물리 주소인 MAC 주소를 사용해서 데이터의 정확한 송수신이 가능하게 한다.

* 전송 단위 - Frame

**3. 계층 3 네트워크 계층(Network Layer)**

네트워크 계층은 데이터를 목적지까지 안전하고 빠르게 전달하는 기능인 **라우팅** 기능을 수행한다. 경로를 선택하고 주소를 정하고 경로에 따라 패킷을 전달해주는 것이 네트워크 계층의 역할이다.

경로 선택 이외에도 트래픽이 한 곳으로 몰리지 않도록 제어하고 패킷의 분할과 병합 및 두 개 이상의 네트워크를 연결하는 인터네트워킹의 역할도 한다.

* 전송 단위 - Packet

**4. 계층 4 전송 계층(Transport Layer)**

전송 계층은 최종 도착지에 위치한 **어떤 프로세스**에게 데이터를 전달할 것인가, 즉 포트 번호를 명시하는 계층이라고 할 수 있다.

전송 계층은 양 끝단(end to end)의 사용자들이 신뢰성있는 데이터를 주고 받을 수 있도록 해주며, 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 해준다.

보통 **TCP** 프로토콜을 이용하며, 포트를 열어서 응용프로그램이 전송을 할 수 있게 한다. 중요한 것은 데이터 전송을 위해 포트 번호가 사용된다는 점이다.

 * 전송 단위 - Segment
 
**5. 계층 5 세션 계층(Session Layer)**

세션 계층은 통신하는 두 기기 사이의 연결 상태를 관장하는 계층으로, 어떠한 방식으로 두 기기가 상호작용할 것인지를 결정한다.

쌍방향으로 동시에 데이터를 주고받을 것인지, 단방향으로 서로 번갈아가며 주고받을 것인지, 일방적으로 데이터를 받기만 할 것인지 등에 관한 방식을 결정한다.

 * 전송 단위 - message
 
**6. 계층 6 표현 계층(Presentation Layer)**

표현 계층은 수신자가 이해할 수 있는 형태로 데이터를 변환하고(인코딩) 데이터 전송의 효율성과 안전성을 보장하기 위해 데이터를 압축하고 암호화하는 계층이다. 통신하는 두 기기가 특성이 같다는 보장이 없기 때문에 데이터의 형태를 변환하는 것이다.

해당 데이터가 TEXT인지, 그림인지, GIF인지의 구분 등이 표현 계층의 역할이다.

 * 전송 단위 - message
 
**7. 계층 7 응용 계층(Application Layer)**

사용자가 어플리케이션에 입력한 정보를 특정 프로토콜(HTTP, SMTP, FTP 등)의 형식에 맞게 표현하는 계층이다. 

사용자와 바로 연결되어 잇으며 응용 SW를 도와주는 계층이다. 사용자로부터 정보를 입력받아 하위 계층으로 전달하고 하위 계층에서 전송한 데이터를 사용자에게 전달한다.

 * 전송 단위 - message
 
## 3. TCP 와 UDP
TCP와 UDP는 OSI 모델과 TCP/IP 모델의 **전송 계층**에서 사용되는 프로토콜이다. 전 송 계층은 데이터의 전달을 담당하는 계층이다.

데이터를 중요하게 생각해서 확실히 주고받고 싶을 때는(정확성을 추구) TCP를 사용한다. 그에 반해 데이터의 신뢰성은 제쳐두고 빨리 보내고 싶을 때는(신속성을 추구) UDP를 사용한다.

### 3-1. TCP(Transmission Control Protocol)
TCP는 전송 계층에서 사용하는 프로토콜이이다. 장치들 사이의 논리적인 접속을 성립(establish)하기 위하여 연결을 설정하여 신뢰성을 보장하는 연결형 서비스이다. TCP는 네트워크에 연결된 컴퓨터에서 실행되는 프로그램 간에 일련의 옥텟(데이터, 메세지, 세그먼트라는 블록 단위)을 안정적으로, 순서대로, 에러없이 교환할 수 있게 한다.

**특징**

 * 연결형 서비스로 연결이 성공해야 통신이 가능하다.
   * 3-way handshake 과정으로 연결을 설정하고 4-way handshake 과정으로 연결을 해제한다.
 * 높은 신뢰성을 보장한다.
 * UDP보다 속도가 느리다.
 * 흐름 제어
   * 데이터를 송신하는 곳과 수신하는 곳의 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지한다.
   * 송신하는 곳에서 감당이 안되게 많은 데이터를 빠르게 보내 수신하는 곳에서 문제가 일어나는 것을 막는다.
 * 혼잡 제어
   * 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지한다.
   * 정보의 소통량이 과다하면 패킷을 조금만 전송하여 혼잡 붕괴 현상이 일어나는 것을 막는다.
 * 신뢰성있는 전송이 중요할 때에 사용한다.
 
### 3-2. TCP Handshake
TCP는 장치들 사이에서 논리적인 접속을 성립(establish)하기 위해서 3-way handshake를 사용하고 종료과정을 위해 4-way handshake를 사용한다.

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile2.uf.tistory.com%2Fimage%2F9910A8345BB0B75F2A0A82"><a href="https://www.crocus.co.kr/1362">https://www.crocus.co.kr/1362</a></p>

**3-way handshake**

* 클라이언트는 접속을 요청하는 SYN 패킷을 보낸다. 이 때 클라이언트는 SYN 패킷을 보냄과 동시에 SYN/ACK 응답을 기다리기 위해 SYN_SENT 상태로 변경된다.
* 서버는 SYN 요청을 받고 요청을 수락하는 ACK 패킷과 SYN 패킷을 보낸다. 그리고 SYN_RCVD 상태로 변하며 클라이언트가 ACK 패킷을 보낼 때 까지 기다린다.
* 클라이언트는 ACK 패킷과 SYN 패킷을 받고 established 로 상태를 변경하고 서버에 ACK 를 전송한다.
* ACK 를 받은 서버는 상태가 established 로 변경된다.

위처럼 3번의 통신이 정상적으로 이루어지면 서로의 포트가 established 상태가 되면서 연결이 된다. ACK 패킷은 신뢰적 데이터 전송을 위해 사용되는 것이다.

**4-way handshake**

* 서버와 클라이언트가 TCP 연결이 되어있는 상태에서 클라이언트가 접속을 끊기 위해 close() 함수를 호출하게 된다. 이후 close() 함수를 호출하면서 FIN 을 보내고 클라이언트는 FIN_WAIT1 상태로 변하게 된다.
* 서버는 클라이언트가 close() 한다는 것을 알게되고 CLOSE_WAIT 상태로 바꾼 후 ACK 를 클라이언트로 전송한다. 즉, 서버는 클라이언트가 끊을 것이라는 신호를 받았고 CLOSE_WAIT 을 통해 자신의 통신이 끝날때까지 기다리는 상태가 된다.
* ACK 를 받은 클라이언트는 FIN_WAIT2 상태로 변경되고 이때 서버는 CLOSE() 함수를 호출하고 FIN 을 클라이언트로 보낸다.
* 서버도 연결을 닫았다는 신호를 클라이언트가 수신하면 서버로 ACK 를 보낸 후 TIME_WAIT 상태로 변경된다.
* 모든 것이 끝나면 CLOSED 상태로 변경된다.


### 3-2. UDP(User Datagram Protocol)
UDP는 전송 계층의 비연결 지향적 프로토콜이다. 비연결 지향적이란 데이터를 주고 받을 때 연결 절차를 거치지 않고 발신자가 일방적으로 데이터를 발산하는 방식을 의미한다. 연결 과정이 없기 때문에 TCP 보다 빠르게 전송이 가능하지만 신뢰성은 떨어진다.

UDP는 데이터 패킷을 순차적으로 보내더라도 서로 다른 통신 선로를 통해 전달 될 수 있다. 먼저 보낸 패킷이 느린 선로를 통해 전송될 경우 나중에 보낸 패킷보다 더 늦게 도착할 수도 있고 최악의 경우 잘못된 선로로 전송되어 유실될 가능성도 있다. 이 경우 TCP와는 다르게 UDP는 재전송을 하지 않는다.

**특징**
 * 비연결형 서비스로 연결 없이 통신이 가능하며 데이터그램 방식을 제공한다.
   * 연결을 위해 할당되는 논리적인 경로가 없다.
   * 때문에 각각의 패킷은 다른 경로로 전송되고, 각각의 패킷은 독립적인 관계를 지니게 된다.
 * 정보를 주고 받을 때 정보를 보내거나 받는다는 신호절차를 거치지 않는다.
 * 신뢰성이 낮다.
 * TCP 보다 속도가 빠르다.
 * 신뢰성보다는 연속성이 중요한 서비스, 실시간 서비스에 사용된다.
 
### 4. HTTP 와 HTTPS
### 5.
...


## 참고
https://it-eldorado.tistory.com/78
