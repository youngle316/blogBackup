# NextJS Tutorial —Part 2

上一部分介绍了 文件结构、路由、导航等。这一部分只介绍预渲染、预渲染的两种方式

# Pre-rendering

这一部分将要介绍 `Pre-rendering`（预渲染）

`NextJS` 会预渲染每一个页面，也就是说 `NextJS` 会为每一个页面提前生成 `HTML` ，而不是从客户端 `JavaScript` 中获取。这样做的优势有：

1.  提高性能，减少白屏时间
    
2.  有助于 `SEO`
    

我们可以将 `create-react-app` 脚手架和 `NextJS` 进行比较，看看预渲染到底是什么

*   使用 `create-react-app` 和 `npx create-next-app@latest` 分别创建两个项目
    

`React` **项目**

首先我们先使用 `npm run start` 运行 `react` 项目，运行成功后，可以看到如下的很熟悉的界面

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877930003/0b8R6ezAQ.png align="left")

页面中元素都在根节点 root 下，然而当看源码时，就会发现只有一个空的根节点 root ，其他的内容都是在 js 脚本中，这样就会导致一个问题，首先一个空的 HTML 先展示出来，再去请求 js 文件，这其中如果有网络等其他因素的干扰，这个白屏的时间就会比较长，并且没有内容，对于 SEO 也不友好

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877945831/0d7ta5rnd.png align="left")

`NextJS` **项目**

那么 `NextJS` 项目又会是怎么样的呢

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877959959/_byC8gIpL.png align="left")

我们可以看到同 React 项目一样，都是存在于根节点下面，不过看源码的话就会又不一样的地方了

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877971883/m3ddrQ4mY.png align="left")

可以看到页面中的内容已经全部存在于 `HTML` 中了，不需要加载额外的 `JS` 再去挂载 `DOM` 节点

那么预渲染有两种方式

## Static Generation

### what is static generation

`static generation` 是在构建时生成 `HTML` 页面的预渲染方法

当你在构建你的应用时，包含构成网页内容的所有数据的 `HTML` 是预先生成的

在任何可能需要预渲染得情况下推荐使用 `static generation`

网页可以构建一次，然后被缓存在CDN中，并且可以非常快速的提供给客户端

### When to use static generation

例如在 blog、电商产品页、文档和营销页面都可以使用

### How to use static generation

`NextJS` 会默认预渲染项目中的每一个页面

当我们构建应用时，每个页面的 `HTML` 将自动静态生成

`static generation` 的预渲染包含不需要额外请求数据和需要额外请求数据。如用 `create-next-app` 新建的项目中，`index.ts` 预渲染出来的 `HTML` 就是没有额外的请求数据。如果我们需要请求额外的数据并将数据同时进行预渲染的话，可以使用 `getStaticProps`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877993818/ffGgJ87Ln.png align="left")

### static generation with getStaticProps

[getStaticProps](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) 是一个方法， 当我们导出了一个方法叫做 `getStaticProps` 时，`NextJS` 就会在预渲染该页面的时候，将这个方法返回值当做 `props` 使用

```javascript
// pages -> users.tsx
import React from "react";

export default function Users({ userList }) {
  console.log("userList", userList);
  return (
    <>
      {userList.map((user) => {
        return <h1 key={user.id}>{user.name}</h1>;
      })}
    </>
  );
}

export async function getStaticProps() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const data = await response.json();
  console.log("data", data);
  return {
    props: {
      userList: data,
    },
  };
}
```

在 pages 目录下新建一个 user.tsx 文件，该文件中导出了一个方法叫做 getStaticProps ，并且返回了一个对象，返回的 userList 可以直接当做该组件的 props 使用

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669878005526/oLnIOEALs.png align="left")

可以看到源码中已经预渲染了 `getStaticProps` 异步请求的数据。需要注意的是打印的 `data` 会显示在 `terminal` 中，并不会在 `console` 中打印

有以下注意的点📢

1.  `getStaticProps` 方法只会在服务端运行，不会在客户端运行，写在该方法中的代码不会被打包到 `JS bundle` 中
    
2.  可以直接在 `getStaticProps` 中写服务端代码，比如文件系统的 `fs` 或者查询数据库的代码。不需要担心在该方法中存在敏感信息，不会被发送到浏览器中
    
3.  只能在 `pages` 目录下的文件中书写，不能在其他文件中运行。只能用作于预渲染
    
4.  `getStaticProps` 必须返回一个对象，这个对象必须包含一个 `key` 为 `props` 的对象
    
