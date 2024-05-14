# React, 최고의 JS 라이브러리



React의 핵심은 **`컴포넌트 기반 아키텍처`와 `Hook`,** **`virtual DOM`**입니다.&#x20;

이 두 가지 개념을 이해하고 활용하는 것이 React를 효과적으로 사용하는 데 가장 중요합니다.



### 1. 컴포넌트 기반 아키텍처

React에서는 UI를 작은 **컴포넌트**로 나눕니다.

각 컴포넌트는 독립적이고 재사용 가능하며, 자신의 상태(state)와 속성(props)을 가질 수 있습니다.



**UI요소를 재사용 가능한 예시**

```jsx
import React from 'react';

// 단순한 컴포넌트 정의
function HelloWorld() {
  return <h1>Hello, World!</h1>;
}

// App 컴포넌트에서 HelloWorld 컴포넌트를 사용
function App() {
  return (
    <div>
      <HelloWorld />
      <HelloWorld />
      <HelloWorld />
    </div>
  );
}

export default App;
```



**UI요소에 props를 전달하면 한 컴포넌트로 다양한 UI를 만들 수 있습니다.**

```javascript
import React from 'react';

// props를 넘겨줌 컴포넌트 정의
function HelloWorld({text}) {
  return <h1>{text}, World!</h1>;
}

// App 컴포넌트에서 HelloWorld 컴포넌트를 사용
function App() {
  return (
    <div>
      <HelloWorld text={"Hello"}/>
      <HelloWorld text={"Halo"}/>
      <HelloWorld text={"안녕"}/>
    </div>
  );
}

export default App;
```



**반응을 가능하게 만들려면 useState를 쓸 수 있습니다.**

```javascript
import React from 'react';

// props를 넘겨줌 컴포넌트 정의
function HelloWorld({text}) {
  return <h1>{text}, World!</h1>;
}

// App 컴포넌트에서 HelloWorld 컴포넌트를 사용
function App() {
  return (
    <div>
      <HelloWorld text={"Hello"}/>
      <HelloWorld text={"Halo"}/>
      <HelloWorld text={"안녕"}/>
    </div>
  );
}

export default App;
```



### 1-1. 기타 유용한 컴포넌트

### Profiler <a href="#effect-hooks" id="effect-hooks"></a>

렌더링 성능을 측정할 수 있습니다.

### Suspense <a href="#effect-hooks" id="effect-hooks"></a>

```javascript
const [isPending, startTransition] = useTransition()
```

이런 식으로 하여, pending인 과정을 컨트롤 할 수 있습니다.

허나 이런 pending은 코드 흐름을 읽는데 방해됩니다.

Suspense를 통해 pending 데이터를 캐치하고 로직과 분리하여 편하게 관리할 수 있습니다.





### 2. React Hook은 2가지 종류가 있습니다.

react에서 hook은 로직을 담당합니다.

그리고 로직의 종류에 따라 2가지 종류로 나뉩니다.



1. props와 state를 활용해서 jsx를 내놓는 렌더링 코드
2. 입력 필드를 업데이트하거나 서버에 요청을 보내는 등 렌더링과 관련 없는 이벤트 핸들러



### 2-1. 렌더링 코드와 Hook



렌더링 시에 쓰이는 코드들은 순수해야 합니다.

수학처럼 결과가 딱 나와야 한다는 것입니다.

렌더링 중에 DOM을 조작하거나, 네트워크 요청을 해서는 안됩니다.



**렌더링 시에 데이터를 기억하고, 변경될 시에 재렌더링이 필요할 때**

useState -> 렌더링할 때 쓸 데이터를 기억하고, 다시 렌더링할 때

useReducer -> useState와 동일하지만, state 변경을 한 곳에서 엄격히 관리하기 위해



**\*state를 만들 때는 꼭 필요한지 생각해야 하는 상황입니ㅏㄷ.**

