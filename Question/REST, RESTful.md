> **REST가 뭐에요? RESTful에 대해서 설명해주세요.**

**REST가 무엇인가?**
REST는 **분산 시스템 설계를 위한 아키텍처 스타일**이다.
아키텍처 스타일이라는건 쉽게 말하면 **제약 조건의 집합**이라고 보면 된다.

RESTful은 무엇인가?

RESTful은 위의 **제약 조건의 집합(아키텍처 스타일, 아키텍처 원칙)을 모두 만족**하는 것을 의미한다.

REST라는 아키텍처 스타일이 있는거고 RESTful API라는 말은 REST 아키텍처 원칙을 모두 만족하는 API라는 뜻이다.

우리가 REST와 RESTful을 동일한 의미로 사용하곤 하는데 엄격하게는 다르다는 것을 알 수 있다.

->이로써 REST와 RESTful, RESTful API가 무엇인지, 어떻게 다른지를 말할 수 있게 되었다.

**REST가 필요한 이유는 뭘까?**

**1. 위에서 말한 것과 같이 분산 시스템을 위해서다.**

거대한 애플리케이션을 모듈, 기능별로 분리하기 쉬워졌다. RESTful API를 서비스하기만 하면 어떤 다른 모듈 또는 애플리케이션들이라도 RESTful API를 통해 상호간에 통신을 할 수 있기 때문이다.

**2. WEB브라우저 외의 클라이언트를 위해서다. (멀티 플랫폼)**

웹 페이지를 위한 HTML 및 이미지등을 보내던 것과 달리 이제는 데이터만 보내면 여러 클라이언트에서 해당 데이터를 적절히 보여주기만 하면 된다.

예를 들어 모바일 애플리케이션으로 html같은 파일을 보내는 것은 무겁고 브라우저가 모든 앱에 있는 것은 아니기 때문에 알맞지 않았는데 RESTful API를 사용하면서 데이터만 주고 받기 때문에 여러 클라이언트가 자유롭고 부담없이 데이터를 이용할 수 있다.

서버도 요청한 데이터만 깔끔하게 보내주면되기 때문에 가벼워지고 유지보수성도 좋아졌다.

**REST의 구성 요소**

HTTP URI = 자원

HTTP Method = 행위

MIME Type = 표현 방식

