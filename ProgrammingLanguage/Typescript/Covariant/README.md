# 한줄평
* return값이 좁은타입은 넓은 타입에 대입된다
* 매개변수는 넓은타입이 좁은 타입으로 대입된다

# 객체는 상세할수록 좁은타임
```tsx
type A = {name: string}
type B = {age: number}

typeAB = A | B // 넓은타입
type C = {name: string, age: number} // 더 좁은 타입

// 객체 리터럴 검사 추가되어 잉여성 검사가 추가됨
const c : C = {name: 'jaegeun', age" 29, married: false} // 넓은타입에 좁은타입 넣었는데 에러 ??
```

# 공변성과 반공변성
타입스크립트를 할 때 많이 당황하는 개념이 타입 간의 포함관계다.

어떤 타입은 다른 타입에 들어가는데 어떤 타입은 안 들어가서 당황스러운 경우가 있다.    
이럴 때는 공변성과 반공변성이라는 개념을 알아야 한다.  

1. 공변성: A->B일 때 T\<A> -> T\<B>인 경우
2. 반공변성: A->B일 때 T\<B> -> T\<A>인 경우
3. 이변성: A->B일 때 T\<A> -> T\<B>도 되고 T\<B> -> T\<A>도 되는 경우
4. 불변성: A->B더라도 T\<A> -> T\<B>도 안 되고 T\<B> -> T\<A>도 안 되는 경우


이렇게 네 가지로 정리할 수 있다.   
타입스크립트에서 타입 간에 서로 대입을 할 때 대입이 되는 경우와  
대입이 안 되는 경우를 파악하기 위해 알아두어야 한다.  

기본적으로 타입스크립트에서는 공변성을 갖고 있지만, 함수의 매개변수는 반공변성을 갖고 있다  
(strict 모드 기준, 정확히는 strictFunctionTypes 옵션).  
strict 모드가 아니라면 이변성을 갖고 있다.

strict 모드가 적용된 상황이다.  
항상 코드에서 누가 더 포함 범위가 넓은 타입인가를 확인해야 한다.  
함수의 return 타입을 보면 b가 a보다 넓은 타입이다.   
number -> number | string이니까요. 즉, a -> b 다.

```tsx
function a(x: string): number {
  return 0;
}
type B = (x: string) => number | string;
let b: B = a;
```

따라서 a를 b에 대입하는 것은 문제되지 않는다.  
좁은 타입을 넓은 타입에 대입할 수 있으니까   
이게 공변성이다. 

반대로 해볼까요? b -> a인 상황에서 b에 a를 대입하려고 한다.

```tsx
function a(x: string): number | string {
  return 0;
}
type B = (x: string) => number;
let b: B = a; // string | number' 형식은 'number' 형식에 할당할 수 없다.
```

넓은 타입을 좁은 타입에 대입하려고 하니 문제가 생긴다    
여기서 strict 모드를 해제하면 어떻게 될까요? 여전히 에러가 발생한다.   
즉, 리턴값에 대해서는 항상 공변성을 가진다고 볼 수 있다.

그런데 매개변수의 경우는 반공변성을 가진다.  
매개변수를 보면 string -> string | number인 상황이다.   
b -> a인 상황인데 a를 b에 대입할 수 있다.

```tsx
function a(x: string | number): number {
  return 0;
}
type B = (x: string) => number;
let b: B = a;
```

반대는 불가능하다.

```tsx
function a(x: string): number {
  return 0;
}
type B = (x: string | number) => number;
let b: B = a; // string | number' 형식은 'number' 형식에 할당할 수 없습니다. 
```

에러가 발생하게 된다.  
즉 매개변수의 경우는 반공변성을 가진다.

여기서 strict 모드를 해제하면 어떻게 될까?  
return값이었을 때와는 다르게 에러가 발생하지 않는다!   

즉, 매개변수는 strict 모드일때는 반공변성,  
strict 모드가 아닐 때는 이변성을 가진다.

앞으로 이들간의 관계를 참고해서 대입하거나 대입하지 않으면 된다.  
매개변수는 반공변성이라는 것만 기억해도 될 것 같다.

> 참고: 기본적으로 매개변수가 이변성을 가지는 이유는 리턴값은 공변성을 가지는데    
> 매개변수는 반공변성을 가짐으로 인해서 생기는 문제들이 생각보다 많이 때문이다. 

`(x: string) => string, (x: string | number) => string | number` 는 상호간에 대입이 불가능하다.  

리턴값은 공변성, 매개변수는 반공변성이기 때문이다.  
하지만 대입할 일이 생각보다 많다.  
이런 문제 때문에 기본적으로 이변성을 갖게 해두었다.


# 참고 자료
[zerocho](https://www.zerocho.com/category/TypeScript/post/5faa8c657753bd00048a27d8)
