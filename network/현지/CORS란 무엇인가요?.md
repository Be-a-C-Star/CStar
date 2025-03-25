# CORS란 무엇인가요?

## origin

출처는 아래 3가지 요소가 완전히 일치해야 동일한 출처로 간주된다.

- 프로토콜 (http / https)
- 도메인 (example.com)
- 포트 (80, 443 등)

## SOP (Same Origin Policy)

SOP는 웹 보안 모델 중 하나로, 서로 다른 출처(origin) 간의 리소스 접근을 제한하는 정책이다.
웹 사이트 간 악의적인 요청(예: CSRF, XSS 등)을 방지하기 위해 브라우저가 기본적으로 적용하는 보안 정책이다.

## CORS의 의미

CORS는 SOP의 보안 정책을 유지하면서도, 특정 출처(origin)에 한해 리소스 공유를 허용하는 방법이다.
즉, 서버가 특정 도메인에서 오는 요청을 허용하도록 설정하면, SOP의 제한을 우회할 수 있다.

### preflight request와 simple request

CORS 요청은 Simple Request(단순 요청) 와 Preflight Request(사전 요청) 두 가지로 나뉜다.

- Simple Request: 사전 요청(Preflight) 없이 바로 서버로 요청을 보내는 방식이다. 브라우저가 추가적인 확인 없이 바로 요청을 수행하고 응답을 처리한다.

- Preflight Request: Simple Request 조건을 만족하지 않는 요청은 브라우저가 요청 전에 OPTIONS 메서드로 사전 확인(Preflight Request)을 보낸다. 실제 요청 전에 서버가 해당 요청을 허용하는지 확인하는 과정이다.

### Access-Control-Allow-Origin 헤더

Access-Control-Allow-Origin은 CORS 정책에서 특정 출처(origin)에서의 요청을 허용할지를 결정하는 HTTP 응답 헤더다.
클라이언트(브라우저)가 요청을 보냈을 때, 서버가 이 헤더를 응답에 포함하면 브라우저는 해당 출처에서의 요청을 허용한다.
