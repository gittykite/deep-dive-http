# 5장 웹서버

HTTP 완벽 가이드 책을 읽고 이해한 내용을 정리한 글입니다.

---

이 장에서 공부할 수 있는 내용은 아래와 같습니다.

- 여러 종류의 소프트웨어 및 하드웨어 웹 서버
- HTTP 통신을 진단해주는 간단한 웹 서버를 작성
- 어떻게 웹 서버가 HTTP 트랜잭션을 처리하는지 단계별 설명

## 5.1 다채로운 웹 서버

```웹 서버```는 리소스에 대한 HTTP 요청을 받아서 콘텐츠를 클라이언트에게 돌려줍니다.

### 5.1.1 웹 서버 구현

웹 서버는 HTTP 프로토콜을 구현하고, 웹 리소스를 관리하고, 웹 서버 관리 기능을 제공합니다. 운영체제는 컴퓨터 시스템의 하드웨어 관리, TCP/IP 네트워크 지원, 웹 리소스 유지하기 위한 파일 시스템, 연산 활동을 제어하기 위한 프로세스 관리를 제공합니다.

### 5.1.2 다목적 소프트웨어 웹 서버

```다목적 소프트웨어 웹 서버```는 네트워크에 연결된 표준 컴퓨터 시스템에서 동작합니다. 아파치나 오픈 소스 소프트웨어를 사용할 수도 있습니다.

### 5.1.3 임베디드 웹 서버

```임베디드 웹 서버```는 일반 소비자용 제품에 내장될 목적으로 만들어진 작은 웹 서버입니다.

임베디드 웹 서버는 사용자가 그들의 일반 소비자용 기기를 간편한 웹 브라우저 인터페이스로 관리할 수 있게 해준다.

## 5.2 진짜 웹 서버가 하는 일

1. 커넥션을 맺습니다. - 클라이언트의 접속을 받아들이거나, 원치 않는 클라이언트라면 닫습니다.
2. 요청을 받습니다. - HTTP 요청 메시지를 네트워크로부터 읽어 들입니다.
3. 요청을 처리합니다. - 요청 메시지를 해석하고 행동을 취합니다.
4. 리소스에 접근합니다. - 메시지에서 지정한 리소스에 접근합니다.
5. 응답을 만듭니다. - 올바른 헤더를 포함한 HTTP 응답 메시지를 생성합니다.
6. 응답을 보냅니다. - 응답을 클라이언트에게 돌려줍니다.
7. 트랜잭션을 로그로 남깁니다. - 로그파일에 트랜잭션 완료에 대한 기록을 남깁니다.

## 5.4 단계 1: 클라이언트 커넥션 수락

클라이언트가 이미 서버에 대해 열려있는 지속적 커넥션을 갖고 있다면, 클라이언트는 요청을 보내기 위해 그 커넥션을 사용할 수 있고 그렇지 않다면 서버에 대한 새 커넥션을 열 필요가 있습니다.

### 5.4.1 새 커넥션 다루기

1. 클라이언트 -> 웹 서버에 TCP 커넥션 요청
2. 웹 서버가 커넥션을 맺으면 TCP 커넥셔에서 IP 주소를 추출하여 클라이언트의 IP 주소나 호스트명이 인가되지 않았거나 악의적이라고 알려진 경우 닫을 수 있습니다.

### 5.4.2 클라이언트 호스트 명 식별

웹 서버는 역방향 DNS 를 사용해서 클라이언트의 IP 주소를 클라이언트의 호스트명으로 변환하도록 설정되어 있습니다. 웹 서버는 클라이언트 호스트 명을 구체적인 접근 제어와 로깅을 위해 사용할 수 있으며 웹 트랜잭션을 느려지게 할 수 있으므로 특정 콘텐츠에 대해서만 켜두거나 분석을 꺼두기도 합니다.

## 5.5 단계 2: 요청 메시지 수신

