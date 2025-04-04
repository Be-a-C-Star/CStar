# DeepDive:대규모 트래픽으로 인한 서버 과부하 해결방법

## 서버 과부하의 의미

서버가 리소스를 소진하여 들어오는 요청을 처리하지 못할 때 발생한다. 이 때 서버는 사용자의 웹요청을 처리하지 못해 응답없음이 뜨게 된다.

## 모니터링을 통한 자원 할당

서버 과부하로 응답없음이 뜨는 이유 중 하나는 자원의 한계점 도달이다. 모니터링을 통한 자원의 적절한 할당을 통해 해결할 수 있다.

- AWS 오토스케일링: 서비스 이용불가능 상태 발생 이전 cloud watch가 계속해서 모니터링하여 서버 대수를 늘려주는 방법
- netdata: 무료 모니터링 서비스이며, 이를 기반으로 지속적인 모니터링을 통해 자원할당을 할 수 있다.

### 모니터링의 이유

1. 어떤 페이지에 어떤 트래픽이 얼마나 발생했는지 파악 가능
2. 어떤 네트워크에서 병목현상이 일어났는지 파악 가능

## 로드밸런서

로드밸런서로 트래픽을 분산할 수 있다. 또한, 한 서버에 장애가 발생하면 로드 밸런서는 트래픽을 다른 기능 서버로 리디렉션하여 시스템 중단을 방지할 수도 있다.

## 블랙스완 프로토콜

블랙스완이란 예측할 수 없는 사고가 일어난 것을 말한다. 구글은 블랙스완이 발생하면 다음과 같은 수칙을 따른다.

1. 영향을 받은 시스템과 각 시스템의 상대적 위험 수준을 확인
2. 잠재적으로 영향을 받을 수 있는 내부의 모든 팀에 연락
3. 최대한 빨리 취약점에 영향 받는 모든 시스템 업데이트
4. 복원계획을 포함한 우리의 대응 과정을 파트너와 고객 등 외부에 전달
