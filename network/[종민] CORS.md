### 📌 SOP (Same-Origin Policy, 동일 출처 정책)
SOP는 웹 보안 정책 중 하나로, 브라우저에서 서로 다른 출처(Origin) 간의 리소스 접근을 제한하는 원칙이다. SOP가 적용되면 한 출처에서 실행 중인 스크립트가 다른 출처의 리소스에 직접 접근할 수 없다.

#### 🔹 **Origin(출처)**
- URL을 기준으로 **프로토콜 + 호스트 + 포트**가 모두 일치하는 경우 같은 출처(Same-Origin)로 간주한다.
- 예를 들어, 다음 두 URL은 다른 출처(Different-Origin)이다.
  - `https://example.com:443` ⬅️✅ (Same-Origin)
  - `https://example.com:8443` ⬅️❌ (Different-Origin, 포트 다름)
  - `http://example.com:443` ⬅️❌ (Different-Origin, 프로토콜 다름)
  - `https://sub.example.com:443` ⬅️❌ (Different-Origin, 도메인이 다름)

#### 🔹 **SOP의 영향**
- **XHR 및 Fetch API**: 기본적으로 다른 출처의 리소스를 요청할 수 없다.
- **DOM 조작**: 다른 출처의 페이지 내용을 직접 변경하거나 읽을 수 없다.
- **쿠키 및 저장소**: 브라우저의 `localStorage`, `sessionStorage`, `IndexedDB` 등은 동일 출처에서만 접근 가능하다.

#### 🔹 **예외 사항**
- `<script>` 태그를 통한 **JSONP 방식 요청** (과거에 많이 사용됨)
- **CORS (Cross-Origin Resource Sharing) 정책을 허용하는 서버에서 제공하는 리소스**
- **프록시 서버를 활용한 동일 출처 우회**

---

### 📌 CORS (Cross-Origin Resource Sharing)
CORS는 **SOP의 제한을 우회하기 위한 보안 메커니즘**으로, 서버가 특정 출처에서의 요청을 허용할 수 있도록 한다. 서버는 특정 HTTP 헤더(`Access-Control-*`)를 응답에 포함하여 허용 여부를 브라우저에게 전달한다.

#### 🔹 **CORS 동작 방식**
1. 클라이언트(브라우저)가 다른 출처(Origin)의 서버에 HTTP 요청을 보낸다.
2. 서버가 해당 출처의 요청을 허용하는지 결정한다.
3. 허용되면 `Access-Control-Allow-Origin` 등의 CORS 관련 헤더를 포함하여 응답한다.
4. 브라우저는 응답을 검사한 후 요청을 허용하거나 차단한다.

#### 🔹 **CORS 관련 주요 HTTP 응답 헤더**
| 헤더 | 설명 |
|------|------|
| `Access-Control-Allow-Origin` | 허용할 Origin(예: `https://example.com`) 또는 `*`(모든 Origin 허용) |
| `Access-Control-Allow-Methods` | 허용할 HTTP 메서드(예: `GET, POST, PUT`) |
| `Access-Control-Allow-Headers` | 요청 시 허용할 헤더(예: `Content-Type, Authorization`) |
| `Access-Control-Allow-Credentials` | `true` 설정 시 인증 정보(Cookie, Authorization) 포함 요청 허용 |
| `Access-Control-Expose-Headers` | 클라이언트에서 접근 가능한 응답 헤더 지정 |

---

### 📌 Preflight Request (사전 요청)
브라우저는 보안상의 이유로 특정한 CORS 요청에 대해 실제 요청을 보내기 전에 **Preflight Request(사전 요청)** 을 수행한다. 이는 `OPTIONS` 메서드를 사용하여 서버가 요청을 허용하는지 확인하는 과정이다.

#### 🔹 **Preflight Request가 발생하는 조건**
다음 조건 중 하나라도 만족하면 브라우저는 **Preflight Request** 를 먼저 수행한다.
1. `Content-Type`이 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain` **이외의 값**인 경우
2. `Authorization`, `X-Custom-Header` 등 **CORS-safe 헤더 이외의 헤더**가 포함된 경우
3. `PUT`, `DELETE`, `PATCH` 등 **`GET` 또는 `POST` 이외의 HTTP 메서드**를 사용하는 경우
4. 요청에 **`credentials: include` 옵션이 사용된 경우**

#### 🔹 **Preflight Request 예제**
```http
OPTIONS /api/data HTTP/1.1
Origin: https://example.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-Custom-Header
```
위와 같이 클라이언트가 `OPTIONS` 요청을 보내면, 서버는 허용 여부를 응답해야 한다.

#### 🔹 **서버의 응답**
```http
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: POST
Access-Control-Allow-Headers: X-Custom-Header
Access-Control-Max-Age: 86400
```
- `Access-Control-Max-Age`: 사전 요청 결과를 캐시하는 시간(초 단위)

---

### 📌 Simple Request (단순 요청)
CORS 요청 중 **Preflight Request 없이 바로 요청이 가능한 경우**를 **Simple Request(단순 요청)** 이라고 한다.

#### 🔹 **Simple Request 조건**
1. HTTP 메서드는 **GET, POST, HEAD** 중 하나여야 한다.
2. `Content-Type`은 다음 중 하나여야 한다.
   - `application/x-www-form-urlencoded`
   - `multipart/form-data`
   - `text/plain`
3. **CORS-safe 헤더만 포함**해야 한다.
   - `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`(위 3가지 값 중 하나일 때)

#### 🔹 **Simple Request 예제**
```http
GET /api/data HTTP/1.1
Origin: https://example.com
```
서버가 CORS를 허용하면 응답 시 다음과 같은 헤더를 포함한다.
```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://example.com
```
위와 같은 응답을 받으면 브라우저는 요청을 정상적으로 처리한다.

---

### 📌 Access-Control-* 헤더
CORS를 제어하는 HTTP 헤더들은 `Access-Control-` 접두사를 가진다.

| 헤더 | 설명 |
|------|------|
| `Access-Control-Allow-Origin` | 요청을 허용할 출처 (Origin) 지정 |
| `Access-Control-Allow-Methods` | 허용할 HTTP 메서드 지정 |
| `Access-Control-Allow-Headers` | 요청에서 허용할 HTTP 헤더 지정 |
| `Access-Control-Allow-Credentials` | `true` 설정 시 인증 정보 포함 요청 허용 |
| `Access-Control-Expose-Headers` | 클라이언트가 접근할 수 있도록 노출할 헤더 지정 |
| `Access-Control-Max-Age` | Preflight Request의 캐시 유지 시간 지정 |

---

### 📌 정리
1. **SOP(Same-Origin Policy)**: 웹 보안 정책으로, 다른 출처 간의 리소스 접근을 제한함.
2. **CORS(Cross-Origin Resource Sharing)**: SOP의 제한을 우회하기 위한 정책으로, 서버가 특정 출처의 요청을 허용할 수 있도록 설정.
3. **Preflight Request(사전 요청)**: CORS 요청이 특정 조건을 충족하면 브라우저가 먼저 `OPTIONS` 요청을 보내 서버가 허용하는지 확인.
4. **Simple Request(단순 요청)**: 특정 조건을 만족하는 CORS 요청으로, `OPTIONS` 사전 요청 없이 바로 처리됨.
5. **Access-Control-* 헤더**: 서버가 CORS 요청을 제어하기 위해 응답에 포함하는 HTTP 헤더.
