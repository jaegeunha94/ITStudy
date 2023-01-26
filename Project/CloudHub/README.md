# [CloudHub](https://github.com/snetsystems/cloudhub)

# Cloudhub Setting
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

---

# 작업
## CentOS7
### CentOS7 - NGinx
* HTTP 2.0 적용
* [NGinx에 WebSocket용 옵션 설정](https://github.com/jaegeunha94/ITStudy/blob/main/Network/HTTP/Header/Upgrade/README.md#nginx-websocket-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EC%82%AC%EC%9A%A9-%EC%8B%9C-conf-%ED%8C%8C%EC%9D%BC-%EC%84%A4%EC%A0%95)

---

# Commit 내역
### [Add an input filter to telegraf test function on UI. #348](https://github.com/snetsystems/cloudhub/commit/d08b08427a597106c55971a5c2f2f07c8eeaac48)
* Telegraf Plugin Test 화면 추가

### [Prevent writing telegraf.conf file when conf file isn't changed in telegraf test. #348](https://github.com/snetsystems/cloudhub/commit/84baa90090b01306ba235de465362df956346c91)
* telegraf conf 파일 변경되지 않았을 때 telegraf conf 파일 덮어쓰는 동작 막기

### [Add the export/import function of topology map. #351](https://github.com/snetsystems/cloudhub/commit/313033480edf06bfac67720d66345221b5fd3f55)
* Topology 탭에 import 및 export 기능 추가

### [Move topology save button to the back of export button #351](https://github.com/snetsystems/cloudhub/commit/5be3cad4cbc03946523d0a54b84de02f1808fa25)
* Topology 탭에 export 버튼 위치 변경

### [At 371 Change logics of Test and Apply buttons in Agent Configuration](https://github.com/snetsystems/cloudhub/commit/38c15fb53cc15767ed792ed04a59ab897fb3f643)
* AgentConfiguration 화면의 Test 및 Apply 로직 수정

### [At #362 Add Toml Basic Toggle Radio Button in ServiceConfig](https://github.com/snetsystems/cloudhub/commit/717dc89f876480985dcd21ba1d02d4c2c23b2959)
* ServiceConfig 탭의 토글 버튼과 기능 추가

### [Add testing cloud input plugin before writing telegraf conf file in ServiceConfig](https://github.com/snetsystems/cloudhub/commit/6488e96de9dc334e43fa7e3221b3751633134db3)
* Cloud Input Plugin 적용 전 Telegraf 테스트 로직 후 성공 시 Conf 파일 적용하기

### [At #371 Fix Telegraf test filename in AgentConfiguration](https://github.com/snetsystems/cloudhub/commit/f475386dfcd81a16c16cfbe54317a5cf9b2d7f15)
* AgentConfiguration 탭의 Telegraf Test File 명 변경

### [At #362: Fix Telegraf test filename in ServiceConfig](https://github.com/snetsystems/cloudhub/commit/16eb9c8b59ac5fc1cf13b690661863d55c0c2c1c)
* ServiceConfig 탭의 Telegraf Test File 명 변경


### [At #362 Fix Remove checking statusText when response status is 200 in ServiceConfig, AgentControl and GridLayoutRenderer](https://github.com/snetsystems/cloudhub/commit/46bf191c032f3f49b3920854e50f7083e2a5c823)
* NGinx 적용 후 변경된 response status에 맞게 분기문 변경
