# nextjs.org/learn demo content

이 저장소는 Next.js, https://nextjs.org/learn 에 있는 컨텐츠의 데모 소스이다. 아래 내용들은 해당 페이지의 내용을 짧은 워크샵을 위해서 한글로 요약한 버전이다. 더 많은 정보를 원한다면 해당 페이지를 방문 해보자.

> This repository is meant to be used with the Next.js tutorial on nextjs.org/learn. This content is summary from content of the learning site in Korean for mini workshop. If you would like see more content? Please visit it

## 데모 소스 Setup

전체 소스를 받는 방법은 두가지, 하나는 압출파일을 받거나 Clone 을 한다.

- [**Download a zipped version of this repository**](https://github.com/zeit/next-learn-demo/archive/master.zip)

- **Clone the repository with git** by running the following command:
  ```bash
  git clone git@github.com:zeit/next-learn-demo.git
  ```

## Next.js

- An intuitive page-based routing system (with support for dynamic routes)
- Automatically statically optimizes page(s) when possible
- Server-side renders page(s) with blocking data requirements
- Automatic code splitting for faster page loads
- Client-side routing with optimized page prefetching
- Webpack-based dev environment which supports Hot Module Replacement (HMR)
- API routes to build your API with serverless functions, with the same simple router used for pages
- Customizable with community plugins and with your own Babel and Webpack configurations

## Getting Started

- 프로젝트를 생성하고 필요한 디펜던시들을 다운로드 한다

```sh
mkdir hello-next
cd hello-next
npm init -y
npm install --save react react-dom next
```

- 라우팅을 위한 페이지 경로를 생성.

```sh
mkdir pages
```

- package.json 의 scripts 에 아래 명령을 추가 한다

```json
"scripts": {
  "dev": "next",
  "build": "next build",
  "start": "next start"
}
```

- 로컬 서버를 동작 시켜서 동작 확인 한다

```sh
npm run dev
```

- `pages/index.js` 를 생성하고 아래 코드를 입력, 다시 앱 동작을 확인 한다

```js
const Index = () => (
  <div>
    <p>Hello Next.js</p>
  </div>
);

export default Index;
```

## Navigate Between Pages

- `1-navigate-between-pages` 예제를 사용한다. 먼저 디펜던시들을 설치하고 개발 서버를 동작 시킨다.

```sh
cd 1-navigate-between-pages
npm install
npm run dev
```

- `next/link` 를 사용하여 두 개의 페이지를 연결, `pages/index.js` 의 코드를 다음으로 업데이트 한다.

```js
// import
import Link from 'next/link';

const Index = () => (
  <div>
    // 링크 추가
    <Link href="/about">
      <a>About Page</a>
    </Link>
    <p>Hello Next.js</p>
  </div>
);

export default Index;
```

## Using Shared Components

- `2-using-shared-components` 예제를 사용한다. 먼저 디펜던시들을 설치하고 개발 서버를 동작 시킨다.

- `component/Header.js` 컴포넌트를 생성한다.

```js
import Link from 'next/link';

const linkStyle = {
  marginRight: 15
};

const Header = () => (
  <div>
    <Link href="/">
      <a style={linkStyle}>Home</a>
    </Link>
    <Link href="/about">
      <a style={linkStyle}>About</a>
    </Link>
  </div>
);

export default Header;
```

- `index.js` 를 아래 처럼 바꾸고 동작을 확인한다. 반영이 안 된다면 개발서버를 재시작 한다.

```js
import Header from '../components/Header';

export default function Index() {
  return (
    <div>
      <Header />
      <p>Hello Next.js</p>
    </div>
  );
}
```

- `components/MyLayout.js` 를 생성해서 Layout 을 잡아주는 컴포넌트를 생성 한다.

```js
import Header from './Header';

const layoutStyle = {
  margin: 20,
  padding: 20,
  border: '1px solid #DDD'
};

const Layout = props => (
  <div style={layoutStyle}>
    <Header />
    {props.children}
  </div>
);

export default Layout;
```

- Layout 컴포넌트를 이용해서 `pages/index.js` 와 `pages/about.js` 페이지를 업데이트 한다.

```js
// pages/index.js

import Layout from '../components/MyLayout';

export default function Index() {
  return (
    <Layout>
      <p>Hello Next.js</p>
    </Layout>
  );
}
```

```js
// pages/about.js

import Layout from '../components/MyLayout';

export default function About() {
  return (
    <Layout>
      <p>This is the about page</p>
    </Layout>
  );
}
```

## Create Dynamic Pages

- `3-create-dynamic-pages` 예제를 사용한다. 먼저 디펜던시들을 설치하고 개발 서버를 동작 시킨다.

- `pages/index.js` 에 PostLink 를 사용 하여 블로그 링크리스트를 추가 한다.

```js
import Layout from '../components/MyLayout';
import Link from 'next/link';

const PostLink = props => (
  <li>
    <Link href={`/post?title=${props.title}`}>
      <a>{props.title}</a>
    </Link>
  </li>
);
export default function Blog() {
  return (
    <Layout>
      <h1>My Blog</h1>
      <ul>
        <PostLink title="Hello Next.js" />
        <PostLink title="Learn Next.js is awesome" />
        <PostLink title="Deploy apps with Zeit" />
      </ul>
    </Layout>
  );
}
```

- `pages/post.js` 를 생성해서 아래 코드를 추가합니다. `title` 를 `useRouter` 이용해서 query string 에 접근 합니다.

```js
import { useRouter } from 'next/router';
import Layout from '../components/MyLayout';

const Page = () => {
  const router = useRouter();

  return (
    <Layout>
      <h1>{router.query.title}</h1>
      <p>This is the blog post content.</p>
    </Layout>
  );
};

export default Page;
```

## Clean URLs with Dynamic Routing

- `4-clean-urls` 예제를 사용한다. 먼저 디펜던시들을 설치하고 개발 서버를 동작 시킨다.

- `pages/p/[id].js` 로 긴 url 을 `p/[id]` 형태로 보여 준다.

- `pages` 아래에 `p` 디렉토리를 생성하고 그 아래에 `[id].js` 파일을 생성한다. 페이지 이름 안에 `[]` 는 해당 페이지를 Dynamic Route 로 만들어 주고 라우터에 id 를 넘겨준다.

```sh
mkdir p
touch p/[id].js
```

- 아래 코드와 같이 `title` 대신 `router.query.id` 를 사용한다. 

```js
import Layout from '../../components/MyLayout';

export default function Post() {
  const router = useRouter();

  return (
    <Layout>
      <h1>{router.query.id}</h1>
      <p>This is the blog post content.</p>
    </Layout>
  );
}
```

- `pages/index.js` 의 코드를 Dynamic Route 를 사용하는 형태로 바꾸어 준다.

```js
import Layout from '../components/MyLayout';
import Link from 'next/link';

const PostLink = props => (
  <li>
    <Link href="/p/[id]" as={`/p/${props.id}`}>
      <a>{props.id}</a>
    </Link>
  </li>
);

export default function Blog() {
  return (
    <Layout>
      <h1>My Blog</h1>
      <ul>
        <PostLink id="hello-nextjs" />
        <PostLink id="learn-nextjs" />
        <PostLink id="deploy-nextjs" />
      </ul>
    </Layout>
  );
}
```

## Fetching Data for Pages

- `6-fetching-data` 예제를 사용한다. 먼저 디펜던시들을 설치하고 개발 서버를 동작 시킨다.

- 먼저 `isomorphic-unfetch` 패키지를 설치 한다.

```sh
npm install --save isomorphic-unfetch
```

- `pages/index.js` 의 코드를 베트맨 정보를 가져오는 코드로 수정한다. 페이지의 `getInitialProps` 를 사용하여 데이터를 가져오며 `getInitialProps` 는 Page 컴포넌트에서만 사용 가능 하다.


```js
import Layout from '../components/MyLayout';
import Link from 'next/link';
import fetch from 'isomorphic-unfetch';

const Index = props => (
  // props 로 넘어 온 shows 데이터를 보여준다.
  <Layout>
    <h1>Batman TV Shows</h1>
    <ul>
      {props.shows.map(show => (
        <li key={show.id}>
          <Link href="/p/[id]" as={`/p/${show.id}`}>
            <a>{show.name}</a>
          </Link>
        </li>
      ))}
    </ul>
  </Layout>
);

// 컴포넌트 렌더링 전에 데이터를 가져와서 props 로 넘겨준다.
Index.getInitialProps = async function() {
  const res = await fetch('https://api.tvmaze.com/search/shows?q=batman');
  const data = await res.json();

  console.log(`Show data fetched. Count: ${data.length}`);

  return {
    shows: data.map(entry => entry.show)
  };
};

export default Index;
```

- `pages/p/[id].js`페이지를 개별 영화의 정보를 보여주도록 아래 코드로 수정한다.

```js
import Layout from '../../components/MyLayout';
import fetch from 'isomorphic-unfetch';

const Post = props => (
  // props 로 넘어 온 show 데이터를 보여준다.
  <Layout>
    <h1>{props.show.name}</h1>
    <p>{props.show.summary.replace(/<[/]?[pb]>/g, '')}</p>
    <img src={props.show.image.medium} />
  </Layout>
);

// 컴포넌트 렌더링 전에 개별 영화 정보를 id 를 사용하여 가져 온다.
Post.getInitialProps = async function(context) {
  const { id } = context.query;
  const res = await fetch(`https://api.tvmaze.com/shows/${id}`);
  const show = await res.json();

  console.log(`Fetched show: ${show.name}`);

  return { show };
};

