# 探索React生态圈

原文：[http://www.csdn.net/article/2015-08-04/2825370-react](http://www.csdn.net/article/2015-08-04/2825370-react)

## 目标平台

* 多Targets的根本就是提高开发者体验

## 数据处理

* Flux - 结合React的MC
* GraphQL - 客户端可以自己查询数据，避免冗余的endpoints
* Relay - Facebook给出的GraphQL方案，Relay让组件可以自定义其所需要GraphQL数据格式，在组件实例化的时候再去GraphQL服务器获取数据。Relay也会自动构建嵌套组件的GraphQL查询，这样多个嵌套的组件只需要发一次请求。
* Immutability - 大型React应用必备，提高性能。

## 工具

* Webpack
* Babel
* React-hot-reload
* React Developer Tool
 		
## 新的挑战

* 动画 - Cheng Lou的React Tween State
* Flux库与Relay
* CSS - cssnext项目; React中的inline css;