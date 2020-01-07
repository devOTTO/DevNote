# TOAST 기초 실습
	
## Instance 생성 후 접속 실습 - 참고 링크 : [toast 유튜브](https://www.youtube.com/watch?v=qPclADHluk0&list=PL42XJKPNDepYOHjEXL12p9EAItp2InPlj&index=1)
### Issue#1 : Floating IP 미할당 (solved)
|구분	|Fixed IP (사설)	|Floating IP (공인)|
|-------|:-------:|:-------:|
|서비스 내 IP 할당 방식|	DHCP	|사용자 지정|
|유효구간	|VPC 내부에서만 접근 가능|	VPC 외부에서 접근 가능|
		
  > [출처](https://meetup.toast.com/posts/169) 
		
*Floating IP(공인) : 외부에서 인스턴스에 접근하기 위해 할당되어야 하는 IP
![image](https://user-images.githubusercontent.com/42940194/71861393-928f8e80-3139-11ea-83d6-105465b27e26.png)

		
* NAT(Network Address Translation) : IP 패킷의 TCP/UDP 포트 숫자와 소스 및 목적지의 IP 주소 등을 재기록 하면서 라우터를 통해 네트워크 트래픽을 주고 받는 기술
  * 사용 목적 : 인터넷의 공인 IP 주소 절약
  * 인터넷이란 공공망과 연결되는 사용자들의 고유한 사설망을 침입자들로 부터 보호

* IPV4 - CIDR(Classless Inter-Domain Routing) 
  * EX) 192.30.250.00/18 : 전체 32BIT 중 18bit 가 네트워크 주소 부분, 나머지 14 bit가 특정 호스트 주소 라는 
> [참고](https://yagi815.tistory.com/171)
		
	
### Issue#2 : unprotected private key (solved) 
![image](https://user-images.githubusercontent.com/42940194/71861592-35e0a380-313a-11ea-9ca7-59414d04b4e2.png)
  * puttygen에서 생성 시 암호어구 사용해 해결 