export default Post;
```

## Styling Components

- `7-styling-components` 예제를 사용한다. 먼저 디펜던시들을 설치하고 개발 서버를 동작 시킨다.

- `pages/index.js` 에 `styled-jsx` 를 사용해서 스타일링을 아래 코드처럼 한다. `<style jsx>{``}</style>` 문법을 사용한다

```js
import Layout from '../components/MyLayout';
import Link from 'next/link';

function getPosts() {
  return [
    { id: 'hello-nextjs', title: 'Hello Next.js' },
    { id: 'learn-nextjs', title: 'Learn Next.js is awesome' },
    { id: 'deploy-nextjs', title: 'Deploy apps with ZEIT' }
  ];
}

const PostLink = ({ post }) => (
  <li>
    <Link href="/p/[id]" as={`/p/${post.id}`}>
      <a>{post.title}</a>
    </Link>
  </li>
);

export default function Blog() {
  return (
    <Layout>
      <h1>My Blog</h1>
      <ul>
        {getPosts().map(post => (
          <PostLink key={post.id} post={post} />
        ))}
      </ul>
      <style jsx>{`
        h1,
        a {
          font-family: 'Arial';
        }

        ul {
          padding: 0;
        }

        li {
          list-style: none;
          margin: 5px 0;
        }

        a {
          text-decoration: none;
          color: blue;
        }

        a:hover {
          opacity: 0.6;
        }
      `}</style>
    </Layout>
  );
}
```

- `<li>` 에 스타일이 적용 되기 위해서는 `PostLink` 에 스타일을 적용해 주어야 한다. `<li>` 와 `<a>` 의 스타일을 옮긴다.

```js
const PostLink = ({ post }) => (
  <li>
    <Link href="/p/[id]" as={`/p/${post.id}`}>
      <a>{post.title}</a>
    </Link>
    <style jsx>{`
      li {
        list-style: none;
        margin: 5px 0;
      }

      a {
        text-decoration: none;
        color: blue;
        font-family: 'Arial';
      }

      a:hover {
        opacity: 0.6;
      }
    `}</style>
  </li>
);
```

- Post 에 Markdown 을 사용하기 위해서 먼저 `react-markdown` 을 설치 한다.

```sh
npm install --save react-markdown
```

- `pages/p/[id].js` 의 코드를 아래 처럼 업데이트 한다. `<style jsx global>{``}</style>` 문법을 사용하여 글로벌 스타일링을 적용 한다.

```js
import { useRouter } from 'next/router';
import Markdown from 'react-markdown';
import Layout from '../../components/MyLayout';

