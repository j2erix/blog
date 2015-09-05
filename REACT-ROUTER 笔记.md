# REACT/ROUTER 笔记
***
原文地址：[http://rackt.github.io/react-router/](http://rackt.github.io/react-router/)

## React Router概览
> 重点是React Router工作流部分，理解了这部分，就对React Router有了最主要的认识。

React Router是专为React.js设计的完整的路由解决方案。它无痛得将你的应用的组件与URL关联起来，并对 **嵌套**、 **过渡** 和 **服务端渲染** 提供一流的支持。
> 这句话是整套文档的中心。这里的嵌套和过渡两个概念的主体具体是指什么还有待确认。  
> 根据下文可知，这里的嵌套指的是复杂的路由情况下，URL和组件的嵌套。

对比一下：

* 当没有React Router的时候，App路由的实现可能是依赖于监听hash的变化，使用switch分支选择相应的组件进行渲染的方式。这种方式当路由开始变得复杂（比如出现嵌套关系）的时候实现也会变得复杂而难以维护。
* React Router采用JSX语法来做路由配置，因为JSX语法适用于利用属性来表现视图的层级结构。路由在这个配置文件中统一管理，很好的解决了URL和组件嵌套的问题。

### React Router的工作流程
在React Router的工作流中，有三个组成部分。

首先是组件，包括根组件和页面组件等等其他组件：

```
// 第一部分
var RouteHandler = Router.RouteHandler;

var App = React.createClass({
  render () {
    return (
      <div>
        <h1>App</h1>
        <RouteHandler/>
      </div>
    )
  }
});

var Message = React.createClass({
  render () {
    return <h3>Message</h3>;
  }
});

```
这里的`App`就是根组件，而`Message`就是一个页面组件。

然后是路由配置，是一个JSX语法的组件：

```
// 第二部分
var routes = (
  <Route handler={App}>
    <Route path="about" handler={About}/>
    <Route path="inbox" handler={Inbox}>
      <Route path="messages/:id" handler={Message}/>
    </Route>
  </Route>
);
```
这个组件描述了整个应用的根组件和不同的`path`包括嵌套的`path`应该采用哪个组件去处理。

最后是启动代码：

```	
// 第三部分
Router.run(routes, Router.HashLocation, (Root) => {
	React.render(<Root/>, document.body);
});
```
这段代码是React Router的入口，它表示Router根据URL的Hash值，去路由配置`routes`中选择匹配的子组件替换跟组件`App`的`RouteHandler`部分，然后产出一个`Root`组件交给回调函数，回调函数再将`Root`渲染到页面中。

### 获取URl参数
当匹配的path中有动态部分如`:id`的时候，组件的`this.props.params`属性中就会被赋以响应的参数，如下：

```
<Route path="messages/:id" handler={Message}/>
```

```
var Message = React.createClass({
  componentDidMount: function () {
    // from the path `/inbox/messages/:id`
    var id = this.props.params.id;
    fetchMessage(id, function (err, message) {
      this.setState({ message: message });
    })
  },
  // ...
});
```

### 再说URL和组件的嵌套
在React Router中，URL和组件的嵌套是各自独立的。看个例子就明白了：

```
var routes = (
  <Route handler={App}>
    <Route path="inbox" handler={Inbox}>
      <Route path="messages/:id" handler={Message}/>
      <Route path="/archive/messages/:id" handler={Message}/>
    </Route>
  </Route>
);
```
这样就给了路由的设计足够的自由度。

## 路由配置
React Router提供了四种路由配置对象，分别是：

* DefaultRoute
* NotFoundRoute
* Redirect
* Route

关于每个配置对象的用处和配置参数，官方文档有较为清晰的阐述，参见章节：[http://rackt.github.io/react-router/#DefaultRoute](http://rackt.github.io/react-router/#DefaultRoute)

## 顶级方法

### Router.create
用于创建一个新的router，至于它的真正的用处，文档上给了这样一句话：
> Useful to pass around your app to be able to call transitionTo and friends w/o being inside of a component.

具体是什么意思还有待理解。

下面一段代码可以更好的理解Router.create和Router.run方法的关系：

```
// the more common API is
Router.run(routes, Router.HistoryLocation, callback);

// which is just a shortcut for
var router = Router.create({
  routes: routes,
  location: Router.HistoryLocation
});

router.run(callback);
```
 
### Router.run
这是React Router的主要API，用途上面已经讲过，就不在赘述了。

重点是这个API的多种用法，值得细究。具体例子参见文档：[http://rackt.github.io/react-router/#Router.run](http://rackt.github.io/react-router/#Router.run)

## 组件
React Router提供了一些路由相关的组件：

* Link：写在页面组件中，用于产生导向其他路由的链接
* Root：是最后渲染再页面上的根组件
* Route Handler：写在配置中，用于将path与相应处理器匹配
* RouteHandler：被相应路由匹配而激活的组件

## Locations
React Router支持多种路由方式：

* Custom Location：自定义路由，一般不用
* HashLocation：用Hash作为路由
* HistoryLocation：用HTML5的History API作为路由
* RefreshLocation：是HistoryLocation的一个后备，会引起页面刷新
* StaticLocation：...
* TestLocation：...

## Scroll Behavior 滚动条行为

* ImitateBrowserBehavior：滚动条是否回到之前的位置
* ScrollToTopBehavior：滚动条滚到顶部

## Mixins 混入

* Navigation：提供了可编程的一系列导航API
* State：与当前路由状态有关的一些列API

## Misc 宏

* Transition：提供了路由过渡过程中的一些Hooks