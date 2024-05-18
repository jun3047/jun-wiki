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

App Router는 Next.js의 라우팅 시스템의 핵심입니다. 라우팅 설정은 프로젝트의 디렉토리 구조를 통해 자동으로 이루어집니다.&#x20;



**app/layout.tsx**

루트 레이아웃은 디렉터리의 최상위 수준에서 정의되며 app 모든 경로에 적용됩니다. 이 레이아웃은 **필수**이며 서버에서 반환된 초기 HTML을 수정할 수 있도록 html, body를 포함해야 합니다 .

```javascript
//app/layout.tsx

import type { Metadata } from 'next'
 
export const metadata: Metadata = {
  title: 'Next.js',
} //무조건 이런 식으로 따로 적어야 함

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>
        {/* Layout UI */}
        <main>{children}</main>
      </body>
    </html>
  )
}

```

* 상위 레이아웃과 해당 하위 레이아웃 간에 데이터를 전달하는 것은 불가능합니다. 그러나 경로에서 동일한 데이터를 두 번 이상 가져올 수 있으며 React는 성능에 영향을 주지 않고 [자동으로 요청의 중복을 제거](https://nextjs.org/docs/app/building-your-application/caching#request-memoization) 합니다 .
* 레이아웃에는 액세스할 수 없습니다 `pathname`( [자세히 알아보기](https://nextjs.org/docs/app/api-reference/file-conventions/layout) ). 그러나 가져온 클라이언트 구성 요소는 후크를 사용하여 경로 이름에 액세스할 수 있습니다 [`usePathname`](https://nextjs.org/docs/app/api-reference/functions/use-pathname).





**app/경로/layout.tsx**

각 라우팅 폴더에 `layout.js / jsx / tsx` 형식으로 파일을 생성하면 해당 폴더 하위에 모두 적용되는 틀이 생깁니다.

```javascript
// app/경로/layout.tsx
 
import { usePathname } from 'next/navigation'
import Link from 'next/link'
 
export function NavLinks() {
  const pathname = usePathname()
 
  return (
    <nav>
      <Link className={`link ${pathname === '/' ? 'active' : ''}`} href="/">
        Home
      </Link>
      <Link
        className={`link ${pathname === '/about' ? 'active' : ''}`}
        href="/about"
      >
        About
      </Link>
    </nav>
  )
}
```



**app/\_경로/layout.tsx:** 폴더로서의 역할을 할 뿐 url이 생성되지 않습니다.

**app/경로/page.tsx:** 실제 그려질 page입니다. url을 생성합니다.





**app/경로/error.js**: 에러 시에 표시됩니다.

```tsx
'use client'// Error components must be Client Components 

import { useEffect } from 'react'

export default function Error({
    error,
    reset,
}: {
    error: Error & { digest?: string }
    reset: () => void
}) {

    useEffect(() => {    
        console.error(error) // Log the error to an error reporting service
    }, [error])
    
    return (
        <div>
            <h2>Something went wrong!</h2>
            <button
                onClick={() => reset()}> // Attempt to recover by trying to re-render the segment
                Try again
            </button>
        </div>
    )
}                
```

**app/global-error.js: 에러 시에 표시됩니다. html body 필수 입니다.**\
**app/loading.js: 로딩 중에 표시됩니다.**



**app/경로/\[slug]/page.js:** 동적으로 라우팅합니다.

**app/경로/\[...slug]/page.js:** 깊이있는 (ex. blog/a/b/a)까지 포함합니다.

**app/shop/\[\[...slug]]/page.js:**&#x20;



* 동적 라우팅에서 generateStaticParams를 이용하면, 빌드시 정적으로 페이지가 생성됩니다.

<pre class="language-javascript"><code class="lang-javascript">export async function <a data-footnote-ref href="#user-content-fn-1">generateStaticParams</a>() {
    const posts = await fetch('https://.../posts').then((res) => res.json())
                
    return posts.map((post) => ({
                slug: post.slug
    }))
}   
</code></pre>





**app/(묶음그룹)/layout.js:** 개발 시 유용함을 위해서, 불리할 수 있으며 각각의 그룹마다 layout.js를 따로 둘 수 있습니다. 이는 루트 경로에도 해당됩니다.

<figure><img src=".gitbook/assets/스크린샷 2024-05-17 오후 11.31.06.png" alt=""><figcaption></figcaption></figure>



**api/route.js**: 해당 경로에 url을 생성합니다.





**폴더 구성 방법**

1. components, lib등 경로 외부로 분리하고, app을 라우팅 목적으로 사용
2. app내에 라우팅을 포함한 components, lib을 모두 모음
3. 외부 경로의 components, lib에서는 공통 사항을 넣고, \
   app의 해당 경로에서 사용하는 목적의 components, lib 또한 사용



**병렬 경로**

app/@슬롯A/page.js\
app/@슬롯B/page.js\
app/layout.js

```javascript
export default function Layout({
    children,
    team,
    analytics,
}: {
    children: React.ReactNode
    analytics: React.ReactNode
    team: React.ReactNode
}) { 

    return (
        <>
            {children}
            {team}
            {analytics}
        </>
    )
}
```

이를 이용해서 로그인에 따른 페이지, 탭 그룹, 모달 등을 구현할 수 있습니다.



**경로 가로채기**

경로를 가로채서, 앱 내부에서 이동했을 때랑 외부(공유 혹은 리로딩)에서 접근했을 때를 다르게 구현할 수 있습니다.



* `(.)`**동일한 수준** 의 세그먼트를 일치시키려면
* `(..)`**한 수준 위의** 세그먼트와 일치시키려면
* `(..)(..)`**두 수준 위의** 세그먼트와 일치시키려면
* `(...)`**루트** `app` 디렉터리 의 세그먼트를 일치시키려면\


<figure><img src=".gitbook/assets/스크린샷 2024-05-18 오전 12.59.14.png" alt=""><figcaption></figcaption></figure>





**app/not-found.tsx: 말 그대로 not-found 상황에서 대응해줍니다.**

```javascript
import Link from 'next/link'
import { headers } from 'next/headers'
 
export default async function NotFound() {
  const headersList = headers()
  const domain = headersList.get('host')
  const data = await getSiteData(domain)
  return (
    <div>
      <h2>Not Found: {data.name}</h2>
      <p>Could not find requested resource</p>
      <p>
        View <Link href="/blog">all posts</Link>
      </p>
    </div>
  )
}

```





### 서버 설정



**api/route.ts**\
**api/경로/route.ts**

```javascript
export async function POST() {
  const res = await fetch('https://data.mongodb-api.com/...', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'API-Key': process.env.DATA_API_KEY!,
    },
    body: JSON.stringify({ time: new Date().toISOString() }),
  })
 
  const data = await res.json()
 
  return Response.json(data)
}
```



**app/경로/\[slog]/route.ts**

```javascript
export async function GET( request: Request, { params }:
    {
        params: { slug: string }}
    ){
    const slug = params.slug // 'a', 'b', or 'c' 
    
    //나머지 코드
}
```



**app/경로/route.ts에서 쿼리 매개변수 설정**

```javascript
import { type NextRequest } from 'next/server'
 
export function GET(request: NextRequest) {
  const searchParams = request.nextUrl.searchParams
  const query = searchParams.get('query')
  
  // query is "hello" for /api/search?query=hello
}
```







**+ next.config.json**

cors 설정과 basePath

```javascript
async headers() {
    return [
      {
        source: "/api/:path*",
        headers: [
          {
            key: "Access-Control-Allow-Origin",
            value: "*", // Set your origin
          },
          {
            key: "Access-Control-Allow-Methods",
            value: "GET, POST, PUT, DELETE, OPTIONS",
          },
          {
            key: "Access-Control-Allow-Headers",
            value: "Content-Type, Authorization",
          },
        ],
      },
    ];
  },

module.exports = { basePath: '/docs', }
```



패스한 사항: Redirecting, [국제화](https://nextjs.org/docs/app/building-your-application/routing/internationalization#routing-overview)





### **Data Fetching**



1.  **서버에서 데이터 페칭하기**

    * 백엔드 데이터 자원에 직접 접근해야 할 때
    * 민감한 정보를 클라이언트에 노출하지 않으려 할 때
    * 클라이언트와 서버 간의 통신을 최소화하고 성능을 최적화할 때

    ```typescript
    // app/page.tsx
    import React from 'react';

    async function getData() {
      const res = await fetch('https://api.example.com/...');
      if (!res.ok) {
        throw new Error('데이터 가져오기 실패');
      }
      return res.json();
    }

    export default async function Page() {
      const data = await getData();
      return <main>{JSON.stringify(data)}</main>;
    }
    ```

    *   **캐싱**

        * 기본적으로 Next.js는 `fetch` 요청을 자동으로 캐싱합니다. 캐싱을 비활성화하거나 재검증 간격을 설정할 수 있습니다.

        ```typescript
        // 캐싱 설정 예제
        fetch('https://...', { cache: 'force-cache' }); // 기본 값
        fetch('https://...', { cache: 'no-store' }); // 캐시 비활성화
        fetch('https://...', { next: { revalidate: 3600 } }); // 1시간마다 재검증
        ```





2. **클라이언트에서 데이터 페칭하기**

* 서버 액션을 사용하여 데이터 뮤테이션이 필요할 때
* 서버 컴포넌트에서 직접 데이터를 가져올 때

```typescript
// app/actions.ts
'use server';

export async function create() {
  // 데이터 생성 로직
}

// app/ui/button.tsx
'use client';
import { create } from '@/app/actions';

export function Button() {
  return <button onClick={create}>Create</button>;
}
```



**병렬 및 순차 데이터 페칭**

* 순차 데이터 페칭이 필요할 때
* 요청이 서로 의존적인 경우

```typescript
// app/artist/[username]/page.tsx
import React, { Suspense } from 'react';

async function Playlists({ artistID }) {
  const playlists = await getArtistPlaylists(artistID);
  return (
    <ul>
      {playlists.map((playlist) => (
        <li key={playlist.id}>{playlist.name}</li>
      ))}
    </ul>
  );
}

export default async function Page({ params: { username } }) {
  const artist = await getArtist(username);
  return (
    <>
      <h1>{artist.name}</h1>
      <Suspense fallback={<div>Loading...</div>}>
        <Playlists artistID={artist.id} />
      </Suspense>
    </>
  );
}
```



**병렬 데이터 페칭이 필요할 때**

* 독립적인 데이터를 동시에 가져와야 할 때

```typescript
// app/artist/[username]/page.tsx
import React from 'react';
import Albums from './albums';

async function getArtist(username) {
  const res = await fetch(`https://api.example.com/artist/${username}`);
  return res.json();
}

async function getArtistAlbums(username) {
  const res = await fetch(`https://api.example.com/artist/${username}/albums`);
  return res.json();
}

export default async function Page({ params: { username } }) {
  const artistData = getArtist(username);
  const albumsData = getArtistAlbums(username);

  const [artist, albums] = await Promise.all([artistData, albumsData]);

  return (
    <>
      <h1>{artist.name}</h1>
      <Albums list={albums}></Albums>
    </>
  );
}
```



**서버 액션과 데이터 뮤테이션**

* 폼 데이터를 처리할 때
* 서버에서 데이터 유효성 검사를 할 때

```typescript
// app/invoices/page.tsx
import React from 'react';

export default function Page() {
  async function createInvoice(formData) {
    'use server';

    const rawFormData = {
      customerId: formData.get('customerId'),
      amount: formData.get('amount'),
      status: formData.get('status'),
    };

    // 데이터 뮤테이션 로직
  }

  return <form action={createInvoice}>...</form>;
}
```

\






### Rendering



**1. 서버 컴포넌트 (Server Components)**



서버 컴포넌트의 장점

* **데이터 페칭**: 데이터를 서버에서 가져와 클라이언트의 요청을 줄이고 성능을 향상시킴.
* **보안**: 민감한 데이터를 클라이언트에 노출하지 않고 서버에서만 처리.
* **캐싱**: 서버에서 렌더링된 결과를 캐시하여 성능을 최적화.
* **성능**: 초기 페이지 로드 시간과 FCP(First Contentful Paint)를 줄여 사용자 경험을 개선.
* **검색 엔진 최적화**: 서버에서 렌더링된 HTML을 통해 검색 엔진과 소셜 네트워크에서 페이지를 더 잘 인식.



```typescript
// app/page.tsx
import React from 'react';

async function getData() {
  const res = await fetch('https://api.example.com/...');
  if (!res.ok) {
    throw new Error('데이터 가져오기 실패');
  }
  return res.json();
}

export default async function Page() {
  const data = await getData();
  return <main>{JSON.stringify(data)}</main>;
}
```





#### 2. 클라이언트 컴포넌트 (Client Components)



**클라이언트 컴포넌트의 장점**

* **인터랙티비티**: 상태, 효과, 이벤트 리스너를 사용하여 즉각적인 사용자 피드백 제공.
* **브라우저 API**: geolocation, localStorage와 같은 브라우저 API 사용 가능.

```typescript
// app/counter.tsx
'use client'

import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```



#### 3. 서버와 클라이언트 구성 패턴 (Server and Client Composition Patterns)



**서버와 클라이언트 컴포넌트 사용 구분 정리**

* **서버 컴포넌트**: 데이터 페칭, 백엔드 자원 접근, 민감한 정보 보호, 클라이언트 측 자바스크립트 줄이기.
* **클라이언트 컴포넌트**: 인터랙티브 UI, 상태 및 라이프사이클 사용, 브라우저 전용 API 사용.

**서버 전용 코드 보호**

```typescript
// lib/data.ts
import 'server-only';

export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      authorization: process.env.API_KEY,
    },
  });
  return res.json();
}
```

**클라이언트 컴포넌트로 래핑**

```typescript
// app/carousel.tsx
'use client'

