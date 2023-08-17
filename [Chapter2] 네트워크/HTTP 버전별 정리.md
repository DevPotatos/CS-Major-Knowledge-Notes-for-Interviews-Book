## HTTP

### HTTP란?

인터넷 상에서 데이터를 주고 받기 위한 서버/클라이언트 모델을 따르는 프로토콜이다.

클라이언트가 HTTP를 통해 요청(request)하면 서버가 요청을 처리해서 응답(response)한다. 이를 통해, 클라이언트와 서버의 역할이 분리되어 각자의 역할에 집중할 수 있게 된다. 



### 특징

- **Stateless** - 무상태
  - 서버가 클라이언트의 상태를 보존하지 않는다.
  - 상태 유지가 필요한 경우, 세션과 쿠키를 이용한다.
  - 서버가 클라이언트의 상태를 관리하지 않기 때문에 **서버 확장(scale-out)에 용이**하다.
- **Connectionless** - 비연결성
  - HTTP/1.0 기준
  - 서버에 연결하고 요청해서 응답을 받으면 TCP/IP 연결을 끊어버린다. 
    이를 통해 **최소한의 자원으로 서버를 유지**할 수 있다.
  - 자원을 받을 때마다 TCP 연결 및 종료를 반복하게 되므로 **트래픽이 많은 서비스에서 한계**를 보인다.
    이를 해결하기 위해 **HTTP 지속 연결**을 사용하게 되었다.



## HTTP/1.0

HTTP/1.0은 HTTP/0.9에 비해 브라우저와 서버 모두 좀 더 융통성을 가지도록 빠르게 확장되었다.



**일반적인 요청과 응답**

```http
GET /mypage.html HTTP/1.0
User-Agent: NCSA_Mosaic/2.0 (Windows 3.1)

200 OK
Date: Tue, 15 Nov 1994 08:12:31 GMT
Server: CERN/3.0 libwww/2.17
Content-Type: text/html
<HTML>
A page with an image
  <IMG SRC="/myimage.gif">
</HTML>
```



### 특징

- 버전 정보가 각 요청 사이 내로 전송되기 시작했다.
- 응답의 시작 부분에 상태 코드 라인이 붙어 브라우저가 요청에 대한 성공과 실패를 알 수 있고 그 결과에 대한 동작을 할 수 있게 되었다.
- HTTP 헤더 개념의 도입으로 HRML 파일 외 다른 문서들을 전송할 수 있게 되었다.
- **한 연결당 하나의 요청을 처리하도록 설계되었다.**
  - RTT란 패킷이 목적지에 도달하고 나서 다시 출발지로 돌아오기까지 걸리는 시간이며 패킷 왕복 시간을 의미한다.
  - 서버로부터 파일을 가져올 때마다 TCP의 3-웨이 핸드셰이크를 계속 열어야 하기 때문에 RTT가 증가하는 문제점이 발생한다. 즉, 성능 저하가 발생한다.



## HTTP/1.1

### 지속 연결(Persistent Connection)

HTTP/1.0에서 비연결성으로 인해 발생하는 성능 저하 문제를 해결하기 위해 **지속 연결** 개념이 도입되었다. **지속 연결이란 지정한 시간 동안 커넥션을 닫지 않는 방식이다.** 연결을 유지하면서 여러 개의 요청, 응답을 처리함으로써 아래 그림과 같이 성능이 개선되었다. 

<figure align="center"> 
  <img src="https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F37024fde-ab57-4953-9fd1-62dda7951b1f%2FUntitled.png&blockId=1fca311e-5841-45d2-8770-a9821f766e86" width="400"> <img src="https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F46a04f27-85b1-4573-931a-88e7df14c211%2FUntitled.png&blockId=318906c9-6dfc-4a2e-a813-0d08920d69ac" width="400">
  <figcaption>비연결성 & 지속 연결</figcaption>
</figure>



### 파이프라이닝(Pipelining)

**HTTP 요청은 연속적으로 발생하고 순차적으로 동작한다.** 앞선 요청의 응답이 완료되어야만 이후 요청에 대해 응답할 수 있다는 의미이다. 따라서, 문서 안에 포함된 다수의 자원(이미지, 동영상, css 파일, js 파일 등)을 처리하려면 요청할 리소스 개수에 비례해서 **대기 시간이 길어지는 문제**가 발생한다.
이를 해결하기 위해 파이프라이닝 개념이 도입되었다. 파이프라이닝이란 **하나의 커넥션에서 첫 번째 요청에 대한 응답이 완전히 전송되기 전에 다음 요청 전송하는 방식**을 의미한다.

