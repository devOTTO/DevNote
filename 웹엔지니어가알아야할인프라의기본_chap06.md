## Chapter 6 웹 서비스 운용 2 : 상태 모니터링

### 6.2 상태 모니터링 데이터를 읽는 방법 - OS

#### CPU 관련 그래프

##### CPU Usage의 항목과 의미

| 항목 | 의미 |
| --- | --- |
| CPU User | 사용자 영역(커널 영역이 아님)에서의 CPU 이용률 |
| CPU Nice | Nice 처리된 부분의 CPU 이용률 |
| CPU System | 커널 영역의 CPU 이용률 |
| CPU Idle | Idle 상태의 CPU 이용률 |
| CPU Iowait | I/O 대기의 CPU 이용률 |
| CPU Irq | Irq(인터럽트) 처리의 CPU 이용률 |
| CPU Softirq | SoftIrq(소프트웨어 인터럽트) 처리의 CPU 이용률 |
| CPU Steal | 가상머신이 가로챈 만큼의 CPU 이용률 |
| CPU Guest | 가상머신이 이용한 만큼의 CPU 이용률 |
