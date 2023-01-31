# Node.js의 path 모듈로 경로 다루기
## 운영체제
파일이나 디렉토리의 경로를 다루는 게 뭐가 어렵다고 굳이 별도의 모듈이 필요할까?

사실 경로 처리는 많은 분들이 간과하지만 알고 보면  
은근히 오류가 발생할 소지가 많은 부분이다.

일단 유닉스 계열 운영체제와 윈도우 운영체제는 서로 다른 문자로 디렉토리 구조를 표현한다.  
유닉스 계열 운영체제에서는 / 문자를 사용하는 반면에 윈도우 운영체제에서는 \ 문자를 사용한다.

## 유닉스 계열
```
$ pwd
/Users/daleseo
```

## 윈도우
```
$ cd
C:\Users\daleseo
```

또한 이 두 진영의 운영체제는 PATH 환경변수를 표현할 때도 서로 다룬 문자를 사용하여 여러 경로를 나열한다.  
유닉스 계열 운영체제에서는 : 문자를 사용하는 반면에, 윈도우 운영체제에서는 ; 문자를 사용한다.

## 유닉스 계열
```
$ echo $PATH
/Users/daleseo/.nvm/versions/node/v17.0.1/bin:/Users/daleseo/.pyenv/shims:/Users/daleseo/.pyenv/bin:/opt/homebrew/bin:/opt/homebrew/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

## 윈도우
```
$ echo %PATH%
C:\Windows\system32;C:\Windows;C:\Program Files\node\;C:\Program Files\python\;C:\Program Files\java
```

/Users/daleseo는 윈도우에서는 하나의 디렉토리로 인식되며, C:\Users\daleseo는 유닉스에서는 하나의 디렉토리로 인식된다.  
따라서 파일이나 디렉토리의 경로를 단순히 문자열을  
이용하여 접근하면 프로그램이 특정 운영체제에서만 돌아갈 위험이 생긴다.

Node.js에서는 path 모듈을 제공하여 자바스크립트 개발자들이  
이러한 위험없이 경로를 다룰 수 있도록 도와주는 것이다.

# 불러오기
path 모듈은 Node.js에 내장되어 있어 있기 때문에 별도의 라이브러리 설치없이 바로 불러와서 사용할 수 있다.

CommonJS 모듈 시스템을 사용하는 프로젝트에서는 require 키워드로 불러오고,  
ES 모듈 시스템을 사용하는 프로젝트에서는 import 키워드를 사용할 수 있다.

```jsx
// CommonJS Modules
const path = require("path");
```

```jsx
// ES Modules
import path from "path";
```

# 경로 구분자
path 모듈은 위에서 설명드린 운영체제 별로 상이할 수 있는  
경로 관련 구분자를 sep와 delimiter라는 속성으로 제공하고 있다.

예를 들어, MacOS를 사용하고 있는  
컴퓨터에서는 다음과 같이 속성값이 확인됩니다.

```
> { sep: path.sep, delimiter: path.delimiter }
{ sep: '/', delimiter: ':' }
```

## 경로 만들기
운영체제 별로 경로를 구분짓는 방법이 상이하기 때문에  
코드가 실행되는 운영체제를 고려하여 경로를 만들어내는 것이 매우 중요하다.

join() 함수는 여러 개의 문자열을 가변 인자(variadic arguments)로 받아서  
하나의 완전한 경로로 조합해준다.

```jsx
> path.join("Users", "daleseo", "test.txt")
'Users/daleseo/test.txt'
```

MacOS에서는 위와 같이 / 문자를 사용하여 디렉토리를 구분해주고 있다.  
동일한 코드를 윈도우에서 실행했더라면 'Users\daleseo\test.txt'가 반환되었을 것이다.

경로를 만들어 내는 또 다른 방법으로 resolve() 함수를 사용할 수도 있다.  
resolve() 함수는 마치 터미널에서 cd 명령어를 연속해서 날리는 것처럼 작동을 하고 항상 절대 경로를 반환한다.

```jsx
> path.resolve("/Users", "../daleseo", "test.txt")
'/daleseo/test.txt'
```

예를 들어, 위와 같이 resolve() 함수를 사용하면,  
터미널에서 아래와 같이 디렉토리를 이동 후에 도착하는 파일의 경로를 얻게 됩니다.

```
$ cd Users
$ cd ../daleseo
$ ls test.txt
```

두번째 인자에서 ../를 사용하여 상위 디렉토리로 이동했기 때문에  
최종적인 경로는 '\/daleseo/test.txt'이 되는 것이다.

이해를 돕기위해 이번에는 윈도우에서 살짝씩 다른 인자로 resolve() 함수를 호출해보겠다.

```
$ path.resolve('/a', 'b', 'c')
'C:\a\b\c'
$ path.resolve('/a', '/b', 'c')
'C:\b\c'
$ path.resolve('/a', '/b', '/c')
'C:\c'
```

여기서 눈 여겨 볼 부분은 개발자가 경로 구분자로 / 문자와 \ 문자 중 어떤 것을 사용하든 크게 상관이 없다는 것이다.  
이 것이 path 모듈을 사용해서 경로 처리를 할 때의 가장 큰 이점이되겠습니다.

# 디렉토리 얻기
dirname() 함수를 사용하면 주어진 경로에서 파일 이름을 제외한 디렉토리 부분은 얻을 수 있다.

```
> path.dirname("/Users/daleseo/test.txt")
'/Users/daleseo'
```

# 파일 이름 얻기
basename() 함수를 사용하면 주어진 경로에서 파일 이름을 얻을 수 있다.

```
> path.basename("/Users/daleseo/test.txt")
'test.txt'
```

두번째 인자로 .을 포함한 확장자를 넘기면 순수한 파일 이름만 얻게 된다.
```
> path.basename("/Users/daleseo/test.txt", ".txt")
'test'
```

# 파일 확장자 얻기
extname() 함수를 사용하면 주어진 경로에서 파일의  
.을 포함한 확장자를 얻을 수 있다.

```
> path.extname("/Users/daleseo/test.txt")
'.txt'
```

# 절대 경로인지 알아내기
주어진 경로가 절대 경로인지 상대 경로인지 알아내려면  
isAbsolute() 함수를 사용한다.

```
> path.isAbsolute("/Users/daleseo/test.txt")
true
> path.isAbsolute("./test.txt")
false
```

# 경로 단순화 하기
./, ../, / 문자를 남용한 경로는 파일 시스템에서 정확히 어느 위치를 나타내는지 햇갈리는 경우가 있다.  
이럴 때는 normalize() 함수를 사용하여 불필요한 부분을 정리하여 경로를 단순화 할 수 있다.

```
> path.normalize("/Users/../daleseo//test.txt")
'/daleseo/test.txt'
```

# 상대 경로 구하기
간혹 어떤 경로를 기준으로 다른 경로의 상대 경로를 알고 싶은 경우가 있다.  
이럴 때는 relative() 함수에 기준의 되는 경로를 첫번째 인자로 대상이 되는 경로를  
두번째 인자로 넘기면 상대 경로를 구해준다.

```
> path.relative("/Users", "/Users/daleseo/test.txt")
'daleseo/test.txt'
```

# 참조
[daleseo](https://www.daleseo.com/js-node-path/)
