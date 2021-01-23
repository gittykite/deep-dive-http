## 14.1 HTTP를 안전하게 만들기

- 사용자는 웹 트랜잭션을 중요한 일에 사용하므로, 안전한 방식의 HTTP를 필요로 함
- 우리는 다음을 제공해줄 수 있는 HTTP 보안 기술이 필요
    - **서버 인증** - 클라이언트가 위조 서버가 아닌 진짜와 통신 중임을 알 수 있어야 함
    - **클라이언트 인증** - 서버가 가짜가 아닌 진짜 사용자와 통신 중임을 알 수 있어야 함
    - **무결성** - 클라이언트와 서버는 그들의 데이터가 위조 되는 것으로부터 안전해야 함
    - **암호화** - 클라이언트와 서버는 도청에 대한 걱정없이 통신할 수 있어야 함
    - **효율** - 저렴한 클라이언트/서버도 이용가능하도록 알고리즘이 충분히 빨라야 함
    - **편재성(Ubiquity)** - 프로토콜은 거의 모든 클라이언트와 서버에서 지원되어야 함
    - **관리상 확장성** - 누구든지 어디서든 즉각적인 보안 통신을 할 수 있어야 함
    - **적응성** - 현재 알려진 최선의 보안 방법을 지원해야 함
    - **사회적 생존성** - 사회의 문화, 정치적 요구를 만족시켜야 함

### 14.1.1 HTTPS

- HTTPS는 HTTP 를 안전하게 만드는 방식 중에 가장 인기 있는 것으로, 넷스케이프 커뮤니티 주식회사(Netscape Communications Corporation)에서 만들어, 모든 주류 브라우저/서버에서 지원 중
- HTTPS로 접근 여부는 url이 `https://` 로 시작하는 것으로 알 수 있음. (몇 브라우저는 보안 아이콘도 같이 제공)
- HTTPS를 사용하면, 모든 HTTP 요청/응답 데이터는 네트워크로 보내지기 전에 암호화 됨
    - HTTPS는 하부에 전송 레벨 암호 보안 계층을 제공함으로써 동작
    - 이 보안 계층은 안전 소켓 계층(Secure Sockets Layer, SSL) 또는 전송 계층 보안(Transport Layer Security, TLS)을 이용하여 구현
    - SSL과 TLS는 매우 비슷하므로, 엄밀하진 않지만 통칭하여 'SSL'이라고 부름

- 어려운 인코딩/디코딩 작업은 대부분 SSL 라이브러리 안에서 일어나므로, 보안 HTTP를 사용하기위해 웹 클라이언트와 서버가 프로토콜을 처리하는 로직을 변경할 필요는 없음
    - 대부분, TCP 입력/출력 호출을 SSL 호출로 대체하고, 보안 정보 설정, 관리를 위한 몇가지 호출을 추가하면 됨

## 14.2 디지털 암호학

- HTTPS에 대한 배경지식으로 디지털 암호학의 중요 내용을 간략히 설명
- **암호**: 텍스트를 아무나 읽지 못하도록 인코딩하는 알고리즘
- **키**: 암호의 동작을 변경하는 숫자로 된 매개 변수
- **대칭키 암호 체계**: 인코딩/디코딩에 같은 키를 사용하는 알고리즘
- **비대칭키 암호 체계**: 인코딩/디코딩에 다른 키를 사용하는 알고리즘
- **공개키 암호법**: 비밀 메시지를 전달하는 수백만 대의 컴퓨터를 쉽게 만들 수 있는 시스템
- **디지털 서명**: 메시지가 위변조 되지 않았음을 입증하는 체크섬
- **디지털 인증서**: 신뢰할 만한 조직에 의해 서명되고 검증된 신원 확인 정보

### 14.2.1 비밀 코드의 기술과 과학

- 암호법(cryptography): 메시지 인코딩/디코딩에 대한 과학이자 기술
    - 단순 암호화 뿐 아니라, 메시지 위변조 방지에도 사용 가능

### 14.2.2 암호(cipher)

- 암호: 시지를 인코딩하는 특정한 방법과, 디코딩하는 방법.
    - 인코딩 전의 원본 메세지: 텍스트 혹은 평문(plaintext, cleartext)
    - 암호가 적용된 메시지: 암호문(ciphertext)
