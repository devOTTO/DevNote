## Database

### MySQL

#### MySQL의 구조
![image](https://user-images.githubusercontent.com/42940194/72713877-25f0a700-3bb1-11ea-85d2-2a7054030115.png)
> MySQL, MySQL 서버 = MySQL 엔진 + 스토리지 엔진

1. Connector - Client 부분

2. MySQL 엔진
* Connection Pool : 클라이언트로부터 접속 및 쿼리 요청을 처리
* Parser/Optimizer : SQL 파서 및 전처리기, 쿼리의 최적화된 실행
* SQL Interface : DML, DDL, Stored Procedure, View, Cursor, Trigger 등의 지원을 위한 Interface로 모든 SQL 함수에 대한 지원을 제공하는 모듈
* 보조 저장소 : MyISAM의 키 캐시, InnoDB의 버퍼 풀
* 요청된 SQL 문장을 분석하거나 최적화 하는 등의 DBMS의 두뇌 해당 처리

3. 스토리지 엔진
* 실제 데이터를 디스크 스토리지에 저장하거나 디스크 스토리지로부터 데이터를 읽음
* 여러개의 스토리지 엔진 동시 사용 가능(동일한  DB 내의 테이블 마다 다르게 지정 가능)

4. 핸들러 API
* 각 스토리지 엔진에 쓰기 또는 읽기를 요청하는 handler가 사용하는 API


##### 주요 스토리지 엔진
1. MyISAM
* ver.5.5 까지 Default Sotrage Engine
* ISAM(Indexed Sequential Access Method) 엔진을 보완한 엔진
* 장점
    * 기본 적인 기능을 제공해 상대적으로 가벼움
    * Select 작업 성능 빠름
    * 복합 검색 가능
* 단점
    * 트랜잭션 세이프 하지 않음
    * Table level locking -> 쓰기 작업 시 속도 저하
    * 데이터 무결성 보장 안됨
    * 외래키 생성 불가능  
2. InnoDB
* ver 5.5 이후 Default Storage Engine
* 장점
    * 데이터 무결성 보장
    * Row level locking 으로 데이터 수정 작업 시 유리
    * 트랜잭션 지원
    * 장애 복구, 외래키 지원
* 단점
    * 엔진이 무거워 작업 속도가 상대적으로 느림
    * 시스템 자원을 많이 사용

> MyISAM 은 정적인 데이터를 저장하고 자주 읽기 작업이 일어나는 테이블에 적합하며, InnoDB의 경우 중요 데이터를 다룰 때 읽고 쓰는 작업이 많은 경우 적합

3. Archive
* 로그 수집에 적합
* 데이터가 메모리 상에 압축됨
* row-level locking
* 한번 Insert된 데이터는 update, delete 불가, 인덱스 지원 X
* 트랜잭션 지원 X

* 스토리지 엔진 비교 표

| 스토리지 엔진 | 버전| 트랜잭션| LOCK 세분성 |강점|약점|
| --- | --- |---|---|---|---|
|MyISAM|모든버전|지원 안함 |동시 삽입 가능<br>테이블 락|SELECT, INSERT, 대량적재|읽기/쓰기<br>혼합 작업|
|MyISAM Merge|모든 버전|지원 안함| 동시 삽입 가능<br>테이블 락|분할 보관,데이터웨어하우징|전역적 테이블 검색| 
|Memory(HEAP)|모든 버전|지원 안함 <br>테이블 락|중간 계산, 통계 참조 데이터| 대량 데이터셋, 지속적인 스토리지|
|InnoDB|모든 버전 |지원|MVCC의 ROW락|트랜잭션 처리|
|Falcon |6.0 이상|지원 | MVCC의 ROW 락|트랜잭션 처리 |
|Archive |4.1 이상|지원 안함|ROW 락|로깅, 집계 분석|임의 접근(인덱싱), 갱신, 삭제|
|CSV|4.1 이상|지원 안함 |테이블 락|로깅, 외부 데이터 대량 적재|임의 접근(인덱싱)|
|Blackhole|4.1 이상|지원 안함 |락 지원 안함|로깅, 복제 아카이브|특수 목적(로깅) 이외의 사용 |
| Federated|5.0 이상 | 지원 안함|락 지원 안함|배포된 데이터 소스 관리|특수 목적(원격 데이터 조회) 이외의 사용 |
|NDB 클러스터|5.0 이상|지원|ROW 락|고도의 가용성|일반적인 사용 어려움|
|PBXT|5.0 이상|지원|MVCC의 ROW 락|트랜잭션 처리, 로깅|클러스터 인덱스 지원 안함| 
| SolidDB|5.0 이상|지원|MVCC의 ROW 락|트랜잭션 처리|


### Oracle
* 오라클의 특징
1. Oracle Management Server
    - 중앙 집중 방식으로 Administration monitoring 가능
    - Multiple databases 튜닝 가능
    - 다른 Admin User들과 공유 가능
2. Oracle Change Manager
    - 변경 Plan 작성 후 실제 구현 전 변경 사항 효과 확인 가능
    - 생산 시스템 방해 없음
3. Administrative Alerts
    - 오류 발생 시 계정 알람 가능
4. Capacity Planning
    - 병목 현상 쉽게 파악 가능
5. Query Optimizer
    - 쿼리 최적화 프로그램을 통해 SQL 실행 효율적인 방법 선택

### DB 이중화 솔루션

> MySQL에서 High Availability & Scalability 를 위해 제공하는 솔루션들은 다음과 같다.
> 1. MySQL Replication
> 2. MySQL Cluster
> 3. Oracle MySQL Cloud Service
> 4. Oracle Clusterware Agent for MySQL
> 5. MySQL with Solaris Cluster

* 각 architecture는 Data Replication, Clusted & Virtualized Systems, Shared-Nothing, Geographically-Replicated Clusters로 나뉜다  

| Requirement	| MySQL Replication| 	MySQL Cluster|
| ------- | --- | --- |
| Availability |
| Platform Support	|All Supported by MySQL Server<br> (https://www.mysql.com/support/supportedplatforms/database.html)	|All Supported by MySQL Cluster<br> (https://www.mysql.com/support/supportedplatforms/cluster.html)
|Automated IP Failover|	No|	Depends on Connector and Configuration|
|Automated Database Failover|	No|	Yes|
|Automatic Data Resynchronization	|No	|Yes|
|Typical Failover Time	|User / Script Dependent|	1 Second and Less|
|Synchronous Replication	|No, Asynchronous and Semisynchronous|	Yes|
|Shared Storage|	No, Distributed|	No, Distributed|
|Geographic redundancy support	|Yes|	Yes, via MySQL Replication|
|Update Schema On-Line	|No|	Yes|
|Scalability|
|Number of Nodes|	One Master, Multiple Slaves	|255|
|Built-in Load Balancing|	Reads, via MySQL Replication|	Yes, Reads and Writes|
|Supports Read-Intensive Workloads|	Yes	|Yes|
|Supports Write-Intensive Workloads|	Yes, via Application-Level Sharding	|Yes, via Auto-Sharding|
|Scale On-Line (add nodes, repartition, etc.)	|No|	Yes|

#### MySQL Replication
* 최소 구성 Master / Slave 구성
* Master DBMS
    * 웹 서버로 부터 데이터 등록/수정/삭제 요청시 바이너리 로그(Binarylog)를 생성해 Slave 서버로 전달
    * 웹 서버로 부터 요청한 데이처 등록/수정/삭제 기능에 주로 사용
* Slave DBMS 
    * Master DBMS로 부터 전달 받은 바이너리 로그를 데이터로 반영
    * 웹서버로 부터 요청을 통해 데이터를 불로오는 용도로 주로 사용

#### MMM(Multi-Master replication Manager for MySQL)

#### MHA(Mysql High Availability for MySQL)

#### MariaDB Galera Cluster(=MySQL)

## 참고자료

* https://skibis.tistory.com/33
* https://12bme.tistory.com/72
* https://ora-sysdba.tistory.com/entry/Oracle-인가-MySQL-인가
* https://blog.cafe24.com/1285
* https://mysqldba.tistory.com/180?category=544409
* http://libqa.com/wiki/163 (NDB)
* http://asuraiv.blogspot.com/2017/07/mysql-storage-engine.html
