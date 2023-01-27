# Reflow와 Repaint의 관계
조건 Reflow가 일어나야 Repaint가 일어나는것은 아니다.

background-color, visibility와 같이 레이아웃에는 영향을 주지 않는 스타일 속성이 변경되었을 때는  
Reflow를 수행할 필요가 없기 때문에 Repaint만 수행하게 된다.
  
  
 # Reflow가 발생할 때
 1. 보이는 DOM 요소를 추가하거나 제거했을 때
 2. 요소의 위치가 바뀌었을 때
 3. 요소의 크기가 바뀌었을 때(마진, 패딩, ㅔㅌ두리 두께, 너비, 높이 등)
 4. 텍스트 내용이 바뀌거나 이미지가 다른 크기의 이미지로 대체되는 등 내용이 바뀌었을 때
 5. 페이지를 처음 표시할 때
 6. 브라우저 창의 크기를 바꿨을 때


# 리플로우와 리페인트 최소화 방법
### 1. 노드를 생성하여 삽입할 때(ex : 행이 100줄인 테이블 생성)
* createElement(), appendChild()를 사용하는 것보다 스크립트 변수에 문서를 만들어 innerHTML을 사용하여 삽입하자

### 2. DOM변경 한번에 처리하기

```javascript
// 한번에 변경하지 않는 경우

var el = document.getELementById('mydiv')

el.style.cssText = 'border-left: 1px; border-right: 2px;'
el.style.cssText += '; border-left: 1px;';
```

```javascript
// 클래스명으로 스타일 한번에 바꾸기
var el = document.getELementById('mydiv')

el.className = 'active';
```

```
//cssText속성으로 스타일을 변경
el.style.cssText = 'border-left:1px; padding: 5px';
```

### 3. DOM요소에서 여러가지를 변경해야 할 경우
 - 요소를 숨기고 변경한 후 다시 드러냄
 - 현재 DOM 바깥에서 문서 조각을 만들어 변경한 후 문서에 복사
 - 현재 요소를 문서 밖의 노드에 복사해서 사본을 변경한 후 원래 요소를 대체함
 - DocumentFragment 생성한 후 여러 값을 변경하고 요소 붙이기

```javascript
var fragment = document.createDocumentFragment();

appendDataToElement(fragment, data);
document.getElementById('myList').appendChild(fragment)

```

### 4. 랜더 트리 변경을 모았다가 한번에 처리하기
* 리플로우마다 다시 계산하려면 비용이 많이 들기 때문에 브라우저 대부분은 랜더 트리변경을 큐에 모았다가 한번에 처리하는 방식으로 리플로우를 최적화합니다.
* 리플로우가 발생하는 렌더트리 강제 변경
  - 오프셋 offsetTop, offsetLeft, offsetWidth, offsetHeight
  - 스크롤 scrollTop, scrollLeft, scrollWidth, scrollHeight
  - 클라이언트 clientTop, clientLeft, clientWidth, clientheight
  - getComputedStyle() (IE에서는 currentStyle 사용)
* 이런 속성과 메서드에서 반환하는 레이아웃 정보는 현재 상태를 그대로 반영해야 하므로 브라우저는 정확한 값을 반환하기 위해 랜더링 큐에 대기중인 변경을 바로 실행하고 리플로우를 실행합니다.
* 스타일을 바꾸는 도중에는 앞에 나열한 속성을 쓰지 않는 것이 최선입니다
* 요청한 레이아웃 정보가  최근에 변경되지 않은 것이거나, 심지어 전혀 관계가 없을 때도 위에 나열한 속성은 랜더링 큐를 비웁니다.


### 5. 레이아웃 정보 나중에 읽어오기
```javascript
// 똑같은 속성을 변경하고 그 스타일 정보를 바로 읽어오는 비효율적 예시
bodystyle.color = 'red';
tmp = computed.backgroundColor;
bodystyle.color = 'white'
tmp = computed.backgroundImage;
```

```javascript
// 효율적으로 수정
bodystyle.color = 'red';
bodystyle.color = 'white'

tmp = computed.backgroundColor;
tmp = computed.backgroundImage;

```

### 6. 스타일을 변경할 경우 가장 하위 노드의 클래스를 변경한다.
* DOM 노드의 크기 또는 위치가 변경되면 하위 노드와 상위 노드에도 영향을 미칠 수 있다.  
* 이 때 가장 하위 노드의 스타일을 변경할 경우, 전체 노드가 아닌 일부 노드로 reflow를 영향을 최소화 할 수 있다.

### 7. 인라인 스타일을 사용하지 않는다.
* 인라인 스타일은 HTML이 파싱될 때, 레이아웃에 영향을 미쳐 추가 리플로우를 발생시킨다.  
* 또한 관심사 분리가 제대로 이루어지지 않으면 유지 보수가 힘들어 진다.

### 8. 애니메이션이 있는 노드는 position을 fixed 또는 absolute로 지정한다.
* 애니메이션 효과는 많은 reflow 비용을 발생시킨다. position 속성을 fixed 또는 absolute의 값으로 주어, 지정된 노드를 전체 노드에서 분리시켜 해당 노드에서만 reflow가 발생하도록 제한시킬 수 있다.
* 애니메이션 효과를 줘야 하는 노드에 position 속성이 적용이 되지 않았다면 애니메이션 시작 시 position 속성 값을 fixed 또는 absolute로 변경하였다가 애니메이션 종료 후 다시 원복시켜서 렌더링을 최적화 할 수 있다.

### 10. table 레이아웃을 피한다.
* \<table>은 점진적으로 렌더링 되지 않고, 모두 로드되고 테이블 너비가 계산된 후에 화면에 그려진다.  
* 테이블 안의 컨텐츠의 값에 따라 테이블 너비가 계산 된다. 따라서 테이블 컨텐츠의 작은 변경만 있어도 테이블 너비가 다시 계산되고  
* 테이블의 모든 노드들이 reflow가 발생한다. 이러한 이유로 \<table>을 레이아웃 용도로 사용하는 일은 피해야 한다.


# 참조
[webclub tistory](https://webclub.tistory.com/346)

[boxfoxs tistory](https://boxfoxs.tistory.com/408)
