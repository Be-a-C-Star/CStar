#### MTU(Maximum Transmission Unit)

###### 네트워크에 연결된 장치가 받아들일 수 있는 최대 데이터 패킷의 크기

- 패킷으로 쪼개질때 MTU를 기준으로 쪼개짐
- 네트워크 통신을 할 때 가능한 가장 큰 PDU의 크기를 뜻함
  - Ex. 터널의 높이제한 3m
- 네트워크 경로 상에 있는 아무 장치나 MTU보다 패킷이 크면 그 패킷은 분할될 수도 있음

- 이 때 통신을 하는 양쪽 끝은 두 장치의 MTU만이 아니라 중간의 모든 라우터, 스위치, 서버를 고려해야 함
- 네트워크 경로 상에 있는 아무 장치나 MTU보다 패킷이 크면 그 패킷은 분할될 수도 있음
  ![alt text](<스크린샷 2024-12-25 오후 2.27.56.png>)
- Router C의 MTU가 1400바이트이기 때문에 패킷이 분할되는 모습

---

##### 패킷이 분할되지 않는 경우

- 패킷을 분할할 수 없어 네트워크 경로 상에 있는 어떠한 라우터나 장치의 MTU를 초과할 때 분할해서 전달하는 것이 아니라 전달을 아예 하지 않을 수도 있음

##### IPv6

- IPv6는 분할을 허용하지 않음

##### IPv4

![alt text](<스크린샷 2024-12-25 오후 2.29.28.png>)

- IPv4 헤더에는 ags라는 필드가 있는데 여기서 bit가 1이되면 "Don't Fragment" 플래그가 활성화된다라는 의미이며 이 때 분할은 불가능함

---

#### MTU, MSS(Maximum Segment Size)

- MTU와 MSS의 차이를 보면 다음과 같이 MTU는 IP헤더와 TCP헤더의 크기까지 합치지만 MSS는 데이터의 크기(payload의 크기)만을 가리킴
- 일반적으로 MTU는 1500바이트이며 MSS는 1460 바이트임
- 따라서 네트워크를 통해 데이터를 보낼 때 MTU가 1500이라도 데이터는 보통 1460바이트 이하의 크기로 보내야 전달이 됨
- 다만, TCP를 쓰지 않는다는 등의 이유로 달라질 수도 있음
  ![alt text](<스크린샷 2024-12-25 오후 2.41.50.png>)
- 여기서 이더넷 헤더 14바이트 FCS 4바이트가 포함된 것을 통틀어 이더넷 프레임이라고 함

  > FCS(Frame Check Sequence)❓
  > 데이터의 에러검출을 돕기 위해 삽입되는 필드(CRC에 의해 생성된 값이 여기에 포함)

- 따라서 이더넷프레임의 크기는 일반적으로 1518바이트임

---

##### ping을 통한 MTU 확인

![alt text](<스크린샷 2024-12-25 오후 2.50.05.png>)
`ping www.google.com -f -l 1500`

- 앞의 코드는 1500바이트씩 패킷을 보낸다는 의미이며 DF는 패킷분할이 안되는 `"Don't Fragment”`를 의미
- 손실률 100%임을 보이고 있음

![alt text](<스크린샷 2024-12-25 오후 2.52.43.png>)
`ping www.google.com -f -l 1472`

- 여기서 ping의 경우 IP 헤더(20바이트) + ICMP 헤더(8바이트)로 요청을 보내는 것이기 때문에 MSS는 1472(1500 - 28)까지 됨
- 따라서 1472바이트씩 데이터를 보내면 올바르게 송수신되는 것을 볼 수 있음

##### netsh를 통한 MTU 확인

![alt text](<스크린샷 2024-12-25 오후 2.54.32.png>)
`netsh interface ipv4 show interfaces`

- MTU가 1500인 것을 알 수 있음

---

#### PMTUD

###### 수신자와 송신자의 경로 상에서 장치가 패킷을 누락한 경우 테스트 패킷의 크기를 낮추면서 MTU에 맞게끔 반복해서 보내는 과정임

Ex. MTU가 1500일 때, 보내려는 패킷이 1500보다 크다면, 패킷의 크기를 줄여가면서 다시 보냄
