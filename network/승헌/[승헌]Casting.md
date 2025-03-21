## 네트워크의 캐스팅

### 캐스팅?

데이터를 전달할 때의 **전송 방식**

데이터를 여러 수신자에게 보내는 방법에 따라 나뉨

## 유니캐스트

### 특징

- 1:1 통신 방식
- 데이터를 특정한 하나의 대상에게만 전송
- 가장 일반적인 전송 형태
- 개별 데이터 통신으로 비효율적일 수 있음

### 예시

- HTTP 통신

## 멀티캐스트

### 특징

- 1:N 통신 방식(다수)
- 데이터를 특정 그룹의 수신자들에게만 전송
- 브로드캐스트보다 효율적
- 네트워크 장치(스위치, 라우터)가 멀티캐스트 지원해야 함

### 예시

- 실시간 화상회의
- 온라인 스트리밍

***라우터:** 서로 다른 네트워크를 연결하고, 네트워크 간의 데이터 흐름을 제어하는 역할*

***스위치:** 하나의 네트워크 내에서 데이터를 전송하고 장치들 간의 연결을 담당*

## 브로드캐스트

### 특징

- 1:N 통신 방식(전체)
- 데이터를 네트워크 내 모든 기기에 전송
- 네트워크 부하 클 수 있음
- 일반적으로 로컬 네트워크(LAN)에서만 사용됨

### 예시

- ARP(Address Resolution Protocol) 요청
    - 데이터 전송 시 네트워크 계층의 IP주소 뿐만 아니라, 데이터 링크 계층의 MAC 주소가 필요함
    - MAC 주소: 네트워크 인터페이스 카드의 고유 식별자
    - IP주소에 대응하는 MAC 주소를 알아내기 위해
    - 네트워크 인터페이스 카드(NIC): 컴퓨터의 네트워크 연결을 위한 하드웨어 장치
- 네트워크 초기 설정

## 애니캐스트

### 특징

- 1:N 중 가장 가까운 1개 대상에게 데이터 전송
- 전송 지연(latency) 최소화 가능

### 예시

- DNS 요청
- CDN(Content Delivery Network) 서버 연결
