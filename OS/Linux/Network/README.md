# 리눅스 네트워크 설정
* 아래의 내용은 CentOS7 기준 네트워크 설정하는 방법이다.

## 네트워크 설정 필드 설명
- DEVICE: nic의 별칭 
- HWADDR: mac 주소
- TYPE: 네트워크 환경
- UUID: 장치 ID; 시스템에서 NIC를 구분하기 위해 부여 / 없어도 시스템에는 무관 
- ON_BOOT: 부팅시 NIC활성화 여부
- NM_CONTROLLED: NETWORK MANAGER 허용 여부
- BOOTPROTO: IP할당 방식; static - 고정 / dhcp - 유동 ( 자동 )
- IPADDR: IP 주소
- PREFIX (= NETMASK): PREFIX - 비트로 표현: 24 / NETMASK - 주소로 표현: 255.255.255.0
- GATEWAY: 게이트웨이 주소
  * -> 8번 , 9번 , 10번 은 7번 ( BOOTPROTO ) 값이 dhcp 일때는 사용 x  
- DNS1: 주 DNS 서버 주소
- DNS2: 보조 DNS 서버 주소

> -> ping 8.8.8.8 이 보내지지않을 경우 - 게이트 웨이 오류   
> -> ping www.google.com 이 보내지지않을 경우 - 도메인 주소 오

### uuid란
In this Linux network configuration, UUID stands for Universally Unique Identifier.  
It is a unique identifier assigned to the network interface enp0s8.

UUIDs are used to uniquely identify devices on a system, such as network interfaces, partitions, or storage devices.  
In this case, the UUID is used by the system to keep track of the network interface and its settings,  
even if the interface name or physical location changes.

UUIDs are generated using various algorithms and are designed to be globally unique,  
meaning that no two UUIDs should be the same.  
This makes them useful for identifying devices across different systems or networks.


> 아래의 명령으로 NIC에 할당된 UUID를 확인할 수 있다.  
> `nmcli connection`


## IP 동적할당
1. vi /etc/sysconfig/network-scripts/ifcfg-{네트워크 인터페이스}

```shell
TYPE=Ethernet
BOOTPROTO=dhcp
NAME=<network interface>
DEVICE=<network interface> // 그대로 알고 있으면 된다. 수정 안해도 된다.
ONBOOT=yes
DNS1=8.8.8.8
...
```

2. systemctl restart network / service network restart



## IP 정적할당
1. vi /etc/sysconfig/network-scripts/ifcfg-{네트워크 인터페이스}

```
// 수정
TYPE=Ethernet
BOOTPROTO=static // IP 동적 할당
NAME=<network interface>
DEVICE=<network interface> // 그대로 알고 있으면 된다. 수정 안해도 된다.
ONBOOT=yes // 자동 어댑터 활성화
IPADDR=<IP Address> // 자신이 원하는 IP로 변경해준다.
NETMASK=<NetMask> // NETMASK 설정
GATEWAY=<Gateway>
DNS1=8.8.8.8
...
```

2. systemctl restart network / service network restart

* PREFIX 값 또는 NETMASK 주소 둘 중 어느 것을 입력하든 상관없다.
* 또 맨 밑의 사용할 DNS 서버 주소 또한 여기에 입력해도 되고 입력하지 않아도 당장은 상관없다.



# 특정 인터페이스 다운/업(재시작)
```shell   
# ifdown ifcfg-eth0  
# ifup ifcfg-eth0 
```


# 참고 자료
[리눅스 youtube](https://www.youtube.com/watch?v=myc2GCENQS8&list=PL0d8NnikouEXVn9FfoX2XVlGgEArLDiLZ&index=36)
