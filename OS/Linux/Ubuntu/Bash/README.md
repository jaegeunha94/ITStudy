
# 우분투 기본 쉘, dash 에서 bash로 변경하는 법
우분투의 기본 쉘은  
bash가 아닌 dash입니다.

하지만 개발용 shell script는  
bash 용으로 작성된 것들이 많으며

이로 인해 호환성 문제가  
발생하는 경우가 있습니다.


## 우분투 기본 쉘 확인하는 방법
기본 쉘을 확인하기 위해서  
/bin/sh 경로를 확인해 봅니다.

```
$ ls -al /bin/sh

lrwxrwxrwx 1 root root 4  9월 12 12:33 /bin/sh -> dash
```

위와 같이

`/bin/sh -> dash`

라고 나온다면 기본쉘이 dash인 겁니다.


## 기본 쉘을 배시로 바꾸는 방법
기본 쉘을 배시로 바꾸려면  
다음과 명령어를 치시면 됩니다..

`$ sudo dpkg-reconfigure dash`

나오는 화면에서   
**'No' 라고 답변**


## 바뀐 기본쉘 다시 확인하기

그러면 아래처럼 바뀝니다.

```
$ ls -al /bin/sh
lrwxrwxrwx 1 root root 4  9월 12 22:34 /bin/sh -> bash
```
