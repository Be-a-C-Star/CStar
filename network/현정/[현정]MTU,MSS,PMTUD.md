## MTU

- Maximum Transmission Unit
- 네트워크에 연결된 장치가 받아들일 수 있는 최대 데이터 패킷의 크기
- 해당 크기를 기준으로 데이터를 쪼개 패킷화한다
- 네트워크 경로 상의 아무 장치든 MTU보다 패킷이 크면 해당 패킷은 분할될 수 있다

### 패킷이 분할되지 않는 경우

패킷을 분할할 수 없고 네트워크 경로상의 어떤 장치의 MTU를 초과할 때 패킷을 아예 전달하지 않을 수도 있다

- IPv6는 분할을 허용하지 않는다
- IPv4에서 flags 필드에 bit가 1이되면 플래그가 활성화되어 분할이 불가능하다

## MTU와 MSS

- MTU는 IP헤더와 TCP헤더의 크기까지 합치지만 MSS는 데이터의 크기만을 가리킨다
- 일반적으로 MTU는 1500byte, MSS는 1460byte
- 데이터를 보낼 때 보통 1460byte 이하의 크기로 보내야 전달이 된다 (여러 이유로 달라질 수 있음)

### MTU 확인

```
ping www.google.com -f -l 1500
```
```
netsh interface ipv4 show interfaces
```

`ping`, `netsh` 명령어를 통해 MTU를 확인할 수 있다


## PMTUD

- Path MTU Discovery
- 경로 상에서 장치가 패킷을 누락한 경우 테스트 패킷의 크기를 낮추며 MTU에 맞게끔 반복해서 보내는 과정