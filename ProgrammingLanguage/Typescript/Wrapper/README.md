# 타입스크립트에서는 변수를 선언할 때, 객체 래퍼 타입으로 선언하지 마라.
1. 타입스크립트에서는 변수를 선언할 때, 객체 래퍼 타입으로 선언하지 마라.
2. 타입스크립트에서는 자바스크립트의 동작을 모델링한다.
3. 객체 래퍼 타입으로 선언하면 자바스크립트의 변수와 동작이 맞지 않을 수 있다.


객체 래퍼 타입이란 Number, String, Boolean 등과 같은 타입이다.  

대문자로 시작한다.   
반면에, 원시 타입이란 number, string, boolean 등과 같은 타입이다.  
대문자로 시작하지 않고 소문자로 시작한다.

자바스크립트에는 6가지 원시 데이터 타입이 있다.   
undefined, null, number, string, boolean, symbol (, bigint)이다.  
이 원시 타입들은 객체 타입이 아닙니다. 단순 데이터 타입이기에 데이터를 저장한다.

이 말은 프로토타입이 없다는 말이고 프로토타입이 없다는 말은 메서드가 없다는 의미가 된다.  
그러나, 우리는 종종 이 원시 타입에 메서드를 사용한다.

str는 원시 데이터 타입(string)이지만 메서드를 사용한다.  
이는 자바스크립트 엔진에서 형 변환이 이루어지기에 가능하다.  
charAt과 같은 메서드를 사용하게 되면  
순간적으로 string 타입이 객체 래퍼를 생성하여 해당 래퍼의 메서드를 사용하게 된다.

그리고 다시 반환될 때는 객체 타입이 아닌 string 원시 타입으로 반환된다.  
객체 타입은 가비지 컬렉팅 됩니다.

가령 위의 예시에서 str[0] = "a"로 할당한 뒤  
str의 값을 다시 확인하면 어떻게 될까요?  

str의 값은 변하지 않습니다.(타입스크립트에서는 에러가 납니다.)   
str의 값은 변하지 않고 "ABCDEF"입니다. 이는 객체 래퍼로 변환된 뒤 0번째 인덱스의 값을 수정하였으나,   
이 객체가 다시 사용되지 않고 버려지기에 원래 str의 값에는 영향을 미치지 않는다.

자바와 같은 언어를 사용할 때는 종종 클래스의 멤버 변수를 객체 래퍼 타입으로 선언한다.  
그러나 타입스크립트에서는 객체 래퍼 타입으로 선언하는 것을 권장하지 않는다.   

타입스크립트는 자바스크립트의 동작을 모델링했다.  
자바스크립트에서 선언되는 변수들은 모두 원시 타입으로 선언된다.  

그러나 타입스크립트에서 변수를 객체 래퍼 타입으로 선언을 하게 된다면  
자바스크립트에서 변수의 동작과 다르게 프로그래밍할 수 있는 여지가 생기게 된다.

가장 조심해야 할 타입은 String이다.  

간혹 다른 언어를 하다가 타입스크립트를 하게 되면   
무의식적으로 string 타입을 String 타입으로 선언하게 되는 경우가 있다.  

사실 string 타입을 String 타입으로 해도 대부분은 문제가 없는 것처럼 동작한다.   
그러나 아래 예를 보면 객체 래퍼 타입을 사용하게 되면 아차! 하는 순간이 있다.

String 타입은 sring 타입에 할당 불가능하다.  
String 타입의 변수는 strnig 타입의 변수로 할당 불가능하다.  

따라서, 타입스크립트에서는 변수를 선언할 때는  
객체 래퍼 타입을 사용해서는 안된다.

 
## Object
사실 원시 타입에 대해서만 이야기했는데 객체 타입도 마찬가지이다.

object 타입 대신에 Object 타입을 사용하는 것을 권장하지 않는다.


# 참고 자료
[junghyunki tistory](https://junghyunkim.tistory.com/entry/%EC%9D%B4%ED%8E%99%ED%8B%B0%EB%B8%8C-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B810-%EA%B0%9D%EC%B2%B4-%EB%9E%98%ED%8D%BC-%ED%83%80%EC%9E%85-%ED%94%BC%ED%95%98%EA%B8%B0)
