# [CloudHub](https://github.com/snetsystems/cloudhub)
클라우드 허브 제품은 서버, 어플리케이션, 가상 머신, 쿠버네티스 등을 모니터링해주는 제품,  
또한 모니터링한 데이터를 시각화하여 보여주고 알람을 설정할 수 있음

# Cloudhub 제품 구성
## 1. SaltStack
인프라의 규모가 커질 수록 서버 설치와 설정에 대한 부담이 늘어날 때 (특히 트래픽이 급격히 늘어나는 경우)  
서버를 구축하기 위해서 대규모 인프라를 관리하기 위한 자동화 관리 시스템이다. 

### Salt-Master
* Saltstack에서 Server 역할을 담당한다. 
* Master는 등록된 Minion 에게 명령을 publishing 하고 그 결과를 취합하여 보여주는 역할이다.  
* 1대의 single master가 minion 수천대까지 관리 가능하다. 

### Salt-Minion
* Agent의 역할이며, 구성 자동화를 하기 위한 대상 서버에 설치하면 된다.  
* Master의 명령을 기다리고 있다가 명령이 오면 그에 맞춰 작업을 수행하게 된다. 
* 만약, 서버에 minion을 설치하기 어려운 상황이라면 Ansible 처럼 SSH로 명령 push가 가능하게도 지원된다.

### ZeroMQ
* Salt-master와 Salt-minion 간 통신에 ZeroMQ 라는 비동기 메시징 라이브러리를 사용한다.
* 따로 설치해야 하는 것은 아니며 Salt-master를 설치하면 zeroMQ도 함께 설치 된다. 
* publish Port로 4505 / Return Port 로 4506 을 사용한다. (포트 수정 가능)


## 2. Telegraf
* 데이터 수집 에이전트
* 플러그인을 기반으로 데이터 수집 가능

### Telegraf 플러그인 종류
#### Input Plugin
* collect metrics from the system, services, or 3rd party APIs

#### Processor Plugins 
* transform, decorate, and/or filter metrics


#### Aggregator Plugins 
* create aggregate metrics (e.g. mean, min, max, quantiles, etc.)


#### Output Plugins write metrics to various destinations
* write metrics to various destinations


## 3. InfluxDB 
* InfluxDB는 시계열 데이터베이스
* Time Seriese Data(시계열 데이터)는 시간에 따라 저장된 데이터
* 시계열 데이터베이스는 '하나 이상의 시간'과 '하나 이상의 값' 쌍을 통해 시계열을 저장하고 서비스하는데 최적화된 소프트웨어 시스템


## 4. Cloudhub
클라우드 허브 제품은 서버, 어플리케이션, 가상 머신, 쿠버네티스 등을 모니터링해주는 제품,  
또한 모니터링한 데이터를 시각화하여 보여주고 알람을 설정할 수 있음


# Cloudhub 개발 환경
## 1. IDE
### Visual Studio Code
* 1.74.2


## 2. 가상화
### Virtual Box
* VirtualBox-6.1.32-149290-Win

## 3. 운영체제
### CentOS
* CentOS-7-x86_64-DVD-2009
  * CentOS Linux release 7.9.2009 (Core)


## 4. 프론트
### node
* node-v12.22.1-x64

### React
* ^16.12.0


## 5. 백엔드
### go 
* 1.16.x