import { Carousel } from 'acme-carousel';

export default Carousel;
```



#### 4. 런타임 (Runtimes)

**Node.js 런타임**

* **사용 사례**: 애플리케이션 렌더링.
* **제한 사항**: 모든 Node.js API 및 호환 패키지 사용 가능.

**Edge 런타임**

* **사용 사례**: 미들웨어 (리다이렉트, 리라이트, 헤더 설정).
* **제한 사항**: 일부 Node.js API 미지원, ISR(Incremental Static Regeneration) 미지원.

**Node.js 런타임 사용 예제**

```typescript
typescript코드 복사// next.config.js
module.exports = {
  reactStrictMode: true,
  experimental: {
    runtime: 'nodejs',
  },
};
```

**Edge 런타임 사용 예제**

```typescript
typescript코드 복사// middleware.ts
export function middleware(request) {
  // Edge 함수는 제한된 API 사용
  return new Response('Hello, Edge!', { status: 200 });
}

export const config = {
  runtime: 'experimental-edge',
};
```

위와 같은 패턴을 사용하여 Next.js 최신 버전에 맞춰 서버 및 클라이언트 컴포넌트를 효과적으로 활용할 수 있습니다. 추가적인 정보가 필요하면 언제든지 알려주세요!

\


\-

아래는 보충이 필요



### Optimizations

Next.js는 성능 최적화를 위한 다양한 도구와 기법을 제공합니다.

#### Image Optimization

Next.js의 Image 컴포넌트를 사용하면 자동으로 이미지 최적화가 이루어집니다.

```javascript
// pages/index.js
import Image from 'next/image';