| 12   | GET /100 HTTP/1.1Host : jeong-pro.tistory.com | [cs](http://colorscripter.com/info#e) |
| ---- | --------------------------------------------- | ------------------------------------- |
|      |                                               |                                       |

위와 같은 Request 메세지가 있으면 URI자원은 "/100" 이고, HTTP Method는 "GET" 이다.

MIME 타입은 보통 Response Http header 메세지에 Content-type으로 쓰인다. 여기서는 없다.

그러면 이해하기를 jeong-pro.tistory.com 서버에 /100 이라는 자원을 GET(조회)하고 싶다는 요청으로 해석이 가능하다. 이게 REST 방식을 이용한 Request 예시다. (참고로 이것은 이해를 위한 것일 뿐 RESTful 하다고는 못한다.) 

| 1234 | HTTP/1.1 200 OKContent-Type : application/json-patch+json [{"title": "helloworld", "author": "jeong-pro"}] |      |
| ---- | ------------------------------------------------------------ | ---- |
|      |                                                              |      |

이런 Response가 왔다고 해보자.

그러면 Content-Type을 보고 클라이언트는 IANA라는 타입들의 명세를 모아놓은 사이트에 가서 application/json-patch+json 이라는 타입의 명세를 확인하고 아래 Body의 내용이 json타입이구나를 알 수 있는 것이다.

> **REST는 알겠고 그러면 그 제약 조건이 뭔데요?**

\1. Client/Server

\2. Stateless : 각 요청에 클라이언트의 context가 서버에 저장되어서는 안된다.

\3. Cacheable : 클라이언트는 응답을 캐싱할 수 있어야 한다.

\4. Layered System : 클라이언트는 서버에 직접 연결되었는지 미들웨어에 연결되었는지 알 필요가 없어야 한다.

\5. Code on demand(option) : 서버에서 코드를 클라이언트에게 보내서 실행하게 할 수 있어야 한다.

\6. **uniform interface** : 자원은 유일하게 식별가능해야하고, HTTP Method로 표현을 담아야 하고, 메세지는 스스로를 설명(self-descriptive)해야하고, 하이퍼링크를 통해서 애플리케이션의 상태가 전이(HATEOAS)되어야 한다.

왜 uniform interface에 강조가 되어있냐면, 1~5번의 제약 조건은 HTTP를 기반으로하는 REST는 HTTP에서 이미 충분히 지켜지고 있는 부분이라서 비교적 덜 주의를 기울여도 된다.

RESTful하려면 저 uniform interface를 잘 지켜야 한다.

그 중에서도 **HATEOAS**와 **self-descriptive**를 잘 지켜야 한다.

필자가 주로 쓰는 Spring에는 spring-data-rest, spring hateoas, spring-rest-doc으로 두 제약을 지키기위해 사용할 수 있는 라이브러리가 있다. (이 포스트는 면접을 위한 포스트일 뿐 사용법과 테스트는 다른 포스트에서 한다.)

HATEOAS는 Link 라는 HTTP 헤더에 다른 리소스를 가리켜 주는 값을 넣는 방법으로 해결한다.

| 12345678 | HTTP/1.1 200 OKContent-Type : application/jsonLink : </spring/1>; rel="previous",    </spring/3>; rel="next";{  "title" : "spring의 모든 것"  "author" : "jeong-pro"}[Colored by Color Scripter](http://colorscripter.com/info#e) | [cs](http://colorscripter.com/info#e) |
| -------- | ------------------------------------------------------------ | ------------------------------------- |
|          |                                                              |                                       |

위와 같이 해당 정보에서 다른 정보로 넘어갈 수 있는 하이퍼링크를 명시해야 한다는 것이다.

완벽한 REST는 무엇일까? WEB이다.

어떤 Application이 생겼다고 브라우저는 버전을 업데이트할 필요가 없고, 브라우저가 해당 application으로 어떻게 요청하는지를 알게 해야할 필요가 없다.

*** 장점**

\- 메세지를 단순하게 표현할 수 있고 WEB의 원칙인 확장에 유연하다. (멀티플랫폼)

\- 별도의 장비나 프로토콜이 필요없이 기존의 HTTP 인프라를 이용할 수 있다. (사용이 용이함)

\- server, client를 완전히 독립적으로 구현할 수 있다.

*** 단점**

\- 표준, 스키마가 없다. 결국은 API 문서가 만들어지는 이유다.

\- 행위에 대한 메소드가 제한적이다. (GET, POST, PUT, DELETE, HEAD, ...)



\* REST는 **분산 시스템 설계**를 위한 이키텍처 스타일이라고 했다.

마이크로서비스라는 말을 들어보았을 것이다. 이 쪽으로 질문이 연계될 수 있다.

RESTful API를 이용해서 하나의 큰 서비스 애플리케이션을 여러 모듈화된 작은 서비스 애플리케이션(마이크로 서비스)들로 나눌 수 있게 됐기 때문이다.



\* REST를 공부하니까 **URI와 URL의 차이점**에 대해서도 이해할 수 있게되었다.

Uniform Resource Identifier, Uniform Resource Locator

REST에서는 모든 것을 Resource로 표현한다. 그리고 그 자원은 유일한 것을 나타낸다. Identifier, 식별자라는 것이다.

반면에 과거의 웹에서는 Identifier의 개념이 따로 필요없었다. html같은 파일들을 주고 받았기 때문에 파일의 위치를 가리키는 Locator를 썼다고 이해하면 된다.

URI가 파일뿐만 아니라 여러 자원들 까지도 포함하는 개념으로 이해할 수 있다.



출처: https://jeong-pro.tistory.com/180 [기본기를 쌓는 정아마추어 코딩블로그]