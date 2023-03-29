# Record Type이란?
TypeScript는 Version 2.1부터 Utility Type인 Record Type을 도입했다.  
Record Type은  Record <Key, Type> 형식으로 키가 Key이고 값이 Type인 객체 타입이다.
 
 
# Record Type vs Index Signature
TypeScript에서 인덱스 시그니처(Index Signature)는 대괄호로 객체를 접근하는 방법이다.
 
다음은 인덱스 시그니처를 사용하여 객체를 생성한다.  

```tsx
type humanInfo = { 
  [name: string]: number 
};

let human:humanInfo = {
  '홍길동': 20,
  '둘리': 30,
  '마이콜': 40
};
```

위에서 작성한 인덱스 시그니처 예제는 Record Type으로 작성 가능하다.

```tsx
type humanInfo = Record<string, number>

let human:humanInfo = {
  '홍길동': 20,
  '둘리': 30,
  '마이콜': 40
};
```

Record Type과 인덱스 시그니처는 상당히 비슷해 보인다.   
그러나 구문 관점에서 인덱스 시그니처가 더 좋다.  
인덱스 시그니처에서 name이라는 Key가 의도를 명확하게 표현하기 때문이다.
            
## ▶ Record Type이 유용한 이유
인덱스 시그니처의 단점으로 문자열 리터럴을 Key로 사용하는 경우 오류가 발생한다.

```tsx
type humanInfo = { 
  [name: '홍길동' | '둘리' | '마이콜']: number 
};
```
하지만, Record Type은 Key로 문자열 리터럴을 허용한다.    
Record Type은 속성을 제한하고 싶은 경우  
문자열 리터럴을 사용하여 Key에 허용 가능한 값을 제한합니다.  

```tsx
type names = '홍길동' | '둘리' | '마이콜';

type humanInfo = Record<names, number>

let human:humanInfo = {
  '홍길동': 20,
  '둘리': 30,
  '마이콜': 40
};
```

또 다른 기능으로는 TypeScript Version 2.9부터 열거형을 Key로 사용할 수 있다.   
문자열 리터럴보다 코드가 깔끔하다는 장점이 있다.

```tsx
enum names {
  "홍길동" = 1,
  "둘리" = 2,
  "마이콜" = 3
}

type humanInfo = Record<names, number>;

let human: humanInfo = {
  1: 20,
  2: 30,
  3: 40
};
```

# keyof와 Record Type을 같이 사용
keyof 키워드는 타입 또는 인터페이스에  
존재하는 모든 키 값을 union 형태로 가져온다.

```tsx
type keyType = {
  a: string,
  b: number
};

// Key의 타입은 "a" | "b"
type Key = keyof keyType;
```

다음은 Record Type을 keyof와 같이 사용하는 예제다.

```tsx
type humanType = {
  name: string;
  age: number;
  address: string;
};

type humanRecordType = Record<keyof humanType, string>;

let human: humanRecordType = {
  name: "또치",
  age: "20",
  address: "서울"
};
```

위 예제처럼 기존 타입의 속성 이름을   
Reocrd Type의 Key 타입으로 하고 싶은 경우    
keyof와 함께 사용할 수 있다.


# 참고 자료
[developer-talk tistory](https://developer-talk.tistory.com/296)
