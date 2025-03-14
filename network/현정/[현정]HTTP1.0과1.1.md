## HTTP/1.0

- 수명이 짧은 연결
- 각 HTTP 요청 당 TCP 핸드셰이크가 발생되어 한 연결 당 하나의 요청을 처리하도록 설계
- 한번 연결할 때마다 TCP 연결을 계속 해야해서 RTT가 늘어나는 문제점이 있다

> RTT(Round Trip Time : 왕복 지연시간)는 신호를 전송하고 해당 신호의 수신 확인에 걸린 시간을 더한 값이자 어떤 메시지가 두 장치 사이를 왕복하는데 걸린 시간 

## HTTP/1.1

- HTTP/1.0의 단점을 보완
- keep-alive default
    - 매 요청마다 TCP를 연결하지 않고 한번 연결된 뒤 끊어질 때까지 계속 HTTP 통신이 가능하도록 함
    - TCP 연결 유지를 알려주는 헤더로 유지 시간인 timeout과 최대 요청수 max를 지정할 수 있다.

```js
const express = require('express'); 
const app = express();
app.get('/',(req, res) =>{
    res.json({"a" : 1})
})
const server = app.listen(12010); 
server.keepAliveTimeout = 30 * 1000;
```

- 호스트 헤더
    - HTTP/1.0은 하나의 호스트만 가진다고 가정하여 헤더에 호스트를 포함하지 않음
    - 서버는 여러 개의 호스트를 가질 수 있음
    - 유연성을 위해 헤더에 특정 호스트를 포함할 수 있게 변경되어 항상 호스트를 포함해서 요청하도록 변경

- 대역폭 최적화
    - 데이터 다운로드 중간에 연결이 끊겨도 이어서 받을 수 있게 변경
    - `Range:bytes=5000-` 라는 헤더를 추가하여 다운로드 재개 요청이 가능해짐

### 요청을 줄이기 위한 기술

HTTP 발전에도 불구하고 RTT는 계속 증가되기 때문에 이를 줄이기 위해 여러가지 기술들이 만들어짐

- 이미지 스프라이트 : 수 많은 이미지를 하나의 이미지로 만들어 하나의 이미지만 다운받고 이를 통해 수 많은 이미지를 다운받는 듯한 효과를 냄
- 코드 압축 : 코드를 압축해서 서빙
- 이미지 Base64 인코딩 : 이미지 파일을 64진법으로 이루어진 문자열로 인코딩, 이미지 서버에 대한 HTTP 요청을 할 필요가 없이 만드는 것을 말한다. 파일 크기가 36% 커지는 단점이 있다.

### HTTP/1.1의 고질적인 문제 : HOL

- Head Of Line Blocking
- 네트워크에서 같은 큐에 있는 패킷이 첫 번째 패킷에 의해 지연될 때 발생하는 성능 저하
- 먼저 다운 받는 파일이 큰 경우 뒤에 다운받을 파일이 지연된다