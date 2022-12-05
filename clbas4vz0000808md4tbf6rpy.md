# Next.js Tutorial — Part 1

> The React Framework for production — Next.js Docs

第一次了解到 `Next.js` 是在开源项目 [nextjs-notion-starter-kit](https://github.com/transitive-bullshit/nextjs-notion-starter-kit)，同步也是用这个项目搭建了一个基于 `notion` 的博客平台并且部署到了 [vercel](https://vercel.com) 上。以及最近经常看到 `Youtube` 上很多前端的项目都是基于 `next.js` 做的。所以有必要学习一下这个框架。 通过 [Next.js](https://nextjs.org/docs/getting-started) 官方文档的阅读，可以看到其中有很多的特性，内容丰富、功能强大，简化优化开发的过程。文章包括以下部分：

*   Project Structure
    
*   Router
    
*   Navigation
    
*   Pre-rendering
    
*   Data Fetching
    
*   API Route
    
*   Styling
    
*   Image
    
*   Typescript
    

# What is Next.js

## The React Framework for production

React

*   不太可能创建一个功能丰富的应用程序，用来部署到生产环境上
    
*   React 是一个用于构建用户界面的库
    
*   需要去为应用的其他特性如路由、样式、权限等再去添加一些额外的内容
    

Next.js

*   一个使用 React 构建用户界面的包
    
*   自动加载了很多的特性帮助你构建一个丰富的应用程序。特性包含路由、样式、权限、`bundle`的优化等
    
*   不需要安装额外的包，`Next.js`提供了一切
    
*   为了实现上述特性，需要遵循一些意见和惯例
    

## Why learn Next.js

`Next.js` 简化了构建一个生产环境的 `React` 应用

1.  `File based routing` - 基于文件的路由
    
2.  `Pre-rendering` - 预渲染
    
3.  `API` 路由
    
4.  支持 `CSS modules`
    
5.  `Authentication` - 认证
    
6.  `Dev and Prod build system` - 开发和生产构建系统
    

# Install Next.js App

可以使用下面的命令安装 `Next.js` 应用

```bash
npx create-next-app@latest --typescript
```

或者查看[官方文档](https://nextjs.org/docs/getting-started)其他方式

# Project Structure

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877757441/mej53bjxu.png align="left")

需注意的是在 Next.js 中 pages 文件夹下的文件会自动生成路由，这部分会在下面介绍到。项目其他目录结构与别的脚手架没有什么大的不同

# Route

在 React 应用中使用路由

*   需要安装第三方包
    
*   需要手动写文件去配置路由
    
*   每一个路由、每一个组件、都需要去配置路由的属性，比较麻烦
    

在 Next.js 应用中使用路由

*   基于文件系统的路由机制
    
*   当在项目中的 `pages` 目录下添加了一个文件，就会自动的变成一个可用的路由
    
*   通过将文件名和嵌套文件夹结构混合和匹配，几乎可以定义最常见的路由模式
    

## Routing with pages

`NextJS` 有个一个基于文件系统的路由器，建立在 pages 文件夹的概念上

当一个文件被添加到 `pages` 目录下时，就会自动相对应的生成可使用的路由

### Scenario 1 root router

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877785524/p2wV0ZJoA.png align="left")

只需将 pages → index.tsx 中的内容修改为 Home Page 即可

### Scenario 2 basic router

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877801434/zkT4_7Oh7.png align="left")

实现方式有两种

*   在 `pages` 目录下创建 `about.tsx` 和 `profile.tsx`
    
*   在 `pages` 目录下创建 `about` 和 `profile` 文件夹；再在两个文件夹中分别创建 `index.tsx`
    

❗需要注意的是区分大小写

### Scenario 3 Nested Routes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877810178/rfBkRmsgX.png align="left")

嵌套路由也就是文件夹的嵌套，实现方式还是两种，同上

在 `pages` 目录下先创建 `blog` 目录

`first` 和 `second` 可以是文件夹其中包含 `index.tsx` 文件；也可以是 `first.tsx` 和 `second.tsx`

### Scenario 4 Dynamic Routes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877822506/Ff0FoxQKN.png align="left")

存在 `product` 列表页和 `Product` 详情页，地址栏中的 `id` 是会发生变化的，是动态的

首先实现 `product` 列表页很简单，在 `pages` 目录下创建 `product` 目录 → `index.tsx` 文件

**重点**：动态 `id` 只需要将文件名用中括号包裹起来即可，如 `[productId].tsx` 。在 `product` 目录下创建 `[productId].tsx` 文件，然后在地址栏中输入 `http://localhost:3000/product/1` ，就会出现 `[productId].tsx` 组件中的内容。

组价内容根据 `productId` 动态展示的话可以通过 `useRouter` 获取路由信息

```javascript
import React from "react";
import { useRouter } from "next/router";

export default function ProductDetail() {
  const { query } = useRouter();
  const productId = query.productId;
  return (
    <>
      <h1>ProductDetail {productId} Page</h1>
    </>
  );
}
```

**额外**：如果在 `product` 目录下存在了 `sweater.tsx` 文件，地址栏输入的是`http://localhost:3000/product/sweater`，那么展示的内容是哪个组件里的呢，答案是`sweater.tsx`

