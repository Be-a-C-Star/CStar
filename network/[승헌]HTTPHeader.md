# HTTPHeader

## HTTP 헤더

- 클라이언트와 서버 간 요청(Request)과 응답(Response)에서 메타데이터를 전달하는 데 사용되는 정보들을 담고 있는 부분
- 기본적으로 일반적인 정보나 서버, 클라이언트가 이해해야 하는 추가적인 설정을 포함하고 있음

## HTTP 요청 헤더 (Request Headers)

- 클라이언트가 서버에 요청을 보낼 때 포함되는 헤더 정보들

### 주요 요청 헤더

1. Host: 요청하려는 서버의 호스트명과 포트번호를 지정
    
    Host: www.example.com
    
2. User-Agent: 요청을 보내는 클라이언트의 소프트웨어 정보 (브라우저 종류, OS 정보 등)
    
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36
    
3. Accept: 클라이언트가 서버로부터 받을 수 있는 미디어 타입 (콘텐츠 형식)을 지정
    
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
    
4. Authorization: 인증 정보를 담고 있는 헤더
    
    토큰이나 기본 인증 방식을 통해 인증을 전달할 때 사용
    
    Authorization: Bearer <token##>
    
5. Cookie: 클라이언트 측에 저장된 쿠키 값을 서버로 전달
    
    Cookie: user_id=12345; session_id=abcdef
    
6. Content-Type: 클라이언트가 서버로 전송할 데이터의 형식을 지정
    
    Content-Type: application/json
    
7. Accept-Encoding: 서버가 클라이언트에게 어떤 방식으로 응답을 압축할 수 있는지 지정
    
    Accept-Encoding: gzip, deflate, br
    

## HTTP 응답 헤더 (Response Headers)

- 서버가 클라이언트의 요청에 대한 응답을 보낼 때 포함되는 헤더 정보들

### 주요 응답 헤더

1. Content-Type: 서버가 응답으로 보내는 데이터의 MIME 타입을 지정
    
    Content-Type: text/html; charset=UTF-8
    
2. Content-Length: 응답 본문의 크기를 바이트 단위로 지정
    
    Content-Length: 1234
    
3. Set-Cookie: 서버가 클라이언트에 보내는 쿠키를 설정하는 데 사용
    
    Set-Cookie: session_id=abcdef; Path=/; HttpOnly
    
4. Location: 클라이언트가 리디렉션해야 할 URL을 지정. 주로 3xx 응답 코드와 함께 사용
    
    Location: https://www.example.com/newpage
    
5. Cache-Control: 응답에 대한 캐시 정책을 지정
    
    Cache-Control: no-cache, no-store, must-revalidate
    
6. Strict-Transport-Security (HSTS): HTTPS 프로토콜을 통해 연결을 강제하는 헤더
    
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    
7. Access-Control-Allow-Origin: CORS(Cross-Origin Resource Sharing) 관련 헤더, 다른 도메인에서의 요청을 허용할지 결정
    
    Access-Control-Allow-Origin: *
    

## HTTP 헤더의 역할

1. 정보 전달
    
    HTTP 헤더는 요청 및 응답에서 다양한 정보를 전달하는 역할
    
    *e.g.* Content-Type은 데이터의 형식, Authorization은 인증 정보를 전달
    
2. 네트워크 제어
    
    Cache-Control과 같은 헤더는 요청과 응답이 어떻게 캐시될지
    
    또는 리디렉션이 어떻게 처리될지 등 네트워크 동작을 제어하는 데 사용됨
    
3. 보안
    
    Authorization 헤더는 인증과 관련된 정보를 포함
    
    Strict-Transport-Security 헤더는 HTTPS를 사용하도록 클라이언트를 강제하여 보안을 강화
    
4. 헤더의 형식
    
    HTTP 헤더는 키: 값 형식으로 되어 있음
    
    *e.g.*
      
        Accept: application/json
        Content-Type: text/html
    
    각 헤더는 개별적으로 줄바꿈으로 구분, 요청 또는 응답의 끝에는 빈 줄(\r\n\r\n)이 있음
