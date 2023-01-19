# CentOS7 hostname 변경하는 법

## 1. CentOS7 hostname 변경 (Runtime)
Runtime에 hostname을  
변경하는 방식입니다.

Runtime에 hostname이 바뀌지만  
일시 적용이기 때문에  
재부팅 후 hostname이 원래대로 돌아갑니다.

영구 반영을 하려면  
\/etc/hostname 파일을 수정해야 합니다.


아래의 명령어를 치면  
Runtime에 hostname 이 변경됩니다.


`echo "사용할 hostname" > /proc/sys/kernel/hostname`


변경된 hostname은 exit를 입력하여   
로그아웃한 뒤에 보입니다.


## 2. CentOS7 hostname 변경 (영구 적용, 재부팅 필요)
hostname을 영구 반영하는 방식은  
두 가지가 있습니다.

다만, 두 가지 방식 모두  
재부팅 후 적용이 되기 때문에  
아래의 방식을 적용 후 재부팅을 하시면 되겠습니다.


### 방식 1
```
vi /etc/hostname

localhost.localdomain 삭제 후 사용할 호스트명 입력
```

### 방식 2
`hostnamectl set-hostname [사용할 호스트명]`



## 3. hostname 확인하는 법
```
[root@hostname ~]# hostname
hostname_출력
```


# 참고자료
[Nirsa 블로그](https://nirsa.tistory.com/145)