### Scenario 5 Nested Dynamic Routes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877859845/j9fwiEfgu.png align="left")

这个场景就是 productId 是动态的，并且 reviewId 也是动态的

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877873728/6Kp3POLiO.png align="left")

*   在 `product` 目录下创建 `[productId]` 目录
    
*   `[productId]` 目录下创建 `review` 目录
    
*   `review` 目录下创建动态的 `reviewId` 文件
    

```javascript
// index.tsx 就是上一个场景中的 [productId].tsx的内容

// [reviewId].tsx
import React from "react";
import { useRouter } from "next/router";

export default function ReviewDetail() {
  const { query } = useRouter();
  const { productId, reviewId } = query;
  return (
    <>
      <h1>
        Review {reviewId} for product {productId}
      </h1>
    </>
  );
}
```

### Scenario 6 Catch all routes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877889308/G6n81hRPA.png align="left")

如图有这样的文档页面，比如有文档中有20个特性，每个特性又有20个概念，那么就会有400个路由，这种该怎么实现呢。

如果要获取地址栏 `docs` 后面的内容，不论后面的内容嵌套了多少层，只需要创建 `[…params].tsx`即可。此时通过 `useRouter` 获取参数时会返回一个对象，对象的值是数组。而非之前单个的字符串了。

```javascript
import React from "react";
import { useRouter } from "next/router";

export default function DocsDetail() {
  const { query } = useRouter();
  const { params } = query;
  return (
    <>
      {params &&
        Array.isArray(params) &&
        params.map((item) => {
          return <h1 key={item}>{item}</h1>;
        })}
      <h1>DocsDetail Page</h1>
    </>
  );
}
```

此时如果地址栏是`http://localhost:3000/docs`时，由于没有在 `docs` 目录下创建 `index.tsx` 文件，页面会报 404。修改这个问题有两种方法：

1.  添加 `index.tsx` 文件
    
2.  将 `[…params].tsx` 修改为 `[[…params]].tsx` 即可
    

## Navigation

路由有了，就需要在路由之间的跳转。那么跳转的方式有两种

1.  使用 `Link` 组件
    
2.  使用 `useRouter`
    

### Link Component Navigation

使用 `NextJS` 提供的 `Link` 组件，可以很方便的跳转

```javascript
import Link from "next/link";

<Link href="/blog">Blog Page</Link>
```

`Link` 组件中传入 `href` 属性，值为对应的路由地址

**注意**：在新版本中不要在 `Link` 组件中添加 `a` 标签，具体查看[官方文档](https://nextjs.org/docs/messages/invalid-new-link-with-extra-anchor)

`Link` 中还可以传 `replace` 属性 ，replace 会替换当前浏览器的历史状态而不是再添加一个新的 `url`

### Navigating Programmatically

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669877910569/Oujg28gnh.png align="left")

如图我们需要点击 `PlaceOrder` 按钮，跳转到 `product` 页面

这种情况下就需要用到 `useRouter` ，而不是 `Link` 组件了

```javascript
import Link from "next/link";
import { useRouter } from "next/router";

export default function Home() {
  const router = useRouter();

  const handleGoProduct = () => {
    router.push("/product");
  };

  return (
    <>
      <div>
        <button onClick={handleGoProduct}>Place Order</button>
      </div>
    </>
  );
}
```

同时 `router` 也有 `replace` 方法，效果与 `Link` 中的 `replace` 相同

## Custom 404 Page

路由总会有不匹配的时候，`NextJS` 给我们提供一个 `404` 页面，但是如果我们想自定义 `404` 页面时，只需要在 `pages` 目录下创建一个 `404.tsx` 组件

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

但是，nextJS 报了这样一个错

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669878051733/M_E3vjlL0.png align="left")

提示动态的 SSG 页面需要 `getStaticPaths` ，那么 这个方法是个什么东西呢并且为什么会报这么一个错呢

### SSG with getStaticPaths

[getStaticPaths](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths)：当使用了动态路由和 `getStaticProps` 在你的页面时，就需要使用到该方法。

**为什么需要？**

### Inspecting getStaticPaths Builds

![Image.png](https://res.craft.do/user/full/e3a51d3a-b71b-18ee-2829-5fcba515b69e/doc/87E44E6B-FB4E-4316-A877-D06E3856E404/ECFEB54F-6A85-42F5-BFDC-DB8426CB4F95_2/yF2ll4vXZVqTBBy1ozb9zLBXLya2AIS11NmXHRLCS8sz/Image.png align="left")
![Image.png](https://res.craft.do/user/full/e3a51d3a-b71b-18ee-2829-5fcba515b69e/doc/87E44E6B-FB4E-4316-A877-D06E3856E404/19E19912-C475-4F3F-A79F-08F3B050A5BD_2/qxAbxA3SBKtommRH3xWGZyCHbUTk9EVxE7Bv7i1Hy64z/Image.png align="left")

### Fetching Paths for getStaticPaths

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

以上就是 NextJS 教程第一部分，内容很多。

图片和内容来自 [Youtube Codevolution](https://www.youtube.com/@Codevolution) 博主出的 [NextJS Turorial](https://www.youtube.com/watch?v=9P8mASSREYM&list=PLC3y8-rFHvwgC9mj0qv972IO5DmD-H0ZH)