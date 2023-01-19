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

