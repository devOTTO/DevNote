# 사전 학습

## 모니터링

* 모니터링 : 어떤 대상을 감시, 관찰한다는 뜻으로 모니터링의 목적은 지속적인 감시, 감찰을 통해 대상의 상태나 가용성, 변화 등을 확인하고 대비하는 것

### 모니터링 지표

> CPU > 메모리 > 디스크 > 네트워크 순으로 부족 여부 확인
> 자원 모니터링은 5초나 10초 간격

#### CPU

* CPU의 성능 지표
* CPU 자원 사용량
* 실행 큐 사용량
* CPU 사용 유형 중 System / IO wait 사용률
* 프로세스 별 CPU 사용률 분포 균등한가?

#### 메모리

* 메모리가 부족한가? (swap 이 발생하고 있는가)
* 서버 전체 또는 개별 프로세스 단위로 메모리 사용량이 지속적으로 증가하는가?
* 파일 캐시 영역으로 사용하는 메모리량은 얼마인가?

#### 디스크

* 디스크의 서비스 시간은 디스크 대기 시간을 포함해서 얼마인가?
* 특정 디스크로 입출력이 집중됐는가?
* 파일 시스템 중 공간이 부족한 곳은 있는가?
* 디스크 스프라이트가 잘 구성돼 있는가?
* 스토리지 채널 병목이 있는가?

#### 네트워크

* 네트워크 데이터 전송량은 얼마인가?
* 네트워크 재전송량이 많은가?
* RTT(Round-Trip Time) 시간이 높은가?

### 모니터링

#### CPU Monitoring

* 안정적인 운영을 위한 CPU 사용율 기준
    * CPU 사용률 : CPU 70% 이하
    * CPU 실행큐 : CPU core 당 평균 3개 이하
* 전체 CPU 기준으로 성능 평가를 할 때는 이중화 구성 등에 따른 부분 장애시에 대한 변화량/사용량을 맞춰서 판단할 필요가 있다.
* CPU 사용량 상세 분석
    * System : 커널모드 시스템 콜 호출에 사용된 CPU 사용량.
        * 시스템 콜이라 함은 프로세스 제어, 장치 관리, 파일과 네트워크 관리, 입출력 처리, 시스템 정보와 시간 관리 등에 관련된 함수
        * 일반적으로 전체 사용량 대비 30% 미만인데, system cpu가 높게 나타난다면 어떤 유형의 시스템 콜인지 분석할 필요가 있다
        * 시스템 콜 모니터링 도구 : truss / pstack / jstack
    * User : 일반 함수를 실행하는데 사용된 CPU 사용량. WAS나 DB 서버 경우 User CPU를 사용한다.
    * IO Wait : 프로세스나 스레드가 CPU를 할당받아 사용할 수 있는 상태가 됐으나, 입출력이 완료되기를 기다리고 있어 CPU를 사용할 수 없는 상태의 사용량
        * 일반적으로 전체 사용량 대비 20% 미만인데, 높게 나타난다면 입출력이 과다하게 발생하는 것은 아닌지 디스크 사용률 등을 확인하면 된다.
        * io wait 모니터링 도구 : netstat / iostat / sar
    * Idle : 사용하지 않는 CPU 여유율
* 기타 모니터링 방법
    * CPU core 수 확인 방법 (리눅스 기준) : cat /proc/cpuinfo
    * 커널 처리 비트 확인 방법 (리눅스 기준) : getconf WORD\_BIT
    * CPU를 많이 사용하는 스레드 식별하는 방법 : top \-H \, ps \-eLf \| grep pid
    * 프로세스의 누적 CPU 사용량 확인하는 방법 : ps -ef

#### Memory Monitoring

* 메모리 사용패턴이 100% 라고 해도 부족하지 않으면 성능저하가 발생하지 않는다.
    * 운영체제 측면에서는 페이징스페이스에 대한 스왑 발생 여부로 판단.
    * 자바기반 솔루션 (was 등) 등은 설정한 힙메모리 이상을 사용하지 않기 때문에 운영체제 메모리 부족이 발생할 가능성은 적다.
