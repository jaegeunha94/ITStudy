# 타입 단언 단점
타입 선언 방식에서는 필요한 속성이 없으면 오류가 발생하지만,  
타입 단언 방식에서는 오류가 발생하지 않는다.

또, 타입 선언 방식에서는 타입의 프로퍼티 외에 다른 프로퍼티가 추가되면 오류가 발생한다.  
그러나 타입 단언 방식에서는 역시 오류가 발생하지 않는다.

하지만 지정해둔 name의 타입과는 다른 타입이 할당 되었을 때에는 오류가 발생한다.

# 타입 단언보다는 마입 명제를 사용하자
타입 단언을 피하기 위한 타입 명제 사용
올바른 방식으로 타입스크립트를 사용한다면, 명시적 타입 단언(value as SomeType과 같은)을  
사용하는 경우는 거의 없다.

```tsx
type Circle = { kind: 'circle'; radius: number };
type Rect = { kind: 'rect'; width: number; height: number };
type Shape = Circle | Rect;

function isCircle(shape: Shape) {
  return shape.kind === 'circle';
}

function isRect(shape: Shape) {
  return shape.kind === 'rect';
}

const myShapes: Shape[] = getShapes();

// 타입스크립트가 필터링 된 것을 모르기 때문에 에러가 발생한다.
// 타입 좁히기
const circles: Circle[] = myShapes.filter(isCircle);

// 다음과 같은 단언을 추가하고 싶을 수 있다.
// const circles = myShapes.filter(isCircle) as Circle[];
보다 우아한 해결책은 isCircle과 isRect를 타입 명제를 반환하도록 변경하여  
filter 호출 후 타입스크립트가 타입을 좁힐 수 있도록 도와주는 것이다.

function isCircle(shape: Shape): shape is Circle {
    return shape.kind === 'circle';
}

function isRect(shape: Shape): shape is Rect {
    return shape.kind === 'rect';
}
```

// 이제 Circle[] 타입을 올바르게 유추한다.

`const circles = myShapes.filter(isCircle);`


# 아무렇게나 as 단언을 사용할 수 있는 것은 아니다.
`as 단언을 하려면 객체의 현재 타입이 단언될 타입의 상위 집합 혹은 부분 집합이어야 한다.`
  
unknown 타입으로 단언 후에 다른 타입으로 변환하는 것은 위험하다.  
따라서 권장하지는 않습니다만 써야만 할 때에는 주의가 필요로 한다.


# 타입 단언은 언제 사용해야 할까?
타입스크립트가 판단하는 것보다 작성자의 판단이 더 정확할 때 !대표적으로 DOM에 접근하는 경우와     
click event, keyup event와 같이 e.target이 확정적으로 있는 경우가 있다.

```tsx
// "strictNullChecks": true 설정을 했다면 다음과 같은 오류가 나타난다.
const app:HTMLElement = document.querySelector('#app')

// Type 'HTMLElement | null' is not assignable to type 'HTMLElement'.
// Type 'null' is not assignable to type 'HTMLElement'.

// 아래와 같이 타입 단언을 사용해도 괜찮다.
const app = document.querySelector('#app') as HTMLElement
const app = document.querySelector('#app')!

// 하지만 아래와 같은 형태도 좋아보인다.
const app = document.querySelector('#app');
if(app) {...code}
```

# 타입 단언에서 서브 타입이 아닌 타입으로 타입 단언하기
타입 단언은 분명 타입을 강제로 변환시키는건데 왜 문제가 있을까?  
any, unknown을 제외한 타입이 할당되어 있거나 원시타입일 경우 타입의 범위가 정해진다.    

해당 범위안에 있는 타입들을 서브 타입이라고 하는데  
이 범위를 넘어가는 타입으로 타입 단언을 하려고 하면 에러가 발생한다.    
왠만해서는 이런 상황이 나오진 않겠지만 다음과 같이 해결 할 수 있다.

`const imNumber = '123' as number; // error`

// 내가 보기엔 실수인거같은데? 진짜로 변경할려고 한거면 unknown(any도 된다)으로 먼저 변경해라
// Conversion of type 'string' to type 'number' may be a mistake because neither type sufficiently   
overlaps with the other. If this was intentional, convert the expression to 'unknown' first.

