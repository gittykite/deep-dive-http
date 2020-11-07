# 01. HTTP 개관

## 1.1 HTTP: 인터넷의 멀티미디어 배달부

* HTTP(Hypertext Transfer Protocol)는 웹의 구성요소들이 서로 대화할 때 사용하는 프로토콜이다.
* HTTP는 전 세계 웹 서버로부터 이미지, HTML 페이지, 텍스트 파일, 동영상, 음성 파일 등 대량의 정보를 사람들의 PC에 설치된 브라우저에 빠르고 간편하고 정확하게 전달한다.
* HTTP는 신뢰성 있는 데이터 전송 프로토콜을 사용하여 데이터 전송 중 손상되거나 꼬이지 않음을 보장하며, 인터넷 애플리케이션 개발자가 HTTP 통신이 전송 중 파괴, 중복, 왜곡될 걱정 없이 기능 구현에 집중할 수 있게 한다.


## 1.2 웹 클라이언트와 서버

* 웹 서버는 HTTP 프로토콜로 의사소통하기 때문에 보통 HTTP 서버라고 불린다.
* 웹 서버는 인터넷 데이터를 저장하고 있다가, 웹브라우저와 같은 HTTP 클라이언트가 HTTP 요청을 보내면 요청된 데이터를 HTTP 응답으로 돌려준다.
* HTTP 클라이언트와 HTTP 서버는 월드 와이드 웹의 기본 요소다.


## 1.3 리소스

* 웹 서버는 웹 콘텐츠의 원천인 웹 리소스를 관리하고 제공한다.
* 웹 리소스는 웹에 콘텐츠를 제공하는 모든 것을 말한다.
* 웹 서버 파일 시스템의 정적 파일부터 콘텐츠를 생산하는 동적 콘텐츠 리소스 모두 웹 리소스다.
  * 정적 파일: 텍스트 파일, HTML 파일, docx 파일, JPEG 이미지 파일, AVI 동영상 파일, 그 외 모든 파일
  * 동적 콘텐츠 리소스: 사용자가 누구인지, 몇 시인지에 따라 다른 콘텐츠를 생성하거나 카메라를 통해 라이브 영상을 보여주는 것, 주식 거래, 부동산 데이터베이스 검색, 온라인 쇼핑, 인터넷 검색엔진 등

### 1.3.1 미디어 타입

* HTTP 서버는 웹에서 전송되는 모든 HTTP 객체 각각에 MIME 타입이란 데이터 포맷 라벨을 붙인다.
  * MIME(Multipurpose Internet Extensions, 다목적 인터넷 메일 확장)은 전자메일 시스템 사이에서 메시지가 오갈 때의 문제를 해결하기 위해 설계되었고, HTTP에서도 멀티미디어 콘텐츠를 기술하고 라벨을 붙이기 위해 채택되었다.
* 웹 서버는 데이터 콘텐츠와 함께 MIME 타입을 보내주고, 웹브라우저는 이 MIME 타입을 통해 다룰 수 있는 객체인지 확인한다.
* MIME 타입은 사선(/)으로 구분된 주 타입 Primary Object Type과 부 타입 Specific Subtype으로 이루어진 문자열 라벨이다.
  * `text/html` : HTML로 작성된 텍스트 문서
  * `text/plain` : plain ASCII 텍스트 문서
  * `image/jpeg` : JPEG 이미지
  * `image/gif` : GIF 이미지
  * `video/quicktime` : 애플 퀵타임 동영상
  * `application/vnd.ms-powerpoint` : 마이크로소프트 파워포인트 프레젠테이션


### 1.3.2 URI

* URI(Uniform resource indentifier, 통합 자원 식별자)는 서버 리소스 이름이다.
  * URI로 정보 리소스를 고유하게 식별하고 위치를 지정할 수 있으며, 클라이언트는 이를 지목할 수 있다.
* URI에는 URL과 URN 두 가지가 있다.

### 1.3.3 URL

* URL(Uniform resource locator, 통합 자원 지시자)은 리소스 식별자의 가장 흔한 형태로, 특정 서버의 한 리소스에 대한 구체적 위치를 서술한다.
  * 오늘날 대부분의 URI는 URL이다.
* `http://`<sup>1</sup> `yeseulo.kr`<sup>2</sup> `/projects/deep-dive-http.gif`<sup>3</sup>
  1) 스킴 Scheme: 리소스 접근을 위해 사용되는 프로토콜을 서술, 보통 HTTP 프로토콜(http://)
  2) 서버의 인터넷 주소: yeseulo.kr로 이동하라
  3) 웹 서버의 리소스 가리킴: /projects/deep-dive-http.md 라고 불리는 리소스를 가져와라