커넥션에 데이터가 도착하면, 웹 서버는 네트워크 커넥션에서 데이터를 읽어 파싱하여 요청 메시지를 구성합니다. 네트워크 커넥션은 언제든 무효화 될 수 있으므로 웹 서버는 데이터를 네트워크로부터 읽어 메시지 일부분을 메모리에 임시로 저장해 둘 필요가 있습니다.

**요청 메시지 파싱 할 때, 웹 서버가 하는 일**

1. 요청 메서드, URI, 버전 번호는 스페이스 한 개로 분리되어 있으며, CRLF (줄바꿈) 로 끝나는 요청줄을 파싱합니다.
2. CRLF 로 끝나는 메시지 헤더를 읽습니다.
3. CRLF 로 끝나는 빈 줄인 헤더의 끝을 찾습니다.
4. 요청 본문이 있다면, 읽고 Content-Length 로 길이를 정의합니다.


**메시지의 내부 표현**

요청 메시지에 대한 포인터와 길이를 자료 구조에 담고, 헤더는 속도가 빠른 룩업 테이블에 저장되어 각 필드에 신속하게 접근할 수 있습니다.


**커넥션 입력/출력 처리 아키텍처**

> 단일 스레드 웹 서버
    
한 번에 하나씩 요청을 처리하고, 트랜잭션이 완료되면 그 다음 커넥션이 처리됩니다.

> 멀티프로세스와 멀티스레드 웹 서버

여러 개의 프로세스 & 고효율 스레드를 할당하여 여러 요청을 동시에 처리합니다. 많은 멀티스레드 웹 서비스는 스레드/프로세스의 최대 개수에 대해 제한을 정합니다.

> 다중 I/O 서버

대량의 커넥션을 지원할 수 있는 아키텍쳐로 커넥션의 상태가 바뀌면 커녁션에 대한 처리가 수행되고, 완료되면 목록으로 다시 돌아가서 다른 커넥션들을 다시 확인합니다. 커넥션에 실제로 할 일이 있을 때만 수행하기 때문에 리소스를 낭비하지 않습니다.

> 다중 멀티스레드 웹 서버

CPU 여러 개의 이점을 살릴 수 있는 멀티스레딩 + 다중화 아키텍쳐입니다. 여러 개의 스레드는 열려있는 커넥션을 감시하고 커넥션에 대해 작업이 필요할 때만 수행합니다.


## 5.6 단계 3: 리소스의 매핑과 접근

웹 서버는 HTML, JPEG 같은 미리 만들어진 콘텐츠를 제공하며 서버 위에서 동작하는 애플리케이션을 통해 동적 콘텐츠도 제공합니다. 
클라이언트가 URI를 보내면 웹 서버는 알맞는 콘텐츠를 찾아서 콘텐츠의 원천을 식별하고 응답으로 보냅니다.

### 5.6.1 Docroot

웹 서버는 여러 종류의 리소스 매핑을 지원합니다. 그 중 가장 단순한 형태는 요청 URI를 웹 서버의 파일 시스템 안에 있는 파일 이름으로 사용하는 것으로 특별한 폴더인 ```docroot```를 웹 콘텐츠를 위해 예약 해둡니다.

예를 들어, 웹 서버가 /usr/local/httpd/files 라는 문서 루트를 가지고 있고 /specials/saw-blade.gif 에 대한 요청이 도착한다면 웹 서버는 ```/usr/local/httpd/files/specials/saw-blade.gif``` 파일을 반환합니다.

서버는 상대적인 URL 이 docroot를 벗어나서 파일 시스템의 docroot 이외 부분이 노출되는 일이 생기지 않도록 주의하므로 ```http://www.joes-hardware.com/../``` 형태로 요청하는 것을 허용하지 않는다.


### 5.7.2 디렉터리 목록

### 5.7.3 동적 콘텐츠 리소스 매핑

웹 서버는 URI 를 요청에 맞게 콘텐츠를 생성하는 프로그램을 통해 동적 리소스에 매핑할 수도 있습니다.

