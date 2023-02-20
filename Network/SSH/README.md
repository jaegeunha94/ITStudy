
# 1. 개요
SSH 프로토콜을 이용한 통신은 기본적으로 서버 클라이언트 모델에 기반한다.  
따라서 SSH 접속을 위해서는 기본적으로 서버에 SSH 서버(= SSH 데몬)가 설치되어 있어야 하고,  
클라이언트에는 SSH 클라이언트가 설치되어 있어야 한다.  
참고로 SSH 프로토콜이 사용하는 포트의 기본값은 22이다.

 클라이언트가 SSH 클라이언트를 이용하여 서버에게 SSH 접속을 요청하는 순간,  
 SSH 접속을 위한 준비 과정(Handshake)이 개시된다.  
 
 이 과정은 크게 두 개다.  
 먼저, 첫 번째 과정은 접속하려는 서버가 올바른 서버인지 검증하고  
 이후의 데이터 통신을 안전하게 진행하기 위한 세션 키를 생성하는 과정이다.  
 그리고 두 번째는 클라이언트가 해당 서버에 대한 올바른 접근 권한을 가지고 있는지 검증하는 과정이다.  
 이러한 모든 과정에 비대칭키 방식의 암호화와 대칭키 방식의 암호화가 숨어 있다  
 각 과정에 대한 자세한 내용은 아래에서 설명한다.


# 2. 서버 인증 및 세션 키 생성
먼저, 올바른 서버에 접속하려는 건지 검증하는 과정이 필요하다.

앞서 말했듯 접속하려는 서버에는 SSH 서버(= SSH 데몬)가 설치되어 있어야 하는데,  
SSH 서버가 설치될 때는 내부적으로 공개키와 개인키가 자동으로 생성된다.  
이러한 상황에서 클라이언트가 해당 서버에게 SSH 접속을 요청하면  
서버는 자신의 공개키를 클라이언트에게 전송하게 된다.

만약 클라이언트가 해당 서버에 최초로 접속 요청을 한 것이라면,  
서버로부터 전달받은 공개키를 로컬에 저장할 건지 물어볼 것이다.  
예를 들면 다음과 같은 문장으로 물어볼 것이다.

여기서 동의한다면 해당 공개키가 홈 디렉토리의 .ssh/known_hosts 파일 안에 추가된다.  
그러면 이후부터는 해당 서버에 접속 요청을 할 때마다 서버로부터 전달받은 공개키가  
로컬에 저장되어 있는 공개키와 같은지 검증하며,  
같다면 올바른 서버로 판단하게 된다.

그런데 이때 함께 진행되는 절차 중 아주 중요한 것이 하나 있는데, 바로 세션 키를 생성하는 것이다.  
세션 키는 곧 클라이언트와 서버가 데이터 통신을 할 때 암호화 및 복호화를 위해 사용할 대칭키를 의미한다.  
그리고 이러한 세션 키를 생성하는 과정은 클라이언트와 서버가 각종 키를 주고받기 때문에  
키 교환(Key Exchange, 줄여서 KEX)이라고 불린다.  
KEX 알고리즘으로 가장 대표적인 것은 디피-헬만(Diffie-Hellman) 알고리즘이다.  



## 디피-헬만 알고리즘
1. Both parties agree on a large prime number, which will serve as a seed value. And both parties agree on an encryption generator(typically AES), which will be used to manipulate the values in a predefined way.
2. Independently, each party comes up with another prime number which is kept secret from the other party. This number is used as the private key for this interaction(different than the private SSH key used for user authentication later).
3. The generated private key, the encryption generator, and the shared prime number are used to generate a public key that is derived from the private key, but which can be shared with the other party.
4. Both participants then exchange their generated public keys.
5. The receiving entity uses their own private key, the other party’s public key, and the original shared prime number to compute a shared secret key. Although this is independently computed by each party, using opposite private and public keys, it will result in the same shared secret key.
6. The shared secret is then used to encrypt all communication that follows.
 

위 알고리즘을 예시를 통해 풀어보면 대략 다음과 같은 식이다.

