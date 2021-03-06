# HTTP 완벽가이드 정리
## 17장 내용 협상과 트랜스코딩
### 17.1 내용 협상 기법

서버에 있는 페이지들 중 어떤 것이 클라이언트에게 맞는지 판단하는 세 가지 다른 방법이 있다.   

1. 클라이언트 주도 협상   
2. 서버 주도 협상   
3. 투명한 협상   

---

### 17.2 클라이언트 주도 협상

* 클라이언트가 요청을 보내면, 서버는 클라이언트에게 선택지를 보내주고, 클라이언트가 선택   
* **장점**   
  * 서버 입장에서 가장 구현하기 쉽다.   
  * 클라이언트는 최선의 선택을 할 수 있다.   
* **단점**   
  * 대기시간이 증가한다. 즉, 올바른 콘텐츠를 얻으려면 최소 두 번의 요청이 필요하다.   
  * 여러 개의 URL을 요구한다. (ex- www.google.com/ko, www.google.com/en)   
  
### 17.3 서버 주도 협상

* 서버가 클라이언트의 요청 헤더를 검증해서 어떤 버전을 제공할지 결정   
* **장점**   
  * 클라이언트 주도 협상보다 빠름   
  * HTTP는 서버가 가장 적절한 것을 선택할 수 있도록 q값 메커니즘을 제공하고, 서버가 다운스트림 장치에게   
    요청이 어떻게 평가되는지 말해줄 수 있도록 하기 위해 Vary 헤더를 제공한다.   
* **단점**   
  * 헤더에 맞는 것이 없으면, 서버는 추측을 해야만 한다.   

#### 17.3.1 내용 협상 헤더

클라이언트는 아래 나열된 HTTP 헤더들을 이용하여 자신의 선호 정보를 보낸다.   

* ```Accept``` : 서버가 어떤 미디어 타입으로 보내도 되는지 알려준다.   
* ```Accept-Language``` : 서버가 어떤 언어로 보내도 되는지 알려준다.   
* ```Accept-Charset``` : 서버가 어떤 차셋으로 보내도 되는지 알려준다.   
* ```Accept-Encoding``` : 서버가 어떤 인코딩으로 보내도 되는지 알려준다.   

서버는 클라이언트 Accept 관련 헤더들을 적절한 엔터티 헤더들과 짝을 짓는다.   

Accept 관련 헤더 | 엔터티 헤더
:---|:---
Accept|Content-Type
Accept-Language|Content-Language
Accept-Charset|Content-Type
Accept-Encoding|Content-Encoding
   
#### 17.3.2 내용 협상 헤더의 품질값

```
Accept-Language: en;q=0.5, fr;q=0.0 nl;q=1.0, tr;q=0.0
```   
q값은 0.0부터 1.0까지의 값을 가지며 0.0은 가장 낮은 선호도 1.0은 가장 높은 선호도이다.   
위의 예시로 보면 네덜란드어(nl)를 가장 선호하며 어떤 경우에는 영어도 선호한다.   
그러나 어떤 경우라도 프랑스어나 터키어 버전은 원하지 않는다는 것을 알 수 있다.   

#### 17.3.3 그 외의 헤더들에 의해 결정

* User-Agent와 같은 클라이언트의 다른 요청 헤더들을 이용해 알맞은 요청을 만든다.   
* HTTP 프로토콜은 서버가 응답에 넣어 보낼 수 있는 Vary 헤더를 정의한다.   

#### 17.3.4 아파치의 내용 협상

아파치 웹 서버거 내용 협상을 지원하는 방법은 다음과 같다.   

* 웹 사이트 디렉터리에서, 배리언트(variant)를 갖는 웹 사이트의 각 URI를 위한 type-map 파일을 만든다.   
  type-map 파일은 모든 배리언트와 그들 각각에 대응하는 내용 협상 헤더들을 나열한다.   
* 아파치가 그 디렉터리에 대해 자동으로 type-map 파일을 생성하도록 하는 MultiViews 지시어를 켠다.   
   
***type-map 파일***

```
URI: joes-hardware.html

URI: joes-hardware.en.html
Content-type: text/html
Content-language: en

URI: joes-hardware.fr.de.html
Content-type: text/html;charset=iso-8859-2
Content-language: fr, de
```   

type-map 파일을 통해, 아파치 서버는 영어로 요청한 클라이언트에게는 joes-hardware.en.html 파일을,   
프랑스어로 요청한 클라이언트 에게는 joes-hardware.fr.de.html 파일을 보낸다.   

***MultiViews***

