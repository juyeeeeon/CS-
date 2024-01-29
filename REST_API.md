# REST API
## REST API가 무엇인가?
- REST(Representational State Transfer)는 소프트웨어 아키텍처 스타일로, 웹 아키텍처 디자인의 가이드
- REST 설계 원칙을 따르는 모든 API(Application Programming Interface)를 RESTful 이라고 한다.
- 간단히 말해서, REST API는 클라이언트와 서버가 통신하는 것과 동일한 방식으로 두 컴퓨터가 HTTP(Hypertext Transfer Protocol)을 통해 통신하는 매체이다.

## REST API 디자인
- JSON(JavaScript Object Notation)을 데이터 전송 및 수신 형식으로 사용
- endpoints에서 동사 대신 명사 사용
	- GET, POST, PUT, PATCH, DELETE등 의 HTTP 메서드가 이미 기본적인 CRUD(Create, Read, Update, Delete) 작업을 수행하기 위한 동사 형태로 되어 있기 때문

    ```Markdown
      https://mysite.com/getPosts (X)
      https://mysite.com/createPost (X)

      https://mysite.com/posts(O)
    ```
- 복수 명사를 사용한 name collections 사용
	```Markdown
        https://mysite.com/post/123 (X)

        https://mysite.com/posts/123 (O)    
    ```
- 오류 처리에 상태 코드 사용
    - 100 - 199 : Informational Responses
    - 300 - 399 : Redirects
    - 400 - 499 : Client-side errors
    - 500 - 599 : Server-side errors

- endpoints에 중첩을 사용하여 관계 표시
    ```Markdown
        https://mysite.com/posts/author
        https://mysite.com/posts/postId/comments
        ## 3개 이상의 중첩은 가독성을 떨어뜨림
    ```

- filtering, sorting, pagination을 사용하여 요청된 데이터 검색
    ```Markdown
        https://mysite.com/posts?tags=javascript
    ```

- 보안을 위한 SSL의 사용
    ```Markdown
        ## SSL에서 실행 O
        https://mysite.com/posts

        ## SSL에서 실행 X 
        http://mysite.com/posts
    ```
- 버전 관리
    ```Markdown
        https://mysite.com/v1/
        https://mysite.com/v2
    ```
- API 문서 제공
    - 클라이언트가 올바르게 사용하는 방법을 배우고 파악할 수 있도록 API 문서 제공

## REST API의 특징
1. Client-Server Architecture

RESTful API는 클라이언트-서버 아키텍처로 구축되어있다. 즉, 클라이언트가 서버에 요청을 보내고 서버가 응답을 다시 보낸다. 클라이언트는 HTTP 요청을 할 수 있는 모든 장치나 애플리케이션일 수 있으며, 서버는 API를 제공하고 클라이언트 요청에 응답하는 애플리케이션이다. 이러한 특성을 통해 우려 사항을 분리할 수 있으므로 두 구성 요소를 독립적으로 개발, 유지 관리 및 확장하기가 더 쉬워진다.

2. Statelessness

RESTful API는 Stateless이다. 즉, 클라이언트가 서버에 보낸 각 요청에는 이전 요청이나 서버 측 저장소에 의존하지 않고 서버가 요청을 이행하는 데 필요한 모든 정보가 포함되어 있다. 이것이 인증된 모든 REST 요청이 요청 헤더에 인증 토큰을 전달해야 하는 이유이다.

이렇게 하면 요청 크기가 늘어나지만 두 개의 별도 요청에 걸쳐 상태 정보를 저장할 걱정 없이 서버를 확장할 수 있다.

3. Cacheability

서버의 부하를 줄이는 방법을 활용하는 것이 중요하다. 따라서 RESTful API는 일종의 캐싱을 구현한다. 이는 클라이언트가 API 응답을 캐시할 수 있으므로 동일한 리소스에 대한 후속 요청에서 응답 시간이 더 빨라질 수 있음을 의미한다. 이렇게 하면 서버가 각 요청에 대해 동일한 응답을 생성할 필요가 없으므로 서버의 로드가 줄어들고 성능이 향상된다.

REST의 각 요청은 인증 토큰과 서버에서 작동하는 데 필요한 관련 상태를 전달해야 하기 때문에 이 특성의 이점은 트래픽이 괜찮은 모든 API에서 매우 빠르게 드러난다. 캐시 시간 제한을 5초로 설정하더라도 몇 초 동안 수천 또는 수백만 건의 요청을 방지할 수 있다.

4. Layered System

