# Node与Express开发 - 词汇

## Node与Express

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

## 质量保证

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

## 请求与响应

响应报头
>正如浏览器以请求报头的形式发送隐藏信息到服务器，当服务器响应时，同样会回传一些浏览器没必要渲染和显示的信息，通常是元数据和服务器信息。我们已经熟悉内容类型头信息，它告诉浏览器正在被传输的内容类型（网页、图片、样式表、客户端脚本等）。特别要注意的是，不管URL路径是什么，浏览器都根据内容类型报头处理信息。因此你可以通过一个叫作/image.jpg的路径提供网页，也可以通过一个叫作/text.html的路径提供图片。（这样做并不合情理，这里要讲的重点是路径是抽象的，浏览器只根据内容类型来决定内容该如何渲染。）除了内容类型之外，报头还会指出响应信息是否被压缩，以及使用的是哪种编码。响应报头还可以包含关于浏览器对资源缓存时长的提示。优化网站时需要着重考虑这一点，我们将在第16章详细讨论。响应报头还经常会包含一些关于服务器的信息，一般会指出服务器的类型，有时甚至会包含操作系统的详细信息。返回服务器信息存在一个问题，那就是它会给黑客一个可乘之机，从而使站点陷入危险。非常重视安全的服务器经常忽略此信息，甚至提供虚假信息。禁用Express的`X-Powered-By`头信息很简单：

`app.disable('x-powered-by');`

***
互联网媒体类型
>内容类型报头信息极其重要，没有它，客户端很难判断如何渲染接收到的内容。内容类型报头就是一种互联网媒体类型，由一个类型、一个子类型以及可选的参数组成。例如，`text/html;charset=UTF-8`说明类型是text，子类型是html，字符编码是UTF-8。互联网编号分配机构维护了一个官方的互联网媒体类型清单（http://www.iana.org/assignments/media-types/media-types.xhtml）。我们常见的content type、Internet media type和MIME type是可以互换的。MIME（多用途互联网邮件扩展）是互联网媒体类型的前身，它们大部分是相同的。

`text/html`, `text/css`, `text/javascript`, `image/png`, `image/jpg`, `application/x-www-form-urlendcoded`, `application/json`, `multipart/form-data`

***
请求对象：`req`或`request`
> 请求对象（通常传递到回调方法，这意味着你可以随意命名，通常命名为`req`或`request`）的生命周期始于Node的一个核心对象`http.IncomingMessage`的实例。Express添加了一些附加功能。我们来看看请求对象中最有用的属性和方法（除了来自Node的`req.headers`和`req.url`，所有这些方法都由Express添加）。

`req.params`, `req.query`, `req.url`等

***
响应对象：`res`或`response`
> 响应对象（通常传递到回调方法，这意味着你可以随意命名它，通常命名为res、resp或response）的生命周期始于Node核心对象http.ServerResponse的实例。Express添加了一些附加功能。我们来看看响应对象中最有用的属性和方法（所有这些方法都是由Express添加的）。

`res.status()`, `res.set()`, `res.json()`, `res.jsonp()`, `res.type()`等

`res.cookie()`
> 设置或清除客户端cookies值。需要中间件支持，详见第9章。

`res.redirect([status],url)`
> 重定向浏览器。默认重定向代码是302（建立）。通常，你应尽量减少重定向，除非永久移动一个页面，这种情况应当使用代码301（永久移动）。

`res.attachment([filename])`,`res.download(path,[filename],[callback])`
> 这两种方法会将响应报头Content-Disposition设为attachment，这样浏览器就会选择下载而不是展现内容。你可以指定filename给浏览器作为对用户的提示。用res.download可以指定要下载的文件，而res.attachment只是设置报头。另外，你还要将内容发送到客户端。

`res.sendFile(path,[option],[callback])`
> 这个方法可根据路径读取指定文件并将内容发送到客户端。使用该方法很方便。使用静态中间件，并将发送到客户端的文件放在公共目录下，这很容易。然而，如果你想根据条件在相同的URL下提供不同的资源，这个方法可以派上用场。

`res.locals,res.render(view,[locals],callback)`
> res.locals是一个对象，包含用于渲染视图的默认上下文。res.render使用配置的模板引擎渲染视图（不能把res.render的locals参数与res.locals混为一谈，上下文在res.locals中会被重写，但在没有被重写的情况下仍然可用）。res.render的默认响应代码为200，使用res.status可以指定一个不同的代码。视图渲染将在第7章深入讨论。

## Handlebars模板引擎

Handlebars：模板引擎

