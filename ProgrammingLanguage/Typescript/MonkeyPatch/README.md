# 몽키 패치
프로그램을 확장하거나, 로컬 시스템 소프트웨어를 지원하고 수정하는 방법이다.  
(그냥 런타임 중 코드를 수정한다는 의미)  

- 오직 실행중인 프로그램의 인스턴스에 영향을 미친다.

자바스크립트의 특징중 하나는 선언된 객체와 클래스에 임의의 속성을 추가할 수 있을만큼 유연하다.  
예를 들어 브라우저 환경에서는 document, window와 같은 객체에도 값을 할당하여 전역변수를 만들기도 한다.

```tsx
window.monkey = 'Tamarin';
document.monkey = 'Howler';
```

또는 domElement에 직접 접근하여 데이터를 추가하기 위해서도 사용한다.

```tsx
const el = document.getElementById('colobus');
el.home = 'tree';
```

심지어 내장 프로토타입에도 속성을 추가할 수 있다.

```tsx
RegExp.prototype.monkey = 'Capuchin';
/123/.monkey; // Capuchin;
```

다만 자바스크립트에서 해당되는 것이며 타입스크립트에서는 문제가 발생한다.  
타입 체커는 Document와 HTMLElement의 속성에 대해서는 알고는 있지만  
임의로 추가한 속성에 대해서는 모른다.


## 타입 확장하기
```tsx
document.monkey = 'Tarmarin';
//error `monkey` not in document;

//any 사용하기
(document as any).monkey = 'Tamarin'; // 정상
```

하지만 타입안정성을 상실하고 타입스크립트 언어 서비스를 사용할 수 없다.


## 타입 보강하기
```tsx
interface Document {
  monkey: string;
}
document.monkey = 'Tamarin'; //pass
```

interface 타입의 특성을 이용하여 document 타입을 보강하여 사용한다.  
any보다 좋은 이유는 다음과같다.

* 타입이 더 안전하다. 타입오류를 표시해준다.
* TSDoc을 이용해 속성에 주석을 붙일 수 있다.
* 속성 자동완성 사용이 가능하다.
* 몽키 패치가 어떤 부분에 적용되었는지 기록이 남는다.

## Global 타입 보강하기.
모듈의 관점에서 타입을 보강하는 방법이다.

```tsx
export {};
declare global {
  interface Document {
    /**몽키 패치 속(genus) 또는 종(species) */
    monkey: string;
  }
}
document.monkey = 'Tamarin'; //pass
```

## 타입 단언문 사용하기
```tsx
interface MonkeyDocument extends Document {
  /**몽키 패치 속(genus) 또는 종(species) */
  monkey: string;
}
(document as MonkeyDocument).monkey = 'Macaque';
```

Document를 extends하여 사용하기 떄문에 타입은 안전하고  
Document 타입을 그대로 사용하는 것이 아닌 별도로 빼내어서 사용했기 때문에    
모듈영역에서도 안전하다.  
따라서 몽키패치된 속성을 사용하는 곳에서만 단언을 사용하면 된다.

그래도 몽키패치는 되도록 사용하면 좋지 않다.

전역 변수나 DOM에 데이터를 저장하지 말고, 데이터를 분리하여 사용해야한다.  
내장 타입에 데이터를 저장하는 경우,   
안전한 타입 접근법 중 하나(보강, 사용자정의 인터페이스로 단언)을 사용해야한다.    
보강의 모듈 영역 문제를 이해해야한다.
