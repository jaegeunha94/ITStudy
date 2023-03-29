# ❗블룸버그가 TypeScript를 대규모로 도입하며 배운 것들 
1. TypeScript 는 JavaScript + Types 일수 있다.
2. TS는 빠르게 발전하므로, 최신 컴파일러를 따라가는게 좋다.
3. 일관된 tsconfig 설정은 가치가 있다.
4. 어떤 위치에 종속성을 명시하느냐가 중요하다.
ㅤ→ Ambient Modules 사용
5. Type의 중복 제거는 중요하다.
6. 암시적인 타입 종속성은 피해야 한다.
7. 선언 파일엔 세가지 Export 모드가 있다 : global, module, implicit exports
ㅤ→ 가능하면 module로
8. 패키지의 캡슐화는 위반 가능하다.
9. 자동 생성된 선언들은 디펜던시로부터 타입 인라인 가능
10. 생성된 선언들은 필수가 아닌 종속성들을 포함 가능


# 더 좋은 타입스크립트 만드는 팁
1. 선언된 타입과 좁혀진(narrowed) 타입의 이해
2. [옵셔널 필드 대신에 구분된 유니온 사용](https://github.com/ha-jae-geun/jaegeunha/tree/master/PL/Typescript/type/Union_InterSection#union-intersection-type)
3. [타입 단언을 피하기 위한 타입 명제 사용](https://github.com/ha-jae-geun/jaegeunha/tree/master/PL/Typescript/type/TypeAssertions)
4. [철저한 검사를 통한 컴파일시 처리되지 않은 케이스 체크](https://github.com/ha-jae-geun/jaegeunha/blob/master/PL/Typescript/type/never/README.md)
5. [interface보다 type을 사용](https://github.com/ha-jae-geun/jaegeunha/blob/master/PL/Typescript/Interface/README.md#interface%EB%B3%B4%EB%8B%A4-type%EC%9D%84-%EC%82%AC%EC%9A%A9)
6. 적절한 상황에는 배열보다는 튜플을 사용: 튜플은 간결하면서 안전한 타입입니다.
7. [추론된 타입이 일반적이거나 구체적이도록 제어](https://github.com/ha-jae-geun/jaegeunha/blob/master/PL/Typescript/Utility/Record/satisfy/README.md)
8. [추가 제네릭 타입 매개 변수를 만들기 위해 infer를 사용](https://github.com/ha-jae-geun/jaegeunha/blob/master/PL/Typescript/Generics/infer/README.md)