* 插入： `{{}}`
* html不转义： `{{{}}}`
* 注释：`{{! 注释 }}`
* 遍历：`{{#each items }} {{/each}}`
* 如果：`{{#if ../currencies}} {{/if}}`
* 除非：`{{unless}} {{/unless}}`
* 局部文件： `{{> partial_name}}`
* 局部文件子目录： `{{> social/facebook}}`
* 段落：`{{#section}} {{/section}}`

***
Handlebars > helpers
> Handlebars下的辅助方法，添加section辅助方法，可以实现视图往布局中插入段落。

## 表单处理

`body-parser`中间件
> 用于处理`post`请求的body

`3XX`状态码

* `301`: 永久重定向
* `302`: 暂时重定向
* `303`: 告诉浏览器“是的，你的请求有效，可以在这里找到响应”，只能使用GET请求响应，并且不会缓存重定向目标。
* `307`: 与`303`相近，至于是POST还是GET，跟重定向前保持一致。

`req.xhr`和`req.accepts`：
> 是否ajax请求；检查请求的Accept，确认请求接受怎样的返回。

文件上传

* formidabel
* enctype="multipart/form-data"

jQuery文件上传
> Sebastian Tschan的jQuery File Upload（http://blueimp.github.io/jQuery-File-Upload）> 

## Cookie

cookie
	
	res.cookie('monster', 'nom nom'); 


签名cookie

	// credentials.js
	module.exports = {
		cookieSecret: '把你的cookie秘钥放在这里',
	};
	
	// app.js
	var credentials = require('./credentials.js');
	app.use(require('cookie-parser')(credentials.cookieSecret));
	res.cookie('signed_monster', 'nom nom', { signed: true });
	

删除cookie

	res.clearCookie('monster');


禁用cookie

	
	
篡改cookie

XSS与cookie
> 这几年出现了一种叫作跨站脚本攻击 （XSS）的攻击方式。XSS攻击中有一种技术就涉及用恶意的JavaScript修改cookie中的内容。所以不要轻易相信返回到你的服务器的cookie内容。用签名cookie会有帮助（不管是用户修改的还是恶意JavaScript修改的，这些篡改都会在签名cookie中留下明显的痕迹），并且还可以设定选项指明cookie只能由服务器修改。这些cookie的用途会受限，但它们肯定更安全。

会话优于cookie

请求与响应中的cookie
> 当服务器希望客户端保存一个cookie时，它会发送一个响应头`Set-Cookie`，其中包含名称/值对。当客户端向服务器发送含有`cookie`的请求时，它会发送多个请求头`Cookie`，其中包含这些`cookie`的值。

cookie秘钥
> 为了保证cookie的安全，必须有一个cookie秘钥。cookie秘钥是一个字符串，服务器知道它是什么，它会在cookie发送到客户端之前对cookie加密。这是一个不需要记住的密码，所以可以是随机字符串。我一般用一个随机密码生成器（受xkcd启发，http://preshing.com/20110811/xkcd-password-generator）来生成cookie秘钥。

凭证外化
> 在一个外部文件（js、json或xml）中维护一些凭证文件，如cookie秘钥、数据库密码和API令牌等。记得在.gitignore中加入这个文件，如credentials.js

cookie-parser
> 在程序中开始设置和访问cookie之前，需要先引入中间件cookie-parser。首先npm install --save cookie-parse。

cookie选项

* `domain`
* `path`
* `maxAge` - 指定客户端应该保存cookie多长时间，单位是毫秒。如果你省略了这一选项，浏览器关闭时cookie就会被删掉。（你也可以用expiration指定cookie过期的日期，但语法很麻烦。我建议用maxAge。）

* `secure` - 指定该cookie只通过安全（HTTPS）连接发送。
* `httpOnly` - 将这个选项设为true表明这个cookie只能由服务器修改。也就是说客户端JavaScript不能修改它。这有助于防范XSS攻击。
* `signed` - 设为true会对这个cookie签名，这样就需要用res.signedCookies而不是res.cookies访问它。被篡改的签名cookie会被服务器拒绝，并且cookie值会重置为它的原始值。

## 会话

express-session

	app.use(require('cookie-parser')(credentials.cookieSecret));
	app.use(require('express-session')());
	
* `key` -  存放唯一会话标识的cookie名称。默认为connect.sid。
* `store` - 会话存储的实例。默认为一个MemoryStore的实例，可以满足我们当前的要求。第13章将会介绍如何使用数据库存储。
* `cookie` - 会话cookie的cookie设置 （path、domain、secure等）。适用于常规的cookie默认值。

