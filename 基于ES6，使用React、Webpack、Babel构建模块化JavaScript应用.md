# 基于ES6，使用React、Webpack、Babel构建模块化JavaScript应用
***

## 编译
> webpack, gulp.js

由于目前浏览器对ES6的新特性并不完全支持，所以我们需要将ES6编译为ES5。同时，我们希望使用NPM管理自定义组建，所以需要一个兼容CommonJS的工具集。

Browerify和Webpack都能够满足我们的大部分需求，考虑到Webpack对样式、图片、字体文件等得兼容更好，所以我们选用Webpack。

在Webpack的基础上，我们加入Gulp.js，来辅助完成单元测试、代码覆盖率检测、静态资源服务等任务。

## 使用ES6
> babel.js

我们将使用Babel.js作为ES6的转义器，因为它有更好的生态系统（插件、扩展等）。此外，它还可以转义JSX，这样就可以弃用标准的JSX转义器了。

## 自给自足
> sass-loader, component style namespace

一个自给自足的组建应该包含自身的默认样式，考虑到CSS默认使用的是全局命名空间，所以需要手工给每个组件创建自己的明明空间。然后使用sass-loader加载组件的样式，然后像导入文件一样用`import`导入组建文件中。

## 发布到NPM包
> 自动化测试封装CI, es6, main, prepublish

打包组建的时候，在package.json中添加构建指令，并在预发布阶段运行构建。使用编译后的ES5代码作为主文件，通过package.json的额外字段暴露ES6代码。

这样不管是再ES5中引入模块，还是要使用模块的源代码都能正常工作。

## 单元测试和代码覆盖率
> mocha, jsdom, istanbul

## 组建交互
> postal.js, channel, topic

## 代码检查和源码映射
> eslint, preloaders, devtool

对jsx文件添加eslint-loader作为预加载器

