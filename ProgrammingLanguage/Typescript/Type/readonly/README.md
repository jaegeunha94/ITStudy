# 변경 관련된 오류 방지를 위해 readonly 사용하기
1. 객체 property 앞에 붙는 readonly와 array, 튜플 타입 앞에 붙는 readonly의 차이를 이해하자.
2. 객체의 readonly property는 얕게(shallow) 동작한다.
3. readonly number[] 타입은 number[] 타입의 상위 집합이다.

타입스크립트에는 readonly라는 키워드가 있다.  
변경 불가능을 위한 목적으로 사용되지만 두 가지 다른 쓰임새가 있다.  
해당 사실에 주목하면서 이번 아이템을 살펴보면 좋을 것 같다.

## (1) 객체 타입 Property 앞에 붙는 readonly
자바스크립트에서 상수 키워드인 const 키워드는 재할당을 금지하는 키워드다.  
원시 타입의 변수에게 const를 이용하여 생성을 하면 그 내용을 직관적으로 이해할 수 있다.  
그러나 const를 이용하여 객체를 생성하면 조금 헷갈릴만한 내용이 있다.

## const로 선언한 객체의 속성은 변경 가능하다.
const로 선언된 객체의 속성을 바꿀 수 있다.  
const로 선언된 객체는 해당 식별자에 다른 객체를 할당할 수 없지만,  
해당 객체의 속성의 변경은 가능하다.
 
즉, 재할당 가능과 객체의 속성 변경 가능은 서로 독립적인 내용이다.   
자바스크립트에서 객체의 속성을 변경을 막으려면 Object.freeze 함수를 이용하면 된다.

Object.freeze 함수를 통해 someObject의 변경을 막을 수 있다.  
그리고 타입스크립트에서는 readonly 키워드를 통해 특정 속성의 변경을 막을 수 있다.

readonly 키워드를 통해 객체 속성의 변경을 막았다.  
readonly 접근 제어자를 사용할 때에 주의할 점이 있다.  
그건 바로 readonly 접근 제어자가 얕게(shallow) 동작하는 것이다.


## readonlyType.prop.innerProp 속성은 변경 가능하다. 
readonlType.prop 속성은 변경 불가능하다.   
위 예시에서 readonlyType의 prop은 readonly 키워드가 있기 때문에 변경 불가능하다.   
그러나 prop의 innerProp의 경우에는 readonly 키워드가 아니기 때문에 변경 가능하다.

유틸리티 타입 Readonly은 기존 타입의 속성들 모두를 readonly로 만들어 준다.

객체의 속성을 모두 readonly로 만드는 유틸리티 타입 Readonly.  
그리고 유틸리티 타입 Readonly는 이렇게 생겼다.

```tsx
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};
```

(직접 유틸리티를 만들어보는 연습을 하면 좋습니다.)

## (2) Array, 튜플 타입 앞에 붙는 readonly
number[]와 같은 Array 타입 혹은 튜플 타입 앞에도  
readonly 키워드를 사용할 수 있다.  

이는 배열 혹은 튜플을 수정할 수 없음을 의미한다.


## readonly number[] 타입은 수정이 불가능하다.  
불변성을 가진 배열로 선언하고자 할 때에 사용될 수 있다.  
그리고  함수의 인자 타입에서 사용하여 인자의 불변성을 유지하는 목적으로도 사용할 수 있다.


## readonly로 선언된 함수 인자는 변경 불가능하다.
함수 내에서 해당 인자를 변경하지 않는다면 readonly 접근 제어자를 사용하는 것이 좋다.  
불변성을 중요시하는 함수형 프로그래밍의 관점에서 볼 때 좋은 습관이다.

이렇게 배열 혹은 튜플 앞에서 readonly 키워드의 특징은 아래와 같다.

* readonly 접근 제어자가 있으면 배열의 요소를 읽을 수 있으나, 새롭게 추가하거나 변경할 수 없다.  
* 배열을 변경하는 pop, push 와 같은 함수들을 사용할 수 없다.  
* length 속성을 읽을 수는 있으나, 변경 불가능하다.   
* readonly number[] 타입보다는 number[] 타입의 기능이 더 많다.  
* 즉, readonly number[] 타입은 number[] 타입의 상위 집합이다.
* 따라서, number[] 타입은 readonly number[] 타입에 할당 가능하다.  
* 반대로 reaonly number[] 타입은 number[] 타입에 할당 불가능하다.


# 참고 자료
[junghyunkim tistory](https://junghyunkim.tistory.com/entry/%EC%9D%B4%ED%8E%99%ED%8B%B0%EB%B8%8C-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B817-%EB%B3%80%EA%B2%BD-%EA%B4%80%EB%A0%A8%EB%90%9C-%EC%98%A4%EB%A5%98-%EB%B0%A9%EC%A7%80%EB%A5%BC-%EC%9C%84%ED%95%B4-readonly-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0?category=987590)
)
