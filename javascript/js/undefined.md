# 기본 문법

이것만 알면 됨 (JS etc) js1 + 코테 내용 \[2시간]



### 세미콜론은 항상 씁시다.

세미콜론은 _생략할 수 있습니다. 허나 예외적인 상황이 있기에, 쓰는 걸 권장합니다._



### 최상단의 "use strict"은 모던한 방식을 의미합니다.

레거시 코드의 호완성을 유지하기 위해, "use strict"은 ECMAScript5(ES5) 이상의 변경 사항을 강제합니다.&#x20;

아래 사항에서 엄격모드 ("use strict")이 적용됩니다.

1\) class와 import를 사용할 때\
2\) 함수 혹은 파일 위에서 "use strict"이 작성될 때\
3\) 프레임워크에서 기본으로 지원할 때



### 적절한 변수를 선언합시다

* 숫자와 문자를 사용하되 첫 글자는 숫자가 될 수 없습니다.
* 특수기호는 `$`와 `_`만 사용할 수 있습니다.

**카멜 표기법**은 단어를 차례대로 나열하면서 첫 단어를 제외한 각 단어의 첫 글자를 대문자로 작성합니다.&#x20;

```
userName
```

| 가변값      | 불변값      | 상수값        |
| -------- | -------- | ---------- |
| let      | const    | const      |
| userName | userName | USER\_NAME |

또한 user / visitor / man등 같은 의미를 가진 여러 표현을 지양하고, 사람이 읽을 수 있고 역할을 잘 나타내는 이름을 붙여야 합니다.





### 자바스크립트에는 8가지 자료형이 있습니다.

숫자형\
BigInt (숫자 끝에 n)

불린형\
문자형

객체 (함수를 포함)\
심볼 (객체의 고유 식별자)

undefined (초기값이 없을 때)\
null (비어 있는 값)

`typeof` 연산자는 값의 자료형을 반환해줍니다. 그런데 두 가지 예외 사항이 있습니다.

```javascript
typeof null == "object" // 언어 자체의 오류
typeof function(){} == "function" // 함수는 특별하게 취급됩니다.
```



### 형변환

형변환은 핵심 중 하나입니다.



### 연산자

**산술 연산자** `* + - / % **`&#x20;

이항 덧셈 연산자 `+`는 피연산자 중 하나가 문자열일 때 나머지 하나를 문자형으로 바꾸고 두 문자열을 연결합니다.

```javascript
alert( '1' + 2 ); // '12', 문자열
alert( 1 + '2' ); // '12', 문자열
```

**할당 연산자** `=`

할당 연산자 `=`에 산술연자를 붙여 복합 할당 연산자로 활용할 수 있습니다.

**조건부 연산자** `?`

자바스크립트 연산자 중 유일하게 매개변수가 3개인 연산자입니다.

```javascript
cond ? 'true면 반환' : 'false면 반환'
```

**논리 연산자** `&&(AND) ||(OR)!(NOT)`

```javascript
true && false // 하나라도 false면 false
true || false // 하나라도 true면 true
!true // false 
```

**nullish** `??`

```javascript
a ?? b //(a가 null이나 undefined면 b 반환)
```

**비교 연산자** `< > <= >= == ===`

동등 연산자 `==`는 형이 다른 값끼리 비교할 때 피연산자의 자료형을 숫자형으로 바꾼 후 비교를 진행합니다. `null`과 `undefined`는 자기끼리 비교할 땐 참을 반환하지만 다른 자료형과 비교할 땐 거짓을 반환합니다.

```javascript
alert( 0 == false ); // true
alert( 0 == '' ); // true
```

기타 비교 연산자들 `< > <= >=` 역시 피연산자의 자료형을 숫자형으로 바꾼 후 비교를 진행합니다.

일치 연산자 `===`는 피연산자의 형을 변환하지 않습니다. 형이 다르면 무조건 다르다고 평가합니다.

`null`과 `undefined`는 특별한 값입니다. 두 값을 `==` 연산자로 비교하면 `true`를 반환하지만, 다른 값과 비교하면 무조건 `false`를 반환합니다.

크고 작음을 비교하는 연산자의 피연산자로 문자열이 들어오면 글자 단위로 크기 비교가 이뤄집니다. 다른 타입의 값이 들어오면 숫자형으로 형 변환한 후 비교를 진행합니다.



이외에도 **비트 연산자, 쉼표 연산자** 등이 있습니다.



### 적절한 반복문 선택

js에는 많은 반복문이 있습니다.

