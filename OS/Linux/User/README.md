# useradd adduser 차이
CentOS는 차이가 없다.

우분투에선  
useradd : 순수하게 '계정만' 생성해주고, 기본 쉘인 sh가 할당된다.  
홈 디렉터리나 비밀번호 등은 따로 설정이 필요하다. 

adduser : 계정 생성 시 비밀번호와 사용자의 기타 정보를 입력받는다.  
옵션을 통해 기본 쉘이나 로그인 옵션 등도 설정할 수 있다.  
홈 디렉토리도 자동 생성된다.

왠만하면 adduser를 사용하는게 좋다



# 🔖사용자 관리 관련 파일

## 📑/etc/passwd
* 사용자의 기본 정보를 저장하고 있음

### /etc/passwd 구성
1. 사용자 이름
2. 암호(진짜 암호가 아닌 임의의 값이 기록되어 있음)
3. 사용자 ID
4. 그룹 ID
5. 주석 
6. 홈 디렉터리
7. 로그인 셸

콜론을 기준으로 7번째 필드를 출력

`cut -d : -f 7 /etc/passwd`

## ❗/bin/bash
* /bin/bash: root 사용자가 로그인할 때 사용되는 쉘
* cat /etc/passwd | grep bash
 * 시스템이 아닌 로그인할 때 사용하는 계정


## 시스템 계정 종류

### root
* 시스템에서 모든 권한을 가지고 있는 최고 권한 사용자
* 시스템 내의 보호되는 파일이나 퍼미션 등의 제한 사항에 대해 대부분 영향을 받지 않는다
* UID가 0번이다

### bin
* 시스템의 구동중인 바이너리 파일을 관리하기 위한 계정

### daemon
* 백그라운드 프로세스에 대한 작업을 제어하기 위한 시스템 계정

### adm
* 시스템 로깅이나 특정 작업을 관리하는 시스템 계정

### lp
* 프린트를 위한 계정

### gdm
* Gnome Display 관리 서비스 계정


## 📑/etc/shadow
* 사용자의 패스워드를 저장
* 설정한 패스워드가 암호화 -> 암호문 -> crypt(Salt값 추가 + 암호문 1) -> 암호문2
* 같은 패스워드라도 다른 값으로 암호화되어 저장
* $암호화 종류$salt$암호화된 값
* 1970년 이후로 비밀번호 바뀐지 얼마나 됐는지 알 수 있음


## passwd
* 비밀번호 변경 



## 📑/etc/group
* 그룹에 대한 정보를 저장하고 있음
  
  
  
# 🔖사용자 초기화 파일
* 적용되는 범위가 다름

## 📑/etc/profile
* 시스템 전역에 걸쳐 환경을 설정하는 파일
* 모든 사용자가 적용되는 파일


## 📑~/.profile
* `개별 사용자`의 홈 디렉토리에 들어있는 파일, 해당 사용자의 설정을 변경할 때 사용


## 📑~/.bashrc
* `개별 사용자`의 홈 디렉토리에 있는 파일
* 해당 사용자의 쉘 관련 설정을 변경할 때 사용
* 환경 변수, 쉘 프롬프트 모양(명령어 앞에 붙는 내용), 별명 기능(alias), 쉘 옵션 정의 등 설정 가능

## ❗ su - root
* su - root
  * 사용자 초기화 파일 적용
* su root
  * 사용자 초기화 파일 적용 x


## 📑환경변수
* 시스템 환경에 대한 설정을 저장하고 있는 변수
* HOME: 사용자의 홈디렉토리
* PATH: 실행파일을 찾는 경로
* LANG: 프로그램 사용시 기본 지원되는 언어
* SHELL: 로그인해서 사용하는 쉘
* EDITOR: 기본 편집기의 이름
* PS1: 명령프롬프트 변수
