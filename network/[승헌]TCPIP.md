# TCP/IP

## TCP/IP

### TCP/IP??

- Transmission Control Protocol / Internet Protocol
- 네트워크 통신을 가능하게 해주는 표준 프로토콜 스택(4계층)
- 데이터 분할, 전송, 라우팅, 복구 등 모든 과정 관리

### 특징

1. 계층구조: 4계층 모델로 동작, 독립적 설계와 관리
2. 표준화: 전 세계 표준으로 다양한 환경에서 호환성
3. 유연성: 네트워크 크기 관계 없이 작동
4. 신뢰성: 데이터 손실 방지 및 복구 메커니즘 제공

### 계층구조

- 네트워크 액세스
- 인터넷
- 전송(Transport)
- 애플리케이션

### 계층 구성의 이유

1. 역할분담
    1. 각 계층이 특정 기능만 집중적으로 처리하도록 설계
    2. 전송 계층의 데이터 신뢰성 보장, 인터넷 계층의 데이터 전달
2. 유연성
    1. 새로운 프로토콜 추가/수정 시 다른 계층에 영향 없음
    2. 계층 간 인터페이스가 명확히 정의됨
3. 효율성
    1. 하드웨어, 소프트웨어 모두 계층 구조를 따름
    2. 네트워크 장치 한 호환성 뛰어남

## TCP/IP 계층 모델

### Application Layer, 애플리케이션 계층

- 사용자와 직접 상호작용
- HTTP, FTP, SMTP, DNS 등
- 사용자와 애플리케이션이 네트워크와 통신하도록 도움
- *e.g.* 웹 브라우저 - 서버 HTTP 통신

※ FTP: 파일 전송용, 21번포트, 데이터 암호화X → 보안취약

※ SMTP: Simple Mail Transfer Protocol
