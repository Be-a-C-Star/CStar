## **로컬스토리지(Local Storage)**

### **로컬스토리지란?**

**클라이언트 측에서 데이터를 영구적으로 저장할 수 있는 공간**

- 브라우저를 닫거나 컴퓨터를 껐다가 켜도 데이터가 남아있음
- window.localStorage를 통해 접근
- 보안이 중요한 데이터를 저장하는 것은 위험함

### **로컬스토리지와 오리진**

- 오리진(Origin, 동일 출처 정책)에 따라 웹사이트마다 **각각의 로컬스토리지 공간**을 가짐
- https://example.com의 로컬스토리지는 https://another.com에서 접근할 수 없음
- 동일 도메인, 프로토콜, 포트에서만 공유 가능

### **로컬스토리지 사용법**

```
// 데이터 저장
localStorage.setItem("key", "value");

// 데이터 가져오기
const data = localStorage.getItem("key");
console.log(data); // "value"

// 데이터 삭제
localStorage.removeItem("key");

// 모든 데이터 삭제
localStorage.clear();
```

### **활용 사례**

- **다크 모드**
- **장바구니 데이터 (로그인 없는 경우)**
- **폼 입력 값 자동 저장**

## **세션스토리지(Session Storage)**

### **세션스토리지란?**

**브라우저가 열려 있는 동안에만 데이터를 유지하는 저장소**

- **브라우저를 닫으면 데이터가 자동 삭제**
- 같은 탭에서만 데이터 공유 가능, **새로운 탭이나 창에서는 접근 불가**
- window.sessionStorage를 통해 접근 가능

### **세션스토리지 사용법**

```
// 데이터 저장
sessionStorage.setItem("isLoggedIn", "true");

// 데이터 가져오기
const isLoggedIn = sessionStorage.getItem("isLoggedIn");
console.log(isLoggedIn); // "true"

// 데이터 삭제
sessionStorage.removeItem("isLoggedIn");

// 모든 데이터 삭제
sessionStorage.clear();
```

### **활용 사례**

- **로그인 세션 유지 (탭이 닫히면 자동 로그아웃)**
- **페이지 이동 시 상태 유지 (ex: 특정 탭에서만 활성화된 필터링 상태 저장)**

## **쿠키(Cookie)**

### **쿠키란?**

**작은 데이터 조각을 브라우저에 저장하여 서버와 클라이언트 간 상태를 유지**하는 데 사용

- 서버 또는 클라이언트에서 생성 가능
- 특정 시간이 지나면 자동 삭제 가능 (유효 기간 설정)
- HTTP 요청 시 자동으로 서버에 전송됨

### **쿠키의 종류**

| **쿠키 종류** | **설명** |
| --- | --- |
| **세션 쿠키** | 브라우저가 닫힐 때 삭제됨 (expires 설정 없음) |
| **영구 쿠키** | 만료일(expires)을 설정하여 유지 가능 |
| **HTTPOnly 쿠키** | JavaScript에서 접근 불가 (보안 강화) |
| **Secure 쿠키** | HTTPS에서만 전송 가능 |
| **SameSite 쿠키** | CSRF 공격 방지 (strict, lax, none 설정 가능) |

### **쿠키 문법**

**(1) 쿠키 설정 (JavaScript)**

쿠키 저장

`document.cookie = "username=chatgpt; path=/; max-age=3600";`

- path=/ → 사이트 전역에서 쿠키 사용
- max-age=3600 → 1시간 동안 유지

**(2) 쿠키 가져오기**

`document.cookie`

**(3) 쿠키 삭제**

`document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/";`

- 직접 삭제하는 메서드 없음
- 만료날짜를 과거로 설정해서 삭제 + 빈 값 덮어쓰기
- 경로는 동일하게 지정해서 기존 쿠키 찾아서 삭제할 수 있도록 함

### **활용 사례**

- **사용자 로그인 정보 저장 (*e.g.* 자동 로그인)**
- **방문자 트래킹 및 분석 (*e.g.* GA 같은 웹 트래킹 도구)**
- **광고 및 개인화된 콘텐츠**

## **로컬스토리지 vs 세션스토리지 vs 쿠키 비교**

| **항목** | **로컬스토리지** | **세션스토리지** | **쿠키** |
| --- | --- | --- | --- |
| **수명** | 영구 저장 | 탭/브라우저 닫으면 삭제 | 유효기간 설정 가능 |
| **저장 가능 용량** | 5MB 이상 | 5MB 이상 | 4KB |
| **서버 전송 여부** | X (클라이언트 전용) | X (클라이언트 전용) | O (HTTP 요청마다 서버로 전송) |
| **보안** | X (JS로 접근 가능) | X (JS로 접근 가능) | O (HttpOnly 설정 시 JS 접근 불가) |
| **접근 가능 범위** | 같은 오리진 내 모든 페이지 | 같은 탭 내에서만 가능 | 같은 도메인 내에서만 가능 |
| **사용 목적** | 장기 데이터 저장 (설정, 테마, 캐시) | 세션 내 상태 저장 (폼, 일시적 상태) | 로그인 유지, 인증 정보, 서버와 상태 유지 |

## 캐싱전략

### **로컬스토리지**

- **사용자의 설정 정보를 저장해야 할 때**
- **장바구니, 다크모드 등 개인화된 데이터 유지가 필요할 때**

### **세션스토리지**

- **로그인 상태를 탭에서만 유지하고 싶을 때**
- **새로고침은 유지, 브라우저를 닫으면 삭제**

### **쿠키**

- **자동 로그인**
- **서버와 인증 정보 통신 필요할 때**
- **CSRF 보호, 보안이 중요한 정보 저장 (HttpOnly 쿠키 활용)**

### **실제 프로젝트에서 어떻게 활용할까?**

1. **자동 로그인 기능 (쿠키 + 로컬스토리지)**

```
// 로그인 시, 서버에서 JWT 토큰을 받아 로컬스토리지와 쿠키에 저장
localStorage.setItem("token", "my-jwt-token");
document.cookie = "authToken=my-jwt-token; Secure; HttpOnly;";
```

2. **장바구니 정보 저장 (로컬스토리지)**

```
const cart = JSON.parse(localStorage.getItem("cart")) || [];
cart.push({ id: 1, name: "product A" });
localStorage.setItem("cart", JSON.stringify(cart));
```

3. **입력폼 상태 저장 (세션스토리지)**

```
sessionStorage.setItem("temp", JSON.stringify({ email: "mail@example.com" }));
```
