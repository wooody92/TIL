## 오늘의 강의

### Overview

- 아마존의 장점?
- 탄력성 : 서버를 내가 원할 때 원하는 만큼 만들 수 있다.

### Auto-Scaling

- 트래픽의 사용량을 바탕으로 자동으로 필요한 만큼 서버를 열어준다.
- 수직확장
  - 인스턴스의 성능을 올리는 것
  - 개발자 입장에서는 쉽다.
- 수평확장
  - 인스턴스 갯수 늘리는 것
  - 인프라 입장에서는 쉽다.
  - JWT를 사용하면 수평확장에 좋다.
- 직접 오토스케일링을 구현하려면 어떻게 해야 할까?
- ELB (Elastic Load Balancer)
  - 사용자는 ELB에 요청을 하고 ELB에서 트래픽을 서버에 분배한다.
  - AWS에서 관리하기 때문에 트래픽 관리 및 트래픽 공격에 강하다.
  - CPU 사용률을 모니터링을 하고 Auto Scaling이 서버를 늘리고 줄이고 한다.
  - 서버를 제거 할때는 트래픽 차단 후 트래픽을 다른 서버로 옮기고 서버를 제거한다.
- Cloud Watch
- Sticky Session을 사용하면 수평확장을 신경쓰지 않아도 된다.
- Auto Scaling Service들을 이용하여 다양한 배포 전략을 사용 할 수 있다.
- Blue Green Deployment