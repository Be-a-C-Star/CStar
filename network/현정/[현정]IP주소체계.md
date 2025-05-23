# IP 주소 체계

## 이진수

- 0 과 1 로 표현하는 수
- 십진수와 구별하기 위해 아래 방법 이용
    - 숫자 뒤에 b를 덧붙임 (binary의 약자)
    - 숫자 뒤에 (2)를 덧붙임 (주로 수학)
    - 숫자 앞에 0b를 덧붙임
- 오른쪽 끝에서 각각의 자리는 1부터 2의 제곱으로 늘어나며 수를 표현한다
- 각 자리는 bit라고 할 수 있다

## IPv4

> ex) 172.16.254.1

- 32bit로 표현되는 주소체계
- 2^32개의 주소를 표현
- 8bit 단위로 점을 찍어 4개로 구분해서 표현
- 갯수가 부족하여 NAT, 서브네팅 등 부수적인 기술이 생겨남
- CRC를 통해 손상 패킷을 확인하는 체크섬 필드 존재
- 가변길이

## IPv6

> ex) 2001:0DB8:AC10:FE01:0000:0000:0000:0000 -> 2001:0DB8:AC10:FE01::

- 128bit로 표현되는 주소체계
- 2^128개의 주소를 표현
- 많은 주소 처리가 가능하여 부수적인 기술이 불필요함
- 16bit씩 8개로 구분, 16진수로 변환되어 콜론으로 구분하여 표시, 연속되는 0은 생략 가능
- 앞 64bit는 네트워크 주소, 뒤 64bit는 인터페이스 주소
- IPSec 내장 : 데이터 패킷을 암호화하는 보안 네트워크 프로토콜 제품군
- 단순한 헤더 포맷
- 체크섬 필드는 상위 프로토콜에 존재하므로 헤더 효율화를 위해 체크섬 필드 사라짐
- UDP와 함께 사용할 경우 UDP 헤더에 체크섬 설정이 필요함
- 고정길이(40바이트)

**CRC** 

순환중복검사로 네트워크상에서 데이터에 오류가 있는지 확인하기 위한 체크값을 결정하는 방식
데이터를 전송하기 전에 주어진 데이터의 값에 따라 CRC 값을 계산하여 데이터에 붙여 전송하고, 데이터 전송이 끝난 후 받은 데이터의 값으로 다시 CRC 값을 계산하게 된다. 이어서 두 값을 비교하고, 이 두 값이 다르면 데이터 전송 과정에서 잡음 등에 의해 오류가 덧붙여 전송된 것임을 알 수 있다.

**IPv6에서 TTL은 HOP limit로 대체**

IPv4에서 TTL 필드는 패킷이 네트워크에서 무한순환하지 않도록 하는 변수. 패킷이 네트워크에서 라우터를 거칠 때마다TTL 값이 1씩 감소한다. 값이 0이 되면 패킷이 폐기된다.

## 클래스풀(Classful IP Addressing)

- 네트워크 주소를 매기고 그에 따라 크기를 다르게 구분하여 클래스를 할당하는 주소쳬계
- 구분하는 기준(옥텟)을 서브넷 마스크라 칭한다

### 클래스 A

- 2^24 - 2개 : 한 네트워크 당 16,777,214 호스트 ID / 0
- 네트워크 주소 범위 : 1 ~ 126 (0은 특수주소, 127은 루프백 주소임으로 제외)

### 클래스 B

- 2^16 - 2개 : 한 네트워크 당 65534 호스트 ID / 10
- 네트워크 주소 범위 : 128 ~ 191

### 클래스 C

- 2^8 - 2개 : 한 네트워크 당 254 호스트 ID / 110
- 네트워크 주소 범위 : 192 ~ 223

> 주소 중 맨 앞자리는 네트어크 주소, 마지막은 브로드캐스팅 주소임으로 사용하지 않고 남겨둔다. 클래스풀의 경우 네트워크의 크기가 작으면 큰 네트워크가 필요한 조직은 여러 개를 확보해야 한다는 단점이 있다. 또한 작은 네트워크가 필요한 조직은 너무 많은 IP를 가져가 낭비되는 문제도 있다.

## 클래스리스와 서브넷마스크, 서브네팅

클래스풀의 단점을 해결하기 위해 나온 클래스리스는 클래스로 나누지 않고 서브넷마스크를 중심으로 나눈다.

- 서브네팅 : 네트워크를 나눈다는 의미
- 서브넷 : 서브네트워크, 쪼개진 네트워크
- 서브넷마스크 : 서브네트워크를 위한 비트마스크

### 클래스풀과 클래스리스의 차이

클래스풀 네트워킹

- IP 주소를 A, B, C, D, E 클래스 기반으로 구분
- 각 클래스는 고정된 서브넷 마스크를 가짐
    - A : 255.0.0.0
    - B : 255.255.0.0
    - C : 255.255.255.0
- 고정된 서브넷 마스크를 사용하여 네트워크와 호스트를 나눔
- IP 주소의 첫 번째 옥텟에 따라 클래스가 결정된다

클래스리스 네트워킹

- 클래스 기반의 제한을 없앰
- 가변 길이 서브넷 마스크를 사용하여 네트워크와 호스트를 동적으로 나눔
- CIDR 표기법을 사용하여 네트워크를 나타냄

### CIDR 표기법

네트워크 주소를 더욱 효율적으로 표현하고 사용하기 위해 개발된 방식으로 IP 주소와 서브넷 마스크를 슬래시(/)로 구분하여 나타낸다. 예를 들어 주소 뒤에 /24가 있다면 서브넷 마스크의 길이는 24bit이다.

### 서브넷마스크

- 네트워크 주소는 모두 1, 호스트 주소 부분은 0 으로 설정해서 나눈다
- 255.255.255.0 = 256개를 기반으로 나눈다

## 공인 IP, 사설 IP, NAT

IP주소가 부족한 것을 공인IP와 사설IP로 나누고 중간에 NAT을 통해 해결한다
내부 네트워크 망에서는 사설 IP로 서로 통신하고 외부와 통신할 때는 공인 IP로 통신하게 된다

### NAT

- 패킷이 전송되는 동안 패킷의 IP주소를 변경하거나 IP주소를 다른 IP주소로 매핑하는 방법
- 내부 네트워크 IP가 노출되지 않는다