5.  `getStaticProps` 会运行在构建的时候
    

### Running static generation builds

`NextJS` 的预渲染有一个比较高级的地方，接着上一节，比如我们在 pages → index.tsx 根路由下通过 `Link` 导航到了一个 `Users` 。

```javascript
// index.tsx
import React from "react";
import Link from "next/link";

export default function Home() {
  return (
    <>
      <h1>NextJS pre-rendering</h1>
      <Link href="/users">users</Link>
    </>
  );
}
```

这时我们运行 npm run build 后再 npm run start。我们可以看到 users 中的内容已经提前被请求到了，无论是文件还是 getStaticProps 中的数据。

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669878022131/ZgNvOnzVJ.png align="left")

需要注意的是如果使用 `router` 进行跳转或者没有跳转代码时，是不会提前请求 `users` 的内容

那如果 `users` 中还有 `Link` 呢，会不会也提前请求。答案是不会的，只有进入 `users` 中才会请求其中关联的 `Link` 组件

### SSG with Dynamic Parameters

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669878034998/BL0M5juG5.png align="left")

如图我们要做这么一个功能，左侧显示的是博客列表页，右侧显示的是博客详情页，我们要通过路由博客ID去显示对应的博客内容

按照我们之前的写法，创建如下两个文件即可实现该功能

```javascript
// pages -> posts -> index.tsx
import Link from "next/link";
import React from "react";

export default function PostList({ data }) {
  return (
    <>
      <h1>Post List</h1>
      {data.map((item) => {
        return (
          <div key={item.id}>
            <Link href={`/posts/${item.id}`}>
              <h2>
                {item.id} {item.title}
              </h2>
            </Link>
          </div>
        );
      })}
    </>
  );
}

export async function getStaticProps() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts");
  const data = await response.json();

  return {
    props: {
      data,
    },
  };
}
```

```javascript
// pages -> posts -> [postId].tsx
import React from "react";

export default function PostDetail({ postDetail }) {
  return (
    <>
      <h2>
        {postDetail.id} {postDetail.title}
      </h2>
      <p>{postDetail.body}</p>
    </>
  );
}

export async function getStaticProps( context ) {
  const { params } = context;
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/blog/${params.postId}`
  );
  const data = await response.json();
  return {
    props: {
      postDetail: data,
    },
  };
}
```

但是，nextJS 报了这样一个错

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669878051733/M_E3vjlL0.png align="left")

提示动态的 SSG 页面需要 `getStaticPaths` ，那么 这个方法是个什么东西呢并且为什么会报这么一个错呢

### SSG with getStaticPaths

[getStaticPaths](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths)：当使用了动态路由和 `getStaticProps` 在你的页面时，就需要使用到该方法。

**为什么需要？**

在这个例子中，我们使用动态路由并且要预渲染页面 需要使用 `postId` 这个参数，但是 nextJS 不知道这个 `postId` 的值可以是多少。0？1？100？webpack? nextJS? 还是别的什么值，无法进行预渲染，所以就需要 `getStaticProps` 这个方法告诉 `nextJS` 这个值应该是什么。

```javascript
// [postId].tsx
export async function getStaticPaths() {
  return {
    paths: [
      { params: { postId: "1" } },
      { params: { postId: "2" } },
      { params: { postId: "3" } },
    ],
    fallback: false,
  };
}
```

只需要在`getStaticProps`的文件中添加上面的代码，即可解决报错问题。其中`Paths`、`params`、`fallback`这三个字段是必备的。上面这段代码告诉 NextJS 需要将 `postId` 为 1、2、3的进行预渲染。

**注意：**`postId`的值必须是 `string` 类型的

### Inspecting getStaticPaths Builds

添加了上面的代码之后，运行 `npm run build`，可以看到 `/posts` 和 `/posts/[postId]` 是SSG 的预渲染，并且是按照 `getStaticPaths` 传参进行的预渲染。

![Image.png](https://res.craft.do/user/full/e3a51d3a-b71b-18ee-2829-5fcba515b69e/doc/87E44E6B-FB4E-4316-A877-D06E3856E404/ECFEB54F-6A85-42F5-BFDC-DB8426CB4F95_2/yF2ll4vXZVqTBBy1ozb9zLBXLya2AIS11NmXHRLCS8sz/Image.png align="left")

通过打包后的文件夹也可以看到，`postId` 为1、2、3的已经生成的`HTML`和对应的`JSON` 文件

![Image.png](https://res.craft.do/user/full/e3a51d3a-b71b-18ee-2829-5fcba515b69e/doc/87E44E6B-FB4E-4316-A877-D06E3856E404/19E19912-C475-4F3F-A79F-08F3B050A5BD_2/qxAbxA3SBKtommRH3xWGZyCHbUTk9EVxE7Bv7i1Hy64z/Image.png align="left")

### Fetching Paths for getStaticPaths

在上面例子中硬编码`postId` 为1、2、3。这在实际项目中是不可能这么写的。所以我们需要提前获取到需要预渲染的所有的`postId`，再将这些postId组装成数组传给`paths`

```javascript
export async function getStaticPaths() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts");
  const data = await response.json();
  const paths = data.map((item) => {
    return { params: { postId: `${item.id}` } };
  });

  return {
    paths,
    fallback: false,
  };
}
```

再次执行 `npm run build` 后，就会预渲染所有的 `post`

![Image.png](https://res.craft.do/user/full/e3a51d3a-b71b-18ee-2829-5fcba515b69e/doc/87E44E6B-FB4E-4316-A877-D06E3856E404/3FBD9213-9BB6-4060-A159-E3CB5A63A652_2/g0VQ3EBsvtofqOyQqJJis0UVZpziN6TrJZDMyDkbrO4z/Image.png align="left")

### getStaticPaths fallback false

`getStaticPaths` 方法返回的 `fallback` 可以赋三个值：`false`、`true`、`blocking`

这一部分讲 `false` 的情况。

当 `fallback` 的值为 `false` 时：

1.  从 `getStaticPaths` return 的 `paths` 将在构建时由 `getStaticProps` 渲染为 `HTML`
    
2.  如果 `fallback` 设置为 `false`， 只要没有在 `Paths` 中存在的路径，都会跳转到404页面
    

什么时候会用到 `false` 呢

*   当应用只有少量的预渲染需求
    
*   不经常添加新页面
    
*   比如博客站点等都是设置为 `false` 的很好的例子
    

### getStaticPaths fallback true

```javascript
import React from "react";
import { useRouter } from "next/router";