使用session
	
	req.session.userName = 'Anonymous';
	var colorScheme = req.session.colorScheme || 'dark';

删除session
	
	req.session.userName = null;      // 这会将'userName'设为null, 但不会移除它
	delete req.session.colorScheme;   // 这会移除'colorScheme'

即显消息  
> “即显”消息（不要跟Adobe Flash搞混了）只是在不破坏用户导航的前提下向用户提供反馈的一种办法。用会话实现即显消息是最简单的方式（也可以用查询字符串，但那样除了URL会更丑外，还会把即显消息放到书签里，这也许不是你想要的结果）。我们先把HTML设置好。我们将会用Bootstrap的警告消息组件显示我们的即显消息，所以请确保你引入了Bootstrap。在你的模板文件里，找个醒目的地方（一般是直接放在网站的标题下面）。  

即显消息 - 实现方式

1. 在页面中添加显示即显消息的片段 {{{flash.message}}}
2. 在路由之前添加中间件，将req.session.flash赋值给req.locals.flash，然后将req.session.flash删除，以保证即显消息只展示一次。
3. 在用户请求页面的时候给req.session.flash赋值，这样页面就能展示即显消息了

会话的用途
> 当你想跨页保存用户的偏好时，可以用会话。会话最常见的用法是提供用户验证信息，你登录后就会创建一个会话。之后你就不用在每次重新加载页面时再登录一次。即便没有用户账号，会话也有用。网站一般都要记住你喜欢如何排列东西，或者你喜欢哪种日期格式，这些都不需要登录。  
> 尽管我建议你优先选择会话而不是cookie，但理解cookie的工作机制也很重要（特别是因为有cookie才能用会话）。它对于你在应用中诊断问题、理解安全性及隐私问题都有帮助。 ￼

## 中间件

中间件
> 从概念上讲，中间件是一种功能的封装方式，具体来说就是封装在程序中处理HTTP请求的功能。从实战上讲，中间件只是一个有3个参数的函数：一个请求对象、一个响应对象和一个next函数，稍后会作解释。（还有一种4个参数的形式，用来做错误处理，这会在本章末尾讲到。）
> 
> 中间件是在管道中执行的。你可以想象一个送水的真实管道。水从一端泵入，然后在到达目的地之前还会经过各种仪表和阀门。这个比喻中很重要的一部分是顺序问题，你把压力表放在阀门之前和之后的效果是不同的。同样，如果你有个向水中注入什么东西的阀门，这个阀门“下游”的所有东西都会含有这个新添加的原料。在Express程序中，通过调用app.use向管道中插入中间件。

“终止”请求 - next
> 那么请求在管道中如何“终止”呢？这是由传给每个中间件的next函数来实现的。如果不调用next()，请求就在那个中间件中终止了。

中间件的重要概念
> 中间件要关注几个点：匹配的路径、回调、错误处理、是否终止、是否返回内容

