#### LAN(local area network)

![](<스크린샷 2024-12-23 오후 3.39.02.png>)
![](<스크린샷 2024-12-23 오후 3.39.18.png>)

- 소규모네트워크(집, 사무실) - 보통 허브나 스위치로 연결된 네트워크를 말함
- 하나의 논리적 주소인 IP를 기반으로 여러개의 물리적 주소인 MAC 주소로 구별하는 네트워크라고도 볼 수 있음
- MAN, WAN보다 높은 안정성, 속도를 가짐
- 유선매체로는 Ethernet 케이블이 주요 사용되며, 무선 기술로는 Wi-Fi가 사용됨
  ![](<스크린샷 2024-12-23 오후 4.23.17.png>)
  > 구체적인 지표에 의한 정의❓
  > 네트워크 매체를 이용하여 동일한 Subnet Mask, 다시 말해 같은 IP 대역을 사용하며 Address Resolution Protocol(ARP)가 닿는 네트워크 매체와 컴퓨터를 묶는 컴퓨터 네트워크
- 예를 들어 네트워크 매체에 연결된 컴퓨터들이 192.168.1.x/24의 IP를 사용하고 Subnet Mask가 일치한다면 이는 같은 LAN에 속해있다고 할 수 있음

##### IP 주소

- IPv4와 IPv6가 있음

| 특징              | IPv4                             | IPv6                                                           |
| ----------------- | -------------------------------- | -------------------------------------------------------------- |
| 주소 길이         | 32비트                           | 128비트                                                        |
| 주소 형식         | 점 구분 10진수 (예: 192.168.0.1) | 콜론 구분 16진수 (예: 2001:0db8:85a3:0000:0000:8a2e:0370:7334) |
| 주소 개수         | 약 43억 개                       | 사실상 무한대                                                  |
| 헤더 크기         | 20-60바이트                      | 40바이트 고정                                                  |
| 헤더 복잡성       | 비교적 단순                      | 더 많은 확장성과 옵션                                          |
| 브로드캐스트 지원 | 지원                             | 지원하지 않음 (멀티캐스트와 애니캐스트 사용)                   |
| NAT 필요성        | 주소 부족으로 인해 필요          | 주소 공간이 충분하여 불필요                                    |
| 자동 구성         | 제한적                           | 향상된 자동 구성 기능 (Stateless Address Autoconfiguration)    |
| 보안              | 외부 프로토콜 사용(IPSec 선택적) | 내장된 IPSec 지원 (기본 제공)                                  |

- 논리적 주소
- 변함
- ex. 투썸에 갔을때와 스타벅스에 갔을 때 와이파이를 연결하면 IP 주소가 서로 다름

##### MAC 주소

- 전 세계적으로 고유한 물리적 주소
- 일반적으로 48비트(6바이트) 길이로 구성됨
- 네트워크 장치의 하드웨어에 영구적으로 할당되며 변하지 않음

##### NAT

- 하나의 IP 주소를 기반으로 여러개의 IP인 척을 하는 기술
- 내부(private) 네트워크의 IP 주소를 외부(public) 네트워크로 패킷을 전송할 때, 사설 IP를 공인 IP로 변환해주는 기술을 말함

##### NAT가 필요한 이유

- Pv4 주소(32비트)가 한정적이기 때문에, 모든 내부 호스트에게 공인 IP를 할당하기 어려움
- NAT를 사용하여 사설 IP 대역(예: 192.168.x.x, 10.x.x.x 등)을 내부 호스트에 자유롭게 할당
- 라우터(혹은 방화벽)가 외부와 통신할 때만 공인 IP 주소를 사용하므로, 공인 IP를 절약 가능
- 내부 네트워크의 실제 IP 주소가 외부에 직접 노출되지 않으므로, 어느 정도 보안이 강화됨
- 내부 IP 대역을 자유롭게 설계할 수 있어 네트워크 구조상 유연함을 제공함

---

#### MAN(metropolitan area network)

- 도시와 도시의 통신망을 뜻하며 2개 이상의 LAN이 연결되어 구성됨
- LAN보다 더 큰 대역폭을 가질 수 있음
- 라우터, 브리지 등으로 연결되는 것이 특징임
  > 라우터❓
  > 패킷의 위치를 추출하여 그 위치에 대한 최적의 경로를 지정하며, 이 경로를 따라 데이터 패킷을 다음 장치로 이동시키는 장치
- 전송매체로 광섬유(FDDI Fiber Distributed Digital Interface, SONET/SDH) 또는 동축 케이블이 사용됨
- MAN의 표준은 IEEE에서 제안한 `DQDB(Distributed Queue Dual Bus)`로 이중 버스 토폴로지를 사용
  ![](<스크린샷 2024-12-23 오후 4.12.13.png>)

##### Ex.

- 서울 나이키 매장에서 에어맥스 신발을 사려고 하는데 매장에 재고가 없다면 전산 시스템을 이용하여 다른 지점의 재고를 파악후 대전에 있는 나이키 매장에서 택배로 배송이 가능함
- 이는 서울과 대전. 도시와 도시를 잇는 네트워크가 있기때문에 가능함

---

#### WAN(wide area network)

- 국가와 국가와의 통신망을 뜻하며 인터넷이라고도 함
- 수많은 라우터를 거쳐 다른 국가와도 연결되는 범위를 뜻함
- frame-relay, PPP(지점 간 프로토콜), 고수준 데이터 링크 제어 프로토콜(HDLC), 동기식 데이터 링크 제어 프로토콜(SDLC) 등이 있음

##### Ex.

- 서울에 있어도 미국에있는 사람과 보이스톡을 할 수 있음
- 인터넷을 이용해서 통신한다 === WAN으로 통신한다
