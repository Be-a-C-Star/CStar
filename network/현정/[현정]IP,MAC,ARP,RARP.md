## IP 주소

```
ipconfig
```

- Internet Protocol address
- 논리적 주소로 네트워크에서 장치들이 서로를 인식하고 통신하기 위해 사용하는 특수한 번호

## MAC 주소

```
ipconfig/all
```

- Media Access Control Address
- 네트워크 인터페이스에 할당된 고유 식별자
- 장치의 NIC에 할당
- 48bit
- OUI(24bit) : IEEE에서 할당한 제조사 코드
- UAA(24bit) : 제조사에서 구별되는 코드
- 물리적 주소에서 앞 3자리는 OUI, 뒤 3자리는 UAA가 된다
- 유일하지 않을 수도 있다

## IEEE

- Institute of Electrical and Electronics Engineers
- 전기/전자/전산 분야의 국제 기구 및 학회

## ARP와 RARP

- MAC 주소는 ARP를 통해 파악이 가능하다
- ARP를 통해 IP 주소를 MAC 주소로 변환
- RARP를 통해 MAC 주소를 IP 주소로 변환

### ARP의 과정

1. IP 주소에 맞는 MAC 주소를 찾기 위해 브로드캐스팅을 통해 연결된 장치에 데이터를 모두 보냄
2. 맞는 장치가 있다면 해당 장치는 데이터를 보낸 장치에게 유니캐스트로 데이터를 전달해 주소를 찾게 됨
