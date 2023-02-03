# Alias 설정 & 사용 방법
Alias는 명령어를 간소화하여 다른 이름으로 사용할 수 있도록 해주는 쉘내부 명령어이다.

즉 복잡한 명령어나 여러 옵션을 사용하는 명령어를 간단한 이름으로 사용할 수 있도록 하는 명령어이다.

 

alias는 간단히 alias명령으로 설정이 가능하다. 

```BASH
# 현재 등록되어있는 모든 alias 출력
$ alias
 
# ls -asl을 lss만 쳐도 실행될수 있게 별칭 설정
$ alias lss='ls -asl' 
 
# 별칭 삭제
$ unalias lss
```

그러나 이 방법은 시스템을 재부팅하고나면  
다시 초기화되므로 매번 적용해야 하는 불편함이 따르게 된다.

 

그래서 이러한 alias를 특정 파일에 설정해두면 매번 부팅시마다  
자동으로 적용되게 할 수 있는데, 가장 대표적인 것은 ~/.bashrc 파일로 설정이 가능하다.

 
bash가 실행될 때 bash의 환경 정보가 포함된 .bashrc 파일을 읽어 들인다.

이 환경 정보에는 별칭 정보도 함께 들어가기 때문에, .bashrc 파일에 별칭을 설정해두면 언제나 별칭이 적용된다.  
.bashrc 파일은 각 계정의 홈 디렉터리에 존재한다. 각 계정의 홈 디렉터리(~)에서 ls -al 명령어를 입력 후 최상단을 보면 찾아볼 수 있다.

```BASH
$ cat ~/.bashrc | less
# pageup / pagedown 키로 페이지를 넘겨서 볼 수 있다.
```

이 처럼 .bashrc 파일에 직접 별칭을 입력해도 되지만,  
기존 alias와 구분하기 위해 우리는 .bash_aliases 파일을 생성해 등록 할 것이다.

> Tip   
> .bash_aliases 파일은 셸 시작 시 가장 마지막에 읽어들이는 파일이다.  
> 따라서 .bashrc 파일에 동일한 별칭이 존재하여도 .bash_aliases 파일의 내용이 적용된다.

```BASH
$ vi .bash_aliases
```

vi 명령어로 .bash_aliases 파일을 생성하고,  
a 또는 i 눌러 입력모드로 진입해서 원하는 별칭 코딩을 한다.

그리고 ESC를 누르고 :wq 를 치고 엔터를 눌러 파일을 저장한다.

이렇게 영구적인 alias 별칭 설정을 마쳤다. (별거 아니죠?)


> Tip  
> .bash_aliases 파일 수정 및 저장 후에는 터미널을 재시작해야 해당 alias가 적용되어 사용 가능하다.

> Tip  
> 만약 모든 사용자에게 적용하기를 원한다면 /etc/profile 과 같은 곳에 선언해 두면 된다.


설정해두면 편한 별칭 모음
 

# git 단축 명령어
개발에서 git을 사용하는 경우 git status 명령을 자주 사용한다.

해당 명령을 gs로 설정해두는 식으로 설정하면 편하다.

```BASH
alias gs='git status'
```

```BASH
alias gl='git log --pretty=format:"%Cgreen%h%Creset %s %C(bold blue)%cr %C(yellow)%d %an %Creset  " --abbrev-commit;'
alias gb="git branch"
 
alias rr="git reset --hard HEAD^^^^^; git clean -df; git pull"
 
alias gco="git checkout"
alias gba="git branch -av"
alias grm="git remote"
 
alias glo="git log --oneline"
alias gln="git log --name-only"
alias glg="git log --pretty=format:\"%h %s\" --graph"
 
alias gcp="git cherry-pick"
alias grf="git reflog"
alias gr-hh="git reset --hard HEAD"
alias gr-hh1="git reset --hard HEAD^"
 
alias gst="git status"
alias gsl="git stash list"
alias gss="git stash save -u"
alias gsa="git stash apply"
alias gsp="git stash pop"
 
alias gcs="git commit -s"
alias gca="git commit --amend"
 
alias gg="git grep"
alias gd="git diff"
```
 

## CD-ROM을 쉽게 마운트 하기
```BASH
# 시스템에 따라 CD-ROM 디바이스의 명칭 또는 마운트 디렉터리가 상이할 수 있음
alias cdrom='mount /dev/cdrom/media/cdrom'
```

## 매번 반복되는 데이터 백업(tar)을 쉽게 하기
```BASH
# backup 이라는 alias 명령어를 만든 후, 이 명령을 cron을 통해 자동화(스케줄링) 하면 된다.
alias backup='tar czvf webS_backup.tar.gz /var/www/html'
```
 

## 웹서버(httpd) 데몬을 쉽게 구동
```BASH
# web 이라는 명령으로 웹서버 구동이 가능 
alias web-'/etc/init.d/httpd'


# web 명령어 사용
web start (시작)
web stop (중지)
web restart (재기동)
```
 

## 시스템 전체 업데이트
```BASH
alias powerup='sudo apt update && sudo apt -y upgrade && sudo apt -y dist-upgrade && sudo apt -y autoremove && sudo apt -y autoclean'
```
 

## 이외의 명령어
```BASH
alias agi="apt-get install"
 
alias h="history"
 
alias fna="find . -name "
 
alias fin="find ./ -iname" # -iname은 대소문자 무시
```

# 참조
[inpa tistory](https://inpa.tistory.com/entry/LINUX-%F0%9F%93%9A-Alias-%EC%84%A4%EC%A0%95-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95-%EC%A0%95%EB%A6%AC-%EB%8B%A8%EC%B6%95%EC%96%B4-%EC%98%88%EC%8B%9CTIP)
