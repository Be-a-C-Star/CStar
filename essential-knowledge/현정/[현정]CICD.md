# CI/CD(Continues Integration/Delivery & Deployment)

CI/CD란 지속적인 통합(Continues Integration), 지속적인 제공/배포(Continues Delivery/Deployment)를 의미한다. 이는 여러명의 개발자가 동시에 개발을 진행할 때 코드의 오류 및 버그를 예방하는 동시에 지속적인 소프트웨어 개발과 업데이트 주기를 유지하는 데 도움이 된다. 애플리케이션이 확장됨에 따라 CI/CD 기능을 활용하면 복잡성을 줄이고 효율성을 높여 워크플로우를 간소화할 수 있다. 또한 코드의 변경 사항을 빠르게 통합할 수 있어서 사용자 피드백을 더 자주 효과적으로 반영할 수 있기 때문에 사용자에게 긍정적인 결과도 제공할 수 있다.

## 파이프라인

코드 구축부터 최종 배포까지 일련의 과정들을 CI/CD 파이프라인이라 하며 총 3가지의 단계로 구성된다.

- continuous integration
  - 코드를 빌드하고 테스트, 머지하는 일련의 과정
  - 파이프라인 자체에 테스트가 포함되어 있어 테스트를 강제하게 만들어 안정성을 높일 수 있다.
- continuous delivery
  - 레포지토리에 릴리즈하는 것
  - 릴리즈란 개발 환경에서 테스트가 완료된 코드를 운영 환경에 적용시키는 것을 의미한다.
- continuous deployment
  - 실제 서비스에 배포하는 것

## Tool

### [Jenkins(젠킨스)](https://www.jenkins.io/)

중앙 빌드 및 지속적 통함 프로세스가 가능하고 여러 OS용 패키지가 포함된 독립형 Java 기반 프로그램.
수백개의 플러그인을 사용할 수 있으며 소프트웨어 개발 프로젝트의 빌드 파이프라인 구성, 빌드 자동화 확립 , 배포 및 테스트 자동화 등을 지원한다.

### [GitHub Actions](https://docs.github.com/ko/actions)

클라우드로 동작하여 별도의 설치가 필요없는, github에서 제공되는 프로그램.
젠킨스에 비해 플러그인은 적으나 스크립트로 추가할 수 있는 기능이 있다. 비동기 CI/CD가 가능하며 Github API를 지원한다.

위 두 가지 외에도 Bitbucket, Bamboo, TeamCity 등 많은 도구들이 존재한다.
