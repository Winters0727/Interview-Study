# HTTP

- HTTP 설명

  - HyperText Transfer Protocol
  - 인터넷 상에서 데이터를 주고 받기 위한 서버/클라이언트 모델을 따르는 표준 프로토콜
  - 애플리케이션 레벨의 프로토콜로 TCP/IP위에서 작동(표준 포트 80번)
  - 작동 방식
    - 클라이언트 : chrome, firefox와 같은 클라이언트 소프트웨어를 통해 URI를 이용해서 서버에 접속하고, 데이터를 요청
    - 서버 : Apache, nginx와 같은 서버 소프트웨어를 통해 클라이언트의 요청을 받아 요청에 대한 응답을 보냄
  - Connectless & Stateless
    - 요청 시에 서버에 연결하고 응답을 받으면 연결을 해제한다.(Connectless)
      - 하지만, 동일한 클라이언트로부터의 많은 요청을 처리하기 어려워 HTTP 1.1부터는 Keep-Alive를 도입하여 연결 정보를 재활용한다.
    - 접속 유지를 최소화하여 더 많은 요청을 처리할 수 있으므로 불특정 다수를 대상으로 하는 서비스에 적합한 방식이다.
    - 연결이 끊긴 이후에는 클라이언트의 이전 상태를 알 수 없다.(Stateless)
    - 이러한 문제를 해결하기 위해 나온 개념이 Cookie와 Session이다.
- HTTP 캐시 설명

  - 클라이언트에서 요청이 들어왔을 때 정적 자원들을 복사 및 저장해놓고, 동일한 URI로 요청이 들어왔을 때 저장해놓은 정적 자원들을 활용하여 웹 페이지의 로딩시간을 단축시키고 트래픽의 사용량을 줄인다.
  - HTTP 캐시의 종류
    - Browser Cache
    - Proxy Cache
    - Gateway Cache
  - HTTP 캐시 작동 원리
    - URI로 요청을 보내면 웹 브라우저가 하드 디스크에 정적 자원들의 유무를 참조
    - 없을 경우, 캐시 미스(cache miss)를 발생시키고 캐시를 하드 디스크에 저장
    - 있을 경우, 캐시 히트(cache hit)를 발생시키고 캐시 미스가 발생할 때까지 버퍼에서 캐시를 제공
- 캐시와 세션의 차이

  - 캐시 : 웹 페이지 로딩에 필요한 정적 자원들을 저장하여 **웹 페이지 로딩 속도를 단축**.
  - 세션 : 서버에 필요한 정보를 저장하여 
- 쿠키와 세션이 저장되는곳

  - 쿠키와 세션의 차이점은 **상태 정보의 저장 위치**
    - 쿠키는 클라이언트(=로컬, 사용자 브라우저)에 저장(Key-Value pair)
      - 쿠키에는 이름, 값, 만료 날짜/시간, 경로 정보 등이 있다.
    - 세션은 서버에 저장
      - 클라이언트의 정보를 서버에 두고, 세션 아이디로 인증하여 정보를 이용하는 방식
- 쿠키와 세션
  - 쿠키는 클라이언트의 브라우저 또는 파일로 저장되므로 탈취 및 변조의 위험이 있다. 그에 반해 세션은 서버에 저장되어 있고 인증과정을 거치므로 쿠키에 비해 보안적으로 안전하다.
  - 쿠키는 크기에 제한이 있고, 세션은 크기에 제한이 없다.
  - 세션 ID의 전달에 주로 쿠키를 사용한다. 이로 인해 세션 ID가 탈취 및 변조될 위험이 있다.
  - 세션의 사용은 서버의 자원을 사용하므로 필요에따라 쿠키를 사용한다.
- HTTP 메소드 설명

  - GET, POST, PUT, DELETE...
  - GET은 Request Header에 저장되며, 크기에 제한이 있다. 또한, 동일한 요청에 대해 동일한 응답이 온다.
  - POST는 Request Body에 저장되므로 보안적으로 Header에 저장되는 GET보다 안전하며, 크기에 제한이 없다. 또한, 동일하게 요청을 보내도 다른 응답이 올 수 있다.
- HTTP Keep-Alive

  - HTTP는 Connectless 특징을 가지므로 요청 시에 연결하고 응답 후에 연결을 해제하는 과정을 거친다. 또한, Stateless 특징을 연결을 해제한 이후의 클라이언트의 상태를 알 수 없다.
  - 이는 동일한 클라이언트로부터 많은 요청이 왔을 때 비효율적이며, 이를 해결하기 위해 HTTP 1.1부터 Keep-Alive가 도입되었다.
  - Keep-Alive는 동일한 클라이언트로부터 많은 요청이 들어왔을 때 TCP 연결 정보를 재사용하여 모든 요청이 끝날 때 까지 연결을 유지하는 기능이다.
- css와 자바스크립트를 업데이트 후 배포했는데, 바뀌지 않았는다.왜그럴까?
  - 클라이언트의 하드 디스크(브라우저)에 동일한 URL에 대한 캐시를 가지고 있어 바뀌지 않은 것이다.
  - 클라이언트 사이드에서는 기존에 저장된 캐시를 삭제한 후, 새로고침하여 업데이트 된 CSS/JS 파일을 받으면 된다.
  - 서버 사이드에서는 파일명을 수정하거나 쿼리를 추가하여 브라우저가 다른 URL로 인식하게 만들어 캐시된 파일을 사용하는 것을 방지하면 된다.
- HTTP 1.1과 HTTP 2.0
  - HTTP 1.1의 문제점
    - HOL(Head of Line) Blocking (특정 응답 지연)
      - 사양성의 제한으로 클라이언트의 리퀘스트의 순서와 서버의 응답순서는 동기화해야된다.
    - RTT(Round Trip Time) 증가 (양방향 지연)
      - 전송하는 데이터의 양에 따라 TCP 연결 개수를 증가시켜야하기 때문
    - Header의 크기가 크다. (Cookie)
      - HTTP 1.1에는 많은 메타 정보들을 Header에 저장
      - 자주 방문하는 웹페이지에 다수의 HTTP 요청을 보낼 때, 요청시마다 중복된 헤더 값을 전송
  - HTTP 2.0 개선점
    - Multiplexed Streams : 한번의 TCP 연결을 통해 여러번 데이터 전송 가능
    - Stream Prioritization : 요청의 중요도에 따라 우선순위를 부여
    - Header Compression : Header 정보를 HPACK 압축 방식으로 압축 전송
    - Server Push : HTML 문서에 필요한 리소스를 클라이언트의 요청 없이도 전송 + 캐시 개념 도입
    - Binary Protocol : 텍스트 명령을 네트워크로 전송하는 과정에서 바이너리로 변환