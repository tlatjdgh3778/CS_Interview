# 네트워크(2)

## 4. HTTP 와 HTTPS
### 4-1. HTTP
HTTP(HyperText Transfer Protocol) 란 W3 상에서 정보를 주고 받을 수 있는 프로토콜이다. 주로 TCP 를 사용하고 80번 포트를 사용한다.

HTTP는 클라이언트와 서버 사이에 이루어지는 요청/응답(request/response) 프로토콜이다. 클라이언트인 웹 브라우저가 HTTP를 통해 서버로부터 웹페이지(HTML)나 그림 정보를 **요청**하면 서버는 이 요청에 **응답**하여 필요한 정보를 해당 사용자에게 전달하게 된다.

### 4-2. HTTP의 특징
* **비연결성(Connectionless)**
  * 클라이언트와 서버가 한 번 연결을 맺은 후 클라이언트 요청에 대해 서버가 응답을 하면 맺었던 연결을 끊는 특성이다.
  * 불특정 다수를 대상으로 하는 서비스에 적합한 방식이다. 많은 수의 사용자들이 웹 서비스를 사용하더라도 접속 유지는 최소한으로 할 수 있기 때문에 더 많은 사용자의 요청을 처리할 수 있다.
  * 하지만 잦은 연결/해제에 따른 오버헤드가 생긴다는 단점이 있다.
  * HTTP/1.1 의 KeepAlive 속성으로 오버헤드를 줄일 수 있다.
  
* **무상태(Stateless)**
  * 비연결성으로 인해 연결을 끊어버리기 때문에 클라이언트의 이전 상태를 알 수 없는데 이러한 특징을 **무상태(Stateless)**라고 한다.
  * 클라이언트의 이전 상태를 알 수 없게 되면 클라이언트가 과거에 로그인을 성공하더라도 로그 정보를 유지할 수가 없기 때문에 매번 인증을 해야한다.
  * 상태를 기억하기 위해서 **쿠키, 세션, OAuth, JWT** 같은 방법들이 있다.
  
### 4-3. HTTP 버전

#### 1. HTTP/0.9

HTTP 초기 버전에는 버전 번호가 존재하지 않았다. 차후 버전과 구별하기 위해 0.9로 불리게 되었다. 

```
GET /mypage.html
```

요청은 단일 라인(원 라인 프로토콜)으로 구성되고 메소드는**GET** 만 존재한다.

```
<HTML>
very simple HTML page
</HTML>
```
응답에서도 HTML 파일만 전송이 가능했고, 상태 확인을 위한 코드도 존재하지 않았다.

#### 2. HTTP/1.0

기능이 매우 제한적인 이전 버전을 개선하여 HTTP/1.0 버전이 나오게 되었다.

* 요청에 대한 버전 정보가 붙어서 나왔다(GET 라인에 HTTP/1.0가 붙어있음)
* 응답 코드 첫 줄에 상태 코드(200 OK)가 생겼다.
* 모든 요청과 응답에 대해 헤더 개념이 도입되었다.(확장성의 증가)
  * content-type 헤더로 HTML 뿐만이 아니라 다른 타입의 파일도 전송이 가능해졌다.
  
```
GET /mypage.html HTTP/1.0
User-Agent: NCSA_Mosaic/2.0 (Windows 3.1)

-------

200 OK
Date: Tue, 15 Nov 1994 08:12:31 GMT
Server: CERN/3.0 libwww/2.17
Content-Type: text/html
<HTML>
A page with an image
  <IMG SRC="/myimage.gif">
</HTML>
```

**단기 커넥션**

HTTP/1.0에서는 단기 커넥션 모델만을 제공했다. 

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/4eccaaf7-977a-4ee2-b6df-4d35e818ad5c/image.png"></p>

요청이 보내져야 할 때마다 커넥션들이 매번 생성되었고 응답이 도착한 이후에 연결을 닫는 형태로 유지되었다. 

**단점**

하지만 이렇게 매번 요청이 생길 때마다 새로운 연결이 만들어져야 하므로 성능이 좋지 않았다.

#### 3. HTTP/1.1

HTTP의 첫 번째 표준 버전인 HTTP/1.1은 HTTP/1.0 이 나온지 몇 달 안 돼서 발표되었다. 1.1 버전은 모호함을 명확하게 하고 많은 개선 사항들을 도입했다.

