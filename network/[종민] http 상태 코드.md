

## **1xx (정보 응답)**
> 요청이 수락되었으며, 처리가 계속 진행 중임을 나타낸다. 일반적으로 실무에서는 거의 사용되지 않는다.  

- **100 Continue**: 클라이언트가 보낸 요청의 일부를 수락했으며, 나머지를 계속 보내도 됨.  
- **101 Switching Protocols**: 클라이언트의 프로토콜 변경 요청을 서버가 수락함. (예: HTTP → WebSocket)  

---

## **2xx (성공)**
> 요청이 정상적으로 처리되었음을 나타낸다.  

- **200 OK**: 요청이 성공적으로 처리됨. (일반적인 성공 응답)  
- **201 Created**: 요청이 성공적으로 처리되었으며, 새로운 리소스가 생성됨. (POST 요청의 대표적인 응답)  
- **204 No Content**: 요청은 성공했지만, 반환할 데이터가 없음. (예: DELETE 요청 후)  

---

## **3xx (리다이렉션)**
> 요청을 완료하려면 추가 작업이 필요함.  

- **301 Moved Permanently**: 요청한 리소스의 URL이 영구적으로 변경됨. (SEO에서 중요)  
- **302 Found**: 요청한 리소스가 임시적으로 다른 URL로 이동됨.  
- **304 Not Modified**: 클라이언트가 캐시를 사용해야 함. (캐싱 관련)  

---

## **4xx (클라이언트 오류)**
> 클라이언트 측 문제로 요청이 실패함.  

- **400 Bad Request**: 요청이 잘못됨. (예: 유효하지 않은 JSON 데이터 전송)  
- **401 Unauthorized**: 인증이 필요함. (로그인이 필요할 때)  
- **403 Forbidden**: 접근이 거부됨. (권한 없음)  
- **404 Not Found**: 요청한 리소스를 찾을 수 없음. (잘못된 URL)  
- **405 Method Not Allowed**: 허용되지 않은 HTTP 메서드 사용. (예: `POST`만 허용된 API에 `GET` 요청)  
- **409 Conflict**: 리소스 충돌 발생. (예: 중복 데이터)  
- **429 Too Many Requests**: 너무 많은 요청을 보냄. (Rate Limiting)  

---

## **5xx (서버 오류)**
> 서버 측 문제로 요청이 실패함.  

- **500 Internal Server Error**: 서버 내부 오류 발생. (일반적인 서버 에러)  
- **502 Bad Gateway**: 서버가 잘못된 응답을 받음. (예: 프록시 서버가 다운됨)  
- **503 Service Unavailable**: 서버가 일시적으로 사용 불가능함. (예: 유지보수 중)  
- **504 Gateway Timeout**: 게이트웨이 서버가 응답을 받지 못함. (예: 백엔드 서버 다운)  

---

## **정리: 가장 중요한 상태 코드**
**✅ 필수적으로 기억해야 하는 상태 코드**  
- **200 OK** → 정상 응답  
- **201 Created** → 리소스 생성 (POST 요청)  
- **204 No Content** → 응답 본문 없음 (DELETE 요청 후)  
- **301 Moved Permanently** → URL이 영구적으로 변경됨  
- **302 Found** → 임시적으로 다른 URL로 이동됨  
- **304 Not Modified** → 캐시 사용  
- **400 Bad Request** → 잘못된 요청  
- **401 Unauthorized** → 인증 필요 (로그인 필요)  
- **403 Forbidden** → 접근 금지 (권한 없음)  
- **404 Not Found** → 존재하지 않는 리소스  
- **405 Method Not Allowed** → 잘못된 HTTP 메서드 사용  
- **409 Conflict** → 데이터 충돌 발생  
- **429 Too Many Requests** → 요청이 너무 많음 (Rate Limit 초과)  
- **500 Internal Server Error** → 서버 내부 오류  
- **502 Bad Gateway** → 게이트웨이 서버 오류  
- **503 Service Unavailable** → 서버가 다운됨  

