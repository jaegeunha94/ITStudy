# Typescript 에러 처리

# any 타입 강제변경
가장 직관적이면서도 간단한 해결책이다.

하지만 타입스크립트 개발자라면  
그리 추천되는 방법이 아닌 것은 다들 알 것이다.

```tsx
(async () => {
   try {
      await axios.get('/user/12345'); // axios 라이브러리 코드에서 에러가 발생할 경우
   } catch (err: any) {
      console.error(err.message); // 자체 자바스크립트 에러 객체
      console.error(err.response.data) // axios 라이브러리 에러 객체
   }
})();
```

# as 사용(비추천)
```tsx
try {

} catch(err: unknown) {
  console.log((err as CustomError).response.data)
  // 위에 줄에서 선언 단언해도 밑에 줄에서도 다시 단언 해줘야 한다
}

```
* typescript의 error는 unknown 타입임


# as 타입 변수 저장(비추천)
```tsx
try {

} catch(err: unknown) {
  const customError = err as CustomError
  console.log(customError.response.data)
  
}

```

# 타입 가드(추천)
```tsx
try {

} catch(err: unknown) {
  if(err instanceof CustomError) {
  
  }
}

```

# instanceOf

## 타입 가드 하기
가장 좋은 방법은 instanceof 를 이용하여 타입가드를 하는 것이다.

하지만 이상하게 빨간줄이 발생한다.  
왜냐하면 인터페이스는 컴파일 되면 사라지기 때문에 instanceof 할 개체가 없어지기 때문이다.


## 클래스를 에러 객체로 이용하기
타입스크립트에서의 인터페이스와 클래스의 차이는,  
컴파일 과정을 거친후 js 파일에 코드가 남냐 안남느냐의 차이이다.  

인터페이스는 순전히 타입스크립트 문법이므로 컴파일 되면 코드가 사라진다.  
반면에 클래스는 자바스크립트에서도 있는 문법이라,  
컴퍼일 되도 코드가 남게 된다.  

흔히 자바(Java)의 클래스로 생각하면 안된다.  
자바스크립트의 클래스도 결국 거슬러 올라가면 똑같은 프로토타입 객체이기 때문에   
이런식으로 사용이 가능한 것이다.

정리하자면 다음과 같이 클래스를 타입 객체로서 활용이 가능하다.

```tsx
// 인터페이스는 컴파일되면 자바스크립트에서 사라지기 때문에 err instanceof CusomError 가 불가능하게 된다.
// 그래서 컴파일되도 남아있는 클래스를 타입으로 이용하는 방법을 쓰는 것이다.
class CustomError_Class extends Error {
   response?: {
      data: any;
      status: number;
      headers: string;
   };
}
 
try {
   await axios.get('/user/12345');
} catch (err: unknown) {
   if (err instanceof CustomError_Class) {
      console.error(err.response?.data);
   }
}
```


# 참고 자료
[ko.javascript](https://ko.javascript.info/instanceof)