* **지속 커넥션(Persistent Connection)**
  * 기존 모델의 단점을 해결하기 위해 한 번 열린 커넥션을 재사용하는 지속적인 커넥션 모델
  * Keep-alive 라고도 한다(keep-alive를 통해 최소한 얼마나 열려있을지 설정 가능)
  * 지속 커넥션은 연결을 열어놓고 여러 요청에 재사용함으로써 기존 연결에 대해서 handshake 생략이 가능하다.
  * 하지만 유휴 상태일때도 리소스를 소비하는 단점이 있다.

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/a8788e99-c723-4ad3-9041-734f4bf1eca5/image.png"></p>

* **파이프라이닝(Pipelining)** 
  * 기본적으로 HTTP 요청은 순차적이다. 하나의 요청 후게 응답을 기다린 후 다시 요청을 보내기 때문에 지연이 생길 수 있다.
  * 파이프라이닝은 지속 커넥션을 이용해 응답을 기다리지 않고 요청을 연속적으로 보낼 수 있는 기능이다.
  * 하나의 커넥션으로 다수의 요청과 응답을 처리함으로써 Networt Latency 를 줄일 수 있다.

<p align="center"><img src="https://images.velog.io/images/tlatjdgh3778/post/4dabc04e-033a-47a3-b3cf-b36c85c23b30/image.png"></p>

**단점**

* **Head Of Line Blocking**
  * 파이프라이닝으로 요청을 연속적으로 보낼 수 있게 되었지만 순차적으로 처리한다는 사실은 변함이 없다.
  * 만약 첫 번째 응답이 오래 걸린다면 그 뒤의 응답들은 지연될 수 밖에 없는 문제가 생기게 되는데 이를 **Head Of Line Blocking** 이라고 한다.
  
* **무거운 헤더 구조**
  * 헤더의 값이 중복되는 경우에도 매 요청 시 마다 중복된 헤더값을 전송하게 된다.
  
<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FumZw7%2FbtqEInJBsXf%2FHDLFNWMXA48dzODzbkdqy0%2Fimg.jpg" width="40%"></p>

#### 4. HTTP/2

