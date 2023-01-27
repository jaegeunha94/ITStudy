# 🎭 [코드 스플리팅]
리액트 프로젝트를 사용자에게 제공할 때는 빌드 작업을 거쳐서 배포를 함

빌드 작업 시 동작에 불필요한 주석, 공백 등을 제거하여 크기를 최소화   
브라우저에서 JSX 문법이나 다른 최신 자바스크립트 문법이 원활하게 실행되도록 코드의 트랜스 파일 진행   
이러한 빌드 작업은 웹팩 이라는 도구가 담당함  

별도의 설정을 하지 않으면 프로젝트의 모든 자바스크립트 파일이 하나로 합쳐짐   
즉, 사용자는 초기 로딩을 위해 모든 구성요소 자바스크립트를 가져오기 때문에 쓰지 않는 기능들로 인해 로딩이 길어짐   
CRA로 프로젝트를 빌드할 경우 최소 두 개 이상의 자바스크립티 파일이 생성됨

CRA의 기본 웹팩 설정에는 SplitChunks 라는 기능이 적용됨    
SplitChunks는 node_moudles에서 불러온 파일, 일정 크기 이상의 파일, 여러 파일 간에 공유된 파일을 자동으로 분리   
캐싱의 효과를 효율적으로 만듬  
이와같이 파일을 분리하는 작업을 코드 스플리팅이라고 함  
다만 SplitChunks의 기능을 통한 코드 스플리팅은 단순히 효율적인 캐싱 효과만 보유  

당장 사용하지 않는 컴포넌트의 분리를 위해 코드 비동기 로딩이 존재함 

코드 비동기 로딩을 통해 자바스크립트 함수, 객체, 혹은 컴포넌트를 필요한 시점에 불러와 사용 가능   
본 문서에서는 코드 비동기 로딩 방법으로 3가지를 설명함

1. 자바스크립트 함수 비동기 로딩
2. React.lazy와 Suspense를 통한 컴포넌트 비동기 렌더링
3. Loadable Components를 통한 컴포넌트 비동기 렌더링


# ☝ [자바스크립트 함수 비동기 로딩]
자바스크립트에서 다른 파일에 함수를 가져올 때 우리는 import를 사용한다.  
이 import 된 파일들은 빌드시에 하나로 합쳐지는데,  
자바스크립트에서 이를 분리하고자 한다면 단순히 import를 상단에서 하지 않고   
import() 함수 형태로 메서드 안에서 사용하면, 파일을 따로 분리시켜서 저장한다.

```jsx
// old 버전!
import take from './take'

function app() {
 const onclick = () => { 
 	take();
 }
}
```

```jsx
// new 버전!

function app() {
 const onclick = () => { 
 	import('./take').then(result =>  take().defualt())
 }
}
```

참고로 import를 함수로 사용하면 Promise를 반환한다.   
이 방식을 통해 모듈을 불러올 때, 모듈에서 default로 내보낸 것은 result.default를 참조해야 사용 할 수 있다.

# ✌ [React.lazy와 Suspense 방식]
코드 스플리팅을 위해 리액트 16.6 버전부터 내장된 기능이다.  
이전 버전에서는 import 함수를 통해 불러오고 컴포넌트 자체를 state에 넣는 방식으로 구현을 했었다.

React.lazy는 컴포넌트를 렌더링하는 시점에서 비동기적으로 로딩할 수 있게 해 주는 유틸 함수로 사용방법은 다음과 같다.

`const SplitMe = React.lazy(() => import('./SplitMe'));`

Suspense는 리액트 내장 컴포넌트로서 코드 스플리팅 된 컴포넌트를 로딩하도록 발동 시킬 수 있고,  
로딩이 끝나지 않았을 때 보여 줄 UI를 설정 가능하며 사용방법은 다음과 같다.

```jsx
import { Suspense } from 'react'
<Suspense fallback={<div>loading...</div>}>
  <SplitMe />
</Suspense>
```

# 🖖 [Loadable Components 방식]
Loadable Components는 서드파티 라이브러리이며 서버 사이드 렌더링을 지원하는 이점이 있다.   
또한 렌더링 이전에 필요할 때 스플리팅 된 파일을 미리 불러오는 기능이 존재한다.   
다만 본 자료에서는 서버 사이드 렌더링을 제외한 기본적인 사용법만 소개할 예정이다.

라이브러리 설치가 필요하며 명령어는 다음과 같다.  
`yarn add @loadable/component`

사용법은 React.lazy와 비슷하지만 Suspense를 사용할 필요 없다.

```jsx
import loadable from '@loadable/component';
const SplitMe = loadable(() => import('./SplitMe'));

//본인의 처리하는 코드 등...
<SplitMe />
```

단순히 선언 부에서 loadable을 사용하고 <SplitMe />와 같이 원하는 위치에 사용하기만 해도 코드 스플리트가 적용된다.   
만약 로딩 중에 다른 UI를 보여주고 싶다면 loadable 사용 부분에 추가 인자를 넘겨주면 된다.

```jsx
const SplitMe = loadable(() => import('./SplitMe'), {
  fallback: <div>loading...</div>
});
```

컴포넌트를 미리 불러오는 방법은 .preload(); 함수를 호출하는 것이다.  
다음 예제는 특정 요소에 이벤트를 주어, 마우스를 올리면 로딩을 시작하고, 클릭 시 보여주는 방식이다.

```jsx
import loadable from '@loadable/component';
const SplitMe = loadable(() => import('./SplitMe'), {
  fallback: <div>loading...</div>
});

function app() {
  const [visible, setVisible] = useState(false);
  const onClick = () {
    setVisible(true);
  };
  const onMouseOver = () {
  	SplitMe.preload();
  };
  return (
    <div onClick={onClick} onMouseOver={onMouseOver}>
      {visible && <SplitMe />}     
    </div>
  )
}
```

Loadable Components는 미리 불러오는 기능 외에도  
타임아웃, 로딩 UI 딜레이, 서버 사이드 렌더링 호환 등 다양한 기능을 제공한다.  

다만 React.lazy의 대체제가 아니며 공식 문서에서도 React.lazy를 활용해  
스플리팅이 잘 되면 Loadable Components로 바꿀 필요가 없다고 한다.   
만약 SSR(서버 사이드 렌더링)이 필요하고,  
React.lazy에 기능이 제한적이라고 느껴진다면 도입을 고려할만 하다.


# React.lazy VS LoadAble Components 비교 테이블
라이브러리	|Suspense|	SSR|	라이브러리| 스플리팅
---|---|---|---|---
React.lazy	| ✅| 	❌| 	❌| 	❌
@loadable/component	| ✅| 	✅| 	✅| 	✅


# Tree Shaking
[yceffort](https://yceffort.kr/2021/08/javascript-tree-shaking)


# 🤙 [정리]
두 방법 다 코드 스플리팅이 주 기능이기 때문에 서버 사이드 렌더링 유무에 따라 선택을 해도 좋다.  
내 경우에는 대부분의 프로젝트에서는 추가 라이브러리 대신 React 자체적으로 지원하는 lazy 방식을 택하고,  
서버 사이드 렌더링이 필요한 경우에는 Loadable Components를 활용할 예정이다.


# 참조
[@sugar_ghost](https://velog.io/@sugar_ghost/React%EC%99%80-%EC%BD%94%EB%93%9C-%EC%8A%A4%ED%94%8C%EB%A6%AC%ED%8C%85)
