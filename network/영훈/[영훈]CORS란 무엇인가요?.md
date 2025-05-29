### CORS

###### HTTP 헤더를 기반으로 브라우저가 다른 오리진에 대한 리소스(이미지, CSS, JS, 비디오 등) 로드를 허용할지 말지에 대한 메커니즘

CORS를 알아보기 앞서 오리진과 SOP를 먼저 알아보자.

#### 오리진

![alt text](<스크린샷 2025-03-18 오전 11.11.57.png>)

- 사진과 같이 오리진은 프로토콜, 호스트 네임, 포트까지 합친 주소를 뜻한다.
- https://shopping.naver.com/ns/home 이라는 주소가 있을때, https://shopping.naver.com 까지가 오리진이다.
- 뒤에 오는 주소가 다르다고 해도 앞에 오리진만 같다면 같은 오리진이라고 한다.
- 여기서 포트 번호가 왜 없는지 의문이 들수가 있는데, https의 기본 포트 번호가 443이고 포트 번호는 생략 가능하다.
- www.naver.com이라는 주소에서 www를 호스트라고도 한다.

#### SOP(Same-Origin Policy)

###### 브라우저 상에서 오로지 같은 오리진끼리만 요청을 허가하는 보안 정책

![alt text](<스크린샷 2025-03-18 오전 11.19.19.png>)

- 예를 들어 은행 계좌서버에 로그인하고 악의적인 사이트를 방문하면 내가 모르는 사이에 은행 서버에 요청을 할 수 있게 되어 내 계정 정보가 변경되거나 유출될 수 있다.
- 즉, 악성스크립트가 다른 오리진의 서버에 요청을 보내고 사용자의 리소스를 임의적으로 접근할 수 있는 것이다.
  - 이를 1차적으로 막아주는 것이 바로 SOP 보안 정책이다.
    <br/>

![alt text](<스크린샷 2025-03-18 오전 11.23.05.png>)

- 하지만 다른 오리진끼리 요청해야 하는 순간도 있다.
- 프론트와 백엔드 개발 환경에서 프론트는 보통 3000번 포트로 개발을 하고, 백엔드는 12010 포트로 개발을 한다. 포트 번호가 다르면 다른 오리진이므로 프론트에서 백엔드로 API 요청을 했을때 허용되지 않은 오리진이라면 에러가 날 것이다.
- 또 서비스를 만들 때 사용하는 Open API(구글, 카카오 로그인 등)도 다른 오리진으로 요청을 하게 된다.
- 이렇게 SOP를 브라우저 상에서 조금 더 유연하게 바꿔서 어떠한 경우에는 다른 오리진끼리도 요청 및 응답할 수 있게 만든 메커니즘이 CORS다.

#### Preflight Request와 Simple Request

만약 요청을 보낼 때 다음의 메서드 타입, 헤더에 해당되지 않은게 하나라도 포함이 되어있다면 **Preflight Request**를 보내게 된다.

반대로 다음의 메서드 타입, 헤더를 모두 가진 요청을 **간단한 요청(Simple Request)** 이자 **안전한 요청**이라고 한다.

##### 메서드 타입

- GET
- HEAD
- POST

##### 헤더

- Accept
- Accept-Language
- Content-Language
- Content-Type
  - application/x-www-form-urlencoded
  - multipart/form-data
  - text/plain
- Range (참고로 fireforx 브라우저는 해당 헤더를 허용하지 않습니다.)

프론트엔드 개발자는 주로 서버에 데이터를 요청할때 헤더에 `'Content-Type': 'application/json'` 을 담아서 보내기때문에 대부분의 요청은 Preflight Request으로 보내지게 된다.

그러면 브라우저가 서버에 요청을 하고 CORS를 확인하는 과정을 살펴보자.

1. 요청 시 헤더에 Origin(Ex. http://localhost:3000) 필드를 담아서 보냄
2. 서버에서 응답 헤더로 Access-Control-Allow-Origin(허용할 출처)를 담아서 브라우저로 보냄. 이 때, 브라우저는 Access-Control-Allow-Origin 필드에 Origin에서 보냈던 출처가 있는지 검토한다.
3. 일치하는 출처가 없으면 CORS 에러를 반환한다.

#### preflight request과정

1. OPTIONS 메서드로 해당 서버에 원래의 요청을 보내기 전 요청을 보냄.
2. 요청을 받은 서버는 Access-Control-\* 헤더로 응답.
3. Access-Control-\* 헤더에 요청한 오리진이 존재하지 않는다면 CORS 에러를 보냄.

##### Access-Control-\* 헤더 종류

- Access-Control-Allow-Headers: Content-Type
- Access-Control-Allow-Origin: https://kundol.com
- Access-Control-Allow-Methods: GET, DELETE, HEAD, OPTIONS
  이외에도 Access-Control-Max-Age 등이 있음.
