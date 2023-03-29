# String과는 다른 String Literal
다음과 같은 TypeScript 코드가 있다. b와 c은 string 타입이 맞지만, a은 "Hello World" 타입이다.  
Type Script Playground에서 각 변수명에 mouse over하면 타입을 확인할 수 있다.

```jsx
const a = "Hello World"
let b = "Hello World"
const c: string = "Hello World"
```

b 변수는 let으로 선언되어 재할당될 수 있을 경우 어떤 문자열이든 넣을 수 있으며 그 경우의 수가 무한대이다.  
그렇기 때문에 컴파일러는 이 변수를 string 타입으로 추론한다.  
그리고 c 변수는 명시적으로 string 타입으로 선언했으므로 그냥 string 타입이다.

하지만 a의 경우는 조금 이야기가 달라진다. 컴파일러는 이 변수를 string이 아닌  
조금 더 좁은 타입(narrowed type)으로 선언한 것으로 추론한다. 이 것을 Literal Narrowing이라고 한다.  
(참고로 타입 추론은 TypeScript 컴파일러가 제공하는 뛰어난 기능 중 하나이며,  
개발자가 명시적으로 타입을 선언해 주지 않을 경우 컴파일러가 할당되는 값을 기준으로 타입을 스스로 결정하는 것을 말한다.)

따라서 a의 타입은 string이 아니라 string타입을 좁혀 만든 string literal type이다.  
여기서 "타입을 좁힌다"는 말의 의미는 무한대의 경우의 수를 가질 수 있는 string타입보다 
훨씬 구체적인 string의 부분집합, "Hello World"만을 허용하는 타입을 선언했다는 뜻이다.

따라서 아래와 같이 명시적으로 literal type을 선언하면 let으로 선언된 변수도  
"Hellow World" 타입만을 허용하도록 만들 수도 있다.

```jsx
type HelloWorldType = "Hello World" // literal type

let a: HelloWorldType = "Hello World" // ok
a = "hahaha" // compile error: "hahaha"는 "Hello World"가 아니기 때문.
```

## 📑String 키를 이용한 객체 접근
하지만 string키로 객체에 접근하지 못하는 것은 여러모로 불편하다.  
다음과 같이 Object.keys()에서 리턴되는 값은 string[]이기 때문에 JavaScript에서 사용하던 코드를 그대로 사용하면 컴파일 에러가 발생한다.

```jsx
for (const key of Object.keys(obj)) {
  console.log(obj[key]) // compile error! key가 string타입이다.
}
```

위 예제는 Type assertions을 이용하여 해결할 수도 있다.  
하지만 우리는 string타입 키로 객체에 접근이 가능한지 궁금한 것이므로 그 것이 가능하도록 index signature를 선언하는 방법을 알아본다.


# 📑UnionType
string literal 타입은 열거형 타입처럼 사용할 때 매우 유용하다. 예를 들어 마우스 이벤트를 처리하는 함수가 있다고 하자.  
마우스 이벤트의 종류는 이미 정해져 있을 것이다. JavaScript의 방법대로 이벤트 이름을 string 타입으로 받을 수 있다.  
하지만 오타 혹은 유효하지 않은 이벤트 이름으로 인해 발생하는 런타임에러를 사전에 방지할 수 없다.

```jsx
function handleEvent(event: string) {}
handleEvent("click")
handleEvent("clock") // compile error: 오타. 컴파일 타임에 발견할 수 없다.
handleEvent("hover") // compile error: 유효하지 않은 이벤트 이름. 마찬가지로 컴파일 타임에 걸러낼 수 없다.
```

다음의 예제과 같이 string literal 타입 조합만을 허용하도록 하도록 수정한다.  
여기서 |은 union type을 의미하며 두 개의 타입 이상을 결합할 수 있다.

```jsx
type EventType = "mouseout" | "mouseover" | "click"
function handleEvent(event: EventType) {}
handleEvent("click")
handleEvent("hover") // compile error: Argument of type '"hover"' is not assignable to parameter of type 
```

이렇게 string literal 타입을 활용하면 "clock"과 어이 없는 오타를 컴파일 타임에 알 수 있으며,  
IDE에서 제공하는 suggestion 기능(ctrl + space) 편리함을 누릴 수도 있다.

## 📑Index Signature 선언하기
방법은 간단하다. 아래와 같이 객체에 index signature를 한줄 추가한다.

```jsx
type ObjType = {
  [index: string]: string
  foo: string
  bar: string
}

const obj: ObjType = {
  foo: "hello",
  bar: "world",
}

const propertyName1 = "foo"
const propertyName2: string = "foo"

console.log(obj[propertyName1]) // ok
console.log(obj[propertyName2]) // ok
```
참고로 위에서 사용된 이름인 index는 정해진 키워드가 아니라 개발자가 의미에 맞게 마음대로 쓸 수 있다.

