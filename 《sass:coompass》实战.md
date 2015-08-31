# 《Sass/Coompass》实战

## 话题

### Sass变量

### Sass混合器

混合的写法：
	
	// 声明
	@mixin name() {}
	= name() {} // sass语法
	
	// 引入
	@include name()
	+ name() // sass语法

混合的原理是在每一个引入的地方都复写一遍这个混合的代码，所以混合的弊端也就是会明显的造成产出CSS的代码冗余。

### Sass继承

继承的写法是：

	// 声明
	.error {}
	
	// 引入
	@extend .error
	
继承的原理是将子类选择器与父类选择器同级应用父类选择器的样式规则，子类独有的规则只应用于子类选择器。这样就有效的解决了混合的代码冗余。

如果父类选择器不需要在html中是用，则父类可以使用占位符选择器，如下：
	
	// 占位符
	%error {}

### Sass函数

### CSS重置
	
	// 使用compass的重置模块
	@import "compass/reset"

### 补齐CSS3浏览器厂商前缀

	// 引入
	@import "compass/css3";
	
	// 使用
	@include border-radius(5px);
	
	// 单个边框圆角
	@include border-corner-radius(top, left, 5px);
	
	

### 自动化生成精灵图

	@import "compass/utilities/sprites";
	@import "icons/*.png";
	
	@include all-icons-sprites;
	
	@include icons-sprite(box-add);

### 可配置路径

### 网格布局

	// 创建带blueprint的项目
	compass create my_grid --using blueprint
	
	// 设置网格
	@include column($sidebar-columns);

### 表格辅助器

	// 引入
	@import "compass/utilities/tables";
	
	// 使用
	@include table-scaffolding
	@include inner-table-borders(1px, darken($table-color, 40%));
	@include outer-table-borders(2px);
	@include alternating-rows-and-columns($table-color, adjuest-hue($table-color, -120deg), #222222);
	
### 垂直韵律

### 使老版本浏览器支持HTML5标签

### Compass实现阴影

	@import "compass/css3";
	
	// 文字阴影
	@include text-shadow();
	
	// 盒阴影
	@include box-shadow();

### Compass边框圆角

	@import "compass/css3";
	
	// 支持厂商选择
	$experimental-support-for-opera: false;
	$experimental-support-for-microsoft: false;
	$experimental-support-for-khtml: false;

	@include border-radius(5px);

### Compass背景渐变

	@import "compass/css3";
	
	@include background(linear-gradient(360deg, ......));

### 样式表从开发到上线

### 优化样式表性能

### Compass的资源辅助器

	image-url('logo.png')
	font-url('arial.ttf')
	stylesheet-url('randomfile.xml')