HTTP/2는 [SPDY](https://ko.wikipedia.org/wiki/SPDY)에 기반하고 있으며 HTTP/1.1을 개선한 버전이다.

**Multiplexed Streams**

* 바이너리 프레이밍 계층을 사용해 요청과 응답의 멀티플렉싱을 지원한다.

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F2sn9z%2FbtqLYMb0iZj%2FTHAN93OZUCcelaBfy5jOO0%2Fimg.png" width="70%"></p>

* 기존의 HTTP 1 버전에서의 데이터는 단순한 텍스트 형식으로 구성되어 있었는데, HTTP/2 에서는 바이너리로 인코딩하여 데이터를 전송한다. 데이터를 프레임이라는 단위로 나눠서 관리 / 전송이 가능해졌다.

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile24.uf.tistory.com%2Fimage%2F234FA13E593A31FE278FD6" width="70%"></p>

* 스트림 : 구성된 연결 내에서 전달되는 바이트의 양방향 흐름이며, 하나 이상의 메세지가 전달될 수 있다.
* 프레임 : HTTP/2에서 통신의 최소 단위이며 각 최소 단위에는 하나의 프레임 헤더가 포함된다. 이 프레임 헤더는 최소한으로 프레임이 속하는 스트림을 식별한다.
* 각 프레임의 헤더에 삽입된 스트림 식별자를 통해 프레임을 다시 조립할 수 있다.

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile30.uf.tistory.com%2Fimage%2F251B0644593A20A918923F" width="70%"></p>

* HTTP1 에서는 클라이언트가 여러 병렬 요청을 수행하려는 경우 여러 TCP 연결이 사용되어야 했다. 
* HTTP/2의 바이너리 프레이밍 계층이 이러한 제한을 없애주고 요청 및 응답의 다중화를 지원한다. 이를 위해서 클라이언트와 서버가 HTTP 메세지를 독립된 프레임으로 세분화하고, 이 프레임을 인터리빙 한 다음, 다른 쪽에서 다시 조립하도록 허용함으로써 메시지 간의 순서라는 것이 의미가 없어지게 된다.(Head Of Line Blocking 해결)

**Header Compression**

* HTTP1 버전에서는 헤더의 값이 중복되는 경우에도 매 요청 시 마다 중복된 헤더값을 전송을 했다. 

<p align="center"><img src="https://developers.google.com/web/fundamentals/performance/http2/images/header_compression01.svg?hl=ko" width="70%"></p>

* 이 오버헤드를 줄이고 성능을 개선하기 위해 HTTP/2에서는 HPACK 압축 형식을 사용하여 요청 및 응답 헤더 메타데이터를 압축한다. 

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile4.uf.tistory.com%2Fimage%2F262CA04B593976721AEBD4" width="70%"></p>

* HTTP/2 에서는 헤더를 테이블로 관리하는데 중복으로 선언된 헤더는 인덱스 값만 전송해서 데이터 양을 최소로 한다.
* 만약 새롭게 추가되거나 변경된 헤더는 Huffman Encoding 기법을 사용해서 처리한다.

HTTP/2에 대해 더 자세히 알고 싶다면

<a href="https://www.popit.kr/%EB%82%98%EB%A7%8C-%EB%AA%A8%EB%A5%B4%EA%B3%A0-%EC%9E%88%EB%8D%98-http2/">링크</a>

여기에 자세한 설명이 나와있다.

#### 5. HTTP/3

HTTP/3은 HTTP 프로토콜의 3번째 버전으로 이전 버전은 모두 TCP 기반으로 설계되어 왔었는데 HTTP/3 버전은 UDP를 기반으로 한 QUIC으로 설계를 했다.

...차후 업데이트

#### 6. HTTP의 문제점
 * 암호화 하지 않은 통신이기 때문에 도청이 가능하다.
 * 통신 상대를 확인하지 않기 때문에 위장이 가능하다.
 * 완전성을 증명할 수 없기 때문에 변조가 가능하다.


### 4-2. HTTPS
HTTPS(HyperText Transfer Protocol over Secure Socket Layer)는 HTTP의 보안이 강화된 버전이다. 

SSL 3.1 버전부터 TLS로 명칭이 바뀌면서 SSL과 TLS를 혼용해서 사용하고 있다.

HTTPS는 새로운 프로토콜이 아니라 HTTP 통신하는 소켓 부분을 SSL이나 TLS이라는 프로토콜로 대체하는 것 뿐이다. HTTP는 원래 TCP와 직접 통신했지만, HTTPS에서 HTTP는 SSL과 통신하고 **SSL이 TCP와 통신**하게 된다.

* **공개키/비밀키** : 공개키는 모두가 볼 수 있는 키이며 비밀키는 소유자만이 가지고 있는 키로 암/복호화에 사용된다.
* **대칭키 암호화** : 서버와 클라이언트가 암/복호화에 동일한 비밀키를 사용하는 방식, 키를 공유하는데 어려움이 있으나 속도가 빠르다.
* **비대칭키(공개키) 암호화** : 서버와 클라이언트가 암/복호화에 각각 다른 비밀키를 사용하는 방식. 공개키를 통해서 암호화를 하고 비밀키를 통해서 복호화를 한다. 속도가 느리다.
* **인증기관(CA)** : 클라이언트가 접속을 요청한 서버가 의도한 서버가 맞는지 인증해주는 역할을 하는 보증된 기업들이다. 클라이언트는 서버에 요청을 해서 CA가 발급한 인증서를 받은 뒤 CA의 공개키로 복호화하여 신뢰할 만한 인증서인지 검증한다. CA의 공개키로 복호화가 된다는 것은 CA의 비밀키로 암호화한 경우밖에 없기 때문에 신뢰를 할 수 있다.

**HTTPS 동작 방식**

HTTP의 SSL에서는 공개키 암호화 방식과 대칭키 암호화 방식을 함께 사용한다.

공개키 방식으로 대칭키를 교환하고 다음부터의 통신은 대칭키 암호를 사용하는 방식이다.

* 클라이언트가 서버에게 접속 요청을 하면 서버는 CA에서 발급받은 인증서를 보낸다. 인증서에는 CA의 비밀키로 암호화된 사이트 정보와 공개키가 들어있다.
* 클라이언트는 인증서를 받아 CA의 공개키로 복호화하여 접속 요청한 서버가 신뢰할 만한지 검증한다.
* 복호화가 되면 인증서가 신뢰할 만하기 때문에 데이터를 주고 받을 대칭키를 생성한다.
* 대칭키를 서버의 공개키로 암호화하여 서버에게 전송한다.
* 서버는 자신의 비밀키로 클라이언트가 보낸 대칭키를 복호화한 뒤 그 대칭키를 통해 데이터를 주고 받는다.


### 참고
https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP

https://developer.mozilla.org/ko/docs/Web/HTTP/Connection_management_in_HTTP_1.x

https://developers.google.com/web/fundamentals/performance/http2?hl=ko

<a href="https://www.popit.kr/%EB%82%98%EB%A7%8C-%EB%AA%A8%EB%A5%B4%EA%B3%A0-%EC%9E%88%EB%8D%98-http2/">https://www.popit.kr/</a>

https://blog.sonim1.com/99
