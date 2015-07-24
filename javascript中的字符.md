# JavaScript中的字符

*2015年7月24日下午2:23*

***

编程语言为了存储和处理字符，需要以一定的规范将字符转换成计算机能理解的二进制码。

**JavaScript内部，字符以[UTF-16](http://baike.baidu.com/view/497266.htm)的格式存储。** 比如`'a'`的UTF-16编码为`'\u0061'`。

| 例子     | a                    |
| --------|:--------------------:|
| 2进制码  | 0000 0000 0110 0001  |
| 10进制码 | 97                   |
| 16进制码 | 0x0061               |
| unicode | U+0061               |
| js中表示 | \u0061               |

UTF-16是变长的，一部分的字符是占用1个16位长的码，最大码点为0xFFFF。当某些字符的码点超过0xFFFF时，需要占用2个16位长的码去表示，这也是JS中某些字符的长度为2的原因，如：`"𠮷".length`。

JS中有4个与码点相关的方法：

* `String.prototype.charCodeAt()`
* `String.prototype.codePointAt()` - es6
* `String.fromCharCode()`
* `String.fromCodePoint()` - es6

其中`charCodeAt`和`codePointAt`一个字符串响应未知的字符码点。不同的是，当这个位置的字符的码点超过0xFFFF，也就是占用2个16位时，前者只能获取它前16位的码点值。而后者可以获得它的完成32位的码点值，比如：
	
	var s = "𠮷a";

	s.codePointAt(0) // 134071
	s.codePointAt(1) // 57271

	s.charCodeAt(2) // 97

