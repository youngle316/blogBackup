# NextJS Tutorial —Part 1

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