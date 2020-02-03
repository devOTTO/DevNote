## Database

### Storage
서버에 사용하는 스토리지 종류
#### Internal Disk
- 내장 디스크
- DB용으로 최적화된 스토리지를 개인 PC에 장착하는 것처럼 4~6개 정도를 연결해 사용하는 형태
- 연결할 수 있는 공간과 갯수가 제한적이기 때문에 용량의 한계가 있는 단점
#### DAS(Direct Attached Storage)
- Internal Storage와 같은 SATA나 SAS와 같은 케이블로 연결, 비슷한 성능
- 장점 : 디스크를 모아서 체계적으로 장착할 수 있음
- 단점 : 하나의 본체에만 직접 연결해서 사용 가능, 여러대 동시 연결 불가능
#### NAS(Network Attached Storage)
- Internal Disk와 DAS의 단점인 "용량 확장 한계"와 "여러 컴퓨터에서 동시 사용 불가능" 문제 해결
- TCP/IP를 통해 DB에 연결
- Network를 이용해 여러 컴퓨터에서 동시 연결 사용 가능 but 직접 연결 대비 속도 느림
- 빈번한 I/O에 적합하지 않아 서버용 DB로는 사용 안하는 경향
#### SAN(Storage Area Network)
- DAS로 구축할 수 없는 대용량의 스토리지 공간을 제공하는 장치
- 여러 컴퓨터에서 동시 사용 가능
- 스토리지와 본체가 광케이블로 연결되 빠르고 안정적인 I/O 보장
- 고가의 구축 비용 소모
- 기업 내부 중요 데이터 취급 시 구축

#### 참고 자료
* https://interconnection.tistory.com/96