export default function PostDetail({ postDetail }) {
  const router = useRouter();
  if (router.isFallback) {
    return <h1>Loading</h1>;
  }

  return (
    <>
      <h2>
        {postDetail.id} {postDetail.title}
      </h2>
      <p>{postDetail.body}</p>
    </>
  );
}

export async function getStaticPaths() {
  return {
    paths: [
      { params: { postId: "1" } },
      { params: { postId: "2" } },
      { params: { postId: "3" } },
    ],
    fallback: true,
  };
}

export async function getStaticProps(context) {
  const { params } = context;
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/posts/${params.postId}`
  );
  const data = await response.json();
  return {
    props: {
      postDetail: data,
    },
  };
}
```

1.  从 `getStaticPaths` return 的 `paths` 将在构建时由 `getStaticProps` 渲染为 `HTML`
    
2.  没有在 `Paths` 中存在的路径不会跳转到404页面。但是需要提前创建一个页面也好还是组件也好，给 `fallback` 使用，否则 `build` 的时候是会报错的。
    
3.  当导航到不存在于 `Paths` 中的路径时，首先会展示在上一步创建的组件，NextJS 会在背后静态生成请求路径的 `HTML` 和 `JSON`。
    
4.  完成后，浏览器会收到生成路径的 `HTML` 和 `JSON`。用于展示到页面上，用户的感受就是在后台请求的数据然后展示了出来。
    
5.  上一步请求出来的 HTML 和 JSON 会同步更新到服务器上。再下次再次跳转到该路径时就相当于是预渲染了。
    
6.  如果导航到了一个非法的路径，如不存在的Id等，则会跳转到404页面。
    

### getStaticPaths fallback blocking

设置为 `blocking` 与 `true` 的区别是，当跳转到未定义在 `Paths` 中的路径时，设置为 `blocking` 时 tab 页签是一个自动 `loading` 加载的效果，页面没有变化；而设置为 `true` 时，是需要手动添加一个等待的页面或者组件的。

## Incremental Static Regeneration

`static generation` 优点有很多，`SEO`、可以`CDN`加速、方便使用。但是同样也存在问题

1.  构建时间与应用中的页数成正比
    
2.  一个页面一旦预渲染生成成功，再下一次重新构建之前可能包含过时的数据
    

也就是已经预渲染成功的页面，当后台数据发生变化的时候，页面上展示的数据是不会变化的，会有过时的数据，那么重新 `build` 再上传到服务器也不现实，如果数据变化的间隔过快的话。

这时就要请出另外一个参数 `revalidate` ，在 `getStaticProps` 方法返回的对象中添加这个 key，它的值为 `number` 类型，单位是 `s`

```javascript
export async function getStaticProps() {
  const response = await fetch("http://localhost:4000/production");
  const data = await response.json();
  return {
    props: {
      productData: data,
    },
    revalidate: 30,
  };
}
```

下图演示了 `revalidate` 的工作流程

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669949570250/chXZPHHgP.png align="left")

