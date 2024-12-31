### 애플리케이션 계층

###### HTTP, SMTP, SSH, FTP가 대표적이며 웹 서비스, 이메일 등 서비스를 실질적으로 사람들에게 제공하는 층

#### HTTP

###### 웹에서 클라이언트(브라우저 등)와 서버(웹 서버) 간에 문서나 리소스를 교환하기 위한 프로토콜

- HTTP(Hypertext Transfer Protocol)은 처음에는 서버와 브라우저간에 데이터를 주고 받기 위해 설계된 프로토콜임
- 현재는 브라우저 뿐만 아니라 서버와 서버간의 통신할 때도 많이 이용함

##### 1. HTTP는 헤더를 통한 확장이 쉬움

![alt text](<스크린샷 2024-12-25 오후 3.10.58.png>)

- Ex. 헤더값에다가 어떠한 값을 넣어서 HTTP요청을 할 때 쉽게 다른 값을 추가할 수 있음

##### 2. HTTP는 stateless 함

![alt text](<스크린샷 2024-12-25 오후 3.12.51.png>)

- 클라이언트와 서버 사이에 상태를 유지하지 않음
- 동일한 연결에서 연속적으로 수행되는 두 요청 사이에 연속적인 상태(state)값은 없음

##### 3. TCP/IP 위에서 동작

- HTTP 요청과 응답은 일반적으로 TCP 연결(포트 80, HTTPS는 443) 위에서 전송됨
- TCP 연결을 맺은 뒤, 그 위에서 HTTP 패킷(메시지 형식)을 주고받는 구조임

---

#### SSH(Secure SHhell Protocol)

###### 보안되지 않은 네트워크에서 네트워크 서비스를 안전하게 운영하기 위한 암호화 네트워크 프로토콜

![alt text](<스크린샷 2024-12-25 오후 3.15.35.png>)

`ssh <pem> <user>@<serverIP>`

- 보통 프라이빗 키가 있는 경로에서 이런식으로 키를 명시하고 실행함

![alt text](<스크린샷 2024-12-25 오후 3.16.28.png>)

- 접근해서 이처럼 리눅스 명령어를 통해 CLI 환경에서 작업을 진행함
- 또한 SCP를 이용해서 SSH를 이용해 파일을 전송 가능함

`scp <source> <destination>`

---

#### FTP(File Transfer Protocol)

###### 노드와 노드간의 파일을 전송하는데 사용되는 프로토콜. 지금은 파일을 암호화해서 전송하는 FTPS 또는 SFTP로 대체되고 있음

![alt text](<스크린샷 2024-12-25 오후 3.20.17.png>)

- 대표적인 FTP 소프트웨어로 **파일질라**가 있음
- 로컬 PC에서 원격 서버로 파일을 보내는 모습

---

#### SMTP(Simple Mail Transfer Protocol)

###### 인터넷을 통해 메일을 보낼 때 사용되는 프로토콜

- 보통 서비스를 운영하면 메일링 서비스를 하게 되는데 node.js를 통해 메일을 보낸다면 이를 통해 보내야 함

![alt text](<스크린샷 2024-12-25 오후 3.23.24.png>)

- 자바스크립트 진영에서는 Nodemailer라는 라이브러리가 있음

> Nodemailer❓
> JS를 기반으로 SMTP를 통해 메일을 보낼 수 있는 라이브러리

```javascript
// create reusable transporter object using the default SMTP transport
let transporter = nodemailer.createTransport({
  host: "smtp.ethereal.email",
  port: 587,
  secure: false, // true for 465, false for other ports
  auth: {
    user: testAccount.user, // generated ethereal user
    pass: testAccount.pass, // generated ethereal password
  },
});
```

- 이런식으로 설명을 보면 SMTP를 통해서 보낸다라고 돼있음
