# unknown
* unknown은 `{} | null | undefined` 을 의미한다.

# unknown, any 차이
먼저 가장 작은 집합은 never라는 타입이다.  
이 타입으로 선언이 되면 값을 할당할 수 없다.  
값들이 없는 집합.  
즉, 공집합입니다.

never 타입, 즉 공집합에는 값을 할당할 수 없다.  
반대로 가장 큰 집합은 unknown이다.   
어떠한 값이든 할당할 수 있는 집합이다. 전체 집합이라고 생각하면 된다.

unknow 타입, 즉 전체집합에는 어떠한 값이 들어올 수 있다.   
그러나 unknown은 타입 체커에게 "나는 unknown 타입의 값들을 할당받을 수 있어"라고 말한다면  
any는 타입 체커에게 "내가 무슨 짓을 하든 넌 상관하지 마"라고 말하는 것과 같다.

number 타입에 unknown 타입을 넣으면 에러를 내지만, any 타입은 에러를 내지 않는다.  
함수 doSomethingWithNumber의 파라미터 value는 number타입이다.  
number 타입은 전체 집합인 unknown 타입의 부분 집합이라고 말할 수 있다.  

unknownValue에는 number 타입뿐만 아니라  
string 타입의 값도 할당될 수 있기에 타입 체커는 옳지 않다며 에러를 낸다.  
그러나 any는 타입 체크를 무시하기에 에러를 내지않는다.  
이것이 unknown과 any 타입의 차이점이다.

다음으로는 유닛(unit) 타입이라고도 불리는 리터럴(literal) 타

## 4.8.3. `unknown`
`unknown` 타입은 `any` 값을 나타낸다.
이는 `any` 타입과 유사하지만, 더 안전한데, 그 이유는 `unknown` 값으로 어떤 작업을 하는 것은  
모두 적합하지 않은 것으로 표시되기 때문이다.  
(에러를 발생시킴으로써 잘못된 실행이라는 것을 알 수 있음)


```tsx
function f1(a: any) {
	a.b(); // OK
}
function f2(a: unknown) {
	a.b();
// Object is of type 'unknown'.
}
```

함수의 몸체에 `any` 타입의 값이 없으면서도,  
모든 값을 받아들일 수 있는 함수 타입을 표현할 때 이 `unknown` 타입이 유용하다.  
거꾸로 말하면, 그 반환 타입을 실행 이전에는 알 수 없는(unknown) 함수를 표현할 수 있다.

**unknown-type-safeParse.ts**

```tsx
function safeParse(s: string): unknown {
	return JSON.parse(s);
}

// 이 obj에 대해서는 매우 조심해야 한다.
const obj = safeParse(someRandomString);
```


## 모르는 타입의 값에는 any 대신 unknown을 사용하기
any타입은 타입체킹을 무시하는 타입으로 강력하지만  
typescript의 기능을 충분히 활용하지 못하는 타입과 같다.   
이번 아이템에서는 any, unknown, never의 차이점을 알고간다.  

### any 
어떠한 타입이든 any 타입에 할당이 가능하다.   
Any 타입은 어떠한 타입으로도 할당 가능하다. (never 타입 예외)

### unknown:  
어떠한 타입이든 unknown 타입에 할당이 가능하다.  
unknown타입은 unknown과 any에 타입으로만 할당이 가능하다

### never:  
어떠한 타입도 never에 할당할 수 없다.   
never 타입은 어떠한 타입으로도 할당이 가능하다

```tsx
//any
function sum(a: number, b: number) {
  return a + b;
}

let a: any = 1;
let b: any = 2;

sum(a, b);
//여기서 a,b의 타입은 any이지만 sum의 매개변수에 할당이 가능해진다.

//unknown
let a: unknown = 1;
let b: unknown = 2;

sum(a, b); //ERROR
//Argument of type 'unknown' is not assignable to parameter of type 'number'.
//unknown은 unknown, any를 제외한 타입에는 할당이 불가능하다.

//never
let a: never = 1; //Error
// Type 'number' is not assignable to type 'never'.
let b: never = 2; //Error
// Type 'number' is not assignable to type 'never'.
//어떠한 타입도 never에 할당할 수 없다.
sum(a, b);

//unknown never
let a: unknown = 1;
let b: unknown = 2;

sum(a as never, b as never); //pass
//never은 어떠한 타입으로도 할당이 가능하다. 위와같이 사용하면 안되지만 any와 동일한 동작을한다.
```

# unknown 처럼 사용되었던 object, {} 차이 파악하기

{} 타입은 null과 undefined를 제외한 모든 값을 포함한다.  
object 타입은 모든 비기본형(non-primitive) 타입으로 이루어진다.  
여기에는 true 또는 12 또는 "foo"가 포함되지 않지만 객체와 배열은 포함된다. 

{}타입은 unknown에서 null과 undefined를 제외한 타입이라 생각하면된다.   
따라서 null과 undefined가 불가능하다고 판단되는 경우만 unknown 대신{}`를 사용하면된다.


# 참고자료
[junghyunkim tistory](https://junghyunkim.tistory.com/entry/%EC%9D%B4%ED%8E%99%ED%8B%B0%EB%B8%8C-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B87-%ED%83%80%EC%9E%85%EC%9D%B4-%EA%B0%92%EB%93%A4%EC%9D%98-%EC%A7%91%ED%95%A9%EC%9D%B4%EB%9D%BC%EA%B3%A0-%EC%83%9D%EA%B0%81)