1.  首次打包完成后，一个用户访问了 products 页面，展示的是默认的HTML
    
2.  用户在30s之内再次访问 products 页面，展示还是之前默认的HTML
    
3.  用户在30s之后访问 products 页面，会触发页面重新生成，生成新的HTML。但是页面上展示的还是旧的HTML
    
4.  再次访问 products 页面，如果上一步重新构建成功的话会展示新的HTML
    

`Page Regeneration` 需要注意以下几点

1.  `Page Regeneration`有两个条件
    
    1.  用户触发
        
    2.  在 `revalidate` 时间之后
        
2.  `revalidate` <mark>不是</mark>指页面会自动重新构建
    
3.  `Page Regeneration` 也可能失败，在成功之前会使用缓存的HTML
    

## Server Side Rendering

服务端渲染是第二种预渲染的方式。

先看看 `Static Generation` 有什么问题

**不能在请求时获取数据**

`Static Generation` 就是给数据变化不多，对 SEO 要求高的网页可以使用这种，如果新闻网站使用静态生成，但是每一秒都有新的新闻产生，虽然说可以使用 `ISR`、 `getStaticPaths` 来做。但是仍然是有问题的，数据总归有时候不是最新的。

**不能获取权限**

当网站需要用户的信息去展示相对应的数据时，比如微博、知乎等。`Static Generation` 获取不到用户信息，我们可以直接在客户端使用 useEffect 等方法，但是这样 `SEO` 就又不好了。

所以就有了 `Server Side Rendering` ，可以在请求时进行预渲染而不是构建时。可以在每次请求时或者去获取个人数据时同时又不影响 `SEO`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669967043166/ByMr5zWk6.png align="left")

### SSR with getServerSideProps

`getServerSideProps` 的写法和 `getStaticProps` 一致

```javascript
import React from "react";

export default function News({ newsList }) {
  return (
    <>
      <h1>List of the news</h1>
      {newsList.map((item) => {
        return (
          <div key={item.id}>
            <h2>
              {item.id} {item.title} | {item.category}
            </h2>
          </div>
        );
      })}
    </>
  );
}

export async function getServerSideProps() {
  const response = await fetch("http://localhost:4000/news");
  const data = await response.json();

  return {
    props: {
      newsList: data,
    },
  };
}
```

1.  该方法只会运行在服务端，客户端不会运行该方法
    
2.  该方法中的内容不会被打包到 `bundle` 中
    
3.  可以在该方法中直接写服务端的代码
    
4.  该方法只允许写在 `page` 目录下
    

### SSR with Dynamic Parameters

```javascript
// pages -> news -> [categoryId].tsx
import React from "react";

export default function NewsListByCategory({ newsList }) {
  return (
    <>
      <h1>News List By Category</h1>
      {newsList.map((item) => {
        return (
          <div key={item.id}>
            <h2>
              {item.id} {item.title} | {item.category}
            </h2>
          </div>
        );
      })}
    </>
  );
}

export async function getServerSideProps(context) {
  const { params } = context;
  const response = await fetch(
    `http://localhost:4000/news?category=${params.categoryId}`
  );
  const data = await response.json();

  return {
    props: {
      newsList: data,
    },
  };
}
```

`getServerSideProps` 的参数 [context](https://nextjs.org/docs/api-reference/data-fetching/get-server-side-props#context-parameter) 中包含的值有很多

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670135845849/DoD64neHS.png align="left")

## Client-side Data Fetching

很多情况下是不需要去预渲染的，可以通过客户端去请求。最常用的就是在生命周期或 `useEffect` 方法中去获取数据进行渲染

客户端渲染也可以使用 [SWR](https://swr.vercel.app/zh-CN) 进行优化

内容及图片均来自 Youtube [CodeVolution](https://www.youtube.com/@Codevolution/featured)