### 1.3.4 URN

* URN(Uniform resource name, 통합 자원 이름)은 콘텐츠를 이루는 한 리소스에 대해, 그 리소스의 위치에 영향을 받지 않는 유일무이한 이름 역할을 한다.
  * ex. `urn:ietf:rfc:2141`
* 리소스의 위치를 옮기거나 여러 군데 복사하더라도 그를 지칭하는 URN은 문제없이 동작한다.
* 단, URN은 실험 중인 상태로 널리 채택되지 않았다.


## 1.4 트랜잭션

* HTTP 트랜잭션은 (클라이언트에서 서버로 보내는) 요청 명령과 (서버가 클라이언트에게 돌려주는) 응답 결과로 구성된다.
  * 이 상호작용은 HTTP 메시지라고 불리는 정형화된 데이터 덩어리를 이용해 이루어진다.
  * HTTP 요청 메시지는 명령과 URI를 포함하고,
    ```
      GET /projects/deep-dive-http.gif HTTP/1.0
      Host: yeseulo.kr
    ```
  * HTTP 응답 메시지는 트랜잭션 결과를 포함한다.
    ```
      HTTP/1.0 200 OK
      Content-type: image/gif
      COntent-length: 8572
    ```

### 1.4.1 메서드

* 메서드는 서버에게 어떤 동작이 취해져야 하는지 말해준다: 웹페이지 가져오기, 파일 삭제하기 등
* HTTP는 HTTP 메서드라고 불리는 여러 요청 명령을 지원하며, 모든 HTTP 요청 메시지는 한 개의 메서드를 갖는다.
* 주요 메서드
  * `GET` : 서버에서 클라이언트로 지정한 리소스를 보내라.
  * `PUT` : 클라이언트에서 서버로 보낸 데이터를 지정한 이름의 리소스로 저장하라.
  * `DELETE` : 지정한 리소스를 서버에서 삭제하라.
  * `POST` : 클라이언트 데이터를 서버 게이트웨이 애플리케이션으로 보내라.
  * `HEAD` : 지정한 리소스에 대한 응답에서 HTTP 헤더 부분만 보내라.

### 1.4.2 상태 코드

* 상태 코드는 요청이 성공했는지/추가 조치가 필요한지 클라이언트에게 알려주는 세 자리 숫자다. 모든 HTTP 응답 메시지는 상태 코드와 함께 반환된다.
* 주요 상태 코드
  * `200` : 좋다. 문서가 올바르게 반환되었다.
  * `302` : 다시 보내라. 다른 곳에 가서 리소스를 가져가라.
  * `404` : 없다. 리소스를 찾을 수 없다.
* HTTP는 숫자 상태와 함께 텍스트로 된 '사유 구절 Reason Phrase'을 보낸다. 설명을 위한 것으로 실제 응답 처리에는 숫자 코드가 사용된다.

### 1.4.3 웹페이지는 여러 객체로 이루어질 수 있다

* 애플리케이션은 보통 하나의 작업 수행을 위해 여러 HTTP 트랜잭션을 수행한다.
  * 페이지 레이아웃을 서술하는 HTML 뼈대를 한 번의 트랜잭션으로 가져오고, 첨부된 이미지/그래픽 조각/자바 애플릿 등을 가져오기 위해 추가로 HTTP 트랜잭션을 수행한다.


## 1.5 메시지

* HTTP 메시지는 단순한 줄 단위의 텍스트 문자열로 사람이 읽고 쓰기 쉽다.
* HTTP 메시지에는 클라이언트 > 서버 의 요청 메시지, 서버 > 클라이언트 의 응답 메시지 두 가지가 있다.
* 메시지의 구성
  * 시작줄: 메시지 첫 줄. 요청이라면 무슨 일을 해야하는지, 응답이라면 무슨 일이 일어났는지 나타낸다.
  * 헤더: 시작줄 다음 0개 이상의 해더 필드가 이어진다. 각 헤더 필드는 구문 분석을 쉽게 하기 위해 쌍점(:)으로 구분된 하나의 이름-값 쌍으로 구성된다. 헤더 필드를 추가하려면 한 줄을 더하면 된다. 헤더는 빈 줄로 끝난다.
  * 본문: 헤더 빈 줄 다음. 필요에 따라 본문이 있을 수도 없을 수도 있다. 요청의 본문은 웹 서버로 데이터를 실어 보내고, 응답의 본문은 클라이언트로 데이터를 반환한다. 시작줄이나 헤더와 달리 이진 데이터를 포함할 수도 있다.


