# REST API란 무엇인가요?

REST API는 Representational State Transfer (REST) 원칙을 따르는 API이다.

클라이언트(사용자 혹은 앱)가 서버와 통신하는 방식 중 하나라고 할 수 있다.

REST는 특정한 규칙과 원칙을 따르는 아키텍처 스타일로, HTTP를 기반으로 데이터를 주고받는 방법을 정의한다.

## REST API의 특징

1. Uniform-Interface
   REST API의 Uniform Interface 원칙은 API에서 자원들이 독립적이고 일관된 방식으로 접근하고 조작될 수 있도록 정의하는 규칙입니다. 이 원칙을 통해 각 자원은 고유한 URL로 식별되며, 자원 간의 상호작용은 일관된 방법을 통해 이루어집니다.

### 주요 개념

1. 독립적인 인터페이스 (Independent Interface)

- 각 자원은 독립적인 인터페이스를 가져야 한다.
- 예를 들어, 웹 페이지의 변경 사항이 있을 때, 웹 브라우저를 업데이트하지 않아도 웹 페이지가 잘 작동해야 하는 것처럼, REST API도 클라이언트와 서버가 독립적으로 동작할 수 있어야 한다.
- HTTP 명세나 HTML 명세가 변경되어도 웹 페이지나 API가 제대로 작동해야 한다는 의미이다.

2. 자원 식별 (URL Resource Identification)

- 자원은 고유한 URL을 통해 식별된다.
- 예를 들어, /users/123는 ID가 123인 사용자 자원을 식별한다.

3. 표현을 통한 자원 조작 (Manipulation of Resources through Representations)

- 자원은 HTTP 메서드(GET, POST, PUT, DELETE 등)와 URL을 통해 조작된다.
- 자원은 표현을 통해 조작되며, 표현은 자원의 상태를 나타내는 데이터이다. 예를 들어, GET /users/123 요청은 사용자 123의 데이터를 반환하고, DELETE /users/123은 해당 사용자를 삭제한다.

4. Self-descriptive Messages (자기 설명적인 메시지)

- 각 메시지는 그 자체로 충분한 정보를 담고 있어야 한다.
- HTTP 헤더에 타입을 명시하고, 메시지는 MIME 타입에 맞춰 표현되어야 한다.
- 예를 들어, JSON 데이터를 반환할 때는 HTTP 헤더에 Content-Type: application/json을 명시해야 한다.
- MIME 타입은 데이터 형식과 특성을 나타내며, 예: application/json, text/html, image/png 등이다.

5. HATEOAS (Hypermedia As The Engine Of Application State)

- 클라이언트는 API의 응답에서 다른 가능한 자원들로의 링크를 받을 수 있어야 한다. 즉, 클라이언트가 자원을 요청할 때 그 자원에 관련된 다른 가능한 액션들(다음 요청)을 찾을 수 있는 링크가 포함되어 있어야 한다.
- 예를 들어, GET /users/123을 호출하면, 응답으로 user profile, edit profile, delete profile과 같은 링크가 포함될 수 있다.

## REST API의 URI 규칙

### 1. **Stateless (상태 비저장)**

- REST는 **Stateless** 원칙을 따른다. 이는 **서버가 클라이언트의 상태를 저장하지 않는다**는 의미이다.
- HTTP 자체가 상태를 저장하지 않기 때문에, REST API를 구현할 때 서버는 클라이언트의 세션 정보를 저장하거나 추적하지 않는다.
- 각 요청은 독립적이어야 하며, 요청에 필요한 모든 정보는 클라이언트가 제공해야 한다.

### 2. **Cacheable (캐시 가능)**

- HTTP는 기본적으로 **캐시 가능**하며, 이를 통해 성능을 최적화할 수 있다.
- **GET** 요청에 대해서만 캐싱이 가능하며, 이를 제어하는 방법은 HTTP 헤더(`Cache-Control`, `Last-Modified`, `Etag`)로 설정할 수 있다.

### 3. **Client-Server (클라이언트-서버 구조)**

- REST API는 **클라이언트와 서버가 독립적인 구조**로 동작해야 한다.
- 서버는 API만 제공하고, 클라이언트는 그 API를 사용하여 응답을 처리한다.
- 서버와 클라이언트는 서로 독립적으로 동작할 수 있어야 하며, HTTP 표준만 잘 따르면 두 시스템 간의 원활한 통신이 가능하다.

### 4. **Layered System (계층화된 시스템)**

- REST API는 **계층화된 아키텍처**로 설계된다.
- 클라이언트는 각 계층에 대해 몰라도 되며, 각 계층은 독립적으로 작업할 수 있다.
- 예를 들어, 프록시 서버나 로드 밸런서가 중간에 존재할 수 있으며, 이를 통해 API의 성능을 향상시킬 수 있다.

## REST API URI 규칙

1. **동작은 HTTP 메서드로만 표현**하고, URL에 동작을 포함시키지 않는다.

2. **확장자 표시 금지**: URL에서 `.jpg`, `.png`와 같은 확장자는 사용하지 않는다.

3. **명사로 표현**: URL은 동사가 아닌 명사로 정의해야 한다.

4. **계층적 구조**: URL은 계층적이며, 자원 간 관계를 반영해야 한다.

5. **소문자 사용 및 하이픈(-) 사용**:

6. **적절한 HTTP 응답 상태 코드 사용**:
