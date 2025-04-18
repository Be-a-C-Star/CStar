# IP주소, MAC주소, ARP, RARP

IP(Internet Protocol address)는 논리적 주소이며 컴퓨터 네트워크에서 장치들이 서로를 인식하고 통신을 하기 위해서 사용하는 특수한 번호이며, 물리적 주소인 MAC 주소를 통해 통신한다.

## MAC 주소

MAC 주소 (Media Access Control Address)는 네트워크 인터페이스에 할당된 고유식별자이며 보통 장치의 NIC에 할당된다.

48비트로 이루어져있으며 24비트의 OUI와 24비트의 UAA로 이루어져있다.

- OUI: IEEE에서 할당한 제조사 코드
- UAA: 제조사에서 구별되는 코드

MAC 주소는 유일하지만 유일하지 않을 수 있다. 실수 또는 의도적으로 UAA를 중복되게 만들 수도 있다. 동일 네트워크에서만 중복되지 않으면 문제없긴 하다. 또한 NIC에 고정된 MAC 주소를 변경할 수는 있으나 하지 않는 것을 권장하며 하는 것 자체를 어렵게 한 OS도 있다.

## IEEE (Institute of Electrical and Electronics Engineers)

전기/전자/전산 분야의 국제기구 및 학회이다.

## PC의 NIC

## ARP와 RARP

MAC 주소는 ARP를 통해 파악이 가능하다. ARP를 통해 논리적 주소인 IP 주소를 물리적 주소인 MAC 주소로 변환한다. 이와 반대로 RARP를 통해 물리적 주소인 MAC 주소를 논리적 주소인 IP 주소로 변환하기도 한다.

## ARP의 과정

1. 해당 IP주소에 맞는 MAC 주소를 찾기 위해 해당 데이터를 "브로드캐스팅" 을 통해 연결된 네트워크에 있는 장치한테 모두 보낸다.
2. 맞는 장치가 있다면 해당 장치는 보낸 장치에게 유티캐스트로 데이터를 전달해 주소를 찾게 된다.
