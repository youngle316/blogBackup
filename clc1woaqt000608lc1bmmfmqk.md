# Ant Design Pro 如何自定义配置额外 Webpack 插件

最近需要完成一个需求：血缘图的展示，在 `Github` 上找到了一个开箱即用的项目 [react-lineage-dag](https://github.com/aliyun/react-lineage-dag)，是阿里出的一个用于表示表与表之间，表和其他关联实体之间关系的图。

测试了一下 `demo`，也都符合预期，但是当使用从 `npm` 库中下载下来的包时，编译时报错了，说不认识包中的 `$`，意识到是使用了 `jQuery`了。

解决这个问题也很简单

1. 安装 `jquery`
```shell
npm install jquery --save
```
2. 使用 `ProvidePlugin`

`ProvidePlugin`是一个 `webpack` 插件，用于自动加载某一模块，当一个项目很多地方都使用同一模块时，这个插件可以省去多个地方写 `import/require`。

可以在 webpack 配置文件中 plugin 字段中添加：

```javascript
plugins: [
	new webpack.ProvidePlugin({
		$: 'jquery',
		jQuery: 'jquery',
		'window.jQuery': 'jquery',
		'window.$': 'jquery',
	});
]
```

表示的意思是：当打包时遇到不能识别的`$`、`jQuery`、`window.jQuery`、`window.$`，`webpack`就自动去加载`jquery`模块。

但是如何在`Ant Design Pro`中去使用呢，由于使用了`umi`对`webpack`进行了封装，在`Ant Design Pro`中暴露的位置是 `config/config -> chainWebpack字段中`。具体的用法可以看[chainWebpack官方文档](https://umijs.org/docs/api/config#chainwebpack)

所以只需要在chainWebpack字段中添加如下代码

```javascript
chainWebpack(config, { env, webpack }) {

	config

		.plugin('providerPlugin')

		.use(webpack.ProvidePlugin, [{

			$: 'jquery',

			jQuery: 'jquery',

			'window.jQuery': 'jquery',

			'window.$': 'jquery',

		}]
	)
},
```

配置其他插件，或对`webpack`进行其他配置优化等也可使用相同的方法