드디어 string타입과 literal type 모두를 사용해서 obj에 접근할 수 있게 되었다.  
위처럼 index signature가 선언된 경우 모든 맴버가 그 것에 따라야 한다.  
그렇지 않으면 다음과 같이 에러가 발생한다.

```tsx
type ObjType = {
  [key: string]: string
  foo: string
  bar: number // error! Property 'bar' of type 'number' is not assignable to string index type 'string'.
}
```

## 📑Number 타입 Index Signature
index signture의 number 타입으로 선언하면 다음과 같이 배열 literal 방식으로 할당도 가능하다.

```tsx
interface ArrayLikeType {
  [key: number]: string
}
const obj: ArrayLikeType = ["hello", "world"]
console.log(obj[0], obj[1]) // "hello" "world"
```

## 📑String+Number타입 Index Signature
물론 된다. 하지만 이 경우 배열 literal 방식의 할당이 불가능하다.

```tsx
interface ArrayLikeType {
  [key: number]: string
  [key: string]: string
}

let obj: ArrayLikeType = {
  0: "hello",
  foo: "world",
} // ok.
let foo = "foo"

console.log(obj[0], obj[foo]) // "hello" "world"

obj = ["hello", "world"] // compile error!
```

## 📑세상에 바보 같은 질문은 없다?
다음과 같은 의문이 생겼다. 'string대신 narrowed 타입으로 index signature를 선언할 수 있을까?'  
그래서 다음과 같이 테스트 해봤다.

```tsx
type AllowedKeys = "hello" | "world"

type ObjType = {
  [key: AllowedKeys]: number // error!
}
````

전혀 문제가 없는 것처럼 느껴지는 위 코드는 컴파일 에러를 내뿜는다.  
(물론 나만 문제 없어 보이는 것 일 수 있다.)

An index signature parameter type cannot be a union type.  
Consider using a mapped object type instead.

위 코드의 의도는 특정 key들을 가진 인터페이스를 선언하려는 것이고,  
index signature을 사용하는 의도는 key의 타입을 선언하려는 것이기 때문이다.

에러 메세지는 index signature 타입으로 union type은 사용할 수 없으며  
mapped object type을 사용하라는 설명이다. 

index signature의 타입으로는 string, number 등 몇몇 정해진 타입만 사용이 가능하다.


## 📑Mapped Type의 사용
이미 선언된 타입의 프로퍼티에 어떤 조작을 가하여 새로운 타입을 만드는 것을 mapped types라 한다.  
프로퍼티를 빼거나 추가할 수 있고, optional 혹은 readonly 상태로 바꿀 수 도 있어서 매우 유용하다.

```tsx
type AllowedKeys = "hello" | "world"

type ObjType = {
  [key in AllowedKeys]: number // ok!
}
```

이 코드가 위에서 의도 했던 것과 같은 코드일 것이다.  
얼핏 보면 index signature 선언과 닮아 보이지만 이 코드는 다음 코드와 완전이 동일 하므로  
index signature와는 전혀 관련 없는 코드이다.

```tsx
type ObjType = {
  hello: number
  world: number
}
```

## 📑In 키워드
Mapped type 예제에서 사용된 in 키워드가 혼란을 준 경험이 있기에 부연 설명을 덧붙이고자 한다.  
이 것이 혼란을 주는 이유는 다른 용도의 in operator 가 존재하기 때문이다.

```tsx
const obj = {
  hello: "hello",
  world: "world",
}
console.log("hello" in obj) // true
```

위 코드는 객체가 특정 프로퍼티를 가지는지 체크한다.  
이 연산자는 유니온된 타입을 구분하는 용도로 사용하기도 한다.  
따라서 mapped type에 사용된 in과는 다른 역할을 한다.

Mapped type에서의 in은 for...in의 축약형이라고 생각하는 것이 이해와 기억에 큰 도움이 된다.  
AllowedKeys의 각각의 요소를 하나씩 가져오고 그 것을 이용해 새로운 프로퍼티들을 만드는 과정으로 이해해야 한다.  
이렇게 만들어진 결과는 다음의 선언과 완전히 동일하다.


## 📑마지막 테스트
지금까지 mapped type을 이용한 예제는 index signature와 전혀 관련이 없다는 것을 충분히 설명했다.  
다음과 같이 mapped type과 index signature의 혼용이 가능할지에 대해서도 테스트 해 봤으나 결과는 컴파일에러였다.

```tsx
type AllowedKeys = "hello" | "world"

type ObjType = {
  [index: string]: number
  [key in AllowedKeys]: number // compile error!
}
```

## 📑정리
* String literal은 string의 좁혀진(narrowed) 타입이며, 둘은 같지 않다.
* String타입의 키로 객체에 접근하려면 index signature를 선언해야 한다.
* Mapped type은 유용하나 index signature와는 관련이 없다.
* Mapped type에서의 in은 for...in의 그 것이라고 기억하면 편리하다.


# 참고 자료
[soopdop](https://soopdop.github.io/2020/12/01/index-signatures-in-typescript/)
