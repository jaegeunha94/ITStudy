# DNS 서버 종류

## 기지국 DNS 서버
인터넷을 설치시 각각 통신사가 있다.  
그리고 각각의 통신사마다 DNS 서버가 존재한다.

위에서 다뤘듯이, URL에 Domain Name을 입력했을 때  
해당 IP를 찾기위해 가장먼저 찾는 DNS서버 이다.


## Root DNS 서버
모든 DNS 서버들은 이 Root DNS Server의 주소를 기본적으로 갖고 있다

그래서 모르는 Domain name이 온다면 
가장 먼저 Root DNS에게 물어보게 되는 것이다.

Root DNS는 최상위 DNS서버로 해당 DNS부터 시작해서  
아래 딸린 node DNS 서버에게로 차례차례 물어보게 되는 트리 구조로 짜여져 있다.

도메인이 google.com 이라면 뒤의 문자를 보고  
.com을 관리하는 TLD 서버에게 물어보라고 정보를 주는 것이다.

 

## TLD 서버 (Top-Level Domaion, 최상위 도메인 서버)
인터넷 도메인의 체계에서 최상위는 루트(root)로서  
인터넷도메인의 시작점이 된다.

그리고 이 루트도메인 바로 아래단계에 있는 것을  
1단계도메인이라고 하며 이를 TLD(최상위 도메인)이라고 한다.

TLD(최상위 도메인은 국가명을 나타내는  
국가최상위도메인과 일반적으로 사용되는  
일반 최상위 도메인으로 구분된다.

도메인을 구입할 경우 1단계의 도메인중에 하나를 선택하고  
원하는 도메인명을 지정하여 등록한다.

### TLD 서버 종류
> .biz : 사업  
> .com : 영리 목적의 기업이나 단체   
> .co.국가로 쓰기도 한다.(co.kr 등)   
> .edu : 미국의 4년제 이상 교육기관   
> .info : 정보 관련   
> .jobs : 취업 관련 사이트   
> .name : 개인 사용자   
> .net : 네트워크를 관리하는 기관  
> .org : 비영리 기관  

 
최상위 ICANN(인터넷주소 관리 기구) 아래에 REGISTRY, NIC(국가단위)가 있고,  
REGISTRY 아래에 우리가 흔이 보는 gTLD(.com / .net) 그리고 new gTLD가 있고,  
NIC아래에는 공공사이트에서 쓰는 ccTLD(.kr / .jp) 도메인 주소가 있다.

# 기타
velog.io 와 github.io (깃허브 블로그)는 영국령 인도양 지역의 인터넷 국가 코드 최상위 도메인이다.  
.io 도메인을 쓰면 기존 com, net이 점유하고 있던 도메인들을 벗어나 새롭게 도메인을 확보할 수 있다고 한다.

 
## Second-level DNS 서버 (2차 도메인)
위에서 Root DNS 서버에서 return해준 TLD 서버 주소를  
기지국 DNS 서버에서 받아서 다시 TLD 서버에 요청을 했었다.

그리고 TLD 서버에서는  
Second-level DNS 서버를 return 해준다.

만일 naver.com 이나 google.com을 요청했다면,  
TLD 서버에서 .com을 파악하고 그 앞에 달린 문자열을 보고 네이버나 구글 서버에게 요청을 하는 것이다.

그렇게 요청 받은 Second DNS 서버는 자체적으로 sub 도메인 서버로 또 넘기게 된다.

 
## Sub DNS 서버 (최하위 서버)
서브 도메인 서버는 www. dev. mail. cafe. 등등 을  
구분하는 최하위 서버를 말한다.

naver 서버라도 그 안에서 네이버 홈, 메일, 블로그, 카페 등 여러 서비스가 있다.  
이 서비스들을 구분하는 도메인 네임이라고 보면 된다.


# DNS 문자열 구조
모든 Computer들은 Root domain DNS server의 IP 주소는 알고있다.

그리고 Root domain을 담당하는 DNS 서버는 TLD(Top-level domain)을 담당하는 서버 목록과 IP를,  
TLD(Top-level domain)을 담당하는 DNS 서버는 Second-level domain을 담당하는 서버 목록과 IP를,  
Second-level domain을 담당하는 DNS 서버는 SUb domain을 담당하는 서버 목록과 IP를 알고 있는것이고,

결국, blog.example.com.의 IP주소는  
Sub domain을 전담하고 있는 DNS 서버가 알고 있는 것이다.

1. Host가 Root에 IP를 물어봄  
2. com으로 끝나니까, com을 전담하는 DNS server를 알려줌  
3. example.com을 전담하는 DNS server를 알려줌
4. Sub domain DNS server를 알려줌
5. 해당 Domain에 대한 IP 주소를 Host에게 보냄 
6. IP 주소 get!


# DNS Cache
위 과정을 통해 우리 PC는 "www.naver.com" 의 IP주소를 성공적으로 받아왔었다.

몇 분 후 다시 "www.naver.com"에 방문하려고 했을 때,  
또다시 위와 같은 복잡한 과정을 반복해서 IP 주소를 받아올까?

그러면 너무 비 효율적이다.

때문에, PC에는 DNS Cache라는 Cache를 활용해  
Cache안에 자주쓴는 Domain Name 주소를 저장해 놓는다.

 
이를 확인하는 방법은 Window 기준 다음과 같다.

`> ipconfig /displaydns`

이런 Cache정보가 수십개 나오게 된다.

이 정보를 가지고 저희 PC는 찾는 과정 없이  
막바로 IP주소를 찾을 수 있게 되는 것이다.

위에 TTL(Time To Live)라는 옵션값은 DNS서버나 사용자 PC의 캐쉬(메모리)에  
3600초를 시작으로 매초마다 시간이 감소된다.

0이 되면 메모리에서 사라지게 된다. 

 

# DNS 사용 시 주의할 점
DNS 캐시를 통해서 좀더 빠른 응답속도를 얻을수 있지만 문제점도 존재한다  
바이러스나 네트워크 오류, 혹은 여러가지 이유로 DNS 캐시정보가 변조될수 있기 때문이다  
즉, 특정 도메인을 입력했을 때 원래의 IP 주소가 아닌 해킹사이트 IP 주소로 변경이될 수 있다.  
그래서 주기적으로 캐싱된 DNS 를 정리해 줄 필요가 있다



# 참조
[inpa tistory](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-DNS-%EA%B0%9C%EB%85%90-%EB%8F%99%EC%9E%91-%EC%99%84%EB%B2%BD-%EC%9D%B4%ED%95%B4-%E2%98%85-%EC%95%8C%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC?category=889117)