const Home = () => (
  <div>
    <h1>Optimized Image Example</h1>
    <Image src="/me.png" alt="Picture of the author" width={500} height={500} />
  </div>
);

export default Home;
```

#### Code Splitting

Next.js는 자동으로 페이지 단위로 코드 스플리팅을 수행하여 필요한 코드만 로드합니다. 이를 통해 초기 로딩 시간을 줄일 수 있습니다.

#### Lazy Loading

React의 `React.lazy`와 `Suspense`를 사용하여 컴포넌트를 지연 로드할 수 있습니다.

```javascript
// components/HeavyComponent.js
const HeavyComponent = React.lazy(() => import('./HeavyComponent'));

const Home = () => (
  <div>
    <h1>Home Page</h1>
    <React.Suspense fallback={<div>Loading...</div>}>
      <HeavyComponent />
    </React.Suspense>
  </div>
);

export default Home;
```





### TypeScript

Next.js는 TypeScript를 지원하며, 이를 통해 더 안정적이고 유지보수하기 쉬운 코드를 작성할 수 있습니다.

#### 설정

TypeScript를 Next.js 프로젝트에 설정하려면, 프로젝트 루트에 `tsconfig.json` 파일을 생성합니다. Next.js는 자동으로 필요한 설정 파일을 생성합니다.

```bash
bash코드 복사npx create-next-app@latest --ts
```

#### TypeScript 사용 예시

TypeScript를 사용하여 컴포넌트와 페이지를 작성할 수 있습니다.

```typescript
typescript코드 복사// pages/index.tsx
import { GetStaticProps } from 'next';

