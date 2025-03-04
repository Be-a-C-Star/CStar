

### **RESTful URL 규칙**
REST API를 설계할 때 URL은 다음과 같은 규칙을 따라야 합니다.

1. **리소스는 명사로 표현**  
   - `GET /users` (사용자 목록 조회)  
   - `POST /users` (새 사용자 추가)  
   - `GET /users/{id}` (특정 사용자 조회)  

2. **소문자 사용**  
   - `/Users` ❌ → `/users` ✅  

3. **하이픈(-) 사용, 밑줄(_) 지양**  
   - `/user-profile` (✅) vs `/user_profile` (❌)  

4. **HTTP 메서드를 활용한 동작 표현**  
   - `GET /products` (제품 목록 조회)  
   - `POST /products` (새로운 제품 추가)  
   - `PUT /products/{id}` (제품 정보 수정)  
   - `DELETE /products/{id}` (제품 삭제)  

5. **동사 대신 명사 사용**  
   - `/getUser` ❌ → `/users/{id}` ✅  

6. **계층적 구조 유지**  
   - `GET /users/{id}/orders` (특정 사용자의 주문 내역 조회)  

7. **필터링, 정렬, 페이징은 쿼리스트링 사용**  
   - `GET /products?category=electronics&sort=price&page=2`  
