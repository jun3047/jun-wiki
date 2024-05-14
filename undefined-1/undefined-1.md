# 객체 지향 프로그램에 대해

JavaScript는 프로토타입 기반의 객체지향 언어로, 객체 지향 프로그래밍(OOP)의 개념을 활용하여 프로그램을 구성하는 데 유리합니다.&#x20;

객체 지향 프로그래밍은 프로그램을 수많은 ’객체(object)’라는 기본 단위로 나누고, 이들의 상호작용으로 프로그램의 동작을 서술하는 방식입니다. 객체란 ’메소드(method), 변수(variable)’를 가지며, 특정 역할을 수행하도록 인간이 정의한 추상적인 개념입니다.

객체지향 프로그래밍의 세 가지 핵심 개념은 내부 상태의 캡슐화(encapsulation), 메세징(messaging), 동적 바인딩(dynamic binding)입니다. 이를 통해 우리는 복잡한 시스템을 더 쉽게 관리하고 유지보수할 수 있습니다.





### JS로 이해하는 객체지향



**캡슐화**

캡슐화는 객체의 내부 상태를 보호하고, 외부에서의 접근을 제한하는 방법입니다. 이를 통해 객체 내부의 데이터가 임의로 변경되는 것을 방지하고, 객체의 일관성을 유지할 수 있습니다. JavaScript에서는 클래스와 클로저를 이용해 캡슐화를 구현할 수 있습니다.

```javascript
class Person {
  constructor(name, age) {
    this._name = name;
    this._age = age;
  }

  getName() {
    return this._name;
  }

  setName(name) {
    this._name = name;
  }

  getAge() {
    return this._age;
  }

  setAge(age) {
    if (age > 0) {
      this._age = age;
    } else {
      console.error("Age must be positive");
    }
  }
}

const person1 = new Person("Alice", 30);
console.log(person1.getName()); // Alice
person1.setAge(31);
console.log(person1.getAge()); // 31
```

위 예제에서 Person 클래스는 name과 age라는 두 가지 속성을 가집니다. 이 속성들은 getName, setName, getAge, setAge 메소드를 통해서만 접근 및 변경이 가능합니다. 이렇게 하면 age 속성이 잘못된 값으로 설정되는 것을 방지할 수 있습니다.



**메세징**

메세징은 객체들이 서로 상호작용하는 방법을 의미합니다. 객체는 서로에게 메세지를 보내어 특정 작업을 요청할 수 있으며, 이는 객체의 메소드를 호출하는 형태로 구현됩니다. 메세징의 핵심은 인터페이스(interface)를 통해 이루어집니다.

인터페이스는 객체가 제공하는 메소드들의 집합으로, 다른 객체가 이 객체와 상호작용할 수 있는 방법을 정의합니다. 좋은 인터페이스는 사용하기 쉽고, 객체의 내부 구현을 숨기며, 객체 간의 결합도를 낮춥니다.

```javascript
class BankAccount {
  constructor(owner, balance) {
    this.owner = owner;
    this._balance = balance;
  }

  deposit(amount) {
    if (amount > 0) {
      this._balance += amount;
      console.log(`${amount} deposited. New balance: ${this._balance}`);
    }
  }

  withdraw(amount) {
    if (amount > 0 && amount <= this._balance) {
      this._balance -= amount;
      console.log(`${amount} withdrawn. New balance: ${this._balance}`);
    } else {
      console.log("Withdrawal amount exceeds balance");
    }
  }

  getBalance() {
    return this._balance;
  }
}

const account = new BankAccount("John Doe", 1000);
account.deposit(500);
account.withdraw(200);
console.log(account.getBalance()); // 1300
```

위 예제에서 BankAccount 클래스는 deposit, withdraw, getBalance 메소드를 통해 외부 객체와 상호작용합니다. 이 메소드들은 BankAccount 객체의 인터페이스를 구성하며, 외부 객체는 이 인터페이스를 통해서만 BankAccount 객체와 상호작용할 수 있습니다.



**동적 바인딩**

동적 바인딩은 런타임에 객체의 메소드 호출을 결정하는 방식입니다. 이는 다형성(polymorphism)을 구현하는 데 필수적인 기능으로, 객체 지향 프로그래밍에서 중요한 개념입니다. 동적 바인딩을 통해 우리는 더 추상화되고 재사용 가능한 코드를 작성할 수 있습니다.



JavaScript에서 동적 바인딩을 사용하는 예로는 프로토타입 상속과 메소드 오버라이딩이 있습니다.

