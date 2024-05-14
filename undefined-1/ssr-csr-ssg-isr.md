# 렌더링 방식 (SSR, CSR, SSG, ISR)

웹 애플리케이션 개발에서 렌더링 방식은 사용자 경험(UX)과 성능에 큰 영향을 미칩니다. React와 Next.js를 활용하면 다양한 렌더링 방식을 효율적으로 구현할 수 있습니다. 여기서는 서버사이드 렌더링(SSR), 클라이언트사이드 렌더링(CSR), 정적 사이트 생성(SSG), 점진적 정적 재생성(ISR)에 대해 알아보겠습니다.



### **1. 서버사이드 렌더링 (SSR)**

서버사이드 렌더링(SSR)은 요청 시 서버에서 HTML을 생성하여 클라이언트에 전달하는 방식입니다. Next.js는 React 애플리케이션에서 SSR을 쉽게 구현할 수 있도록 도와줍니다.

| 장점                                                          | 단점                                               |
| ----------------------------------------------------------- | ------------------------------------------------ |
| 초기 페이지 로드 시간이 빠릅니다.                                         | 서버 부하가 증가할 수 있습니다.                               |
| SEO(검색 엔진 최적화)에 유리합니다.                                      | <p>각 요청마다 페이지를 렌더링하므로 <br>서버 측의 자원 사용이 많습니다.</p> |
| <p>클라이언트에서 JavaScript가 실행되기 전에<br>완전한 HTML을 제공할 수 있습니다.</p> |                                                  |



**next의 예시**

```javascript
import React from 'react';

const HomePage = ({ data }) => {
  return (
    <div>
      <h1>Home Page</h1>
      <p>{data.message}</p>
    </div>
  );
};

export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: {
      data,
    },
  };
}

export default HomePage;
```

위 예제에서 getServerSideProps 함수는 서버에서 실행되며, 데이터 페칭 후 페이지를 렌더링할 때 필요한 props를 반환합니다.



### 2. 클라이언트사이드 렌더링 (CSR)&#x20;

클라이언트사이드 렌더링(CSR)은 모든 JavaScript를 클라이언트에서 실행하여 사용자에게 HTML을 동적으로 생성하는 방식입니다. 이는 전통적인 React 애플리케이션에서 일반적으로 사용됩니다.

| 장점                              | 단점                    |
| ------------------------------- | --------------------- |
| 초기 서버 부하가 적습니다.                 | 초기 로드 시간이 길어질 수 있습니다. |
| 복잡한 사용자 인터페이스를 동적으로 관리할 수 있습니다. | SEO에 불리할 수 있습니다.      |



```javascript
import React, { useState, useEffect } from 'react';

const App = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then((response) => response.json())
      .then((data) => setData(data));
  }, []);

  if (!data) {
    return <div>Loading...</div>;
  }

  return (
    <div>
      <h1>Home Page</h1>
      <p>{data.message}</p>
    </div>
  );
};

export default App;
```

위 예제에서 useEffect 훅은 컴포넌트가 마운트된 후 API 요청을 수행하고, 데이터를 가져와서 상태를 업데이트합니다.



### 3. 정적 사이트 생성 (SSG)

정적 사이트 생성(SSG)은 빌드 타임에 HTML을 생성하여 요청 시 서버에서 정적 파일을 제공하는 방식입니다. Next.js는 getStaticProps 함수를 통해 SSG를 지원합니다.

| 장점                          | 단점                      |
| --------------------------- | ----------------------- |
| 서버 부하가 적고, 페이지 로드 속도가 빠릅니다. | 빌드 타임이 길어질 수 있습니다.      |
| SEO에 유리합니다.                 | 동적인 데이터 변경에 대응하기 어렵습니다. |



**next 예시**

```javascript
import React from 'react';

const HomePage = ({ data }) => {
  return (
    <div>
      <h1>Home Page</h1>
      <p>{data.message}</p>
    </div>
  );
};

export async function getStaticProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: {
      data,
    },
  };
}

export default HomePage;
```

위 예제에서 getStaticProps 함수는 빌드 타임에 실행되어 정적 HTML 파일을 생성합니다. 이 파일은 요청 시 서버에서 제공됩니다.



### 4. 점진적 정적 재생성 (ISR)

점진적 정적 재생성(ISR)은 SSG의 장점을 유지하면서도 동적 콘텐츠 업데이트를 가능하게 하는 방식입니다. Next.js는 revalidate 속성을 사용하여 ISR을 구현할 수 있습니다.

| 장점                                               | 단점                                                                                                                                                                                            |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 빠른 초기 로드 속도와 SEO 장점을 유지하면서도 동적인 데이터 업데이트가 가능합니다. | 모든 페이지가 정적이 아니므로 모든 페이지가 정적이 아니므로 복잡성이 증가할 수 있습니다.모든 페이지가 정적이 아니므로 복잡성이 모든 페이지가 정적이 아니므로 복잡성이 증가할 수 있습니다.모든 페이지가 정적이 아니므로 복잡성이 증가할 수 있습니다.모든 페이지가 정적이 아니므로 복잡성이 증가할 수 있습니다.  복잡성이 증가할 수 있습니다. |
| 정적 파일을 일정 간격으로 재생성하여 최신 데이터를 반영할 수 있습니다.         |                                                                                                                                                                                               |



**next 예시**

```javascript
import React from 'react';

const HomePage = ({ data }) => {
  return (
    <div>
      <h1>Home Page</h1>
      <p>{data.message}</p>
    </div>
  );
};

export async function getStaticProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: {
      data,
    },
    revalidate: 10, // 10초마다 페이지를 재생성
  };
}

export default HomePage;
```

위 예제에서 getStaticProps 함수는 SSG와 유사하게 작동하지만, revalidate 속성을 통해 페이지가 10초마다 재생성되도록 설정합니다. 이렇게 하면 사용자에게 최신 데이터를 제공할 수 있습니다.



### 결론

React와 Next.js를 활용하면 다양한 렌더링 방식을 쉽게 구현할 수 있습니다. 각 방식은 장단점이 있으며, 애플리케이션의 요구사항에 맞게 적절한 방식을 선택하는 것이 중요합니다.



• SSR: 서버 부하를 감수하더라도 초기 로드 속도와 SEO가 중요한 경우.

• CSR: 사용자 인터페이스가 복잡하고 동적일 때, 초기 서버 부하를 줄이고자 할 때.

• SSG: 서버 부하를 최소화하고 빠른 페이지 로드를 원할 때, 자주 변경되지 않는 데이터를 다룰 때.

• ISR: 빠른 초기 로드 속도와 SEO의 장점을 유지하면서도 동적 콘텐츠 업데이트가 필요한 경우.



