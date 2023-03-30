# Typescript 에서의  Object와 {}
* Object와 {}는 모든 타입을 의미하는 것임
* null | undefined은 제외


# Typescript에서의 unknown
* unknown = {} | null | undefined


# Typescript 에서의 object type
이 타입을 실제 코드를 작성하면서 보시는 경우가 많이 없는 이유는 
이 타입은 object literal를 표현하는 타입이 아니라  
자바스크립트의 primitive type이 아닌 나머지 타입을 표현하기 위해 존재하기 떄문입니다.

Record 타입의 경우 key에 대한 명확한 타입이 지정되지 않아도  
마치 제너릭 타입처럼 선언한 변수에서  
임의의 속성 값을 조회하는 것이 가능합니다.

```
let recordExample: Record<any, any>; recordExample.foo; // works
```

하지만 object 타입을 사용할 경우  
타입스크립트는 키를 참조하지 못합니다.

```
let objectExample: object; objectExample.foo; // error: Property "foo
```