interface HomeProps {
  data: string;
}

const Home: React.FC<HomeProps> = ({ data }) => (
  <div>
    <h1>Data from server:</h1>
    <p>{data}</p>
  </div>
);

export const getStaticProps: GetStaticProps = async () => {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return { props: { data } };
};

export default Home;
```

#### 타입 정의

Next.js 프로젝트에서 타입을 정의하여 사용하면 코드의 안정성과 가독성을 높일 수 있습니다.

```typescript
typescript코드 복사// types/index.ts
export interface Post {
  id: string;
  title: string;
  content: string;
}

// pages/posts/[id].tsx
import { GetStaticPaths, GetStaticProps } from 'next';
import { Post } from '../../types';

interface PostProps {
  post: Post;
}

const PostPage: React.FC<PostProps> = ({ post }) => (
  <div>
    <h1>{post.title}</h1>
    <p>{post.content}</p>
  </div>
);

export const getStaticPaths: GetStaticPaths = async () => {
  const res = await fetch('https://api.example.com/posts');
  const posts: Post[] = await res.json();

  const paths = posts.map((post) => ({
    params: { id: post.id.toString() },
  }));

  return { paths, fallback: false };
};

export const getStaticProps: GetStaticProps = async ({ params }) => {
  const res = await fetch(`https://api.example.com/posts/${params.id}`);
  const post = await res.json();

  return { props: { post } };
};

export default PostPage;
```

위와 같은 방법으로 Next.js에서 스타일링, 최적화, 그리고 TypeScript를 활용하여 더욱 강력한 웹 애플리케이션을 개발할 수 있습니다.





[^1]: 
