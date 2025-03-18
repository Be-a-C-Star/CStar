# HTTP Header

HTTP 요청 시 헤더와 바디를 주고 받는다. 인터넷 요청을 할 때 웹 브라우저의 개발자 도구 - 네트워크 탭을 통해서 HTTP 통신의 헤더와 바디를 확인할 수 있다. 바디는 통신의 목적인 컨텐츠의 본문들이 담기며 헤더는 바디나 통신을 설명하는 메타 정보가 담겨있다. 헤더는 키 : 값 형태로 설정이 되며  일반 헤더, 요청 헤더, 응답 헤더가 자동으로 생성된다. 서버에서 설정하는 헤더를 응답 헤더, 클라이언트에서 설정한 헤더를 요청 헤더라 한다.

### 일반 헤더

요청한 URL, 메서드, 자원의 출처 노출 여부를 결정하는 보안 정도(Referrer Policy) 등이 포함된다.

### 요청 헤더

클라이언트가 서버에 요청할 때 클라이언트가 설정하거나 자동으로 설정되는 헤더로 메서드, 클라이언트 OS, 브라우저 정보 등이 포함된다.

### 응답 헤더

서버가 클라이언트에게 응답을 보낼 때 설정하거나 자동으로 설정되는 헤더를 뜻한다. 서버의 소프트웨어 정보 등이 포함되지만 대부분의 서버는 보안을 위해 서버 정보를 최대한 숨긴다.

### 응답 헤더 생성 실습

```js
    const http = require('http');
    const hostname = '127.0.0.1';
    const port = 3000;
    const server = http.createServer((req, res) => {
        res.setHeader('Content-Type', 'text/plain; charset=utf-8'); 
        res.setHeader('kundol', "i love you, but you don't love me"); 
        res.end('큰돌, 그는 신인가?!\n');
    });
    server.listen(port, hostname, () => {
        console.log(`Server running at http://${hostname}:${port}/`);
    });
```