```javascript
while (condition) {
  ...
}

do {
  ...
} while (condition);

for(let i = 0; i < 10; i++) {
  ...
}
```

지시자 `break`나 `continue`는 반복문 전체나 현재 실행 중인 반복을 빠져나가는 데 사용됩니다. 레이블은 중첩 반복문을 빠져나갈 때 사용합니다.



### 분기를 나누는 방법 (switch, if)

적절한 반복문 선택할 수 있어야 합니다.

```javascript
switch(x) {
  case 'value1':  // if (x === 'value1')
    ...
    [break]

  case 'value2':  // if (x === 'value2')
    ...
    [break]

  default:
    ...
    [break]
}
```





### 함수

세 가지 방법으로 함수를 만들 수 있습니다.

1.  함수 선언문: 주요 코드 흐름을 차지하는 방식\


    ```javascript
    function sum(a, b) {
      let result = a + b;

      return result;
    }
    ```
2.  함수 표현식: 표현식 형태로 선언된 함수\


    ```javascript
    let sum = function(a, b) {
      let result = a + b;

      return result;
    };
    ```
3.  화살표 함수:\


    ```javascript
    // 화살표(=>) 우측엔 표현식이 있음
    let sum = (a, b) => a + b;

    // 중괄호{ ... }를 사용하면 본문에 여러 줄의 코드를 작성할 수 있음. return문이 꼭 있어야 함.
    let sum = (a, b) => {
      // ...
      return a + b;
    }

    // 인수가 없는 경우
    let sayHi = () => alert("Hello");

    // 인수가 하나인 경우
    let double = n => n * 2;
    ```

매개변수에 기본값을 설정할 수 있습니다. 문법은 다음과 같습니다. `function sum(a = 1, b = 2) {...}`

함수는 항상 무언가를 반환합니다. `return`문이 없는 경우는 `undefined`를 반환합니다.

\


### **권장하는 코딩 스타일**

<figure><img src="../../.gitbook/assets/스크린샷 2024-05-08 오후 4.43.06.png" alt=""><figcaption><p>출처 <a href="https://ko.javascript.info/coding-style">https://ko.javascript.info/coding-style</a></p></figcaption></figure>

또한 함수 선언과 포함된 주요 코드를 먼저 작성하고, 함수를 선언하는 것이 좋습니다.

함수 이름을 잘 선언했다면, 함수 안을 볼 필요가 없습니다.



**Linter**를 통해 자동으로 코드 스타일을 강제할 수 있습니다.

* [JSLint](http://www.jslint.com/) – 역사가 오래된 linter
* [JSHint](http://www.jshint.com/) – JSLint보다 세팅이 좀 더 유연한 linter
* [ESLint](http://eslint.org/) – 가장 최근에 나온 linter



### 주석은 잘 사용해야 좋습니다.



알고리즘이 복잡한 코드를 작성하는 경우나 최적화를 위해 코드를 약간 비틀어 작성할 땐 설명을 적어주어야 합니다.&#x20;

이런 경우를 제외하곤 간결하고 코드 자체만으로 설명이 가능하게 코딩해야 합니다.



**주석에 들어가면 좋은 내용**

* 고차원 수준 아키텍처
* 함수 용례 (인수와 반환 값)
* 당장 봐선 명확해 보이지 않는 해결 방법에 대한 설명

**주석에 들어가면 좋지 않은 내용**

* '코드가 어떻게 동작하는지’와 '코드가 무엇을 하는지’에 대한 설명
* 코드를 간결하게 짤 수 없는 상황이나 코드 자체만으로도 어떤 일을 하는지 충분히 판단할 수 없는 경우에만 주석을 넣으세요.

\


### 객체 연산



Key를 동적으로 할당할 때

```javascript
let fruit = prompt("어떤 과일을 구매하시겠습니까?", "apple");

let bag = {
  [fruit]: 5, // 변수 fruit에서 프로퍼티 이름을 동적으로 받아 옵니다.
};

alert( bag.apple ); // fruit에 "apple"이 할당되었다면, 5가 출력됩니다.
```

\
특정 Key의 존재를 확인 할 때

```javascript
"key" in object
```



Key를 순회할 때

```javascript
for (key in object) {
  // 각 프로퍼티 키(dkaey)를 이용하여 본문(body)을 실행합니다.
}
```

\




#### 생략한 부분

alert, prompt, confirm

[테스트](https://ko.javascript.info/testing-mocha)

[폴리필](https://ko.javascript.info/polyfills) (브라우저 동작에 다룹니다)