`const imNumber = '123' as unknown as number; // pass`


# Type Assertions
TypeScript 프로그래밍을 하다 보면 타입 어설션(단언, Assertion)을 사용해야 할 순간이 오게 된다.
타입 어설션은 컴파일러에게 "이 타입이 특정 타입 임을 단언합니다"라고 말하는 것과 같다.

다른 언어의 타입 캐스트(Cast)와 비슷하지만, 특별한 검사나 데이터 재구성을 수행하지 않는다.  
런타임 시, 영향을 미치지 않으며 오직 `컴파일 과정에서만 사용`된다.  
타입 어설션을 처리하는 방법은 2가지이다. 

타입스크립트는 모르지만,  
우리는 어떤 값의 타입에 대한 정보를 알고있는 경우가 있다.

예를 들어 `document.getElementByID` 를 사용할 때,  
타입스크립트는 `HTMLElement` 같은 종류의 오브젝트가 반환될 것이라고 추측할 뿐이다.  

한편, 만약 당신의 페이지가 이 메서드의 결과로 반드시  
`HTMLCanvasElement` 를 반환하는 경우라면 수동적으로 이를 명시하고 싶을 수도 있다.

## 1번째 방법은 앵글 브라켓(angle-bracket, <>) 문법을 사용하는 것이다.
`let assertion:any = "타입 어설션은 '타입을 단언'한다.";`

// 방법 1: assertion 변수의 타입을 string으로 단언 처리
```tsx
let assertion_count:number = (<string>assertion).length;
```

## 2번째 방법은 as 문법을 사용하는 것아다.
`let assertion:any = "타입 어설션은 '타입을 단언'합니다.";`

```tsx
// 방법 2: assertion 변수의 타입을 string으로 단언 처리
let assertion_count:number = (assertion as string).length;
```

두 방법 모두 결과는 동일하다.  
하지만 `JSX와 함께 사용하는 경우에는 as 문법만 허용`됩니다.

## 예제
이러한 상황에 당신은 타입 어썰션을 사용하여 타입을 더욱 특정할 수 있다.

```tsx
const myCanvas = document.getElementById('main_canvas') as HTMLCanvasElement;
```

타입 주석과 마찬가지로, 타입 어썰션은 컴파일러에 의해 제거되며,  
코드의 런타임 비헤이비어에 영향을 끼치지 않는다.

또한 당신은 꺽쇠 괄호(< >)를 이용하여 이를 표현해줄 수 있다.  
(`.tsx` 파일에서는 동작하지 않는다)

```tsx
...
const myCanvasAngleBracket = <HTMLCanvasElement>document.getElementById('main_canvas');
```

타입스크립트는 타입의 특정성을 조절하는 데에만 타입 어썰션을 쓰도록 허가하고 있다.  
이 규칙은 '불가능한' 강제 타입 변환을 방지한다.

```tsx
const x = 'hello' as number;
// Conversion of type 'string' to type 'number' maybe a mistake
// because neither type sufficiently overlaps with the other. 
// If this was intentional, convert the expression to 'unknown' first.
```

만약 이 규칙이 너무 보수적이라 느껴지는 경우, 강제 타입 변환을 막을 수도 있는데,  
이런 경우 2 개의 타입 어썰션을 사용한다면 해결할 수 있을 것이다. 우선 `any` (혹은 `unknown` )로 어썰션하고,  
그 다음 해당 부분을 소괄호로 감싸고 그 뒤에 원하는 타입으로 어썰션 하면 된다. 

```tsx
const a = (expr as any) as T;
```

# 참고 자료
[junghyunkim tistory](https://junghyunkim.tistory.com/entry/%EC%9D%B4%ED%8E%99%ED%8B%B0%EB%B8%8C-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B89-%ED%83%80%EC%9E%85-%EB%8B%A8%EC%96%B8%EB%B3%B4%EB%8B%A4%EB%8A%94-%ED%83%80%EC%9E%85-%EC%84%A0%EC%96%B8%EC%9D%84-%EC%82%AC%EC%9A%A9?category=987590)