- 수 천년간 암호는 비밀 메시지를 만들기위해 사용되어왔으며, 율리우스 카이사르는 [카이사르 암호](https://ko.wikipedia.org/wiki/%EC%B9%B4%EC%9D%B4%EC%82%AC%EB%A5%B4_%EC%95%94%ED%98%B8)를 사용했음

### 14.2.3 암호 기계

- 암호는 사람이 직접 인코딩/디코딩을 했어야 해서, 상대적으로 간단한 알고리즘으로 시작함
- 기술이 진보하면서 사람들은 보다 복잡한 암호로 인코딩/디코딩하는 기계를 만들기 시작함
    - 가장 유명한 기계식 암호 기계는 제 2차 세계대전당시 독일의 에니그마 암호 기계

### 14.2.4 키가 있는 암호

- **키**: 암호의 동작방식을 변경할 수 있는 다이얼. 누군가 기계를 훔치더라도, 올바른 키 없이는 디코더가 동작하지 않음
    - 예: N번-회전(rotate-by-N) 암호. 같은 입력 메시지도, 키의 값에 따라 다른 출력을 생성

### 14.2.5 디지털 암호

- 디지털 계산의 도래로 두가지 주요한 발전이 있었음
    - 속도, 기능에 대한 기계 장치의 한계에 벗어나, 복잡한 인코딩/디코딩 알고리즘이 가능해 짐
    - 키를 매우 큰 값으로 지원하는 것이 가능해져서, 단일 암호 알고리즘으로 키 값을 달리해 수 조개 이상의 가상 암호 알고리즘을 만들 수 있음. 키가 길수록 인코딩의 조합이 많아져, 키 무작위 추측에 의한 크래킹이 어려워짐
- **디지털 키**: 물리적인 금속 키, 다이얼 설정과 달리 숫자 값으로, 키 값은 인코딩/디코딩 알고리즘에 대한 입력값
- **코딩 알고리즘**: 데이터를 받아 알고리즘, 키 값에 근거하여 인코딩/디코딩하는 함수

## 14.3 대칭키 암호법

- **대칭키**: 인코딩과 디코딩용 키가 같음.
- 발송자/수신자는 모두 동일한 비밀 키 k를 공유하고, 발송자는 해당 키로 암호화하고, 수신자는 같은 키로 평문으로 복원
- 대칭키 암호 알고리즘의 예: DES, Triple-DES, RC2, RC4

### 14.3.1 키 길이와 열거 공격(Enumeration Attack)

- 비밀 키가 알려 지지 않는 게 중요. 대부분 인코딩/디코딩 알고리즘은 공개적으로 알려져있고, 키만 비밀임.
- 열거 공격(Enumeration Attack): 가능한 모든 키 값을 대입해보는 공격
- 좋은 알고리즘은 코드 크래킹을 하려면 열거 공격밖에 없게 만듦.
    - 가능한 키 값이 별로 없다면 열거 공격으로 암호를 깰 수 있음.
- 키 값의 갯수는 키가 몇 비트이며, 얼마나 많은 키가 유효한지(예: RSA는 소수만 키 가능)에 달려있음
    - 대칭키는 보통 모든 값이 유효하므로 비트에 달려있음. 예: 8bit = 2^8 = 256가지 키 값
    - 40비트면 작고 중요하지 않은 업무에 충분. 워크스테이션이 빠르다면 깨질 수 있음.
    - 128비트면 강력한 것으로 간주.
- 긴 키를 사용하는 암호화 소프트웨어는 NSA(미국 국가 안보국)에 의해 수출을 통제

### 14.3.2 공유키 발급하기

- 대칭키 암호의 단점 중 하나는, 발송자/수신자가 둘 다 공유 키를 가져야 한다는 것.
- 만약 N개의 노드가 있고, 각 노드가 상대 N-1 과 은밀하게 대화를 나누어야 한다면, N^2개의 비밀 키가 필요
    - 너무 많은 키로 인해 관리상에 어려움이 있음

## 14.4 공개키 암호법

- 공개키 암호 방식: 두 개의 비대칭 키 사용
    - 각각 호스트 메시지 인코딩용(공개키)/디코딩용(개인키)
- 인코딩 키는 모두를 위해 공개 되어있고(공개키라는 명칭이 붙은 이유), 호스트만 개인 디코딩 키를 알고 있음
    - 대칭 키처럼 키의 쌍이 폭발적으로 증가하는 것을 피할 수 있음
- 공개키 암호화 기술은 보안 프로토콜을 전 세계 모든 컴퓨터 사용자에게 적용하는 것을 가능케 함

### 14.4.1 RSA

- 공개키 비대칭 암호는 아래의 내용을 알더라도, 개인 키를 계산 할 수 없다는 것
    - 공개키(공개이므로 누구나 얻을 수 있음)
    - 가로채서 얻은 암호문의 일부(네트워크 스누핑으로 획득)
    - 메시지와 그것을 암호화한 암호문(인코더에 텍스트를 넣고 실행해서 획득)
- 이 요구를 만족하는 공개키 암호 체계 중 RSA 알고리즘이 유명.
    - 공개키, 평문 일부, 평문에 대한 암호문, RSA 구현 소스 코드까지 알아도, 암호를 크래킹하여 개인키를 찾아내는 것은 어려움

### 14.4.2 혼성 암호 체계와 세션 키

- 비대칭 공개키 암호 방식은, 누구나 공캐키만 알면 안전하게 메시지를 보낼 수 있고, 개인키에 대한 협상을 할 필요가 없음
- 대신 계산이 느린경향이있어, 실제로는 대칭과 비대칭을 섞은 것을 사용함
- 안전한 소통 채널 수립에는 공개 키를 사용하고, 만들어진 채널을 통해 무작위 대칭키를 생성하여 교환 후 나머지 데이터를 암호화 할때는 빠른 대칭키를 사용하는 방식이 흔히 쓰임

## 14.5 디지털 서명

- 암호 체계는 암호화와 해독 뿐 아니라, 누가 메시지를 썼으며 그 메시지가 위조되지 않음을 증명하기위해 서명하는데에 이용할 수 있음
- 디지털 서명(digital signing)이라고 불리며, 인터넷 보안 인증서에게 중요

### 14.5.1 서명은 암호 체크섬이다

- 디지털 서명은 메시지에 붙은 특별한 암호 체크섬.
    - **메시지 저자 입증**: 저자는 저자의 극비 개인키를 갖고 있어, 저자만이 체크섬을 계산할 수 있음
        - 개인 키가 훔쳐진 것이 아님을 가정하며, 대부분 개인키는 시간이 지나면 만료됨. 훔치거나 훼손된 키를 추적하기위한 '폐기 목록 '도 존재
    - **메시지 위조 방지**: 악의적인 공격자가 송신 중 메시지를 수정하면, 체크섬이 메시지와 다르게 됨. 체크섬은 저자의 비밀 개인키와 관련 되어있어, 위조된 메시지에 대한 올바른 체크섬을 날조할 수 없음
- 디지털 서명은 보통 비대칭 공개키에 의해 생성
    - 개인 키는 오직 소유자만이 알고 있으므로, 일종의 지문 처럼 사용
- 메시지 전송과 서명 진행 방식
    1. 노드 A가 가변 길이의 메시지를 정제하고, 고정된 길이의 요약(digest)로 만듦
    2. 노드 A는 그 요약에, 사용자의 개인키를 매개 변수로 하는 '서명' 함수 적용. 사용자만 개인키를 알고 있으므로, 올바를 서명 함수는 서명자가 소유자임을 의미.
    3. 한번 서명이 계산 되면, 노드A는 그것을 메시지 끝에 붙이고, 메세지와 그에 대한 서명 둘다 노드 B에 전송
    4. 메시지를 받은 노드 B가, 메시지가 저자 입증과 위조 확인을 원하면 서명 검사를 할 수 있음. 노드 B는 서명에 공개 키를 이용한 역함수를 적용함. 풀어낸 요약과 노드B가 갖고 있는 버전의 요약이 일치하지 않았다면, 위조가 되었거나 발송자가 개인키를 갖고 있지 않은 것임(=저자가 아님).

## 14.6 디지털 인증서

- 디지털 인증서(digital certificates, certs): 신뢰할 수 있는 기관으로 부터 보증받은 사용자/회사에 대한 정보를 담음
- 현실 세계에는 많은 형태의 신원 증명이 있지만, 위조하기 어려울수록 높은 신뢰를 받으며, 위조하기 쉬울수록 정보를 덜 신뢰함

### 14.6.1 인증서의 내부

- 인증서 내부에는 '인증 기관'에 의해 디지털 서명된 정보의 집합을 포함
    - 대상의 이름(서버, 사람, 조직 등)
    - 유효기간
    - 인증서 발급자(누가 인증서를 보증하는지)
    - 인증서 발급자의 디지털 서명
    - 추가적으로, 디지털 인증서에 대상과 사용된 서명 알고리즘에 대한 서술적인 정보, 대상의 공개 키를 포함
- 누구나 디지털 인증서를 만들 수 있지만, 모두가 인증서의 정보를 보증하고 인증서를 개인 키로 서명할 수 있는 널리 인정받는 서명 권한을 얻을 수 있지 않음

### 14.6.2  X.509 v3 인증서

- 디지털 인증서에 대한 전 세계적인 단일 표준은 없지만, 대부분의 인증서는 X.509라 불리는 표준화된 서식에 저장하고 있음.
    - X.509 v3 인증서는 인증 정보를 파싱 가능한 필드에 넣어 구조화하는 표준화된 방법 제공
    - 다른 종류의 인증서는 다른 필드값을 갖지만, 대부분 X.509 v3 구조를 따름
- X.509 인증서의 필드
    - **버전**: 이 인증서가 따르는 X.509 인증서의 버전 번호. 요즘은 보통 버전 3
    - **일련번호**: 인증기관에 의해 생성된 고유 정수. CA로부터의 각 인증서는 반드시 고유한 일련번호를 가짐
    - **서명 알고리즘 ID**: 서명을 위해 사용된 알고리즘. (예: "RSA 암호화를 이용한 MD2 요약")
    - **인증서 발급자**: 인증서를 발급하고 서명한 기관 이름. X.500 포맷으로 기록되어 있음.
    - **유효 기간**: 인증서가 유효한 기간. 시작일과 종료일로 정의
    - **대상의 이름**: 인증서에 기술된 사람, 조직같은 엔티티. 이 대상의 이름은 X.500 포맷으로 기록됨
    - **대상의 공개 키 정보**: 인증 대상의 공개 키, 공개 키에 사용된 알고리즘, 추가 매개 변수.
    - **발급자의 고유 ID(optional)**: 발급자의 이름이 겹치는 경우를 대비한, 인증서 발급자에 대한 고유 식별자
    - **대상의 고유 ID(optional)**: 대상의 이름이 겹치는 경우를 대비한, 인증 대상에 대한 고유 식별자
    - 확장: 선택적인 확장 필드의 집합(v3 이상에서 지원).
        - 각 확장 필드는 중요한 것인지 여부가 표시되어있음. 중요한 확장은 인증서 사용자에 의해 반드시 이해되어야 함. 만약 인증서 사용자가 이해하지 못하면, 인증서를 거절해야 함.
        - 흔히 사용되는 확장의 예
            - **기본 제약**: 대상과 인증 기관과의 관계
            - **인증서 정책**: 인증서가 어떤 정책 하에 승인되었는지
            - **키 사용**: 공개키가 어떻게 사용될 수 있는지에 대한 제한
- X.509 기반 인증서에는 웹 서버 인증서, 클라이언트 이메일 인증서, 소프트웨어 코드사인(code-signing) 인증서, 인증기관 인증서를 비롯한 몇가지 변종이 있음

### 14.6.3 서버 인증을 위해 인증서 사용하기

- HTTPS 를 통한 안전한 웹 트랜잭션을 시작할 떄, 최신 브라우저는 자동으로 접속한 서버에서 디지털 인증서를 가져옴.
    - 서버가 인증서가 없으면 보안 커넥션은 실패
- 서버 인증서는 다음을 포함한 많은 필드가 있음
    - 웹 사이트의 이름과 호스트 명
    - 웹 사이트의 공개키
    - 서명 기관의 이름
    - 서명 기관의 서명
- 브라우저가 인증서를 받으면 먼저 서명 기관을 검사
    - 그 기관이 공공이 신뢰할만한 기관이라면, 브라우저는 공개키를 이미 알고 있음(브라우저에 설치된채로 출하).
    - 기관을 모르는 곳이라면, 브라우저는 신뢰해야할지 확신할 수 없으므로, 대개 사용자가 서명 기관을 신뢰하는지 확인하기 위한 대화상자 노출

## 14.7 HTTPS의 세부사항

- HTTPS: HTTP + 대칭, 비대칭 인증서 기반 암호 기법의 집합
    - HTTP의 가장 유명한 버안 보전으로, 널리 구현되었고 주류 상용 브라우저 및 서버에 구현되어있음
    - 이러한 암호 기법들의 집합은 분권화된 글로벌 인터넷 환경에서 HTTPS를 매우 안전한 동시에 유연하게 만들어 줌

### 14.7.1 HTTPS 개요

- HTTPS: 보안 전송 계층을 통해 전송되는 HTTP.
- 암호화되지 않은 HTTP 메시지를 TCP를 통해 보내기 전에 암호화 보안계층으로 보냄
- HTTPS 의 보안계층은 SSL, TLS(현대적 대체품) 으로 구현되었으며, 둘 다 SSL로 통칭할 것임(관행)

### 14.7.2 HTTPS 스킴

- 보안 HTTP는 옵션이므로, 웹 서버로의 요청을 만들 때 웹 서버에게 HTTP의 보안 프로토콜 버전을 수행하는 것을 스킴으로 나타냄
    - 보안이 없는 일반적 HTTP의 URL 스킴 접두사는 `http`
    - 보안이 되는 HTTPS 프로토콜에서 URL 스킴 접두사는 `https`
- 웹 브라우저 등의 클라이언트는 웹 리소스에 대한 트랜잯션 수행 요청을 받으면 URL 스킴을 검사
    - URL이 http 스킴을 갖고 있다면, 클라이언트는 80번(기본 값) 포트로 연결하고 일반 HTTP 명령 전송
    - URL이 https 스킴을 갖고 있다면, 클라이언트는 443번(기본 값)포트로 연결하고, 서버와 바이너리 포맷으로 된 몇몇 SSL 보안 매개 변수를 교환하면서 '핸드셰이크'하고 암호화된 HTTP 명령이 뒤 따름
- SSL 트래픽은 바이너리 프로토콜이므로, HTTP와 완전히 다름
- SSL 트래픽은 HTTP와 다른 포트(보통 443 포트)로 전달되며, 만약 SSL, HTTP 모두 80번으로 도착한다면, 대부분의 웹 브라우저는 SSL 트래픽을 잘못된 HTTP로 해석하고 커넥션을 닫음
    - 보안 서비스가 HTTP 쪽으로 좀 더 계층 통합이 되면 포트가 둘 이상 필요할 이유가 사라지겠지만, 사실 큰 문제를 일으키지는 않음

### 14.7.3 보안 전송 셋업

- HTTPS에서의 절차(SSL 보안 계층때문에 약간 더 복잡):
    1. 클라이언트는 먼저 웹 서버의 443포트로 연결
    2. TCP가 연결되고 나면 클라이언트와 서버는 암호법 매개 변수와 교환키를 협상하면서 SSL 계층 초기화
    3. 핸드셰이크가 완료되면 SSL 초기화가 완료되었으므로, 요청 메시지를 보안 계층에 보내 암호화된 요청을 수 있게 됨
    4. SSL을 통해 암호화된 응답을 전달
    5. SSL 닫힘 통지
    6. TCP 커넥션 닫힘 

### 14.7.4 SSL 핸드셰이크

- 암호화된 HTTP 메시지를 보낼 수 있기 전에 클라이언트&서버는 SSL 핸드셰이크를 해야 함
    - 프로토콜 버전 번호 교환
    - 양쪽이 알고 있는 암호 선택
    - 양쪽의 신원을 인증
    - 채널 암호화하기 위한 임시 세션 키 생성
- 암호화된 HTTP 데이터가 네트워크를 오가기도 전에, SSL은 통신을 시작하기 위한 상당한 양의 핸드셰이크 데이터를 주고 받음
- 핸드셰이크는 복잡하지만 단순화하면 다음과 같음
    1. 클라이언트가 암호 후보를 보내고 인증서를 요구
    2. 서버는 선택된 암호와 인증서 전달
    3. 클라이언트가 비밀정보 전달. 클라이언트와 서버는 키를 만듦
    4. 클라이언트와 서버는 서로에게 암호화를 시작한다고 말함

### 14.7.5 서버 인증서

- SSL는 서버 인증서를 클라이언트로, 다시 클라이언트 인증서를 서버로 날라주는 상호 인증 지원
- 오늘날 클라이언트 인증서는 웹 브라우징에선 흔히 쓰이지 않음
    - 대부분 사용자는 개인 클라이언트 인증서를 갖고 있지않고, 웹 서버도 클라이언트 인증서를 요구할 수 있지만 잘 하지 않음
    - 몇몇 회사 설정에서 웹 브라우징을, 보안 이메일, 인트라넷 직원 정보 접근 제어용으로 사용
- 한편 보안 HTTPS 트랜잭션은 항상 서버 인증서를 요구
- 서버 인증서는 조직 이름, 주소, **서버 DNS 도메인 이름**, 그 외 정보를 보여주는 X.509 v3 에서 파생된 인증서
    - 사용자 및 클라이언트 소프트웨어는 인증서를 검증할 수 있음

### 14.7.6 사이트 인증 검사

- SSL 자체는 웹 서버 인증서 검증을 요구하지 않지만, 최신 웹 브라우저는 대부분 간단하게 기본 적 검사를 하고, 그 결과를 더 철저한 검사를 할 수 있는 법과 함께 사용자에게 안내
- 대부분의 웹 브라우저 검사 기법의 기초를 구축한 넷스케이프의 웹 서버 인증 검사 알고리즘 수행 단계는 4가지가 있음

***날짜 검사***

- 브라우저는 인증서가 유효함을 확인하기위해 인증서 시작/종료 일을 검사. 만료되었거나 활성화되지 않았다면 검사가 실패하고 에러를 표시

***서명자 신뢰도 검사***

- 모든 인증서는 서버를 보증하는 인증기관(Certificate Authority, CA)에 의해 서명되어있음
    - 인증서에는 여러 수준이 있으며, 각각은 다른 수준의 배경 검증을 요구
    - 예: 전자상거래 서버 인증서를 발급받고 싶다면, 사업체로서의 법인에 대한 법적 증명 제시
- 누구나 인증서 생성 가능하지만, 몇몇 CA는 잘 알려지고, 인증서 지원자의 신원과 사업을 입증하는 알기 쉬운 절차를 갖고 있음
    - 이러한 이유로 브라우저는 신뢰할만한 서명 기관의 목록을 포함한 채로 배포됨.
- 브라우저가 알려지지 않은(혹은 악의적일 수 있는) 인증기관으로 부터 서명된 인증서를 받으면 경고 노출
- 또한 브라우저는 신뢰할만한 CA가 간접적으로 서명한 인증서를 받아들일 수 있음.
    - 예: 신뢰할만한 CA가 '샘의 서명 가게'를 위한 인증서에 서명하고 '샘의 서명 가게'가 어떤 사이트 인증서에 서명한다면, 브라우저는 그 인증서를 올바른 CA 경로에서 파생 된 것으로 보고 받아들임

***서명 검사***

- 서명 기관이 믿을만 하다고 판단하면, 브라우저는 서명 기관의 공개키를 서명에 적용하여 그 체크섬을 비교하여 무결성을 검사

***사이트 신원 검사***

- 서버가 다른 이의 인증서 복사, 트래픽 가로채기 하는 것을 방지하기 위해 대부분의 브라우저는 인증서의 도메인 이름이 대화중인 서버의 도메인 이름과 비교하여 맞는지 검사
- 서버 인증서에는 보통 단일 도메인 이름이 들어있지만, 몇 CA는 서버 클러스터나 서버 팜을 위해 서버 이름 목록, 서버 이름들에 대한 와일드카드 표현이 들어있는 인증서를 만듦
- 호스트명이 인증서 신원가 맞지 않는다면, 사용자에게 이 사실을 알리거나 잘못된 인증서 에러와 함께 커넥션을 종료

### 14.7.7. 가상 호스팅과 인증서

- 가상 호스트(하나의 서버에 여러 호스트명)로 운영되는 사이트의 보안 트래픽 및 인증서 관리은 까다로움
    - 몇 웹 서버 프로그램은 하나의 인증서만 지원함
    - 만약 사용자가 인증서의 이름과 정확히 맞지 않는 가상 호스트명에 도착하면 경고상자가 나타남
- 이러한 문제를 피하기 위해, 보안 트랜잭션을 시작하는 사용자를 인증서에 나타나는 호스트 명으로 리다이렉트함.
    - 예: `Cajun-Shop.com` 이 있을 때, 호스팅 제공자는 `cajun-Shop.scuresites.com` 을 제공. 사용자가 `https://www.cajun-shop.com` 으로 접속시에 호스트명이 달라 경고가 나타나므로, `cajun-Shop.scuresites.com` 으로 리다이렉트

## 14.8 진짜 HTTPS 클라이언트

- SSL은 복잡한 바이너리 프로토콜이지만, 몇 가지 SSL 클라이언트와 서버 프로그래밍을 쉽게 만들어주는 상용/오픈 소스 라이브러리가 있음

### 14.8.1 OpenSSL

- **[OpenSSL](https://www.openssl.org/)** 은 SSL과 TLS 의 가장 인기 있는 오픈 소스 구현
- 강력한 다목적 암호법 라이브러리인 동시에 SSL, TLS 프로토콜을 구현한 강건하고 완전한 기능을 갖춘 상용 수준의 툴킷을 개발하고자 한 자원봉사자의 협업 결과물

### 14.8.2 간단한 HTTPS 클라이언트

- 기본적인 HTTPS 클라이언트를 작성하기위해 OpenSSL을 이용한 예제 (책 참조)
    - 이 예제에서는 서버와 SSL 커넥션을 맺고, 그 사이트 서버로부터 가져온 신원 정보를 출력하고, HTTP GET 요청을 보안 채널로 보내고, HTTP 응답을 받아 출력함
    - 에러 처리와 인증서 처리 로직은 제외됨

### 14.8.3 우리의 단순한 OpenSSL 클라이언트 실행하기

- 14.8.2 에서 작성한 코드를 실행시켜 봄 (책 참조)

## 14.9 프락시를 통한 보안 트래픽 터널링

- 클라이언트는 종종 대신 웹 서버에 접근해주는 웹 프락시를 사용
- 그러나 클라이언트가 서버로 보낼 데이터를 서버의 공개키로 암호화하기 시작하면, 프락시는 HTTP 헤더를 읽을 수 없음
    - 프락시가 HTTP 헤더를 못읽으면, 요청을 어디에 보낼지 알 수 없음
- HTTPS와 프락시가 잘 동작하도록 어디에 접속하려고 하는지 알려 주기 위한 방법 중, SSL 터널링 프로토콜이 유명
    - 클라이언트는  프락시에게 자기가 연결하고자 하는 안전한 호스트와 포트를 암호화 전에 평문으로 전달
    - HTTP는 CONNECT라는 새 확장 메서드를 이용해 이 평문으로된 정보를 프락시에게 전송
    - 완료되면 클라이언트와 서버 사이에 데이터가 직접 오갈 수 있게해주는 터널 생성
- CONNECT 메서드는 원 서버의 호스트명, 포트를 구분된 형태로 제공하는 한줄로 된 텍스트 명령
    - `호스트:포트` 뒤에는 스페이스 하나와 HTTP 버전 문자열, CRLF, 0 개이상의 HTTP 요청 헤더줄, 빈줄이 차례로 옴
    - 이후 핸드셰이크가 성공하면 SSL 데이터를 포함한

    ```
    CONNECTT home.netscape.com:443 HTTP/1.0
    User-agent: Mozilla/1.1N

    <SSL로 암호화된 데이터가 이 다음에 옴>
    ```

- 프락시는 요청을 평가하여 그것이 유효하고 사용자가 그러한 커넥션을 요청할 수 있도록 허가 받았는지 확인함. 만약 모든 것이 적법하다면 프락시는 목적지 서버로 연결하고 성공하면 `200 Connection Established` 응답을 클라이트에 제공