export default () => {
  const router = useRouter();
  return (
    <Layout>
      <h1>{router.query.id}</h1>
      <div className="markdown">
        <Markdown
          source={`
This is our blog post.
Yes. We can have a [link](/link).
And we can have a title as well.

### This is a title

And here's the content.
      `}
        />
      </div>
      <style jsx global>{`
        .markdown {
          font-family: 'Arial';
        }

        .markdown a {
          text-decoration: none;
          color: blue;
        }

        .markdown a:hover {
          opacity: 0.6;
        }

        .markdown h3 {
          margin: 0;
          padding: 0;
          text-transform: uppercase;
        }
      `}</style>
    </Layout>
  );
};
```

## Deploying a Next.js App

- [▲ZEIT Now](https://zeit.co/home) 는 간편하게 확장성 있는 앱을 배포 할 수 있는 플랫폼 입니다. 먼저 [다운로드](https://8-deploying.ragingwind.now.sh)를 합니다.

```sh
npm i -g now
```

- 그리고 `8-deploying` 경로에서 다음 명령을 입력하고 결과로 나오는 URL 에 접속한다.

```sh
> now                
> Deploying next-learn-demo/8-deploying under your-accound
> Using project 8-deploying
> https://8-deploying-fi0355fhm.now.sh [2s]
> Ready! Deployed to https://8-deploying.your-accound.now.sh [in clipboard] [27s]
```