* 路由处理器（app.get、app.post等，经常被统称为app.VERB）可以被看作只处理特定HTTP谓词（GET、POST等）的中间件。同样，也可以将中间件看作可以处理全部HTTP谓词的路由处理器（基本上等同于app.all，可以处理任何HTTP谓词；对于PURGE之类特别的谓词会有细微的差别，但对于普通的谓词而言，效果是一样的）。
* 路由处理器的第一个参数必须是路径。如果你想让某个路由匹配所有路径，只需用/*。中间件也可以将路径作为第一个参数，但它是可选的（如果忽略这个参数，它会匹配所有路径，就像指定了/\*一样）。
* 路由处理器和中间件的参数中都有回调函数，这个函数有2个、3个或4个参数（从技术上讲也可以有0或1个参数，但这些形式没有意义）。如果有2个或3个参数，头两个参数是请求和响应对象，第三个参数是next函数。如果有4个参数，它就变成了错误处理中间件，第一个参数变成了错误对象，然后依次是请求、响应和next对象。
* 如果不调用next()，管道就会被终止，也不会再有处理器或中间件做后续处理。如果你不调用next()，则应该发送一个响应到客户端（res.send、res.json、res.render等）；如果你不这样做，客户端会被挂起并最终导致超时。
* 如果调用了next()，一般不宜再发送响应到客户端。如果你发送了，管道中后续的中间件或路由处理器还会执行，但它们发送的任何响应都会被忽略。

connect中间件
> 大多数之前捆绑在Express中的中间件都十分基础，所以一定要知道“它去哪了”以及如何得到它。你大概总是需要Connect，所以我建议你把它和Express一起安装（npm install --save connect），并使它在你的程序中可以访问到（var connect = require(connect);）。

常用的中间件

查找中间件
> 目前还没有第三方中间件的“商店”或索引目录。然而，几乎所有的Express中间件都能在npm上找到，所以如果你用npm搜索“Express”“Connect”和“Middleware”，会得到一个相当不错的清单。


## 发送邮件

Nodemailer

SMTP
> 简单邮件传输协议，直接是有可能被打入垃圾邮箱

MSA
> 邮件提交代理

MTA
> 邮件传输代理

邮件头

邮件格式

HTML邮件

## 与生产相关的问题

执行环境

* 开发环境
* 测试环境
* 生产环境

设置环境

	app.set('env', 'production');
	
获取环境

	app.get('env');
	
NODE_ENV

	$ export NODE_ENV=production
	$ node meadowlark.js
	
日志支持

Morgan
> 开发环境使用的日志程序，它的输出是便于查看的彩色文本。

express-logger
> 生产环境使用的日志程序，支持日志循环

扩展网站

向上扩展
> 更快的CPU，更好的架构，更多内核，更多内存，等等。

向外扩展
> 意味着更多的服务器。

持久化
> ，除非所有服务器都能访问到那个文件系统，否则你不应该用本地文件系统做持久化。不过只读数据是个例外，比如日志和备份。比如，我一般会把表单提交的数据备份到本地普通文件中，以防数据库接连失效。一旦遇到数据库中断的情况，到每个服务器上收集文件虽然麻烦，但最起码不会造成破坏。

### 用应用集群扩展

应用集群

1. 讲主程序脚本改为即可直接运行，又可作为模块引入的形式
2. 新开一个集群脚本，在isMaster主线程中为每个cpu开辟工作线程，在工作线程中引入并执行主程序脚本。

`require.main`
> 当直接运行脚本时，require.main === module是true；如果它是false，表明你的脚本是另外一个脚本用require加载进来的。

`cluster`
> 集群

`cluster.fork()`
> 开辟工作线程

`cluster.isMaster`
> 当前上下文为主线程

`cluster.isWorker`
> 当前上下文为工作线程

### 处理未捕获的异常

Express的`try/catch`
> 在Express执行路由处理器时，它把它们封装在一个try/catch块中，所以这不是一个真正的未捕获异常。这不会引起太多问题，Express会在服务器端记录异常，并且访问者会得到一个丑陋的栈输出。然而服务器是稳定的，其他请求还能得到正确处理。

错误页面
> 提供一个定制的错误页面总归是一个好的做法，当错误出现时，它不仅在用户面前显得更专业，还可以让你采取行动。比如，你可以在这个错误处理器中发送一封邮件给开发团队，让他们知道网站出错了。可惜这只能用在Express可以捕获的异常上。

异步异常
> 异步异常Express的`try/catch`捕获不到，会导致程序崩溃
	
	app.get('/epic-fail', function(req, res){ 
		process.nextTick(function(){
    		throw new Error('Kaboom!'); 
  		});  		  		
	});

故障转移机制
> 最容易的故障转移机制是使用集群（就像之前提到的）。如果你的程序是运行在集群模式下的，当一个工作线程死掉后，主线程会繁衍另一个工作线程来取代它。

正常的关闭服务器
> 我们做的第一件事是创建一个域，然后在上面附着一个错误处理器。只要这个域中出现未捕获的错误，就会调用这个函数。我们在这里采取的方式是试图给任何处理中的请求以恰当的响应，然后关闭服务器。根据错误的性质，可能无法响应处理中的请求，所以我们首先要确立关闭服务器的截止时间。在这个例子中，我们允许服务器在5秒内响应处理中的请求（如果它可以）。你所选择的数值取决于你的程序，如果程序经常有长请求，你就应该给更多的时间。一旦确立了截止时间，我们会从集群中断开（如果在集群中），以防止集群给我们分配更多的请求。然后明确告诉服务器我们不再接受新的连接。最后，我们试图传到错误处理路由（next(err)）来响应产生错误的请求。如果那会抛出错误，我们退回去用普通的Node API响应。如果其他的全部失败了，我们会记录错误（客户端得不到响应，最终会超时）。

用多台服务器扩展
> 需要代理服务器，推荐Nginx或HAProxy

### 网站监控

第三方正常运行监控
> UptimeRoot或Pingdom或Site24x7

应用程序故障
> 错误邮件、简单题型服务SNS、Sentry、Airbrake

### 压力测试

loadtest

