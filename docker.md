# 도커

## CHAPTER 01. 도커의 기초

### 01. 도커란 무엇인가

#### 도커란?
* 컨테이너형 가상화 기술을 구현하기 위한 상주 애플리케이션과 이 애플리케이션을 조작하기 위한 명령행 도구로 구성되는 프로덕트
* 컨테이너형 가상화 기술을 사용
  * 가상화 소프트웨어 없이 운영체제의 리소스를 격리해 가상 운영체제로 만들어 오버헤드 적음
<br>  vs 호스트 운영체제형 가상화
  * 운영 체제 위에서 가상화 소프트웨어를 사용해 하드웨어를 에뮬레이션 하는 방법으로 게스트 운영 체제를 만들어 오버헤드가 큼
#### 간단한 예제
* helloworld shell script
```
# !/bin/sh

echo "Hello, World!" 
```
* Dockerfile 작성
```
FROM ubuntu:16.04

COPY helloworld /usr/local/bin
RUN chmod +x /usr/local/bin/helloworld

CMD ["helloworld"]
```
  - FROM : 컨테이너의 원형 역할을 할 도커 이미지(운영 체제) 정의
  - COPY : shell script 파일을 도커 컨테이너 안의 해당 경로로 복사하도록 정의
  - RUN : 도커 컨테이너 안에서 명령 수행 
  - CMD : 완성된 이미지를 도커 컨테이너로 실행하기 전에 먼저 실행할 명령
* Dockerfile을 통해 이미지 빌드 후 실행
```
$ docker image build -t helloworld:latest .

$docker container run helloworld:latest
```

### 02. 도커를 사용하는 의의

#### 도커 사용 이유
* 변화하지 않는 실행 환경으로 멱등성(Idempotency) 확보
* 코드를 통한 실행 환경 구축 및 애플리케이션 구성
* 실행 환경과 애플리케이션의 일체화로 이식성 향상
* 시스템을 구성하는 애플리케이션 및 미들웨어의 관리 용이성
##### 환경 차이로 인한 문제 방지
* 근본적 원인은 인프라의 가변성
* 이를 해결하는 방법
1. 코드로 관리하는 인프라(Infrastructure as Code)
<br>	코드 기반으로 인프라를 정의
2. 불변 인프라(Immutable Infrastructure)
<br>  어떤 시점의 서버 상태를 저장해 복제할 수 있게 하는 개념

##### 애플리케이션 구성 관리의 용이성

## 참고자료
* 도커/쿠버네티스를 활용한 컨테이너 개발 실전 입문 (위키북스)
