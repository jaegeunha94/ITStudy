# BASH IFS
IFS는 Internal Field Separator의 약자로 외부프로그램을 실행할 때  
입력되는 문자열을 나눌 때 기준이 되는 문자를 정의하는 환경 변수이다.

터미널에서 환경변수를 출력해보면 공백문자가 출력 됨을 볼 수 있다.

IFS는 디폴트 값은 공백/탭/개행 문자다. (space, tab, new line)

쉘 스크립트에서 for in 문법을 보면,  
공백문자로 띄워진 하나의 문자열이 마치 배열처럼 하나씩 순회하는 것을 볼 수 있을 것이다.

```BASH
#!/usr/bin/bash 
 
mystring="foo bar baz rab"
 
for word in $mystring; do
  echo "Word: $word"
done
```

```SHELL
$ bash script.sh
Word: foo
Word: bar
Word: baz
Word: rab
```
 

IFS값이 공백이라 공백에 따라 단어가 쪼개진다는 것은 알았다.

그럼 IFS값을 바꾸면 어떻게 될까?

```BASH
#!/usr/bin/bash 
 
IFS=':' # 문자열 구분을 : 로 한다.
mystring="foo bar baz rab"
 
for word in $mystring; do
  echo "Word: $word"
done
```

```SHELL
$ bash script.sh
Word: foo bar baz rab
```

결과에서 볼 수 있듯이, 그대로 하나의 문자열로 출력이 되어버렸다.

단어를 쪼개는 기준의 되는 문자가 공백에서 : 로 바뀌었기 때문이다.

응용하자면 문자열을 "foo:bar:baz:rab" 로 바꾸면  
처음처럼 정상적으로 순회가 될 것이다.

하지만 IFS는 환경변수이기 때문에,  
어느 한 쉘 스크립트에서 IFS값을 바꿔버리면 다른 쉘 스크립트에서 오작동이 일어날 수 있다.

위와 같이 단어 구분을 : 로 바꿨는데,

다른 쉘에서 공백으로 단어를 구분해 루프를 한다고 알고리즘을 짜 놨으면,  
코드가 완전히 망가지게 되기 때문이다.

따라서 만일 IFS을 바꿔서 사용할 일이 있으면,  
반드시 마지막에 다시 초기 IFS값으로 롤백해야 하는 로직을 반드시 구현해 놓아야 한다.

```BASH
#!/usr/bin/bash 
 
PRE_IFS=$IFS # 본래 IFS값을 백업해논다.
IFS=':' # 문자열 구분을 : 로 한다.
mystring="foo bar baz rab"
 
for word in $mystring; do
  echo "Word: $word"
done
 
IFS=$PRE_IFS # IFS 원상 복구
```

> Info  
> IFS는 디폴트 값은 공백/탭/개행 문자다.  
> 만일 공백이 없다면 개행으로 각 단어를 구분한다.  
> 다음은 IFS를 우선값을 개행으로 바꾸는 예제이다.

```BASH
PROGRAMMING="
java programming
python programming
c programming
" # 개행이 있는 문자열
 
echo "-------------------------"
for p in $PROGRAMMING; do
	echo $p # IFS는 공백을 먼저 구분자로 하고, 공백이 없다면 개행을 구분자로 설정한다.
done
 
 
# IFS를 줄바꿈(newline)로 변경
OLD_IFS="$IFS" # 백업
IFS=$'\n'
 
echo "-------------------------"
for p in $PROGRAMMING; do
echo $p
done
 
IFS="$OLD_IFS" # 롤백
```

```SHELL
$ bash test.sh
-------------------------
java
programming
python
programming
c
programming
-------------------------
java programming
python programming
c programming
````


# 참조
[inpa tistory](https://inpa.tistory.com/entry/LINUX-%F0%9F%93%9A-IFSInternal-Field-Separator-%EC%95%8C%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC)
