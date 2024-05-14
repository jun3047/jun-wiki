# Next, 최고의 React Web 프레임워크

(App Router를 기준으로 글을 작성했습니다)

기본적으로 React의 프레임워크이기에 React에서 다룬 모든 사항이 중요합니다.



node 기반 서버를 만드는 풀스택 프레임워크이며,

SSR CSR ISR SSG와 같은 다양한 렌더링을 쉽게 적용할 수 있습니다.



Next의 핵심은 다음과 같습니다.

1. **App Router**: 페이지와 레이아웃을 정의하고, 효과적인 라우팅을 구현합니다.
2. **Data Fetching**: 데이터 패칭, 캐싱, 그리고 리밸리데이션을 관리합니다.
3. **Rendering**: 서버 및 클라이언트 컴포넌트를 사용하여 최적의 렌더링을 제공합니다.



부가적으로 아래 내용까지 다룰 예정입니다.

4. **Styling**
5. **Optimizations**
6. **TypeScript**



가봅시다!



### **App Router**

App Router는 Next.js의 라우팅 시스템의 핵심입니다. 라우팅 설정은 프로젝트의 디렉토리 구조를 통해 자동으로 이루어집니다. 예를 들어, `pages/about.js` 파일을 생성하면 `/about` 경로로 자동으로 설정됩니다.

*   **동적 라우팅**: `[id].js` 형식으로 파일을 생성하면 동적 라우팅을 구현할 수 있습니다.

    ```javascript
    javascript코드 복사// pages/posts/[id].js
    import { useRouter } from 'next/router';

    const Post = () => {
      const router = useRouter();
      const { id } = router.query;

      return <p>Post: {id}</p>;
    };

    export default Post;
    ```



*   **중첩 레이아웃**: `_app.js` 파일에서 공통 레이아웃을 설정하고, 개별 페이지에서 이를 확장할 수 있습니다.

    ```javascript
    javascript코드 복사// pages/_app.js
    import '../styles/globals.css';

    function MyApp({ Component, pageProps }) {
      return <Component {...pageProps} />;
    }

    export default MyApp;
    ```



### **Data Fetching**

Next.js는 다양한 데이터 패칭 방법을 제공합니다. 각 방식은 특정 상황에 적합하며, 적절히 사용하여 최적의 사용자 경험을 제공합니다.

*   **getServerSideProps**: 서버 사이드 렌더링을 위해 사용됩니다.

    ```javascript
    javascript코드 복사// pages/index.js
    export async function getServerSideProps() {
      const res = await fetch('https://api.example.com/data');
      const data = await res.json();

      return { props: { data } };
    }

    const Home = ({ data }) => (
      <div>
        <h1>Data from server:</h1>
        <pre>{JSON.stringify(data, null, 2)}</pre>
      </div>
    );

    export default Home;
    ```
*   **getStaticProps**: 정적 사이트 생성을 위해 사용됩니다.

    ```javascript
    javascript코드 복사// pages/index.js
    export async function getStaticProps() {
      const res = await fetch('https://api.example.com/data');
      const data = await res.json();

      return { props: { data }, revalidate: 10 };
    }

    const Home = ({ data }) => (
      <div>
        <h1>Data from server:</h1>
        <pre>{JSON.stringify(data, null, 2)}</pre>
      </div>
    );

    export default Home;
    ```
*   **getStaticPaths**: 동적 정적 페이지를 생성할 때 사용됩니다.

    ```javascript
    javascript코드 복사// pages/posts/[id].js
    export async function getStaticPaths() {
      const res = await fetch('https://api.example.com/posts');
      const posts = await res.json();

      const paths = posts.map((post) => ({
        params: { id: post.id.toString() },
      }));

      return { paths, fallback: false };
    }

    export async function getStaticProps({ params }) {
      const res = await fetch(`https://api.example.com/posts/${params.id}`);
      const post = await res.json();

      return { props: { post } };
    }

    const Post = ({ post }) => (
      <div>
        <h1>{post.title}</h1>
        <p>{post.content}</p>
      </div>
    );

    export default Post;
    ```



### Rendering

Next.js는 서버 및 클라이언트 컴포넌트를 사용하여 최적의 렌더링을 제공합니다. 서버 컴포넌트는 초기 로드 속도를 향상시키고, 클라이언트 컴포넌트는 인터랙티브한 사용자 경험을 제공합니다.

*   **서버 컴포넌트**: 서버에서 렌더링되어 클라이언트로 전송됩니다.

    ```javascript
    javascript코드 복사// components/ServerComponent.js
    const ServerComponent = ({ data }) => (
      <div>
        <h1>Server Component</h1>
        <p>{data}</p>
      </div>
    );

    export default ServerComponent;
    ```
*   **클라이언트 컴포넌트**: 브라우저에서 실행됩니다.

    ```javascript
    javascript코드 복사// components/ClientComponent.js
    import { useState, useEffect } from 'react';

    const ClientComponent = () => {
      const [data, setData] = useState(null);

      useEffect(() => {
        fetch('/api/data')
          .then((res) => res.json())
          .then((data) => setData(data));
      }, []);

      if (!data) return <p>Loading...</p>;

      return (
        <div>
          <h1>Client Component</h1>
          <p>{data}</p>
        </div>
      );
    };

    export default ClientComponent;
    ```

이와 같은 방식으로 Next.js를 실제 프로젝트에 적용할 수 있습니다. Next.js의 다양한 기능을 활용하여 최적의 웹 애플리케이션을 개발하세요!

\




### Styling

ㅇㅇ



### **Optimizations**

ㅇㅇ



### **TypeScript**

ㅇㅇ