1. 두 개 이상의 state가 항상 함께 업데이트 되나요? 합칠 수 있지 않을까요?
2. 서로의 state 값이 성립하지 않은 경우가 발생하나요? 합치는 것이 좋을 듯 합니다.
3. 이미 있는 state로 만들 수 있는 값을 state로 다시 만들 필요는 없습니다.
4. props를 전달된 값을 state의 init으로 넣는다면 props에 initial나 default를 통해서 명시하세요.
5. 데이터 일관성을 유지해야 하는 경우, 해당 데이터를 2개의 state에 분리하여 보관하면 안 됩니다.
6. 중첩된 데이터는 사용하기 어렵습니다. 웬만하면 평탄화(정규화)해서 사용하세요.



어쩔 수 없이 객체를 중첩해서 구성해야 합다면, useImmer를 활용하여 할당할 수 있습니다.

[**> 자세한 사항**](https://ko.react.dev/learn/choosing-the-state-structure)





**\[탈출구] 렌더링 시에 데이터를 기억만 하고, 재렌더링은 하지 않을 때**

useRef -> 주로 브라우저 api와 관련된 값을 저장하거나, 요소의 상태를 저장할 때 씁니다.



**전역 변수를 사용할 때, props drilling을 해결할 때**

useContext -> props로 전달하지 않고, 머리 있는 컴포넌트로부터 정보 받기 위해 씁니다.

대게는 전역 상태 라이브러리를 사용합니다.



### 2-3. 이벤트 핸들러와 관련된 Hook

주로 React 코드를 벗어난 특정 _외부_ 시스템과 동기화하기 위해 사용됩니다. 이는 브라우저 API, 써드파티 위젯, 네트워크 등을 포함합니다.



이벤트 핸들러와 관련되어. 이 과정에서 유용한 2개의 브라우저 api가 있습니다.

* [`e.stopPropagation()`](https://developer.mozilla.org/docs/Web/API/Event/stopPropagation)은 이벤트 핸들러가 상위 태그에서 실행되지 않도록 멈춥니다.
* [`e.preventDefault()` ](https://developer.mozilla.org/docs/Web/API/Event/preventDefault)는 기본 브라우저 동작을 가진 일부 이벤트가 해당 기본 동작을 실행하지 않도록 방지합니다.



### useEffect <a href="#effect-hooks" id="effect-hooks"></a>

state의 변화를 감지하고 렌더링할 요소를 업데이트하고 싶으면, useEffect를 쓰면 안됩니다.

useEffect는 화면이 다 그려지고, dom 변화가 끝난 후에 실행됩니다.



따라서 ui와 관련된 상태는 setState를 통해 재랜더링되면, 알아서 업데이트가 되게 만들어야 합니다.



추가로, 의존성은 비워두면 안됩니다.

다른 props가 추가 됐을 때, 잘 추가해줍시다.\




### 2-3 기타 유용한 hook

### useCallback <a href="#effect-hooks" id="effect-hooks"></a>

이 과정을 반복하기에 너무 무거운 작업이라면 useMemo로 감쌀 수 있습니다.

### useMemo <a href="#effect-hooks" id="effect-hooks"></a>

캐싱 가능





### 2-3 기타 유용한 api

### lazy <a href="#effect-hooks" id="effect-hooks"></a>

로딩 중인 컴포넌트 코드가 처음으로 렌더링 될 때까지 연기할 수 있습니다.

### memo <a href="#effect-hooks" id="effect-hooks"></a>

컴포넌트 자체를 메모제이션 할 수 있습니다.





### **3.** virtual DOM

가상 DOM은 실제 DOM의 가벼운 복사본입니다.&#x20;

React는 상태(state) 변화가 발생하면 가상 DOM을 먼저 업데이트한 후,&#x20;

실제 DOM과 비교하여 필요한 부분만 업데이트합니다. 이렇게 하면 성능이 최적화됩니다.



가상 DOM을 활용해서 렌더링을 다루는 것이기에,&#x20;

코드를 작성할 때 여러 hook과 api를 사용해서 렌더링을 최적화 하는 것이 중요합니다.



