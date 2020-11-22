# REST API

- REST API 설명

  - REpresentational State Transfer API

  - HTTP(웹)의 장점을 최대한 활용할 수 있는 아키텍처

  - REST의 구성

    - 자원(Resource) - URI
    - 행위(Verb) - HTTP Method
    - 표현(Representations)

  - REST의 특징

    - Uniform Interface : URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일
    - Stateless : 무상태성, 작업을 위한 상태정보를 따로 저장하고 관리하지 않는다. 세션, 쿠키 정보를 별도로 관리하지 않기 때문에 API 서버는 단순히 요청에 대한 응답을 보내주기만 하면 된다. 즉, 서비스의 자유도가 높고 서버에서 불필요한 정보를 처리하지 않으므로 구현이 단순해진다.
    - Cacheable : HTTP라는 기존 웹 표준을 그대로 사용하기 때문에 웹에서 사용하는 기존 인프라를 활용할 수 있으며, 대표적으로 캐시 기능이 있다. HTTP 프로토콜 표준에서 사용하는 Last-Modified나 E-tag를 이용하면 캐싱 구현이 가능하다.
    - Self-descriptiveness(자체 표현 구조) : REST는 REST API 메시지만으로도 어떤 기능을 갖는지 쉽게 이해할 수 있는 자체 표현 구조다.
    - Client - Server 구조 : REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 서로 간의 의존성이 줄어든다.
    - 계층형 구조 : REST 서버는 다중 계층으로 구성될 수 있으며, 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있게 한다.

  - REST API 디자인 가이드

    - URI는 정보의 자원을 표현
    - 자원에 대한 행위(HTTP Method)를 표현

    ```http
    POST /articles/
    
    GET /articles/1
    
    DELETE / articles/2
    ```

    | **METHOD** |  **역할**   |
    | :--------: | :---------: |
    |    POST    | 리소스 생성 |
    |    GET     | 리소스 조회 |
    |    PUT     | 리소스 수정 |
    |   DELETE   | 리소스 삭제 |