### 5.7.4 서버사이드 인클루드 (Server-Side Includes, SSI)

어떤 리소스가 서버사이드 인클루드를 포함하고 있는 것으로 설정되어 있다면, 서버는 그 리소스의 콘텐츠를 클라이언트에게 보내기 전에 처리합니다.

서버는 콘텐츠에 변수 이름이나 내장된 스크립트가 있는 어떤 특별한 패턴이 있는지 검사를 받고, 변수 값이나 실행 가능한 스크립트의 출력 값으로 치환됩니다.

### 5.7.5 접근 제어

웹 서버는 각각의 리소스에 접근 제어를 할당할 수 있습니다. 웹 서버는 클라이언트의 IP 주소에 근거하여 접근을 제어할 수 있고 혹은 리소스에 접근하기 위한 비밀번호를 물어볼 수도 있습니다.

## 5.8 단계 5: 응답 만들기

한번 서버가 리소스를 식별하면, 서버는 요청 메서드를 서술되는 동작을 수행한 뒤 응답 메시지를 반환합니다.

### 5.8.1 응답 엔티티

- 응답 본문의 MIME 타입을 서술하는 Content-Type 헤더
- 응답 본문의 길이를 서술하는 Content-Length 헤더
- 실제 응답 본문의 내용

### 5.8.2 MIME 타입 결정하기

**mime.types**

웹 서버는 MIME 타입을 나타내기 위해 파일 이름의 확장자를 사용할 수 있습니다. 웹 서버는 각 리소스의 MIME 타입을 계산하기 위해 확장자별 MIME 타입이 담겨 있는 파일을 탐색합니다.

**유형 명시(Explicit typing)**

특정 파일이나 디렉터리 안의 파일들이 파일 확장자나 내용에 상관없이 어떤 MIME 타입을 갖도록 웹 서버를 설정할 수 있습니다.

### 5.8.3 리다이렉션

웹 서버는 성공 메시지 대신 리다이렉션 응답을 반환합니다. 리다이렉션 응답 코드는 3XX 으로 Location 응답 헤더는 콘텐츠의 새로운 위치에 대한 URI 를 포함합니다.

- 영구히 리소스가 옮겨진 경우
    - 301 Moved Permanently 상태코드는 이런 종류의 리다이렉트를 위해 사용되고 웹 서버는 클라이언트에게 리소스의 이름이 바뀌었다고 알려줄 수 있습니다.
- 임시로 리소스가 옮겨진 경우
    - 리소스가 임시로 옮겨지는 경우, 303 See Other, 307 Temporary Redirect 상태 코드를 반환하여 임시로 새 위치로 이동하게끔 알려줍니다.
- URL 증강
    - 서버는 문맥 정보를 포함시키기 위해 재 작성된 URL로 리다이렉트합니다. 요청이 도착했을 때, 서버는 상태 정보를 내포한 새 UR을 생성하고 사용자를 새 URL 로 리다이렉트합니다.

- 부하 균형
    - 서버가 과부하되면 클라이언트를 덜 부하가 걸린 서버로 리다이렉트 할 수 있습니다.
- 디렉터리 이름 정규화
    - 클라이언트가 디렉터리 이름에 대한 URI 요청을 할 때, 끝에 ```/```를 빠뜨렸다면, 대부분의 웹 서버는 정상적으로 동작할 수 있도록 추가한 URI 로 리다이렉트합니다.

## 5.9 단계 6: 응답 보내기

서버는 커넥션 상태를 추적해야 하며 지속적인 커넥션은 특별히 주의해서 다룰 필요가 있습니다. 비지속적인 커넥션이라면, 서버는 모든 메시지를 전송했을 때 자신 쪽의 커넥션을 닫습니다.


## 5.10 단계 7: 로깅

트랜잭션이 완료되었을 때 웹 서버는 트랜잭션이 어떻게 수행되었는지에 대한 로그를 파일에 기록합니다.