
# 철저한 검사를 통한 컴파일시 처리되지 않은 케이스 체크
열거형(enum)을 스위치-케이스로 활용할 때, 다른 프로그래밍 언어에서처럼 예상치 못한 경우를 무시하지 않고  
적극적으로 오류를 처리하는 것이 좋습니다.

```tsx
function getArea(shape: Shape) {
  switch (shape.kind) {
    case 'circle':
      return Math.PI * shape.radius ** 2;
    case 'rect':
      return shape.width * shape.height;
    default:
      throw new Error('Unknown shape kind');
  }
}
```

타입스크립트에서 never 타입을 사용하면 정적 타입 검사시에 오류를 조기에 발견할 수 있습니다.

```tsx
function getArea(shape: Shape) {
  switch (shape.kind) {
    case 'circle':
      return Math.PI * shape.radius ** 2;
    case 'rect':
      return shape.width * shape.height;
    default:
      // 아래에서 타입 확인 오류가 발생합니다.
      // 만약 shape.kind가 위에서 처리되지 않았다면.
      const _exhaustiveCheck: never = shape;
      throw new Error('Unknown shape kind');
  }
}
```

이를 통해 새로운 shape에 kind를 추가할 때  
getArea 기능을 업데이트 하는 것을 잊지 않을 수 있습니다.

이 기술의 근거는 never형은 never 타입 이외에는 할당할 수 없다는 것입니다.  
shape.kind의 모든 후보가 케이스 상태에 의해 소진되면  
default에 도달할 수 있는 타입은 없습니다. 

하지만 어떤 후보가 커버되지 않은 경우  
default 지점으로 유출되어 잘못된 할당이 발생합니다.


# 참고 자료
[yceffort](https://yceffort.kr/2022/03/understanding-typescript-never)
