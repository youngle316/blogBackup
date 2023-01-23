# 从0实现React18系列一—搭建项目架构

> 本系列项目是记录 **<mark>从0实现React18</mark>**。通过开源项目 [big-react](https://github.com/BetaSu/big-react) 对应的视频教程 [从0实现React18](https://appjiz2zqrn2142.h5.xiaoeknow.com/v1/goods/goods_detail/p_638035c1e4b07b05581d25db?type=3&channel_id=1085791) 进行学习。

[项目地址](https://github.com/youngle316/Big-React)

[章节地址](https://github.com/youngle316/Big-React/commit/b6d0227e9481a6baed9ad1040bb515d5c227cf5d)

第一章主要是搭建项目架构，主要包括

1. 定义项目结构（`monorepo`）
    
2. 定义开发规范（`lint`，`prettier`，`commit`，`tsc`等）
    
3. 选择打包工具
    

## 定义项目结构

### 该选择 `monorepo` 还是 `mutirepo`

本项目采用 `monorepo` 来定义项目结构，那什么是 `monorepo` 呢？

如 `React`、`Vue3` 等项目都是采用的 `monorepo` 的形式来定义项目结构。它其实就是将所有的服务保存在单一的存储库中，仍然可以独立的部署和管理每一个服务，这些服务可以共享公共库和代码。

一般的业务代码会使用 `mutirepo` 的形式，而这种工具类的库会使用 `monorepo`。

### `Monorepo` 的技术选型

`monorepo` 推荐使用 `yarn` 或 `pnpm` 来进行包管理。我们这里用 `pnpm`

### `pnpm` 的初始化

[安装](https://pnpm.io/installation)：可以使用其中的任意一种方法来下载 `pnpm`

```shell
npm install -g pnpm

// 项目根目录下初始化项目
pnpm init
```

### 初始化 `pnpm-workspace.yaml`

项目根目录下创建 `pnpm-workspace.yaml`

```yaml
packages:  
# 在 packages 文件夹下的都是子目录
- 'packages/*'
```

## 定义开发规范

### `lint`

使用 `eslint` 进行代码规范检查与修复

### `prettier`

使用 `prettier` 进行代码样式风格检查

### `commit` 规范检查

使用 `husky` 用于拦截 `commit` 命令。使用 `commitlint` 对 `git` 提交信息进行检查，并将其整理到 `husky` 中。

### `tsc`

配置 `tsconfig.json` 进行 `tsc` 的处理。

## 选择打包工具

由于我们开发的是库，不是业务项目，希望工具尽可能的简洁、打包产物可读性高。常用的 `webpack` 是大而全，不太适用，**所以选择** `rollup`。

第一章大概就是这些，先把项目架子搭好，使用的都是很熟悉的工具。