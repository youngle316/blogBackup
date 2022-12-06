# test hashnode problem

test
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