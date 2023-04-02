# interface보다 type을 사용
타입스크립트에서 type과 interface는 객체를 선언할 때 매우 유사한 구조이다.    
논란의 여지가 있지만 대부분의 경우 일관되게 type을 사용하고  
다음 중 하나에 해당하는 경우에만 interface를 사용하는 것이 좋다.

1. interface의 "merging" 기능을 활용하고 싶을 때.
2. 클래스/인터페이스 계층을 포함하는 OO 스타일 코드를 가지고 있을 때.

# 타입과 인터페이스 차이
## 차이점 1) 유니온 개념의 유무
type에는 유니온 타입이 있지만, interface에는 유니온 인터페이스가 없다.

```tsx
type TypeAorB = "a" | "b"

interface InterfaceAorB {
  ...?
}
```

또한, 앞서 언급한 것처럼 '&' 인터섹션 또한 존재하지 않는다.    
이로 인해 interface는 복잡한 타입을 만들어 낼 수 없다.

```tsx
interface Layer {
  layout: FillLayout | LineLayout | PointLayout;
  paint: FillPaint | LinePaint | PointPaint;
}
```

문제점이 보이는가?? 위 코드의 문제점은 PointLayout이면서 FillPaint를 가질수 있다.  
즉, layout과 paint간에 서로다른 타입을 가질수 있다는 위험이 있다.
 

## 차이점 2) 튜플과 배열 타입의 간결한 표현
type 키워드를 이용하면 튜플과 배열 타입도 간결하게 표현할 수 있다.  
interface를 사용하게 되면 유사하게 만들 수 있으나  
튜플의 프로토타입 체인 상에 있는 메서드들을 사용할 수 없다.  

type을 이용해서 만든 Tuple 타입. 여러 메서드들을 사용 가능.

interface를 이용하여 만든 유사 트리플 타입.  
다양한 메서드들을 사용할 수 없음.  

## 차이점 3) 선언 병합의 사용 가능 유무
interface에는 type에 없는 강력한 기능 하나를 가지고 있다.  
그건 바로 '선언 병합(declaration merging)'이라고 하는 기능이다.  

선언 병합을 통해서 interface의 속성을 확장해나갈 수 있다. 
interface를 새로 만들지 않은 채로 말이다. 

이를 통해 기존의 interface의 속성을 추가해나갈 수 있다.  

```tsx
보강기능이 있다.
// `보강(augment)`
interface State {
  name: string;
  capital: string;
}
interface State {
  papulation: number;
}

const wyoming: State = {
  name: 'Wyoming',
  capital: 'Cheyenne',
  population: 550,
}; // pass
```

잉여속성체크로인해 오류가 날 것 같아 보이지만 통과가 되는 코드이다.   
위와 같이 타입 속성을 확장하는 것을 '선언 병합'이라고 하는데  
이는 인터페이스에서만 지원이 된다.


## 결론
**정리하자면 type과 interface는 많은 부분에서는 큰 차이가 없다.**

그러나 복잡한 타입을 다룰 때는 type을 이용하는 것이 좋고,  
추후에 속성이 추가될 것을 고려하면 interface를 사용하는 것이 좋다.



목록에 ES2015를 추가하면  
타입스크립트에서는 lib.es2015.d.ts에서 선언된 인터페이스를 병합한다.


# 참고 자료
[smilejayden ](https://velog.io/@smilejayden/type%EC%9D%B4-interface-%EB%B3%B4%EB%8B%A4-%EB%82%AB%EB%8B%A4)

[junghyunkim tistory](https://junghyunkim.tistory.com/entry/%EC%9D%B4%ED%8E%99%ED%8B%B0%EB%B8%8C-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B813-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EC%95%8C%EA%B8%B0?category=987590)
