# 유틸리티 타입이란?
유틸리티 타입은 이미 정의해 놓은 타입을 변환할 때 사용하기 좋은 타입 문법이다.   
유틸리티 타입을 꼭 쓰지 않더라도 기존의 인터페이스,  
제네릭 등의 기본 문법으로 충분히 타입을 변환할 수 있지만  
유틸리티 타입을 쓰면 훨씬 더 간결한 문법으로 타입을 정의할 수 있다.


# extends
제네릭에서 사용되는 T extends U라는 키워드는 T가 U라는 타입인지 를 의미한다.

즉, 위 예시를 해석하면 다음과 같다.

T가 U의 타입 이라면, never (빈 타입)을,  
그렇지 않다면 T 그자체, 즉 원래대로 돌려준다

# Partial
파셜(Partial) 타입은 특정 타입의 부분 집합을  
만족하는 타입을 정의할 수 있다.

```tsx
interface Address {
  email: string;
  address: string;
}

type MayHaveEmail = Partial<Address>;
const me: MayHaveEmail = {}; // 가능
const you: MayHaveEmail = { email: 'test@abc.com' }; // 가능
const all: MayHaveEmail = { email: 'capt@hero.com', address: 'Pangyo' }; // 가능
```

# Pick
픽(Pick) 타입은 특정 타입에서  
몇 개의 속성을 선택(pick)하여 타입을 정의할 수 있다.

```tsx
interface Hero {
  name: string;
  skill: string;
}
const human: Pick<Hero, 'name'> = {
  name: '스킬이 없는 사람',
};

type HasThen<T> = Pick<Promise<T>, 'then' | 'catch'>;
let hasThen: HasThen<number> = Promise.resolve(4);
hasThen.th // 위에서 'then'만 선택하면 'then'만 제공, 'catch' 선택하면 'catch만 제공'
```

# Omit
오밋(Omit) 타입은 특정 타입에서  
지정된 속성만 제거한 타입을 정의해 준다.

```tsx
interface AddressBook {
  name: string;
  phone: number;
  address: string;
  company: string;
}
const phoneBook: Omit<AddressBook, 'address'> = {
  name: '재택근무',
  phone: 12342223333,
  company: '내 방'
}
const chingtao: Omit<AddressBook, 'address'|'company'> = {
  name: '중국집',
  phone: 44455557777
}
```

# modifier -
## Required
```tsx
type R<T> = {
  [Key in keyof T]-? : T[Key]
}
```

## Readonly 제거
```tsx
type R<T> = {
  -readonly [Key in keyof T]-? : T[Key]
}
```