미래 지향적인 API는 모듈식이어야 하며 각 모듈은 투명하게 업데이트하거나 교체할 수 있어야 한다. 따라서 REST를 사용하려면 API를 계층화된 시스템으로 설계해야한다. 여기서 클라이언트는 단일 엔드포인트를 통해 서버와 상호 작용하고 서버는 여러 백엔드 시스템과 상호 작용할 수 있다. 이를 통해 문제를 분리할 수 있으며 클라이언트에 영향을 주지 않고 새 백엔드 시스템을 추가하거나 기존 시스템을 변경하거나 유지 관리를 더 쉽게 수행할 수 있다. 이에 대한 예는 서버가 로드 밸런싱을 위한 메커니즘을 업데이트 하지만, 클라이언트는 이를 인지하지 못한다. 클라이언트는 이전과 동일하게 통신을 계속할 수 있다.

5. Code-On-Demand

이는 의도하지 않은 부작용과 악용을 초래할 수 있으므로 선택적인 특성이다. 이 특성은 서버가 데이터 대신 클라이언트가 실행할 코드를 다시 보낼 수 있음을 뜻한다. 이는 클라이언트의 기능을 확장하고 보다 동적이고 사용자 정의 가능한 상호 작용으로 이어질 수 있다. 그러나 이를 위해서는 클라이언트가 서버가 다시 보내는 코드를 이해하고 실행할 수 있어야 한다. 따라서 이 특성을 준수하는 빈도가 감소했다. 또한 서버가 해킹되면 클라이언트는 서버가 응답하는 모든 것을 실행하므로 자동으로 하이재킹된다. 이 눈에 띄는 보안 문제는 Code-On-Demand의 채택을 방해하기도 했다.

6. Uniform Interface

이는 API가 GET, POST, PUT 및 DELETE와 같은 일반적인 메소드 세트를 사용하여 리소스에 액세스하고, 요청 및 응답에 JSON 또는 XML과 같은 표준 형식을 사용한다는 것을 의미한다. 모든 리소스가 일관된 방식으로 액세스되므로 클라이언트가 API를 더 쉽게 이해하고 API와 상호 작용할 수 있다. 또한 통일된 인터페이스를 사용하면 기존 자원과 메소드에 영향을 주지 않고 새로운 리소스와 메소드를 정의하여 새로운 기능을 추가할 수 있으므로 API 버전 관리를 더 쉽게 구현할 수 있다.

## REST API 장점과 단점
REST API의 장점
- REST API는 API의 단순성으로 인해 이해하고 배우기 쉽다.
- REST API를 사용하면 복잡한 애플리케이션을 구성하고 리소스를 쉽게 사용할 수 있다.
- 높은 로드는 HTTP 프록시 서버 및 캐시의 도움으로 관리될 수 있다.
- REST API는 쉽게 탐색하고 발견할 수 있습니다.
- 특정 목적에 맞게 설계되었는지 여부에 관계없이 새로운 클라이언트가 다른 응용 프로그램에서 작업하는 것을 간단하게 만든다.
- 표준 HTTP 프로시저 콜아웃을 사용하여 데이터와 요청을 검색한다.
- REST API는 코드에 따라 다르며 이를 사용하여 아무런 문제 없이 웹사이트와 데이터를 동기화할 수 있다.
- 사용자는 SOAP 기반 웹 서비스와 비교할 때 동일한 표준 개체 및 데이터 모델에 액세스할 수 있다.
- XML 또는 JSON 형식으로 데이터를 직렬화하여 유연한 형식을 제공한다.
- REST 요청을 확인하기 위해 OAuth 프로토콜을 사용하는 표준 기반 보호를 허용한다.

REST의 단점 또는 과제
- state 부족
    - 대부분의 웹 애플리케이션에는 state 저장 메커니즘이 필요하다. 
    - 상태를 유지해야 하는 부담은 클라이언트에 있으며, 이로 인해 클라이언트 애플리케이션이 무겁고 유지 관리가 어려워진다.
- 보안의 지속성
    - REST는 SOAP와 같은 보안을 적용하지 않는다. 
    - 이것이 REST가 공개 URL에는 적합하지만 클라이언트와 서버 간의 기밀 ​​데이터 전달에는 적합하지 않은 이유이다. 
---
#### [참고]
https://www.freecodecamp.org/news/rest-api-best-practices-rest-endpoint-design-examples/

https://www.scrapingbee.com/blog/six-characteristics-of-rest-api/

https://krify.co/advantages-and-disadvantages-of-rest-api/