# The way to resolve - can not found node:process

## 问题

最近在跟着卡颂大佬做从**0实现React18**项目，项目使用的包管理工具是 `pnpm`，在用`rollup`打包时，会提示 `can not found node:process` 导致打包失败

## 解决过程

1. 感觉是 `node_modules` 中的依赖下载的有问题，删除 `node_modules` 和 `pnpm-lock.yaml`，重新下载依赖，依然不起作用
    
2. 在 `Google` 和 `stackoverflow` 中搜不出来，下载了 `rollup` 的 `plugin`：[@rollup/plugin-node-resolve](https://www.npmjs.com/package/@rollup/plugin-node-resolve)，同样也不生效。
    
3. 这是感觉就是 `node` 的版本问题了，当时 `node` 使用的版本是`14.17.0`，但是看了官网文档也没有提示。因为能正常安装依赖，没感觉 `node` 版本会有问题。
    
4. 甚至还在项目中下载了 `@types/node`，也不起作用
    
5. 这时就把目光放到 `pnpm` 上了，因为之前没有使用过 `pnpm`，看官网是完全支持 `v14` 版本的，但是想着验证一下，由于是通过 `Mono-repo` 管理项目的，比较推荐使用 `yarn` 和 `pnpm` 来管理包，当用 `yarn` 安装依赖时终于出现了错误原因：`rollup3` **版本以上需要使用** `node` **版本大于等于**`v14.18.0`， `pnpm` 就是不提示。
    
6. 只需要升级 `node` 版本就行了
    

## 解决方法

我是通过 `nvm` 管理 `node` 的版本（也建议这样使用，项目一多切换项目的时候不肯能一直重新安装吧）

1. 通过 nvm 安装指定 node 版本
    

```shell
nvm install v16.18.0
```

1. 切换 node 版本
    

```shell
nvm use v16.18.0
```

1. 查看 node 版本
    

```shell
node -v
```

重启`vscode`后，这样应该就可以了，不过在这还是遇到了坑。在 `vscode` 中查看 `node`版本还是之前的旧版本，还需要执行`nvm alias default v16.18.0`这样就可以了。

就这问题了让我看了一下午，赶紧记录一下