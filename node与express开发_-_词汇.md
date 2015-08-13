# Node与Express开发 - 词汇

***
`__dirname`：当前目录	

> `__dirname`会被解析为正在执行的脚本所在的目录。所以如果你的脚本放在`/home/sites/app.js`中，则`__dirname`会被解析为`/home/sites`。不管什么时候，这个全局变量用起来都很方便。如果不这么做，在不同的目录中运行你的程序时很可能会出现难以诊断的错误。

***
Redirect Path：重定向插件

> 我高度推荐你安装一个能显示HTTP请求状态码和所有重定向的浏览器插件。这样在解决重定向问题或者不正确的状态码时会更加容易，它们经常被忽视。对于Chrome来说，Ayima的Redirect Path特别好用。

***
`app.VERB`：VERB是动词的意思

> `app.get`是我们添加路由的方法。在Express文档中写的是`app.VERB`。这并不意味着存在一个叫VERB的方法，它是用来指代HTTP动词的（最常见的是“get” 和“post”）。这个方法有两个参数：一个路径和一个函数。

***
`app.get`：添加get请求的路由

> 这个方法有两个参数：一个路径和一个函数。

***
`res`：express的响应对象

> node的`res`对象有如下方法：`res.writeHead`, `res.end`。express扩展了`res`对象，添加的方法如：`res.send`, `res.set`, `res.type`, `res.status`。

***
`app.use`：添加中间件

> 注意，我们对定制的404和500页面的处理与对普通页面的处理应有所区别：用的不是app.get，而是app.use。app.use是Express添加中间件的一种方法。我们会在第10章更深入地探讨中间件，现在你可以把它看作处理所有没有路由匹配路径的处理器。这里涉及一个非常重要的知识点：在Express中，路由和中间件的添加顺序至关重要。如果我们把404处理器放在所有路由上面，那首页和关于页面就不能用了，访问这些URL得到的都是404。

***
Handlebars：一款模板引擎

> 为了支持Handlebars，我们要用到Eric Ferraiuolo的express3-handlebars包（尽管名字中是express3，但这个包在Express 4.0中也可以使用）。

***
`app.engine`：设置视图引擎
> `app.engine('handlebars', handlebars.engine);`

***
`static`中间件
> static中间件可以将一个或多个目录指派为包含静态资源的目录，其中的资源不经过任何特殊处理直接发送到客户端。你可以在其中放图片、CSS文件、客户端JavaScript文件之类的资源。你应该把static中间件加在所有路由之前。
> `app.use(express.static(__dirname + '/public'));`

***
`app.listen`：express监听端口
> express服务监听端口

***
Web质量
> 到达率 - SEO  
> 功能  
> 可用性  
> 审美

***
QA技术概览
> 页面测试 - Mocha
> 跨页测试 - Zombie.js  
> 逻辑测试 - Mocha
> 去毛 - JSHint  
> 链接检查 - LinkChecker

***
Chai
> 与Mocha配合的断言库

***
TDD vs BDD
> Mocha支持多种“界面”来控制测试的风格。默认界面是行为驱动开发（BDD），它让你以行为的方式思考。在BDD中，你描述组件和它们的行为，然后用测试去验证这些行为。然而，我发现这些测试经常不适合这一模型，然后BDD语言看起来就显得很奇怪。测试驱动开发（TDD）更具可行性，你描述的是测试集和其中的测试。你可以使用两种界面进行自己的测试，但会造成配置上的困难。因此我在本书中坚持使用TDD。如果你喜欢BDD，或者BDD和TDD混合的风格，当然也可以。

***