# 작업
## CentOS7
### CentOS7 - NGinx
* [HTTP 2.0 적용](https://github.com/jaegeunha94/ITStudy/tree/main/Server/Nginx/Configuration#cloudhub-nginx-%EC%84%A4%EC%A0%95)
* [NGinx에 WebSocket용 옵션 설정](https://github.com/jaegeunha94/ITStudy/blob/main/Network/HTTP/Header/Upgrade/README.md#nginx-websocket-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EC%82%AC%EC%9A%A9-%EC%8B%9C-conf-%ED%8C%8C%EC%9D%BC-%EC%84%A4%EC%A0%95)
* [NGinx WebSocket Timeout로 인한 proxy_read_timeout 옵션 적용](https://github.com/jaegeunha94/ITStudy/tree/main/Server/Nginx/Configuration#7-proxy_read_timeout)

---

# Commit 내역
### [Add an input filter to telegraf test function on UI. #348](https://github.com/snetsystems/cloudhub/commit/d08b08427a597106c55971a5c2f2f07c8eeaac48)
* Telegraf Plugin Test 화면 추가

### [Prevent writing telegraf.conf file when conf file isn't changed in telegraf test. #348](https://github.com/snetsystems/cloudhub/commit/84baa90090b01306ba235de465362df956346c91)
* telegraf conf 파일 변경되지 않았을 때 telegraf conf 파일 덮어쓰는 동작 막는 로직 추가

### [Add the export/import function of topology map. #351](https://github.com/snetsystems/cloudhub/commit/313033480edf06bfac67720d66345221b5fd3f55)
* Topology 탭에 import 및 export UI 및 기능 추가

### [Move topology save button to the back of export button #351](https://github.com/snetsystems/cloudhub/commit/5be3cad4cbc03946523d0a54b84de02f1808fa25)
* Topology 탭에 export 버튼 위치 변경

### [Change logics of Test and Apply buttons in Agent Configuration](https://github.com/snetsystems/cloudhub/commit/38c15fb53cc15767ed792ed04a59ab897fb3f643)
* AgentConfiguration 화면의 Test 및 Apply 로직 수정

### [Add Toml Basic Toggle Radio Button in ServiceConfig](https://github.com/snetsystems/cloudhub/commit/717dc89f876480985dcd21ba1d02d4c2c23b2959)
* ServiceConfig 탭의 토글 버튼 UI와 그에 맞는 기능 추가

### [Add testing cloud input plugin before writing telegraf conf file in ServiceConfig](https://github.com/snetsystems/cloudhub/commit/6488e96de9dc334e43fa7e3221b3751633134db3)
* Cloud Input Plugin 적용 전 Telegraf 테스트 로직 후 성공 시 Conf 파일 적용하는 로직 추가

### [Fix Telegraf test filename in AgentConfiguration](https://github.com/snetsystems/cloudhub/commit/f475386dfcd81a16c16cfbe54317a5cf9b2d7f15)
* AgentConfiguration 탭의 Telegraf Test File 명 변경

### [Fix Telegraf test filename in ServiceConfig](https://github.com/snetsystems/cloudhub/commit/16eb9c8b59ac5fc1cf13b690661863d55c0c2c1c)
* ServiceConfig 탭의 Telegraf Test File 명 변경

### [Fix Remove checking statusText when response status is 200 in ServiceConfig, AgentControl and GridLayoutRenderer](https://github.com/snetsystems/cloudhub/commit/46bf191c032f3f49b3920854e50f7083e2a5c823)
* NGinx 적용 후 변경된 response status에 맞게 분기문 변경


### [Add Collector Server Filtering in AgentConfiguration, ServiceConfig](https://github.com/snetsystems/cloudhub/commit/645094cc662c54daf7e20e58ad3aea1842adbfb7)
* AgentConfiguration 탭과 ServiceConfig 탭에 Collector Server를 HostName으로 필터링하는 기능 추가


### [Add selecting IP feature in AgentMinions Console](https://github.com/snetsystems/cloudhub/commit/a067b0c2cf44b2a279b5ca5ac9dc72971d3d0430)
* Agent Minions 탭에서 Conosle을 클릭할 때 IP 선택할 수 있는 UI 추가

### [Modify API Multi Request Logic not to transfer the token of salt to the client](https://github.com/snetsystems/cloudhub/commit/702dfccde62bb4be69d296ee9cd67c11c4bc75e0)
* Salt API를 Multi로 호출하는 함수 로직 수정


### [Add InsecureSkipVerify Option in Salt Reverse Proxy](https://github.com/snetsystems/cloudhub/commit/f4790f0375428c9f9c4b03f2158e774b85e4b10e)
* Salt API 호출 시 HTTPS 적용에 개발자 인증서를 사용하기 때문에 InsecureSkipVerify 옵션을 넣어, 인증서 Verify 작업을 Skip하는 로직 추가


### [Add Support OS ToolTip in Collector Control Tab](https://github.com/snetsystems/cloudhub/commit/6478ca453aaf0f29c731484ada33c3efcafac75b)
* Collector Control Tab에서 OS 운영체제 지원 범위 알려주는 ToolTip UI 추가