```javascript
class Animal {
  speak() {
    console.log("Animal speaks");
  }
}

class Dog extends Animal {
  speak() {
    console.log("Dog barks");
  }
}

class Cat extends Animal {
  speak() {
    console.log("Cat meows");
  }
}

function makeAnimalSpeak(animal) {
  animal.speak();
}

const myDog = new Dog();
const myCat = new Cat();

makeAnimalSpeak(myDog); // Dog barks
makeAnimalSpeak(myCat); // Cat meows
```



위 예제에서 makeAnimalSpeak 함수는 Animal 타입의 객체를 매개변수로 받습니다. 이 함수는 실제로 어떤 타입의 동물이 전달되었는지에 관계없이 speak 메소드를 호출합니다. 이는 동적 바인딩을 통해 런타임에 호출할 메소드를 결정하기 때문입니다.





### **REACT로 이해하는 객체지향**

React는 UI를 구성하는 데 객체지향 프로그래밍의 개념을 잘 활용할 수 있는 JavaScript 라이브러리입니다. React 컴포넌트는 객체로 간주될 수 있으며, 상태(state)와 속성(props)을 통해 캡슐화됩니다. 또한, 컴포넌트 간의 상호작용은 메세징을 통해 이루어집니다.



**캡슐화**

React 컴포넌트는 자신의 상태를 캡슐화하여 외부에서 직접 접근할 수 없도록 합니다. 컴포넌트의 상태는 useState 훅을 사용하여 관리할 수 있습니다.

```javascript
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

export default Counter;
```

위 예제에서 Counter 컴포넌트는 count 상태를 가지고 있으며, 이 상태는 increment 함수에 의해 관리됩니다. count 상태는 Counter 컴포넌트 내부에서만 접근 가능하며, 외부에서는 직접 변경할 수 없습니다.



**메세징**

React 컴포넌트 간의 상호작용은 props를 통해 이루어집니다. 부모 컴포넌트는 자식 컴포넌트에 props를 전달하여 필요한 데이터를 주고받을 수 있습니다.

```javascript
import React from 'react';

function ChildComponent({ message }) {
  return <p>{message}</p>;
}

function ParentComponent() {
  return <ChildComponent message="Hello from parent!" />;
}

export default ParentComponent;
```

위 예제에서 ParentComponent는 ChildComponent에 message라는 props를 전달합니다. ChildComponent는 이 props를 사용하여 메시지를 표시합니다. 이는 메세징을 통한 컴포넌트 간의 상호작용의 예입니다.

\


**동적 바인딩**

React에서는 컴포넌트의 동적 바인딩을 활용하여 다양한 UI를 렌더링할 수 있습니다. 예를 들어, 조건부 렌더링을 통해 다양한 컴포넌트를 동적으로 렌더링할 수 있습니다.

```javascript
import React, { useState } from 'react';

function Greeting({ isLoggedIn }) {
  if (isLoggedIn) {
    return <h1>Welcome back!</h1>;
  } else {
    return <h1>Please sign up.</h1>;
  }
}

function App() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  return (
    <div>
      <Greeting isLoggedIn={isLoggedIn} />
      <button onClick={() => setIsLoggedIn(!isLoggedIn)}>
        {isLoggedIn ? 'Logout' : 'Login'}
      </button>
    </div>
  );
}

export default App;
```



위 예제에서 Greeting 컴포넌트는 isLoggedIn이라는 props를 받아 조건에 따라 다른 메시지를 렌더링합니다. App 컴포넌트는 isLoggedIn 상태를 관리하며, 버튼 클릭을 통해 로그인 상태를 토글합니다. 이는 동적 바인딩을 활용하여 다양한 UI를 동적으로 렌더링하는 예입니다.



**결론**

JavaScript는 프로토타입 기반의 객체지향 언어로서, 캡슐화, 메세징, 동적 바인딩과 같은 객체지향 프로그래밍의 핵심 개념을 통해 효율적으로 프로그램을 구성할 수 있습니다. React와 같은 라이브러리는 이러한 개념을 잘 활용하여 UI를 구성하는 데 큰 도움을 줍니다. 객체지향 프로그래밍을 통해 우리는 더 유지보수하기 쉽고, 확장 가능한 프로그램을 작성할 수 있습니다.



더 나아가, react에는 커스텀 훅을 도입해서 각 컴포넌트와 훅을 객체처럼 여겨 객체지향으로 프로그램을 구현해나갈 수 있습니다.





\-

[**참고 영상**](https://www.youtube.com/watch?v=zgeCwYWzK-k\&ab\_channel=%EC%8B%AC%ED%94%8C%EC%97%90%EB%94%94)
