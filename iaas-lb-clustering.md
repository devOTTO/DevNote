# IaaS, SaaS, PaaS
![image](https://user-images.githubusercontent.com/42940194/71861839-16964600-313b-11ea-98c0-c486488ca9f6.png)
![image](https://user-images.githubusercontent.com/42940194/71861843-1d24bd80-313b-11ea-9bcc-973405b284f1.png)
## Packaged Software
* On-premise, 직접 인프라와 플랫폼, 어플리케이션을 관리하는 모델
* 기업에서 많은 비용이 소모 되며 유지 보수 비용이 큼

## IaaS
* OS부터 상위의 모든 플랫폼이나 어플리케이션을 사용자가 직접 올림
* 고객은 가상 서버 하위의 레벨에 대해 고려할 필요 없음
  * Ex) AWS EC2, OpenStack

## PaaS
* 개발자가 응용 프로그램을 작성할 수 있도록 플랫폼 및 환경을 제공하는 모델
* Node.js, Java와 같은 런타임을 미리 깔아 놓고 소스코드를 넣어 돌리는 구조
* 소스코드만 적어 빌드하고 컴파일은 클라우드에서 하여 결과만 가져오는 것
* 하나의 인프라 기반으로 개발할 수 있다는 장점
  * Ex) Google App Engine, IBM Bluemix

## SaaS
* 소비 관점에서 제공되는 IT방식의 서비스
* Public Cloud에 있는 SW를 웹 브라우저로 불러와 장소/시간 제약 없이 사용 가능
* SaaS 특성상 인터넷 접속이 필요하며 외부의 데이터 노출에 대한 위험이 있음
  * Ex) 웹 메일, 구글 클라우드, 네이버 클라우드, MS오피스365, 드롭박스

# Load Balancer [참고 링크](https://nesoy.github.io/articles/2018-06/Load-Balancer)
* Load Balancing : 서버의 부하 분산을 위해 가상 IP를 통해 여러 서버에 접속하도록 분배하는 기능
![image](https://user-images.githubusercontent.com/42940194/71861880-3d547c80-313b-11ea-81cf-1397360c1d4b.png)
* 하나의 서비스를 하나 이상의 노드가 처리하는 식으로 작동
* 서버의 로드를 클러스터링된 서버별로 균등하게 나누어 주는 서버
* 주요 기능 
  * NAT(Network Address Translation) : 사설 IP 주소를 공인 IP 주소로 바꾸는데 사용하는 통신망의 주요 변조기
  * Tunneling : 인터넷 상에 보이지 않는 통로를 만들어 통신할 수 있는 개념으로 데이터를 캡슐화해서 연결된 상호 간에만 캡슐화된 패킷을 구별해 캡슐화 해제 가능  
  * DSR(Dynamic Source Routing protocol) : 로드 밸런서 사용 시 서버에서 클라이언트로 되돌아 가는 경우 목적지 주소를 스위치의 IP 주소가 아닌 클라이언트의 IP 주소로 전달해 네트워크 스위치를 거치지 않고 바로 클라이언트를 찾아감 
![image](https://user-images.githubusercontent.com/42940194/71861892-47767b00-313b-11ea-8d56-c0cb7b516005.png)

* 종류
  * L2 : Mac 주소 바탕
  * L3 : IP 주소 바탕
  * L4 : Transport Layer(IP와 port) 레벨 - TCP, UDP
  * L7 : Application Layer Level - HTTP, HTTPS, FTP
  ![image](https://user-images.githubusercontent.com/42940194/71861904-565d2d80-313b-11ea-8bc7-9666d473b80e.png)
    > - X-Forwarded-For : HTTP 또는 HTTPS 로드 밸런서를 사용할 때 클라이언트의 IP 주소를 식별하는 데 도움을 줌
    > - X-Forwarded-Proto : 클라이언트가 로드 밸런서 연결에 사용한 프로토콜(HTTP, HTTPS)을 식별하는 데 도움을 줌
    > - X-Forwarded-Port : 클라이언트가 로드 밸런서 연결에 사용한 포트를 식별하는데 도움을 줌

* 서버 선택 기준
  * Round Robin : 선점형 스케줄링 방식으로, 프로세스들 사이에 우선순위를 두지 않고, 순서대로 시간 단위(Time Quantum/Slice)로 CPU를 할당하는 방식의 CPU 스케줄링 알고리즘
  * Least Connections : 연결 개수가 가장 적은 서버를 선택하는 방식으로, 트래픽으로 인해 세션이 길어지는 경우 권장하는 방식
  * Source : 사용자의 IP를 Hashing하여 분배하는 방식으로, 사용자는 항상 같은 서버로 연결되는 것을 보장 받음

* Load Balancer 장애 대비 방법
  * 이중화
![image](https://user-images.githubusercontent.com/42940194/71862074-f2873480-313b-11ea-9f74-a3a2a403351e.png)		
    * 이중화된 Load Balancer들은 서로 Health Check
    * Fail Over 구성 
      * Master 서버와 Standby 서버를 구성하고, Master 서버 Fail 시 Standby 서버가 자동으로 Master 서버의 역할
      * Standby 서버는 평상시에 대기 상태로 있다가 Master서버 Fail 시에만 작동
* 로드 밸런싱 단점
  * 세션 데이터 관리
  * 해결방법 : 세션 고정 - 고정된 세션의 노드에 장애가 발생시 고정 의미 사라져, 장애가 발생해 비활성화된 노드에 대한 고려가 필요
	
# 클러스터링
* 여러 대의 컴퓨터를 똑같은 구성의 서버군을 병렬로 연결한 시스템으로 마치 하나의 컴퓨터처럼 사용하는 것
* Virtual IP 기반으로 구현되며, 서비스를 제공하는 실제 장비는 Physical IP를 가지고 데이터의 처리는 Virtual IP를 통해 처리 (추상화 원칙)
* 로드밸런서에 의해 각 클러스터링된 서버에 의해 서비스가 진행

* 확장 비교
  * Scale-up : 하드웨어 성능을 올리는 방법
  * Scale-out : 하나의 server 대신 여러 대의 server가 나눠서 일하는 법
    * Load balancing, clustering 모두 scale-out 방식

# 로드 밸런싱과 클러스터링
* 로드 밸런싱은 여러대의 서버에 패킷을 부하 분산 시켜주는 것이고<br>
  클러스터링은 여러대의 서버를 하나의 서버로 만들어 주는 것
* 클러스터링은 한 서비스를 제공하는 여러 개의 서버를 하나로 묶어 성능을 높여 많은 양의 패킷을 처리하는 것이고,<br>
  로드 밸런싱은 여러대의 서버에 분산 시켜 주는 것
