## REST API

###### REST API란 RESTful한 API를 말하며 일련의 특징과 규칙 등을 지키는 API를 말함. 2000년에 Roy Thomas Fielding이 쓴 논문에서 처음으로 등장한 개념

### REST API의 특징

#### 1. Uniform-Interface

- API에서 자원들은 각각의 독립적인 인터페이스를 가지며 각각의 자원들이 **url 자원식별, 표현을 통한 자원조작, Self-descriptive messages, HATEOAS 구조**를 가지는 것을 말한다.
- 독립적인 인터페이스
  - 예를 들어 웹페이지를 변경했다고 웹 브라우저를 업데이트하는 일은 없어야 하고 HTTP 명세나 HTML 명세가 변경되어도 웹페이지는 잘 작동해야 하듯이 서로 종속적이지 않은 인터페이스를 말한다.

##### url 자원식별

- **identification of resources**를 말함. 자원은 url로 식별되어야 한다.

##### 표현을 통한 자원조작

- **manipulation of resources through representations**은 url과 GET, DELETE 등 HTTP 표준메서드 등을 통해 자원을 조회, 삭제 등 작업을 설명할 수 있는 정보가 담겨야 하는 것을 말한다.

##### Self-descriptive messages

- HTTP Header에 타입을 명시하고 각 메시지(자원)들은 MIME types에 맞춰 표현되어야 한다.
- 예를 들어 .json를 반환한다면 application/json으로 명시해줘야 한다.
- MIME types는 문서, 파일 등의 특성과 형식을 나타내는 표준이다.
- IETF의 RFC6838에 정의 및 표준화되어 있다. 'font/ttf', 'text/plain', 'text/csv' 등을 말한다.
- 예를 들어 json 타입의 데이터를 보낼 때는 헤더의 'Content-Type' = 'application/json' 을 명시해야 한다.

##### HATEOASS(Hypermedia as the Engine of Application State) 구조

- 하이퍼링크에 따라 다른 페이지를 보여줘야 하며 데이터마다 어떤 URL에서 원했는지 명시해주어야 하는 것을 말한다.
- 보통은 href, links, link, url 속성 중 하나에 해당 데이터의 URL을 담아서 표기해야 한다.(\_links 속성에 담기도 합니다. 링크를 유추할 수 있는 변수명을 쓰면 됨)

---

#### 2. Stateless

- 이 규칙은 HTTP 자체가 Stateless이기 때문에 HTTP를 이용하는 것만으로도 만족된다.
- 그리고 이는 REST API를 제공해주는 서버는 세션(session)을 해당 서버 쪽에 유지하지 않는다는 의미이다.

#### 3. Cacheable

- HTTP는 원래 캐싱이 된다. 아무런 로직을 구현하지 않더라도 자동적으로 캐싱이 된다.
- 새로고침을 하면 304 상태 코드가 뜨면서 원래 있던 js와 css이미지 등을 불러오는 것을 볼 수 있다.
- 이는 GET 메서드에 한정되며 `'Cache-Control:max-age=100'`(100초)이런 식으로
  한정된 시간을 정할 수가 있으며 캐싱된 데이터가 유효한지를 판단하기 위해
  `'Last-modified'`와 `'Etag'`라는 헤더값을 쓴다.
  - `'Etag'`는 전달되는 값에 태그를 붙여서 캐싱되는 자원인지를 확인해주는 것이다.

#### 4. Client-Server 구조

- 클라이언트와 서버가 서로 독립적인 구조를 가져야 한다. 물론 이는 HTTP를 통해 가능한 구조다.
- 서버에서 HTTP 표준만 지킨다면 웹에서는 그에 따른 화면이 잘 나타나게 된다.
- 서버는 그저 API를 제공하고 그 API에 맞는 비즈니스 로직을 처리하면 된다.
- 마찬가지로 클라이언트에서는 HTTP로 받는 로직만 잘 처리하면 되는 것이다.

#### 5. Layered System

- 계층구조로 나눠져 있는 아키텍처를 뜻한다. WEB기반 서비스를 하면 보통 이러한 시스템을 구축하게 된다.

---

### REST API의 URI규칙

#### 자원을 표기하는 URI의 아래의 6가지 규칙을 적용해야 한다

##### 1. 동작은 HTTP 메소드로만 해야 하고 url 에 해당 내용이 들어가면 안됩니다.

- 수정 = put, 삭제 = delete, 추가 = post,조회 = get을 이용해야 합니다. 예를 들어 `'/books/delete/1'` 이렇게 표기하면 안 된다.

##### 2. .jpg, .png 등 확장자는 표시 하지 말아야 한다.

##### 3. 동사가 아닌 명사로만 표기해야 한다.

- 유저가 책을 소유한다라는 것을 표현한다면 `'유저/유저아이디/inclusion/책/책아이디'`로 표현하고
- 유저가 소유한 아파트를 조회한다고 하면 이렇게 표현해야 합니다. `/users/{userid}/aparts` 또한 `/getAllUsers` 식의 동사를 집어넣으면 안된다.

##### 4. 계층적인 내용을 담고 있어야 한다.

- `'/집/아파트/전세'` 이런 식으로 내려가야 한다.

##### 5. 대문자가 아닌 소문자로만 쓰며 너무 길 경우에 바를 써야 할 경우 언더바`_`가 아닌 그냥 바 `-`를 쓴다.

##### 6. HTTP 응답 상태코드를 적재적소에 활용합니다.성공시에는 200, 리다이렉트는 301 등

#### Ex1. 도서관 시스템

```node
app.get("/books");
// 모든 책을 조회합니다.
app.post("/books/:booksid");
// 책을 생성합니다.
app.put("/books/:booksid");
// 책을 수정합니다.
app.get("/books/:booksid");
// 특정 책을 조회합니다.
app.put("/users/:userid/books/:booksid");
// 어떤 유저가 특정 책을 빌립니다.
app.patch("/users/:userid/books/:booksid");
// 어떤 유저가 특정 책을 빌립니다.
```

#### Ex2. 쿼리스트링과 혼합한 url

##### 실제 워드프레스에서 제공하는 REST API

- `/wp/v2/posts?page=2`
  - posts의 2번째 결과물을 나타냄
  - api를 설정할 때 /v2 /v1 으로 버전을 명시해 놓는게 좋다. 이를 통해 현재 버전을 사용하다가 새 버전이 안정되면 자체적으로 마이그레이션할 수 있다.

##### 실제 KAKAO API

`/oauth/token?grant_type=refresh_token&client_id=${REST_API_KEY}`

- REST API는 /를 기반으로만 구축되는 것도 있지만 적절히 쿼리스트링을 혼재해서 쓰기도 한다.
- 검색, 페이지네이션, 정렬 등 매개변수가 많거나 복잡할 때 쿼리스트링을 쓰는게 좋다.