<img src="https://velog.velcdn.com/images/cjh8746/post/2bd4f5cd-f272-4311-b605-f3d9906fc4f2/image.png" width="400"> 



### HTTP/1.1 문제점

- **HOL BLOCKING(Head Of Line Blocking)**
  - HOL Blocking은 **네트워크에서 같은 큐에 있는 패킷이 그 첫 번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상**을 의미한다.

- **무거운 헤더 구조**
  - HTTP/1.1의 헤더에는 쿠키 등 많은 메타데이터가 저장되어있다. 클라이언트가 서버로 보내는 HTTP 요청은 매 요청마다 **중복된 헤더 값을 전송하게 되며** 서버의 도메인과 관련된 쿠키 정보도 헤더에 함께 포함되어 전송된다. 이러한 중복된 헤더 정보로 인해 네트워크 지연과 자원이 소비된다.



## HTTP/2

HTTP/2는 HTTP/1.1를 완전하게 재작성한 것이 아닌 **성능에 초첨을 맞추어 수정한 버전**이다. 멀티플렉싱, 헤더 압축, 서버 푸시, 요청의 우선순위 처리를 지원하는 SPDY에서 파생된 프로토콜이다.



### HTTP/2 목표 및 솔루션

- Latency 감소 => 멀티플렉싱
- 네트워크 통신에 필요한 데이터양 감소 => 헤더 압축
- 서버에서 클라이언트로 먼저 데이터를 보낼 수 있는 방법 => 서버 푸시



### 바이너리 프레이밍 계층(binary framing layer) 및 Multiplexed Streams

**1. 바이너리 프레이밍 계층(binary framing layer)**

<img src="https://web-dev.imgix.net/image/C47gYyWYVMMhDmtYSLOWazuyePF2/2a2cw0nAXe9Mi5txM4Mw.svg" width="400">

HTTP/2에서 **바이너리 프레이밍 계층(binary framing layer)**가 추가되었다. HTTP/1.x는 메시지가 plain text로 구성되고 개행으로 구분된 반면에 **HTTP/2는 메시지를 binary 단위로 구성하고 더 작은 프레임으로 쪼개서 관리한다.** 

즉, HTTP/2에서는 프레임이 모여 하나의 메시지를 구성한다.

- **프레임(frame)**이란 HTTP/2의 통신 최소 단위이며 모든 패킷에는 하나의 프레임 헤더가 포함된다. **프레임 헤더 내부에 프레임 식별자가 존재하여 수신 측에서 응답 순서 상관없이 프레임을 받아도 순서대로 재배치가 가능**하다.
- 메시지가 binary로 구성되어 plain text보다 **파싱 및 전송 속도가 증가하고 오류 발생 가능성이 감소**되었다.



<img src="https://web-dev.imgix.net/image/C47gYyWYVMMhDmtYSLOWazuyePF2/0bfdEw00aKXFDxT0yEKt.svg" width="400">

위 그림과 같이 HTTP/2는 하나의 스트림 안에 한 개 이상의 요청/응답 메시지가 전달될 수 있고 바이너리 인코딩된 프레임으로 구성된 메시지는 목적지에 도착하였을 때 재조립된다.

- Stream이란 시간이 지남에 따라 사용할 수 있게 되는 일련의 데이터 요소를 가리키는 데이터 양방향 흐름을 의미한다.



**2. Multiplexed Streams**

<img src="https://velog.velcdn.com/images%2Fcodenmh0822%2Fpost%2F15642d58-33ba-415e-bf90-e519d91bfd57%2Fimage.png" width="400">



HTTP/2는 Multiplexed Streams를 이용하여 **하나의 커넥션으로 동시에 여러 개의 메시지를 주고 받을 수 있다**.  또한, 앞서 바이너리 프레이밍 계층에서 설명한 것처럼 메시지가 프레임 단위로 쪼개졌기 때문에 응답을 요청 순서에 상관없이 Stream으로 주고 받을 수 있게 되어 **HOL Blocking 문제가 발생하지 않는다.**



### 헤더 압축 (Header Compression)

HTTP/1.1의 경우, 이전 요청과 중복되는 헤더도 똑같이 전송하기 때문에 네트워크 자원이 불필요하게 낭비되었다. HTTP/2에서는 **HPACK 압축 방식**이 도입하여 **헤더 정보를 압축**함으로써 **네트워크 통신에 필요한 데이터양을 감소**시켰다.



**HPACK 압축 방식이란?**

<img src="https://blog.kakaocdn.net/dn/bE78mB/btrRpXYAvZP/i7kSUhkzka85xi2EQeq9n0/img.png" width="400">

Static/Dynamic Header Table을 이용하여 헤더에 중복값이 존재하는지 확인하여 중복 Header를 검출한다. 중복된 헤더인 경우, index값만 전송하고 중복되지 않은 해더인 경우, 헤더 정보의 값을 Huffman Encoding 기법으로 인코딩 처리하여 전송한다. 

Huffman Encoding 기법은 문자열을 문자 단위로 쪼개 빈도수를 세어 빈도가 높은 정보는 높은 비트 수를 사용하여 표현하고 빈도가 낮은 정보는 비트 수를 많이 사용하여 표현해서 **전체 데이터의 표현에 필요한 비트양을 줄이는 원리**이다.



### 서버 푸시(Server Push)

HTTP/1.1에서는 클라이언트가 서버에 요청을 해야 파일을 다운받을 수 있었다면, **HTTP/2는 서버 푸시를 통해 클라이언트 요청 없이 서버가 바로 리소스를 푸시할 수 있다.**

<img src="https://web-dev.imgix.net/image/C47gYyWYVMMhDmtYSLOWazuyePF2/o6lgFK9DiyJmdWwyJnyy.svg" width="400">

서버 푸시(Server Push)란 **클라이언트의 요청이 한 개였음에도 불구하고 여러 개의 응답을 보낼 수 있는 서버의 기능**을 의미한다. 

예를 들어, 유저가 웹사이트에 접속해서 html 파일을 받고 필요한 추가 css 파일이나 js 파일이 있다는 것을 인지하면 추가 리소스들을 위한 요청을 또 보내게 된다. 이떄, 서버 푸시 기능이 있는 HTTP/2는 html 파일을 요청받았을 때 html 파일과 필요한 추가 리소스들을 같이 보낸다.

**서버 푸시 기능을 통해 클라이언트의 요청을 최소화함으로써 불필요한 통신량을 미리 예방할 수 있게 되어 효율성이 증가하게 된다.** 

---

### References

https://shlee0882.tistory.com/107

https://hanamon.kr/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-http-http%EB%9E%80-%ED%8A%B9%EC%A7%95-%EB%AC%B4%EC%83%81%ED%83%9C-%EB%B9%84%EC%97%B0%EA%B2%B0%EC%84%B1/

https://velog.io/@wlwl99/HTTP%EC%9D%98-%ED%8A%B9%EC%A7%95

https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP

https://catsbi.oopy.io/5c0b482c-b427-4052-9030-d2be0810eeb6

https://velog.io/@cjh8746/HTTP-Keep-Alive-%EC%99%80-pipelining-%EA%B7%B8%EB%A6%AC%EA%B3%A0-Multiplexed-Streams%EC%9D%84-%EA%B3%B5%EB%B6%80%ED%95%98%EB%8B%A4%EA%B0%80-%EC%95%8C%EA%B2%8C%EB%90%9C-%EB%B2%84%EC%A0%84%EC%97%B4-HTTP0.9-2.0-%EC%A0%95%EB%A6%AC

https://velog.io/@codenmh0822/HTTP-1.1%EA%B3%BC-HTTP-2.0

https://seo-tory.tistory.com/82

https://web.dev/performance-http2/

https://youtu.be/xcrjamphIp4

https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-HTTP-20-%ED%86%B5%EC%8B%A0-%EA%B8%B0%EC%88%A0-%EC%9D%B4%EC%A0%9C%EB%8A%94-%ED%99%95%EC%8B%A4%ED%9E%88-%EC%9D%B4%ED%95%B4%ED%95%98%EC%9E%90#http_2.0_+_push_%ED%86%B5%EC%8B%A0_%EA%B3%BC%EC%A0%95