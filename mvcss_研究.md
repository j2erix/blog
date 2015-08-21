# MVCSS 研究

## 一、Overview

从建筑架构的角度看，一个应用可以分解成三个主要部分：

* Foundation - 基础
* Components - 组件
* Structures - 拼装组件的结构

## 二、Styleguide

### Basic

关于顺序、空格、缩进、选择器等的相关规范。

### The Numbers Game

关于字号、行高、边距、尺寸等的相关规范。

### Comments

每个文件的注释根据重要性分为四级：

* First Level - 文件级别，如`button`组件的介绍
* Second Level - 修饰、状态等
* Third Level - 修饰的种类，如Size和Theme
* Fourth Level - 代码级别

### Naming Conventions

关于组件、结构、修饰、状态等的修改器，包括以下部分：

#### Tools

MVCSS在_tools.sass中提供了许多工具类，统一使用短写表示。例如：

* .mbm - margin bottom medium
* .mbl - margin bottom large

详见Foundation的Tools章节。

#### Components/Structures

组件和结构通常是个独立的单词，比如：

* icon
* button
* g - grid
* form
* modal

当名字由多个单词组成时，采用驼峰命名法，如`taskList`。

#### Modifiers

修饰器是在组件的默认状态的基础上进行的修改，如“更大”的按钮等，约定是用`--`表示状态，如`btn--s`、`btn--l`。尽量以层级如`a`、`b`或者功能`cancel`、`submit`命名修饰器，不要以样式如`red`、`blue`来命名。

MVCSS准备了许多修饰符的缩写形式：
	
	// 尺寸
	--f  (flush)
	--xs (extra small)
	--s
	--m
	--l
	--xl (extra large)
	
	// 层级
	-a (primary)
	-b (secondary)
	-c (tertiary)
	
#### States

状态一般是JavaScript加上或去掉的，用于表示UI状态，如按钮的激活等。约定使用`is-*`表示，如`is-active`。状态可以单独抽象出来，相同的状态当应用到不同的组件上的时候可能效果各不相同，如`btn`和`nav-item`。

#### Context

环境的概念借鉴自SUIT，大部分的组件都是自包含的，大部分时候都能正常工作。但是有的组件需要依赖父级组件的相应属性的支持。最典型的，比如依赖父组件的定位。

环境约定使用`has-*`表示，比如一个绝对定位的`dropdown`，父组件就需要添加如下样式的选择器：

	.has-dropdown
		position: relative
		
#### Scaffolding

组件或结构的内部子元素，如果需要进行样式的定制，就写在这里。如`dropdown`组件内部的`dropdown-media`。他们也可以有自己的修饰和状态。但是如果一个组件的子元素层级太多，则需要进行重构了。

#### Variables

变量都放到一个独立的文件中，详见Foundation的Config部分。

#### Images 

图片的命名：

* `bg-*` - 背景图
* `logo-*` - logo
* `img-*` - 内容图片
* 如果太多可以用子目录进行组织

***

## 三、Manifest

清单，将之前关于命名和零星的知识点整合起来，MVCSS的目录结构如下：

	application.sass
	foundation/
	  _reset.sass
	  _helpers.sass
	  _config.sass
	  _base.sass
	  _tools.sass
	components/
	structures/
	vendor/

application.sass是样式清单，将其他的sass文件import进来。

### Imports

在application.sass中，foundation的文件按照规定顺序导入，components和structures的文件按照字母顺序导入。

#### 拆分大文件

如果文件较大，则应该将文件拆分成多个小文件，用子目录去组织，如将较大的`_config.sass`拆分如下：
	
	@import 'foundation/config/fonts'
	@import 'foundation/config/colors'
	@import 'foundation/config/base'
	@import 'foundation/config/components'
	@import 'foundation/config/structures'

### Inbox

`application.sass`文件底部有一段`Inbox`的注释，这里用来引入任何临时的样式文件。

***

## 四、Foundation

基础，由以下几部分组成：

* Reset
* Helpers
* Config
* Base
* Tools

### Foundation - Reset

MVCSS默认使用Normalize.css

### Foundation - Helpers

辅助器文件，包括：

* Functions - Sass和Compass提供了大量的辅助函数
* Mixins - MVCSS默认提供了媒体查询的混合`=respond-to()`
* Extends - 即可以直接在HTML标签中使用，也可以被模块继承
* Animations - 放置可以跨项目使用的模块，如`fade-in`，如果是针对某个模块的动画，就定义在模块各自的底部。

Mixins用于带参数的的代码块，Extends则用于不变的代码块。

### Foundation - Config

Config的组成结构如下：

* @Font-face
* Variables
	* Base
	* Colors
	* Fonts
	
项目中的所有变量都放在Config里，除了Palette（调色板），其他的变量都各自附带着前缀：

* $b-* - 基础
* $c-* - 颜色
* $g-* - 断电
* $componentName-* - 组件变量
* $structureName-* - 结构变量

### Foundation - Base

这是项目全局的基础样式，对常用标签设置样式，分成几部分：

* 首先是html和body
* 然后是Block Content块元素
	* ul
	* li
	* p
	* h1,h2,h3,h4
* 再后是Inline Content行内元素
	* a
	* strong
	* em
	* code

### Foundation - Tools

Tools是一些可以直接在HTML中使用的样式片段，通常是短写模式，如`.mbf`。

Tools要保持单一职责原则，一个工具类制作一件事。

应首先考虑组件和结构的完善，再考虑使用工具类。

***

## 五、Components

组件既有能作为容器的，如`g`、`card`，也有只能作为独立内容的如`thumb`。

如何确定一个模块是否属于Component还是Structure，可以问自己如下问题：

* 这个模块是在一个有限的范围里吗？
* 这个模块是否足够独立吗？
* 这个模块不需要做大的改变就能跨项目使用吗？

如果以上任何一个问题的答案是否，那这个模块可能应该归为Structure。

***

## 六、Structures

结构，相对与组件来说就是不那么通用的东西，主要是从一下几个方面考虑：

* 范围大小
* 对于其他模块的依赖
* 跨项目的可移植性

举个例子，比如一个有Logo和导航的网站头，他可能可以用多个Components、Helpers和Tools组合而成。但是当要对它做响应式设计的时候麻烦就来了。

相比组件，结构的可以依赖与其他结构或组件，耦合性相对较强，也是比较有项目针对性的。

Grid(g)在MVCSS中就是一个相对独立的组件，而Collection就是一个结构，它依赖与Grid，而且针对项目有一些特殊的定制。

在可移植性方面，如果一个模块经过少量改动，甚至不用改动就能在多个项目间使用，它应该被归为组件；如果一个模块的主要代码需要改动才能跨项目使用，它就应该被归为结构。

***

## 七、Vendor

放置第三方样式库的地方，如果库是css文件，请将它改成.scss后缀，然后才能@import进application.sass中。






