# JavaScript中的字符

*2015年7月24日下午2:23*

***

编程语言为了存储和处理字符，需要以一定的规则将字符转换成计算机能理解的二进制码。

**JavaScript内部，字符以[UTF-16](http://baike.baidu.com/view/497266.htm)的格式存储。** 比如`'a'`的UTF-16编码为`'\u0061'`。

| 例子     | a                    |
| --------|:--------------------:|
| 2进制码  | 0000 0000 0110 0001  |
| 10进制码 | 97                   |
| 16进制码 | 0x0061               |
| unicode | U+0061               |
| js中表示 | \u0061               |

UTF-16是变长的，一部分的字符是占用1个16位长的码，最大码点为0xFFFF。当某些字符的码点超过0xFFFF时，需要占用2个16位长的码去表示，这也是JS中某些字符的长度为2的原因，如：`"𠮷".length`。

JS中有6个与码点相关的方法：

* `String.prototype.charCodeAt()`
* `String.prototype.codePointAt()` - es6
* `String.fromCharCode()`
* `String.fromCodePoint()` - es6
* `String.prototype.charAt()`
* `String.prototype.at()` - es7

其中`charCodeAt`和`codePointAt`一个字符串相应位置的字符码点。不同的是，当这个位置的字符的码点超过0xFFFF，也就是占用2个16位时，前者只能获取它前16位的码点值。而后者可以获得它的完成32位的码点值，比如：
	
	var s = "𠮷a";

	s.codePointAt(0) // 134071
	s.charCodeAt(0) // 55362
	
	s.codePointAt(1) // 57271
	s.charCodeAt(1) // 57271

	s.codePointAt(2) // 97
	s.charCodeAt(2) // 97

相反`fromCharCode`和`fromCodePoint`则是根据码点获取相应的字符。前者只能正确处理小于0xFFFF的，而后者可以处理包括大于0xFFFF的字符，比如：
	
	var s = "𠮷a";
	
	String.fromCodePoint(134071) // "𠮷"
	String.fromCharCode(134071) // "ஷ"
	
再有`charAt`和`at`用于获取字符串某个索引对应的字符，前者是ES5提供的方法，不能识别大于0xFFFF的字符。后者是ES7提供的方法，可以识别大于0xFFFF的字符，不过现在用还为时尚早。比如：
	
	var s = "𠮷a";
	
	s.charAt(0) // ""
	s.charAt(1) // ""
	s.charAt(2) // "a"
	
	s.at(0) // "𠮷"
	s.at(1) // ""
	s.at(2) // "a"
	
## normalize()

为了更好的支持Unicode对于语调和重音符号的表示，ES6提供了`normalize()`方法，用于进行Unicode正规化，比如：

	'\u01D1'==='\u004F\u030C' //false

	'\u01D1'.length // 1
	'\u004F\u030C'.length // 2

	'\u01D1'.normalize() === '\u004F\u030C'.normalize() // true
	
这种字符在中国环境下不会出现，简单了解即好，详见阮一峰老师的[文章](http://es6.ruanyifeng.com/#docs/string)。

## includes(), startsWith(), endsWith()

以往只能用`indexOf()`方法判断字符串的包含关系，ES6又扩展了一些新的方法：

	var s = "Hello world!";

	s.startsWith("Hello") // true
	s.endsWith("!") // true
	s.includes("o") // true

这几个方法还支持第二个参数，表示开始搜索的位置：

	var s = "Hello world!";

	s.startsWith("world", 6) // true
	s.endsWith("Hello", 5) // true
	s.includes("Hello", 6) // false
	
## repeat()

将一个字符重复几遍：

	"x".repeat(3) // "xxx"
	"hello".repeat(2) // "hellohello"
	