MultiViewes가 켜져 있고 브라우저가 joes-hardware라는 이름의 리소스를 요청했다면,   
서버는 이름에 'joes-hardware'가 들어 있는 모든 파일을 살펴보고 type-map 파일을 생성한다.   

---

### 17.4 투명 협상

* 투명한 중간 장치(주로 프락시 캐시)가 서버를 대신하여 협상한다.   
* **장점**   
  * 웹 서버가 협상을 할 필요가 없다.   
  * 클라이언트 주도 협상보다 빠르다.   
* **단점**   
  * 투명 협상을 어떻게 하는지에 대한 정형화된 명세가 없다.   

서버는 응답에 ```Vary 헤더```를 포함시켜 보냄으로써 중개자에게 내용 협상을 위해 어떤 헤더를 사용하고 있는지 알려줄 수 있다.   

#### 17.4.1 캐시와 얼터네이트(alternate)

캐시는 같은 URL에 대해 두 개의 다른 문서를 갖게 된다.    
이 다른 버전은 ```배리언트(variant)나 얼터네이트(alternate)```로 불린다.   
따라서, 내용 협상은 배리언트 중에서 클라이언트의 요청에 가장 잘 맞는 것을 선택하는 과정이다.   

#### 17.4.2 Vary 헤더

HTTP Vary 응답 헤더는 서버가 문서를 선택하거나 커스텀 콘텐츠를 생성할 때 고려한 클라이언트   
**요청 헤더 모두**(일반적인 내용 협상 헤더 외에 추가로 더해서)를 나열한다.   

예를 들어, 제공된 문서가 User-Agent 헤더에 의존한다면, Vary 헤더는 반드시 "User-Agent"를 포함해야 한다.   

* 캐시는 새 요청이 오면 반드시 캐시된 응답 안에 서버가 보낸 Vary 헤더가 들어있는지 확인해야 한다.   
* 투명 협상을 구현하기 위해 캐시는 반드시 캐시된 배리언트와 함께 클라이언트 요청 헤더와 그에 알맞은 서버 응답   
  헤더 양쪽 모두를 저장해야 한다.   

```
Vary: User-Agent, Cookie
```
서버의 Vary 헤더가 이렇다면, 거대한 수의 다른 User-Agent와 Cookie 값이 많은 배리언트를 만들어 낼 것이다.   

---

### 17.5 트랜스 코딩

> **트랜스 코딩**   
> 서버가 클라이언트의 요구에 맞는 문서를 하나도 갖고 있지 않다면, 기존의 문서를 클라이언트가 사용할 수 있는 무언가로 변환하는 옵션

트랜스코딩에는 포맷 변환, 정보 합성, 내용 주입의 세 종류가 있다.   

#### 17.5.1 포맷 변환

* 포맷변환은 데이터를 클라이언트가 볼 수 있도록 한 포맷에서 다른 포맷으로 변환하는 것이다.   
* 내용 협상 헤더에 의해 주도된다.(User-Agent 헤더에 의해서 주도될 수도 있지만)   
* ```내용 변환, 트랜스코딩``` -> 콘텐츠를 특정 접근 장치에서 볼 수 있도록 하기 위한 것,   
* ```콘텐츠 인코딩, 전송 인코딩``` -> 콘텐츠의 더 효율적인 혹은 안전한 전송을 위한 것   

#### 17.5.2 정보 합성

* 문서에서 정보의 요점을 추출하는 것을 ```정보 합성(information synthesis)```라고 한다.   
* 이 기술은 포털 사이트의 웹페이지 디렉터리와 같은 자동화된 웹페이지 분류 시스템에 의해 종종 사용된다.   

#### 17.5.3 콘텐츠 주입

```내용 주입 트랜스코딩(content-injection transcoding)```은 앞의 두 종류의 트랜스 코딩과 달리   
웹 문서의 양을 늘린다. 예로 자동 광고 생성과 사용자 추적 시스템이 있다.   

지나가는 모든 HTML 페이지에 자동으로 광고를 삽입하는 광고 삽입 트랜스코더를 상상해보자.   
이런 종류의 트랜스코딩은 현재 관련이 있거나 어떻게든 특정 사용자를 대상으로 하는 광고를 그때그때   
동적으로 삽입한다.   

#### 17.5.4 트랜스코딩 vs 정적으로 미리 생성해놓기

트랜스코딩의 대안은 웹 서버에서 웹페이지의 여러 가지 사본을 만드는 것이다.   
예를 들어 하나는 고화질 이미지로 하나는 저화질 이미지로, 하나는 HTML로 하나는 WML로...   
그러나, 이것은 여러 가지 이유로 그다지 현실적인 기법은 아니다.   
