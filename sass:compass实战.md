# Sass/Compass实战

## Sass的主要功能有：

* 变量
* 嵌套 Nest
* 混合 Mixin
* 继承

## Compass是基于Sass的框架

* 自动添加浏览器厂商前缀
* 辅助函数，方便访问文件资源

## Sass的基本语法以及Compass

* 嵌套 - 方便模块划分
* 混合 - 适合一些公用的样式，比如同样风格的border
* 继承 - 适合用于表示元素的继承关系，比如button和button-mini。继承关系的选择器是并列的，公用同一块css，所以相对于混合来说代码更少。
* 导入 - sass的@import能够将多个文件合并为同一个，而css的@import会导致多个请求；给子文件加上下划线比如_button.scss，就可以防止子文件的单独编译。

## SassScript
是一种针对CSS的特殊脚本，提供的功能如下：

* 变量 - 数值，字符串，色彩，布尔值，null，列表
* 运算
* 插补 #{} - 在选择器和属性中使用变量
* 流程控制指令 - if/else for each while 
* 函数
* 注释

## 输出格式

* :nested
* :expanded
* :compact
* :compressed

## Compass模块

Core模块

* CSS3 - 自动添加前缀等，如 @include border-radius(4px);
* Helpers - 辅助函数等，如 image-width(); inline-image();
* Layout
* Reset 
* Typography
* Utilities - 方便的工具混合，如 CSS Sprites

