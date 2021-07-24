---
title: webpack
date: 2021-07-24 14:50:31
tags: webpack
categories: webpack
---

# 基本配置

1、拆分配置和merge

smart：合并webpack配置

```
const { smart } = require('webpack')
```

分别在dev和prod中导入base config，导出合并后的config

```
// webpack.base.config.js 
// webpack.dev.config.js 
// webpack.prod.config.js 
```

2、启用本地服务 dev server

```
//npm install webpack-dev-server
// package.json
script: {
	dev: 'webpack-dev-server --config webpack.dev.config.js'
}
```

```
// webpack.dev.config.js
devServer:{
	port: 8080,
	progress: true, //显示进度条
	contentBase: distPath, //根目录
	open: true, //自动打开浏览器
	compress: true, //启动gzip压缩
	//设置代理
	proxy: {
	  //将匹配到/api/xxx的请求代理到http://localhost:3000/api/xxx
		'/api':'http://localhost:3000'，
		//将匹配到/api2/xxx的请求代理到http://localhost:3000/xxx
		'/api2':{
				target: 'http://localhost:3000',
				pathRewrite: {
				'/api2':'/'
				}
		}
	}
}
```

注意：webpack只能处理js和json文件

3、使用babel-loader处理ES6

需要配置babel

```
//webpack.base.config.js
module: {
	rules: [
		{
			test: /\.js/,
			loader: ['babel-loader'],
			include: srcPath,
			exclude: /node_modules/
		}
	]
}
```

4、处理css

loader的执行顺序是从后往前

- style-loader：将css代码挂载到html页面的style标签当中
- css-loader：解析css文件，把css模块加载到JS代码中，并不会使用这个模块
- postcss-loader：可以增加浏览器适配前缀
- less-loader：解析.less文件

5、处理图片

一般开发环境使用file-loader，生成环境使用url-loader

file-loader：开发环境使用，图片直接打包到根目录下

url-loader：生成环境使用，传入options，将小图片转为base64编码（减少http请求），大图片放到img目录下

# 高级配置

基本配置只能做demo，不能做线上项目

1、多入口

```
entry: {
	index: './src/index',
	other: './src/other'
}
```

```
output: {
	filename: '[name].[contentHash:8].js',
	path: './dist'
}
```

HtmlWebpackPlugin 需要生成多个实例，分别对应多个入口，并且参数chunks只引入该入口的对应的js

```
new HtmlWebpackPlugin({
	template: './src/index.html',
	filename: 'index',
	chunks: ['index']
})
new HtmlWebpackPlugin({
	template: './src/other.html',
	filename: 'other',
	chunks: ['other']
})
```

2、抽离css文件

避免将css插入到style标签中，而是通过link引入css文件

使用MiniCssExtractPlugin，将style-loader 替换为MiniCssExtractPlugin.loader

在plugin中

```
new MiniCssExtractPlugin {
	filename: 'css/main.[contentHash:8].css'
}
```

压缩css

在optimization中使用minimizer

3、抽离公共代码，第三方模块

```
splitChunks: {
	chunks: 'all',//全部
	cacheGroups: {
	// 第三方模块
		vendor: {
			name: 'vendor',// chunk 名称
			priority: 1, // 权限更高，优先抽离
			test: '/node_modules/', // 匹配
			minSize: 0, // 大小限制，太小不抽取
			minChunks:1 // 最少复用几次
		},
		common: {
			//公共模块
			name: 'common',
			priority: 0,
			minSize: 0,
			minChunks: 2
		}
	}
}
```

new HtmlWebpackPlugin中的chunk需要引入

```
chunks:['index','vendor','common']
```

4、懒加载

​	异步代码中import js

5、Babel

​	vue-loader

# 概念

- module：各个源码文件，一切皆模块
- chunk：多模块合并成的
- bundle：最终的输出文件

# webpack性能优化

## 构建速度

- 优化bael-loader：开启缓存，明确范围（include, exclude）
- IgnorePlugin：忽略第三方包指定目录，让这些指定目录不要被打包进去
- noParse：不去解析属性值代表的库的依赖（比如打包的时候依赖了三方库（jquyer、lodash）等，而这些三方库里面没有其他依赖，可以通过配置noParse不去解析文件，提高打包效率）
- happyPack：多进程打包
- ParallelUglifyPlugin：多进程，并行压缩输出的 JS 代码
- 自动刷新
- 热更新
- DLLPlugin：把第三方库代码分离开，并且每次文件更改的时候，它只会打包该项目自身的代码。所以打包速度会更快

## 产出代码

- 小图片base64编码
- bundle加hash，命中缓存
- 懒加载
- 提取公共代码
- CDN
- 使用production
  - 代码压缩
  - 删除调试代码
  - Tree-Shaking
- scope hosting

# ES6 Module Commonjs

ES6 Module 静态引入，编译时引入

Commonjs 动态引入，执行时引入

所以esm可以实现Tree-Shaking

# 总结

- 前端为何要进行打包和构建
  - 体积更小，加载更快
  - 编译高级语言和语法
  - 兼容性和错误提示
  - 统一、高效的开发环境
  - 统一的构建流程和产出标准
  - 集成构建规范（提测、上线）
- loader和plugin
  - loader模块转换器
  - plugin扩展插件
- Babel 和 webpack
  - 语法编译
  - 打包构建工具
- 如何产出一个lib
- 懒加载
