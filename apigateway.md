### API Gateway

#### API Gateway VS API Proxy

* API Gateway 와 Proxy의 차이점이 헤깔려서 조금 조사해보았다.

##### API Proxy

![image](https://user-images.githubusercontent.com/42940194/78902700-164c7480-7ab5-11ea-9a07-371deb0e89df.png)

> API Proxy는 오로지 기존에 존재하고 있는 API 에서만 작동한다.

##### API Gateway

![image](https://user-images.githubusercontent.com/42940194/78902743-29f7db00-7ab5-11ea-8a3f-6099ebfb31a9.png)

> API Gateway는 API Proxy 가 제공하는 기능을 포함

* 비교
![image](https://user-images.githubusercontent.com/42940194/78902761-2f552580-7ab5-11ea-9b96-c98c51f9f44c.png)
* 사용 경우
    * 기존의 API 를 활용해 다른 추가 기능이 필요하지 않을 경우, API Proxy로 충분
    * 추가적인 기능이 더 필요할 경우, API Gateway 를 추가하는 방안이 좋음

#### API Gateway 사용 배경

##### MSA

* Monolithic architecture
    * 하나의 프로젝트에 모든 서비스가 포함 된 것
    * 프로젝트 규모가 커질 수록 빌드, 배포 시간이 커지는 단점
* MSA(MicroService Architecture)
    * 각각의 서비스가 분할되어 하나의 모놀리틱 아키텍처와 같은 구조로 쪼개 지는 것
    * 각 서비스는 독립적으로 배포 가능
    * 각 서비스는 다른 서비스에 대한 의존성 최소화
    * 각 서비스는 개별 프로새스로 구동 되며, REST와 같은 가벼운 방식으로 통신됨

![image](https://user-images.githubusercontent.com/42940194/78902778-34b27000-7ab5-11ea-9e22-378ca87d5ca7.png)

* 장점
* 서비스별 개별 배포가 가능해져, 배포 시 전체 서비스의 중단이 없음
* 확장 시 특정 서비스에 대해 확장 가능
* 장애가 전체 서비스에 확장될 가능성이 적음
* 구조
![image](https://user-images.githubusercontent.com/42940194/78902796-3a0fba80-7ab5-11ea-9007-4f0bca921282.png)

##### API Gateway

* MSA 아키텍쳐 상에서 하나의 큰 서비스는 작은 서비스들로 쪼개지기 떄문에, 클라이언트에서 서비스를 직접 호출하는 형태라면 다음과 같은 문제점 발생 가능
![image](https://user-images.githubusercontent.com/42940194/78902815-3ed46e80-7ab5-11ea-8584-86d1bc0ec011.png)
    * 각 서비스마다 인증/인가 등 공통된 로직 구현
    * API 호출 기록 관리의 어려움
    * 클라이언트에서 여러 마이크로 서비스에 대한 번거로운 호출 필요
    * 내부 비즈니스 로직이 드러나 보안에 취약
* API Gateway 도입 시
![image](https://user-images.githubusercontent.com/42940194/78902827-4267f580-7ab5-11ea-9cc3-681a7f4c619f.png)
* 인증 기능을 가지고 있으며 라우팅 기능까지 핵심적으로 수행하며 모든 API 서버들의 엔드 포인트를 단일화
* SOA의 ESB(Enterprise Service Bus)에서 승계
* SOA(Service Oriented Architecture)?
    * 기존의 애플리케이션들의 기능들을 비즈니스적인 의미를 가지는 기능 단위로 묶어서 표준화된 호출 인터페이스를 통해서 서비스라는 소프트웨어 컴포넌트 단위로 재조합한 후, 이 서비스들을 서로 조합(Orchestration)하여 업무 기능을 구현한 애플리케이션을 만들어내는 소프트웨어 아키텍처
* ESB
    * 비즈니스 내에서 서비스, 애플리케이션, 자원을 연결하고 통합하는 미들웨어로
* SOA와 MSA
![image](https://user-images.githubusercontent.com/42940194/78902833-44ca4f80-7ab5-11ea-8b17-73c169a33991.png)
    * MSA를 SOA의 일종의 부분집합으로 볼 수 있음
* SOA와 MSA의 챠이점

1. SOA : 모듈의 의존성을 줄이되 모듈 내에서 공유할 수 있는 것은 공유하는 정책
MSA : 모듈들이 최대한 독립적으로 운용될 수 있도록 아키텍처를 디자인
2. SOA : 서비스의 Flow를 유지 -> ex) 서비스 내에서 결제 시, 관련된 루틴을 수행해 결제를 지원해 유저에게 제공
MSA : Flow의 구별을 요구 -> 유저에게 관련된 루틴과 결제 루틴을 별도로 이용

###### 주요 기능

1. 인증 및 인가(Authentication and Authorization)

* 인증서 관리, 인증, SSL, 프로토콜 변환과 같은 기능을 API Gateway에서 각 서비스의 부담을 줄임

> TOAST 에서는 ACL, HMAC, JWT 방식 지원

2. 요청 절차의 단순화

* 여러 클라이언트의 요청을 단일 요청으로 대체해 클라이언트와 백엔드 간의 API 통신량을 줄여 주어 대기 시간을 줄이고 효율성을 높임

3. 라우팅 및 로드 밸런싱

* 클라이언트로부터 접수된 메세지에 따라 적절한 서비스에 라우팅 및 부하 분산 가능

> ![image](https://user-images.githubusercontent.com/42940194/78902842-485dd680-7ab5-11ea-8eda-6a5a063dfefc.png)
> <br>Toast에서 Proxy Filter

4. 서비스 오케스트레이션

* 오케스트레이션은 여러개의 마이크로 서비스를 묶어 새로운 서비스를 만드는 개념므로, 오케스트레이션 로직의 과도한 사용은 API Gateway의 성능 저하 유발 가능
