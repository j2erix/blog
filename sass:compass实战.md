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