## 1.6 TCP 커넥션

* TCP(Transmission Control Protocol, 전송 제어 프로토콜) 커넥션을 통해 메시지가 한 곳에서 다른 곳으로 옮겨간다.

### 1.6.1 TCP/IP

* HTTP는 애플리케이션 계층 프로토콜이다. 네트워크 통신의 핵심적인 세부사항을 TCP/IP에 맡긴다.
* TCP는 대중적이고 신뢰할 수 있는 인터넷 전송 프로토콜로 다음을 제공한다.
  * 오류없는 데이터 전송
  * 순서에 맞는 전달(데이터는 언제나 보낸 순서대로 도착)
  * 조각나지 않는 데이터 스트림(언제든 어떤 크기로든 보낼 수 있음)
* TCP/IP는 TCP와 IP가 층을 이루는, 패킷 교환 네트워크 프로토콜의 집합이다. 각 네트워크, 하드웨어 특성을 숨기고 어떤 컴퓨터나 네트워크든 서로 신뢰성 있는 소통을 하게 한다.
* HTTP 네트워크 프로토콜 스택
  ```
    HTTP                    --- 애플리케이션 계층
    TCP                     --- 전송 계층
    IP                      --- 네트워크 계층
    네트워크를 위한 링크 인터페이스 --- 데이터 링크 계층
    물리적인 네트워크 하드웨어     --- 물리 계층
  ```

### 1.6.2 접속, IP 주소 그리고 포트번호

* 인터넷 프로토콜(Internet Protocol, IP) 주소와 포트번호를 사용해 클라이언트와 서버 사이에 TCP/IP 커넥션을 맺어야 HTTP 클라이언트가 서버에 메시지를 전송할 수 있다.
* TCP에서는 서버 컴퓨터에 대한 IP 주소와 그 서버에서 실행 중인 프로그램이 사용하는 포트번호가 필요하다.
  * URL을 통해 IP 주소와 포트번호를 알 수 있다.
  * `http://207.200.83.29:80/index.html`
  * yeseulo.kr 과 같은 도메인 이름 혹은 호스트 명은 이해하기 쉬운 형태의 IP 주소의 별명으로, DNS(Domain Name Service)를 통해 쉽게 IP로 변환될 수 있다.
  * HTTP URL에 포트번호가 생략된 경우 기본값 80이라고 보면 된다.
* 웹 브라우저가 HTTP를 이용해 리소스를 가져오는 순서
  1. 웹브라우저는 서버의 URL에서 호스트 명을 추출한다.
  2. 웹브라우저는 서버의 호스트 명을 IP로 변환한다.
  3. 웹브라우저는 URL에서 포트번호를 추출한다. (있다면)
  4. 웹브라우저는 웹 서버와 TCP 커넥션을 맺는다.
  5. 웹브라우저는 서버에 HTTP 요청을 보낸다.
  6. 서버는 웹브라우저에 HTTP 응답을 돌려준다.
  7. 커넥션이 닫히면 웹브라우저는 문서를 보여준다.

### 1.6.3 텔넷 Telnet을 이용한 실제 예제

```bash
  telnet yeseulo.kr 80
  ...
  GET /index.html HTTP/1.1
  Host: yeseulo.kr

  ...

  HTTP/1.1 200 OK
```


## 1.7 프로토콜 버전

* 오늘날 HTTP 프로토콜 버전은 여러 가지다.
* HTTP/0.9
  * 1991년의 HTTP 프로토타입. 간단한 HTML 객체를 받아오기 위해 만들어진 것으로, 심각한 디자인 결함이 다수 있고 구식 클라이언트하고만 사용할 수 있다.
  * GET 메서드만 지원하고 멀티미디어 콘텐츠에 대한 MIME 타입이나 HTTP 헤더, 버전 번호는 지원하지 않는다.
  * HTTP/1.0으로 금방 대체되었다.
* HTTP/1.0
  * 처음 널리 쓰이기 시작한 HTTP 버전이다.
  * 버전 번호, HTTP 헤더, 추가 메서드, 멀티미디어 객체 처리를 추가했다.
  * 시각적으로 매력적인 웹페이지와 상호작용하는 폼을 실현했고 월드 와이드 웹을 대세로 만들었다.
  * 그러나 결코 잘 정의된 명세가 아니며, HTTP가 상업적/학술적으로 급성장하던 시기에 만들어진, 잘 동작하는 용례 모음에 가깝다.
