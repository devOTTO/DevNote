### Apache , Tomcat

* Apache?
    * 정적인 데이터를 처리하는 웹서버
    * 단순 HTML, 이미지를 처리하는 서버라면 빠르고 안정적
* Apache Tomcat?
    * 동적인 데이터를 처리하는 웹서버
    * WAS(Web Application Server), 웹 서버와 웹 컨테이너의 결합
    * DB 연결, 데이터 조작 등과 같은 처리시 활용
* Tomcat Worker
    * 웹 서버로부터의 서블릿 요청을 톰캣 프로세스에게 전달하여 요청을 처리하는 톰캣 인스턴스
    * 대부분 하나의 worker를 사용하나, load balancing이나 site 파티셔닝을 위해 여러개의 worker를 사용 할 수 있다.
    * 워커 타입 : ajp12, ajp13, jni, lb 등
* WAS의 구성
    * 사용자 요청(웹 브라우저) -> 웹 서버 -> WAS(동적 처리) -> 웹 서버 -> 사용자 응답 메세지(웹 브라우저)
    ![image](https://user-images.githubusercontent.com/42940194/71963976-1633a300-3240-11ea-846a-c601d117bc42.png)

* 아파치와 톰캣의 차이점과 톰캣 앞에 아파치를 두는 이유
    * 톰캣에서 아파치의 기능(웹 서비스 데몬, httpd)를 포함
    * static 파일(css, js, html, image)는 tomcat 앞 httpd를 두어, 외부 요청은 apache httpd가 받고, tomcat 내 처리해야할 리소스를 tomcat이 처리하게 하는 구조는 tomcat3에서 흔한 구조

> tomcat 5.5 부터 httpd의 native 모듈을 사용해 static 파일을 처리하는기능을 제공해, 단순 static 파일 처리의 성능만을 위해 apache httpd를 두는 것은 불필요

> httpd 서버를 간단한 로드밸런싱을 위해 사용하거나, 하나의 서버에서 php, java application을 함께 사용하는 경우 등에 httpd를 앞에 두고 tomcat 연결

### mod\_jk

* AJP(Apache Jserv Protocol) : 아파치에서 웹서버 뒤에 있는 어플리케이션 서버로부터 웹서버로 들어오는 요청을 위임할 수 있는 바이너리 프로토콜
* 아파치와 톰캣이 연동하기 위해 **AJP**를 통해 서로 통신으로, mod\_jk와 같은 모듈이 AJP 프로토콜을 사용해 톰캣과 연동하기 위해 만들어진 모듈
* Apache httpd web server와 tomcat을 연계하는 방법
    1. tomcat connector(mod\_jk)
        * URL 이나 컨텐츠별로 유연한 매핑이 가능(이미지등 정적 컨텐츠와 .php 스크립트는 웹서버, .서블릿은 톰캣에서 처리)
        * 전용 바이너리 프로토콜이므로 mod\_proxy 보다 빠른 경우가 많음.
        * 장점
            * 발전된 load balancer
            * 발전된 노드 장애 감지
            * 8K 이상의 큰 AJP 패킷 지원
        * 단점
            * 별도 모듈을 빌드하고 관리 필요
            ![image](https://user-images.githubusercontent.com/42940194/71964007-264b8280-3240-11ea-8cb5-5ac729fb582b.png)

    2. mod\_proxy
        * 별도 모듈 설치가 필요없고(apache 기본 모듈) 설정이 간편함
        * 장점
            * 컴파일 하고 관리할 별도 모듈이 필요 없이 mod\_proxy, mod\_proxy\_http, mod\_proxy\_ajp, mod\_proxy\_balancer가 표준 apache 2.2 이상 배포
            * 동일한 balancer에 http, https, AJP 프로토콜 사용 지원
        * 단점
            * mod\_proxy\_ajp는 8k 이상 패킷 사이즈 지원 X
            * 기본적은 load balancer
        * HTTP 리버스 프락시로 동작하므로 특정 WAS에 의존적이지 않으므로 HTTP 를 제공하는 모든 WAS에 적용 가능
        * Forward Proxy
        ![image](https://user-images.githubusercontent.com/42940194/71964052-37948f00-3240-11ea-94a6-cd87ad6e258b.png)

            * 특정 사이트 연결시 사용자 pc가 직접 연결하는 것이 아닌 forward proxy 서버가 요청을 받아 사이트에 연결 후 그 결과를 client에 forward
            * 특징 : 대게 캐싱 기능을 가져 자주 사용되는 컨텐츠일 경우, 성능 향상
        * Reverse Proxy
        ![image](https://user-images.githubusercontent.com/42940194/71964123-572bb780-3240-11ea-8248-4da546d40bf1.png)
            * WAS나 DB의 경우 내부 망에 위치하므로, proxy server를 두고 proxy가 내부 서비스와 통신해 결과를 client에게 제공하는 방식으로 서비스
        * mod\_proxy\_http
            * mod\_proxy 모듈의 http 관련 버전으로, mod\_jk와 동일한 기능을 대부분 제공하지만 HTTP 프로토콜을 통해 통신
            * HTTPD와 Tomcat 사이의 통신을 암호화할 경우, mod\_proxy\_http 사용이 더 쉬움
            * SSL 필요 시 mod\_jk가 더 쉬움
        * mod\_proxy\_ajp
            * ajp 를 지원하는 mod\_proxy 모듈
        * mod\_proxy\_balancer
            * provide load balancing support for HTTP, FTP and AJP13 protocols

### mod_ssl

* provides SSL v3 and TLS v1.x support for the Apache HTTP Server. SSL v2 is no longer supported.

### 세션 클러스터링

* 세션은 WAS 자체에 저장되기 때문에, 두 대 이상의 WAS를 이용하는 경우, 또는 failover이나 auto scaling으로 대체된 WAS 에게도 세션이 공유되어야 하기 때문에 동일한 세션을 여러 곳에서 사용할 수 있도록 관리하는 것
* clustering [참고](https://github.com/devOTTO/DevNote/blob/master/iaas-lb-clustering.md#%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0%EB%A7%81)

## 참고 자료

* https://ithub.tistory.com/101
* https://limmmee.tistory.com/4
* https://wikidocs.net/16276
* https://cornswrold.tistory.com/329
* https://joont.tistory.com/55
* https://goddaehee.tistory.com/77
* https://www.lesstif.com/pages/viewpage.action?pageId=20776817
* https://soul0.tistory.com/522
* http://publib.boulder.ibm.com/httpserv/manual70/mod/mod\_proxy\_balancer.html
* https://httpd.apache.org/docs/current/mod/mod\_ssl.html
