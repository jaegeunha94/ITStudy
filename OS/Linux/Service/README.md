# 서비스 목록 보는법
```
전체 서비스 목록
# systemctl list-unit-files

disabled 서비스 목록 
# systemctl list-unit-files | grep disabled 


active 서비스 목록
# systemctl list-units --state active

failed 서비스 목록
# systemctl list-units --state failed
```


# Servie 파일 경로
## 1. /usr/lib/systemd/system/
RPM pakage로 설치하면 그에 대한 systemd 파일이 이 경로에 생성된다.   
예를 들어서 httpd 패키지를 설치하면 이 경로에 httpd.service 파일이 생성됨.

## 2. /run/systemd/system/
런타임에 생성된 Systemd unit files.

## 3. /etc/systemd/system
systemctl enable로 생성된 파일들이 저장된다.  

예시로 httpd를 부팅시에 자동으로 올라오도록 enable하면 해당 디렉터리 하위의   
multi-user.target.wants에 httpd 서비스 파일에 대한 심볼릭 링크가 생성된다.   

참고로 이때 multi-user.target은 run level3(텍스트 모드)로 서버를 사용할때  
default로 설정되는 Systemd Target이다. 


# Service 파일 탐색 
1. /usr/lib/systemd/system/
   * 여기를 우선으로 탐색
2. /etc/systemd/system/multi-user.target.wants
 * symbolic link로 만들어야함



# systemctl 명령어
`systemctl [명령] [서비스명]`


## systemctl 명령어 종류
* start : 서비스 시작
* stop : 서비스 중지
* status : 서비스 상태 확인 (서비스가 구동 중인지 아닌지 알 수 있음)
* restart : 서비스 재시작 (중지 -> 시작) : 보통 변경한 설정 후에 많이 사용
* reload : 서비스를 중지하지 않고 설정 값을 반영 (서비스가 중지되면 안되는 경우 사용)
* enable : 시스템이 재부팅하면 자동으로 서비스 실행하도록 등록
* disable : enable 한 서비스 해제


# 데몬 삭제 하는법
1. systemctl stop [servicename]
2. systemctl disable [servicename]
3. rm /etc/systemd/system/[servicename]
4. rm /etc/systemd/system/[servicename] # and symlinks that might be related
5. rm /usr/lib/systemd/system/[servicename]
6. rm /usr/lib/systemd/system/[servicename] # and symlinks that might be related
7. systemctl daemon-reload
8. systemctl reset-failed