* 메모리 관련 용어
    * pagingspace : 메인메모리가 부족할 때 사용하는 디스크 공간, swap space라고도 부름.
        * 운영체제가 메인 메모리 크기 이상의 메모리를 사용할 수 있도록 해주는 역할.
        * 성능 관점에서보면 디스크에 프로세스 메모리를 쓰고 읽는 스와핑 작업으로 인해 큰 성능 저하가 발생한다.
        * pagingspace 크기 확인 방법 : cat /proc/swaps
    * paging :
    * swapping : 메모리에서 페이지 혹은 세그먼트 단위 데이터를 교환하는 것. 컴퓨터가 메모리보다 큰 프로그램을 실행하거나 메모리보다 큰 데이터 파일을 다룰 수 있게 함.
    * page fault : 페이징 방식의 가상 메모리에서 CPU가 사용하려는 페이지가 메인 메모리에 없는 경우.
    * page in : 가상 메모리에서 page fault가 발생했을때 디스크에 있는 프로그램이나 데이터를 메모리로 로드하는 것.
    * page out : 가상 메모리에서 page fault가 발생했을 때 메모리가 부족하면 기존 메모리를 디스크로 기록하는 것.
    * page scan rate : 운영체제는 일정량의 여유 페이지를 확보해서 필요한 경우 즉시 제공하기 위해 page scan이라는 검색을 통해 해제함으로써 여분으로 확보할 페이지를 찾는다.
* 안정적인 운영을 위한 메모리 판단 기준
    * 메모리 사용률 : 100% 이하
    * 지속적으로 프로세스 스와핑이 발생하지 않는지 확인
* 일반적인 메모리 사용률 산출
    * 전체 메모리 total : used + free + buffers + cached
    * 실제 free 메모리 : free + buffers + cached
    * 실제 메모리 사용량 : total - (free+buffers+cached)
* 메모리 총량 확인 방법 : cat /proc/meminfo
* 프로세스 메모리 사용량 확인 방법 : ps -euf
* 파일 캐시 확인 명령 : /proc/sys/vm/buffermem, free -m, vmstat -v

#### Disk I/O

* 시스템상 디스크 영향도 확인
    * 자원 영향도 : CPU 사용률에서 io wait 가 비정상적으로 높은지?
    * 디스크 영향도 : 디스크 대기시간과 서비스 시간이 높은지?
    * 어플리케이션 영향도
        * pstack/jstack 으로 스택을 반복 수집했을 때 서비스 중인 스레드 가운데 파일 입출력 관련 작업 중인 스레드의 비중이 높은지
        * lsof/pfiles로 확인한 로그 파일이나 데이터 파일의 크기가 큰지?
        * truss/tusc로 모니터링 했을 때 발생하는 입출력의 양이 많은지?
* 디스크 입출력 모니터링 방법
    * sar -d
    * iostat
* 파일시스템 여유율 확인 방법
    * df -k

## Grafana

* 시계열 데이터를 저장하는 DB를 데이터 소스로 연계해 고급스러운 대시보드를 만들수 있음

## Graphite

* 시계열 데이터 저장에 특화된 DB와 그래프 관련 API를 제공하는 서버로 구성
* 데이터 수집은 사용자가 비교적 쉽게 만들 수 있으며, 다양한 연계 툴을 활용 가능
* 분산 아키텍쳐로 유연히 확장 가능하며, 수집 데이터가 많은 대규모 인프라에도 활용 가능
![image](https://user-images.githubusercontent.com/42940194/75229619-a1b7c200-57f5-11ea-9183-b32cea9a617d.png)
    * 비슷하지만 다른 ELK 로그 분석 시스템이 있다.
    <br>![image](https://user-images.githubusercontent.com/42940194/75229580-8cdb2e80-57f5-11ea-9920-49ba37b1012a.png)
## Monitoring Tools

### Collectd
* System and application metrics collector
* collectd gathers statistics about the system it is running on and stores this information. Those statistics can then be used to find current performance bottlenecks (i.e. performance analysis) and predict future system load (i.e. capacity planning). Or if you just want pretty graphs of your private server and are fed up with some homegrown solution you're at the right place, too.

## Log Management
### Fluentd 
* Unified logging layer
*  Fluentd collects events from various data sources and writes them to files, RDBMS, NoSQL, IaaS, SaaS, Hadoop and so on. Fluentd helps you unify your logging infrastructure.


### Telegraf









## 참고자료

* https://chigon.tistory.com/entry/성능-테스트시-서버-모니터링-방법-정리
* https://www.slideshare.net/LeeIGoo/db-wgraphitegrafana
* https://stackshare.io/stackups/collectd-vs-fluentd