1. Client and Server select and agree on public numbers P = 23, G = 9.
2. Client selects a private key a = 4 and Server selects a private key b = 3.
3. Client and Server compute a public key respectively.
   - Client : x =(9^4 mod 23) = (6561 mod 23) = 6
   - Server : y = (9^3 mod 23) = (729 mod 23)  = 16
4. Client and Server exchange the public keys. Then Client receives the public key y =16 and Server receives the public key x = 6.
5. Client and Server compute symmetric keys, which will be the same(ka == kb).
   - Client : ka = y^a mod p = 65536 mod 23 = 9
   - Server : kb = x^b mod p = 216 mod 23 = 9
6. 9 is the shared secret, whici will be used as a session key.

# 3. 클라이언트 인증
이제 클라이언트가 해당 서버에 대한 올바른 접근 권한을 가지고 있는지 검증하는 과정이 필요하다.  
이 과정은 종류가 몇 개 존재하는데, 이는 서버가 어떤 종류의 인증 방식을 받아들이냐에 따라 달라진다.

 
## 3-1. 비밀번호 인증

가장 단순한 인증 방식은 바로 비밀번호를 이용하는 것이다.  
사용자가 입력한 비밀번호를 2번 과정에서 생성한 세션 키로 암호화하여 서버에게 보내고,  
서버가 이를 검증하면 끝이기 때문이다. 하지만 전송되는 비밀번호가 암호화가 된다고 해도  
이 방식은 일반적으로 권장되지 않는다. 

왜냐하면 자동화된 스크립트를 이용하면 적절한 길이의 비밀번호는 쉽게 뚫을 수 있기 때문이다.  
따라서 이어서 소개할, SSH 키 페어 기반의 인증 방식을 사용하는 것이 더욱더 권장된다.

 
## 3-2. SSH 키 페어(공개키, 개인키) 인증

먼저, 이 인증 방식을 사용하기 위해서는 클라이언트에서 키 페어,  
즉 공개키와 개인키를 생성한 후 해당 공개키를 서버 홈 디렉토리의 .ssh/authorized_keys 파일 안에 작성해둬야 한다.  
참고로 AWS에서 EC2를 생성하는 경우에는 이 과정이 AWS에 의해 자동으로 진행되며,  
그렇게 생성된 개인키를 사용자가 (최초 한 번) 다운로드할 수 있게 한다.  
이러한 준비가 완료되었다는 가정 하에, 인증 절차는 다음과 같이 진행된다.

 
* 클라이언트는 해당 서버에 접속하기 위해 사용할 키 페어의 ID를 서버에게 전송한다.
* 서버는 해당 ID에 매칭 되는 공개키가 홈 디렉토리의 .ssh/authorized_keys 파일 안에 작성되어 있는지 찾는다.
* 존재한다면, 난수 값을 생성하고 이를 해당 공개키로 암호화하여 클라이언트에게 전송한다.
* 클라이언트는 전달받은 암호화된 난수 값을 해당 서버의 개인키로 복호화한다. (일반적으로 SSH 클라이언트는 어떠한 개인키를 사용할 것인지 선택하게끔 되어 있다.)
* 이후 복호화한 난수 값을 이용하여 MD5 해시 값을 계산하고 이를 다시 서버에게 전송한다.
* 서버도 원래의 난수 값을 이용하여 MD5 해시 값을 계산하고, 클라이언트로부터 받은 값과 같은지 검사한다.
* 같다면, 올바른 클라이언트임이 인증되어 이제부터 본격적인 데이터 통신이 가능해진다.


# 4. 대칭키 기반의 데이터 통신
데이터 통신을 위한 모든 과정이 끝났다.  
이제 2번 과정에서 생성한 세션 키를 이용하여 서로 데이터를 주고받으며 통신할 수 있게 되었다.  
앞서 말했듯 세션 키는 대칭키이기 때문에,  
보낼 때 세션 키로 암호화하고 받을 때 세션 키로 복호화하면 된다.   

통신이 종료되면,  
즉 SSH 세션이 종료되면 해당 세션 키는 더 이상 무의미해지며 사용할 수 없게 된다.



# 참조
[eldorado tistory](https://it-eldorado.tistory.com/157)
