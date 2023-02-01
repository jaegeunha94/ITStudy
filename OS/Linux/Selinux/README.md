# SELinux란?
SELinux(Security-Enhanced Linux)는 관리자가 시스템 액세스 권한을  
효과적으로 제어할 수 있게 하는 Linux® 시스템용 보안 아키텍처다.  
이는 원래 미국 국가안보국(NSA)이 LSM(Linux Security Module)을 사용하는 
Linux 커널에 대한 일련의 패치로 개발한 것입니다.  

SELinux는 2000년에 오픈소스 커뮤니티에 릴리스되어  
2003년에 업스트림 Linux 커널로 통합되었다.

즉, SELinux 는 Linux의 보안을 강화해 주는 보안 강화 커널이고  
zero-day 공격 및 buffer overflow 등  
어플리케이션 취약점으로 인한 해킹을 방지해 주는 핵심 구성요소이다.

특정 서비스가 SELinux 때문에 동작하지 않는다면 SELinux 를 끄기 보다는  
해당 서비스가 SELinux 하에서 잘 동작하도록 설정을 수정하는걸 권장한다.

# 📑SELinux는 어떻게 작동하나요?
SELinux는 시스템의 애플리케이션, 프로세스,  
파일에 대한 액세스 제어를 정의한다.  

액세스할 수 있는 항목과 액세스할 수 없는 항목을 SELinux에 지정하는  
룰 세트인 보안 정책을 사용하여 정책에서 허용되는 액세스를 적용한다. 

주체로 알려진 애플리케이션이나 프로세스가 파일과 같은 객체에 대한 액세스를 요청하는 경우,    
SELinux는 주체와 객체에 대해 권한이 캐시되는 AVC(Access Vector Cashe)를 확인한다.

SELinux가 캐시된 권한에 기반한 액세스에 대한 결정을 내릴 수 없는 경우  
보안 서버에 요청을 전송한다.  

보안 서버는 애플리케이션이나 프로세스의 보안 컨텍스트와 파일을 확인한다.  
보안 컨텍스트는 SELinux 보안 정책 데이터베이스에서 적용된다.  
그러면 권한이 허용되거나 거부된다. 

권한이 거부되면 "avc: denied"라는 메시지가 

`/var/log.messages`에 나타난다.

# 📑SELinux 구성 방법 
SELinux를 설정하여 시스템을 보호할 수 있는 몇 가지 방법이 있다.  
가장 일반적인 방법은 타겟 정책 또는 다단계 보안(Multi-Level Security, MLS)이다.

타겟 정책은 기본 옵션으로서 다양한 프로세스와 태스크 및 서비스를 처리한다.  
MLS는 매우 복잡할 수 있으며, 일반적으로 정부 기관에서만 사용한다. 

/etc/sysconfig/selinux 파일을 보면 시스템이  
어떤 방법으로 실행되어야 하는지 알 수 있다.  

파일에는 SELinux가 허용 모드인지 실행 모드인지 아니면  
비활성화 모드인지와 어떤 정책을 로드해야 하는지를 보여주는 섹션이 있다.



# 📑SELinux 오류를 처리하는 방법
SELinux에서 오류가 발생하는 경우 해결해야 하는 사항이 있다.  
다음의 4가지 일반적인 문제 중 하나일 수 있다.

1. 레이블이 잘못된 경우: 레이블이 올바르지 않은 경우 툴을 사용하여 레이블을 수정할 수 있다.
2. 정책 수정이 필요한 경우: 변경 사항에 대해 SELinux에 알려야 하거나 정책을 조정해야 할 수도 있다.  
  * 부울 또는 정책 모듈을 사용하여 이를 해결할 수 있다.
3. 정책에 버그가 있는 경우: 정책에 해결해야 하는 버그가 존재할 수 있다.
4. 시스템이 침입을 당한 경우: 대부분의 경우 SELinux가 시스템을 보호할 수는 있으나 시스템 손상이 일어날 가능성은 여전히 존재한다.  
  * 이런 상황이 의심되는 경우 즉시 조치를 취해야 한다.


> 부울이란?  
> 부울(boolean)이란 SELinux의 기능에 대한 활성화/비활성화 설정이다.  
> SELinux 기능을 켜거나 끌 수 있는 수백 가지 설정이 있으며   
> 이 중 다수는 이미 사전 정의되어 있다.  
> getsebool -a를 실행하여 시스템에 이미 설정된 부울을 찾을 수 있다.
> 

# SELinux 동작 모드
enforce, permissive, disable 세 가지 모드가 있으며 RHEL/CentOS 를 설치하면  
default 로 enforce mode 로 동작하며 SELinux 의 rule 에 어긋나는 operation 은 거부된다.

현재 SELinux 의 동작 모드는 sestatus 명령어로 확인할 수 있다.

# SELinux 모드 확인

`$ sestatus`

```
SELinux status: enabled
SELinuxfs mount: /selinux
Current mode: enforcing
Mode from config file: enforcing
Policy version: 24
Policy from config file: targeted
Plain text
```

Permissive mode 는 rule 에 어긋나는 동작이 있을 경우 audit log 를 남기고 해당 operation 은 허용된다.

개발 서버일 경우 특정 daemon 이나 서비스에 문제가 있을 경우 setenforce 0 으로  
Permissive mode 로 전환하여 문제 해결후 enforce mode 로 전환하는걸 추천한다.

```
$ setenforce 0
$ sestatus 

SELinux status: enabled
SELinuxfs mount: /selinux
Current mode: permissive
Mode from config file: enforcing
Policy version: 24
Policy from config file: targeted
```


# SELinux 해제

인터넷에 연결된 리눅스 서버라면 SELinux 해제는 결코 추천하지 않는다.  
해제할 경우 다시 활성화 시키려면 재부팅이 필요하며 재부팅시 모든 자원에 대해  
보안 레이블을 설정해야 하므로 부팅 시간이 매우 오래 걸릴 수 있다.  

## 1. SELinux 설정 파일을 편집기로 연다.
```
RHEL/CentOS 8

vi /etc/selinux/config
```
```
RHEL/CentOS 7 이전

vi /etc/sysconfig/selinux
```

## 2. SELINUX=enforcing 을 SELINUX=disabled 로 변경후 저장한다.
```
SELINUX=disabled
```

## 3. reboot

SELinux 를 해제후 다시 켤 경우 relabel 이 필요하며 이때 잘못된 설정이 있을 경우 부팅이 안 되거나  
ssh 로 원격 접속이 불가능할 수 있으므로 enforcing 모드가 아닌 permissive 로 설정후 재부팅하는 것을 권장



# SELinux 사용시 port 관리 방법
## 오픈된 http port 확인방법
`# semanage port -l | grep http_port_t`

## http port 오픈하기 위해 추가해주는 방법
`# semanage port -a -t http_port_t -p tcp 33081`



# 참조
[redhat](https://www.redhat.com/ko/topics/linux/what-is-selinux)

[lesstif](https://www.lesstif.com/system-admin/centos-selinux-6979732.html)