* HTTP/1.0+
  * 1990년대 중반, 월드 와이드 웹이 성공하면서 HTTP에 기능을 추가했다.
  * 오래 지속되는 `keep-alive` 커넥션, 가상 호스팅 지원, 프락시 연결 지원 등 많은 기능이 사실상 표준으로 추가되었고, 이 규격 외의 확장된 HTTP 버전을 흔히 HTTP/1.0+ 라고 부른다.
* HTTP/1.1
  * HTTP 설계의 구조적 결함 교정과 성능 최적화, 잘못된 기능 제거에 집중한 버전으로, 더 복잡해진 웹 애플리케이션과 배포를 지원한다.
  * 현재의 HTTP 버전이다.
* HTTP/2.0
  * HTTP/1.1 성능 문제를 개선하기 위해 구글의 SPDY 프로토콜을 기반으로 설계가 진행 중인 버전이다.


## 1.8 웹의 구성 요소

### 1.8.1 프락시

* 클라이언트와 서버 사이에 위치한 HTTP 중개자로, 웹 보안과 애플리케이션 통합, 성능 최적화를 위한 중요 구성 요소이다.
* 클라이언트의 모든 HTTP 요청을 받아 (대개 요청을 수정하여) 서버에 전달한다.
* 사용자를 대신해서 서버에 접근하며 주로 보안을 위해 사용된다.
* 다운로드할 때 바이러스를 검출하거나 성인 콘텐츠를 차단하는 등 요청과 응답을 필터링한다.

### 1.8.2 캐시

* 웹캐시와 캐시 프락시는 자주 찾는 웹페이지의 사본을 저장해두는 HTTP 프락시 서버다.
* 클라이언트는 멀리 떨어진 웹 서버 보다 근처의 캐시에서 더 빨리 문서를 다운로드 할 수 있다.
* HTTP는 캐시의 효율적인 동작과 캐시된 콘텐츠의 최신 버전 유지, 프라이버시 보호를 위한 많은 기능을 정의한다.

### 1.8.3 게이트웨이

* 게이트웨이는 다른 서버들의 중개 서버로, 주로 HTTP 트래픽을 다른 프로토콜로 변환하기 위해 사용된다.
* 언제나 스스로가 리소스를 갖고 있는 진짜 서버인 것 처럼 요청을 다뤄서, 클라이언트는 게이트웨이와 통신하고 있음을 알아채지 못할 것이다.
* HTTP/FTP 게이트위이는 FTP URI에 대한 요청을 받아들인 뒤 FTP 프로토콜을 이용해 문서를 가져오고, 가져온 문서는 HTTP 메시지에 담겨 클라이언트에게 보낸다.

### 1.8.4 터널

* 두 커넥션 사이에서 raw 데이터를 열어보지 않고 그대로 전달해주는, 단순히 HTTP 통신을 전달하기만 하는 특별한 프락시다.
* HTTP 터널은 주로 비 HTTP 데이터를 하나 이상의 HTTP 연결을 통해 그대로 전송해주기 위해 사용한다.
* ex. 암호화된 SSL 트래픽을 HTTP 커넥션으로 전송하여 웹 트래픽만 허용하는 사내 방화벽을 통과시킬 때 터널 활용
  * HTTP/SSL 터널은 HTTP 요청을 받아 목적지의 주소와 포트번호로 커넥션을 맺고, 이후부터 암호화된 SSL 트래픽을 HTTP 채널을 통해 목적지 서버로 전송할 수 있게 한다.

### 1.8.5 에이전트

* (사용자) 에이전트는 사용자를 위해 HTTP 요청을 만들어주는 클라이언트 프로그램으로 웹 요청을 만드는 애플리케이션은 모두 HTTP 에이전트다.
* 기본적으로 웹브라우저 뿐 아니라, '스파이더'나 '웹로봇'과 같이 사람의 통제 없이 스스로 웹을 돌아다니며 HTTP 트랜잭션을 일으키고 콘텐츠를 받아오는 자동화된 사용자 에이전트도 있다.


## 추가 정보

* [W3C HTTP - Hypertext Transfer Protocol](http://www.w3.org/Protocols)
* [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt)
* [W3C Why a new protocol?](https://www.w3.org/Protocols/WhyHTTP.html)
* [W3C A Little History of the World Wide Web](https://www.w3.org/History.html)