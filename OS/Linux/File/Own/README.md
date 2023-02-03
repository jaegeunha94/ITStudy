# chown 명령어로 소유자 변경하는 방법
`$ chown [OPTIONS] USER[:GROUP] FILE(s)`

## 소유자 변경
ls -l 명령어는 파일의 소유자가 누구인지 보여준다.

명령어를 입력하면 아래와 같은 결과가 출력된다. 
여기서 js js는 Owner와 Group을 의미한다.
첫번째 js가 Owner를 의미하고, 두번째가 js가 Group을 의미한다.

```
$ ls -l
-rwxr-xr-x 1 js js 6  3월 10 16:02 file1.txt
```

이제 chown 명령어를 사용하여 Owner를 root로 변경해보자.  
(변경하려는 Owner가 root이기 때문에 sudo를 사용해야 한다)

`$ sudo chown root file1.txt`

## 이제는 Group을 root로 변경='
앞에 :를 붙이면 그룹의 소유자가 변경된다.

```
$ sudo chown :root file1.txt

$ ls -l
-rwxr-xr-x 1 root root 6  3월 10 16:02 file1.txt
```

Owner와 Group을 동시에 변경하려면   
owner:group처럼 모두 입력해주면 된다.

```
$ sudo chown js:js file1.txt

$ ls -l
-rwxr-xr-x 1 js js 6  3월 10 16:02 file1.txt
```

추가로, root와 같은 유저 이름(문자열) 대신  
유저 아이디(숫자)를 입력할 수도 있다.

`$ chown 1000:1000 file1.txt`

참고로 UserName은 UserId로 관리되고 있는데요.  
id 명령어를 치시면 유저이름과 유저 아이디를 볼 수 있다.

```
$ id
uid=1000(js) gid=1000(js) groups=1000(js),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),105(kvm),121(lpadmin),131(sambashare),133(docker)
$ id root
uid=0(root) gid=0(root) groups=0(root)
```


## 하위 폴더 소유자 모두 변경하기(Recursive)
위의 예제는 파일 1개의 소유자를 변경하는 것이다.

Recursive하게(재귀적으로) 모든 하위 폴더의 소유자를 함께 변경하고 싶을 때가 있다.

아래 명령어처럼 -R 옵션을 추가하면 설정하려는 폴더와  
그 폴더의 모든 하위 파일들의 소유자들도 함께 변경이 된다.

`$ chown -R js:js folder`

위 명령어 수행 후,  
하위 파일들의 소유자를 확인해보면 모두 변경된 것을 볼 수 있다.



# 참조
[araikuma tistory](https://araikuma.tistory.com/117)
