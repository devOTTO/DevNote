### HAProxy

* 기존 하드 웨어 스위치를 대체하는 소프트웨어 로드 밸런서
* 네트워크 스위치에서 제공하는 L4, L7 기능 및 로드 밸런서 기능을 제공
* reverse proxy 동작 방식 : 실제 서버 요청에 대해서 서버 앞 단에 존재하면서, 서버로 들어오는 요청을 대신 받아 서버에 전달하고 요청한 곳에 그 결과를 다시 전달
* 동작 흐름
    1. 최초 접근 시 서버에 요청 전달
    2. 응답 시 쿠키(cookie)에서 서버 정보 추가 후 반환
    3. 재요청 시 proxy 에서 쿠키 정보 확인 > 최조 요청 서버로 전달
    4. 다시 접근 시 쿠키 추가 없이 전달 > 클라이언트에 쿠키 정보가 계속 존재함(쿠키 재사용)
    ![image](https://user-images.githubusercontent.com/42940194/72327140-da8f5200-36f3-11ea-9882-4415d0a0cf5f.png)


### 성능 테스트

#### 성능 테스트 항목

* Application
    * TPS(Transactoin Per Second)
    * 응답 시간(Response Time)
* Middleware
    * Message Queue
        * RabbitMQ
    * Database
        * MySQL(slow query, Index)
    * Web Server
        * Apache(Network outbound io (bandwith))
        * Tomcat(Idle Thread)
* Infra
    * CPU
    * Memory(Swapping)
    * Disk IO(파일 시스템)
    * Network IO(고용량의 파일이나 이미지 전송에서 병목)

#### 테스트 종류

##### 성능(Performance) Test

* 시스템의 요소가 특정 상황에서 어느 정도의 성능을 보이는지 측정하는 테스트
* 기존 시스템에 대한 benchmarking 하는 것
* 어프리케이션의 결함을 찾는 목적이 아님
* 성능에 대한 정확하고 면밀한 모니터링 필요

##### 부하(Load) Test

* 임계치의 한계에 도다할 때까지 시스템에 부하를 꾸준히 증가시키며 진행하는 테스트
* 부하 상황에서 시스템이 어떻게 동작하는지 모니터링 하고 정보를 얻음
* 발생시키는 부하는 실제 시스템에 적용될 예상 트래픽
* `volume test` 또는 `endurance test`
* Web SErver, Database, Infra 등 모든 요소의 한계를 찾아 미래에 발생할 부하에 대비하는 것이 목표

##### 스트레스(Stress) Test

* 시스템이 과부하 상태에서 어떻게 작동하는 지 검사하는 테스트
* 시스템의 실패를 확인하고 모니터링하는 과정이 정상적으로 이루어 지는지 검사
* 민감한 정보나 보안상의 문제가 노출되지 않는지 검사
* 장애 조치와 복구 절차가 효과적이고 효율적인지 검사

##### Soak Test

* 지속적으로 부하를 가해, 메모리 누수나 자원 누수를 알아내는 테스트

##### Negative Test

* 부하를 발생시킨 상태에서 특정 시스템 구성 요소 중 일부를 제거하는 테스트

##### Fatigue Test

* 대역폭 용량을 뛰어넘는 부하를 발생시켜 테스트

#### 그래프 해석

![image](https://user-images.githubusercontent.com/42940194/72327157-e4b15080-36f3-11ea-972c-95bf93108751.png)


##### 포화점(Saturation Point)

* 시스템 처리의 한계점

##### 버클존(Buckle Zone)

* 시스템이 과부하로 인해 내부 자원이 서로 경쟁 상태나 적체 상태가 심해져 최대 처리량 보다 더 떨어지는 경우

> 포화점에서 얻은 최대 처리량을 실제 최대 성능으로 보면 안됨<br>
> 최대 처리량을 재 정의 하고 최대 처리량과 여유를 두는 것이 좋음

#### 성능 관련 항목

> Users
> Time
> TPS

##### Users

* Active User
    * 서버에 부하를 주고 있는 User
    * ex) 메뉴나 링크를 누르고 결과를 기다리는 사용자
    * 성능 테스트시 Virtual User와 거의 동일
* Concurrent User
    * 서버에 부하를 주고 있거나, 줄 가능성이 매우 높은 서비스에 접속중인 사용자
    * 웹 페이지를 띄워 놓거나 앱을 실행하는 사용자
* Active User와 Concurrent 사용자의 비율을 아는 것이 성능 테스트 시 중요
    * 비율에 따라 실제 성능을 좀 더 정확하게 테스트 하고 파악 가능

##### Time

* User와 System, Server 의 Time 관점은 다름
* 성능 테스트시 고려

##### TPS

* TPS(Transaction Per Second)
    * 초당 얼마나 많은 요청을 처리할 수 있는지
* TPS & Time
    * TPS는 Scale out/up을 통해 증가 가능
    * Time은 Scale을 통해 불가능
    * Tuning을 통해 두 부분 다 개선 가능
* TPS & User / Time & User
    * TPS & User : User가 증가하면 TPS가 증가하다 어느 순간 증가 X
    * Time & User : 일정 수준에 도달 시 까지 Time 유지하다가 병목 증상 발생시 급격히 증가

##### Workload

* 주어진 시간 동안 컴퓨터가 처리한 일의 양 혹은 그러기 위해 부과된 연속된 일
* Web 기반 시스템에서는 주로 HTTP 요청이 Workload

##### Metric

* 응답 시간(Response Time)
    * 인터넷에서 패킷이 라우팅 되는 시간 포함
    * 평균 응답 시간(Mean Time)이 Web Server형의 시스템의 성능 분석에서 중요
* 처리량(Throughput)
    * PPS(Page Per Second) : 웹 시스템에서 특정 페이지에 대한 요청
    * RPS(Request Per Second)
* TPS(Transaction Per Second)
* MFLOPS(Millions of Floating-point Operations Per Second) : CPU의 연산 속도

##### Reliability

* 에러의 확률 혹은 에러간의 평균 시간으로 측정
* Error-Free Seconds

##### Bottleneck

* 구성 컴포넌트 중 활용도가 가장 높은 컴포넌트를 지칭
* 주요 병목 지점
* Top3
    * Web Page / App
    * DB
    * Network

### 성능 테스트 도구

#### nGrinder

* Grinder 기반 네이버에서 개발
* A platform for stress tests that enables you to execute script creation, test execution, monitoring, and result report generator simultaneously.
* 구조
![image](https://user-images.githubusercontent.com/42940194/72327282-175b4900-36f4-11ea-9b3b-5e383673ae52.png)


1. Controller
    * Performance Testing을 위한 web interface 제공
    * 테스트 프로세스 조정 및 테스트 통계 대조, 표시
    * 사용자가 스크립트를 작성하고 수정할 수 있음
2. Agent
    * Agent Mode : 대상 시스템에 로드를 가하는 프로세스 및 스레드 실행
    * Moniter Mode : 대상 시스템의 성능(ex: CPU/메모리) 모니터링
3. Target
    * 실험 대상이 되는 서버

#### JMeter

* Java 기반 오픈소스
* GUI/non-GUI Mode 지원
* 분산 테스트 지원

### APM(Application Performance Management)

* 응용 소프트웨어의 성능과 서비스 이용성을 감시하고 관리하는 데 초점을 둔 시스템 관리 원칙
* 운영 중인 시스템에 대한 응답시간과 같은 성능이나 처리량, 가용성, 및 기능적 무결성, 신뢰성을 확보하고 다운타임의 최소화 등을 통해 안정적인 시스템 운영을 위한 도구로써 부하량, 접속자 파악, 장애진단, 통계, 분석 등을 목적으로 하는 성능 모니터링 제품

#### PinPoint

* 대규모 분산 시스템의 성능을 분석하고 문제를 진단, 처리 하는 java 플랫폼, 네이버 개발
* 특징
    * 분산된 어플리케이션의 메세지를 추적할 수 있는 분산 트랜잭션 추적
    * 애플리케이션 구성을 파악할 수 있는 애플리케이션 토폴로지 자동 발견
    * 대규모 서버군을 지원할 수 있는 수평 확장성
    * 코드 수준의 가시성을 제공해 문제 발생 지점과 병목 구간을 쉽게 발견
    * bytecode instrumentation 기법으로 코드를 수정하지 않고 원하는 기능을 추가
* 기능
    1. 서버 맵 : 대상 서버와 연결된 다른 서버와의 관계 다이어그램
    2. 스캐터 : 요청별 응답시간에 따른 그래프
    3. request detail : 스캐터에서 선택된 요청의 스택트레이스 뷰
    4. 에러 발생 요청 표시 : 에러가 발생한 요청 빨간색으로 표시
    5. 서버의 jar 목록 표시 : 서버에 접속하지 않아도 관련된 jar 목록 확인 가능
* 구성
![image](https://user-images.githubusercontent.com/42940194/72327253-10343b00-36f4-11ea-88c0-2727e6293fa8.png)
* DB - HBase(하둡 분산 데이터 베이스 기반)
* Web UI
* Collector - 대상이 되는 타켓 서버의 정보를 HBase가 저장/ Web UI 보여주는 것을 쌓아둠
* Agent - 각각의 대상 서버에 pinpoint agent를 줘 각각의 pinpoint agent가 collector의 데이터를 udp/tcp + thrift를 통해 보내주고 디비에 저장 후 web ui가 보여줌

#### 제니퍼

#### Scouter

#### SPOF(Single Point of Failure)

* 시스템 구성 요소 중에서 동작하지 않으면 전체 시스템이 중단되는 요소
![image.png](/files/2656352302509501846)
* 방지 방법
    * High Availability(HA)
        * 시스템 고장 발생 시 빠른 시간 내에 치료하여 다시 정상적으로 서비스할 수 있는 상태를 분석하기 위해 사용되는 척도
    * Fault Tolerant(FT)
        * 시스템 내의 어느 한 부품 또는 한 모듈에 장애가 발생하더라도 시스템 운영에 지장을 주지 않도록 설계된 컴퓨터 시스템
        * [세부 링크](https://m.blog.naver.com/PostView.nhn?blogId=unsa94&logNo=140016461423&proxyReferer=https://www.google.com/)
    * Load Balancing
    * RAID(Redundant Array of Independent/Inexpensive Disks)
        * 여러 개의 드라이브 집합을 하나의 장치처럼 사용 하는 것
        * [세부 링크](https://jwprogramming.tistory.com/24)

#### 참고자료

* https://d2.naver.com/helloworld/284659
* https://d2.naver.com/helloworld/1194202
* https://github.com/naver/ngrinder
* https://github.com/naver/ngrinder/wiki/Architecture
* https://ko.wikipedia.org/wiki/애플리케이션\_성능\_관리
* https://atyou73.github.io/itrepo/post/compare-loadtest-tool-1/
* https://ko.wikipedia.org/wiki/단일\_장애점
* https://expitly.tistory.com/44
* https://www.popit.kr/우아한-테크-세미나-자바-성능-모니터링-이야기-후기/
* https://sugerent.tistory.com/523
