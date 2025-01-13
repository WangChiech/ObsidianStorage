
## unicode
[官网](https://home.unicode.org/)

```js
for (let i = 0; i < 128; i++) {
	document.write(
		i +
		" <span style='background-color: lightgreen;'>" +
		String.fromCharCode(i) +
		"</span><br/>"
	)
}
```

**CJK Unified Ideographs**
 
 最常用的中文字符（其实是中日韩塞在一起的，其中不包含各阶段的增补字符）
 
 区域：`U+4E00 - U+9FFF` (20989)

**BMP**

 四个十六进制位能表示的字符，
 
 此区域兼容性更好，如 JavaScript 中 String 的 charCode 系列的 API (`String.fromCharCode()`、`''.charCodeAt()`) 就只能处理 BMP 里面的字符，如果超过四个 16 进制位的字符则使用，`String.fromCodePoint()`、`''.codePointAt()`

## 词法

InputElement
	WhiteSpace
		`<TAB>`
			`U+0009`
		`<VT>`
		`<FF>`
		`<SP>`
			推荐全用此空格，其他空格要用的话则采用 `\u` 转义
		`<NBSP>`
			`&nbsp;` 一种有效的空格，其前后两个字符一起换行，不会在中间折断
		`<ZWNBSP>`
			`U+FEFF` zero width no-break space(历史上，对于文本不知道是什么格式的，有什么大头、小头，微软想了个解决办法，在每个文本前都查一个零宽的空格，根据零宽的空格的字节顺序反推整个文件的编码格式，此方式称 BOM `bit order mask`)
	LineTerminator
		`<LF>`
			`U+000A` LINE FEED (LF)
		`<CR>`（回车）
			`U+000D` CARRIAGE RETURN (CR)
		`<LS>` （不建议使用）
			`U+2028` LINE SEPARATOR
		`<PS>`（不建议使用）
			`U+2029` PARAGRAPH SEPARATOR
	Comment
		单行注释：//
		多行注释：`/*` 和 `*/` 之间的文本会被当作注释
	Token
		Punctuator
		Keywords
		IdentifierName
			keywords
			Identifier
				命名规则：以 UnicodeIDStart（unicode 标了一批能够表示 ID_Start 或者 ID_Continue 的字符） 或 $ 或 _ 开头。（数字一定不可以）
				变量名：不可以和关键字重合
				属性名：可以和关键字重合
			Future reserved words：enum
		Literal
			Number
				IEEE 754 Double Float
					sign（1）
					Exponent（11）
					Fraction（52）
				Grammar
					DecimalLiteral
						0
						0.
						.2
						1e3
					BinaryIntegerLiteral
						0b111
					OctalIntegerLiteral
						0o10
					HexIntegerLiteral
						0xFF
				Practice
					Safe Integer 
						Number.MAX_SAFE_INTEGER.toString(16) => "1fffffffffffff" (共14位)
					Float Compare
						Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON
		String
			Character（字符）
			Code Point（码点）
			Encoding
				ASCII
					0-128
				Unicode
					UTF
						UTF8（把字节中的几个比特位做成了控制位，比如以 1110 开头的会被拆成三个字节存储，如中文“一”在 UTF16 无损，即使用 16 位 2 字节，在 UTF8 中要占三个字节）
						UTF16（JavaScript 基本上是以 UTF16 在内存中存储的，所以不承认 BMP 之外的字符是一个字符）
				UCS
					U+0000 - U+FFFF
					Unicode 的一个子集（在之初 UCS 组织和 Unicode 组织同步做的各自的字符集）
				GB（只收录了ASCII 字符和中文）
					GB2312
					GBK（GB13000）
					GB18030
				ISO-8859
					一系列欧洲国家的自己的标准
				BIG5
					繁体中文（台湾制定的）
			> JavaScript String 只承认 Unicode，准确的说 JavaScript 字符串是 UCS 编码，因为超出 BMP 范围的字符会被当作两个字符，只有用 codePoint 系列的 API 才能正确解析。
			> 每一种字符集都可能对应着多种编码方式，码点定了，但码点怎么存，
			
			Grammar
				“
				‘
				``
				转义
					\x 后接两个 16 进制位
					\u 后接四个 16 进制位
					\b 0x0008 backspace
					\t 0x0009 character tabulation
					\n 0x000A line feed
					\v 0x000B
					\f 0x000C
					\r 0x000D carriage return
					\" 0x0022
					\' 0x0027
					\\ 0x005C

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308251241537.png)

```
// 当 “/” 能被当作除法时会被当作除法解析，否则当作正则语法解析

var a;
a
/a/g
// 会被当作 a 除 a 除 g
```

Number 二进制精度测试实现
```html
<div id="app">
	<span v-for="v, i of bits">
		<input
			:class="i > 0 ? i > 11 ? 'fraction' : 'exponent' : 'sign'"
			v-model="bits[i]"
			/>
		<input v-if="i == 11" value="1" disabled="disabled"/>
		<br v-if="i == 31"/>
	</span>
	<br/>
	<input style="width: 5em;" v-model="value"/>
</div>
```

```js
window.vm = new Vue({
	el: '#app',
	data: {
		bits: Array(65).join(0).split('').map(v => Number(v)),
		value: 0
	},
	watch: {
		bits(val) {
			console.log('bits', val)
			const bytes = new Uint8Array(8)
			const memory = new Float64Array(bytes.buffer)
			for (var i = 0; i < 8; i++) {
				var byte = 0
				for (var j = 0; j < 8; j++) {
					byte = byte << 1
					byte != Number(val[i * 8 + j])
					console.log(byte, val[i * 8 + j])
				}
				console.log('byte', byte)
				bytes[7 - i] = byte
			}
			this.value = memory[0]
		},
		value (val) {
			const bytes = new Unit8Array(8)
			const memory = new Float64Array(bytes.buffer)
			memory[0] = (val)
			console.log('****')
			for (var i = 0; i < 8; i++) {
				var byte = bytes[i]
				console.log(byte)
				for (var j = 0; j < 8; j++) {
					this.bits[(8 - i) * 8 - j - 1] = byte & 1
					byte = byte >> 1
				}	
			}
		}
	}
})
```
## 参考名词：

- [字符集](https://zh.wikipedia.org/zh/%E5%AD%97%E7%AC%A6%E7%BC%96%E7%A0%81)：字符编码（英语：Character encoding）、字集码是把字符集中的字符编码为指定集合中某一对象（例如：比特模式、自然数序列、8 位组或者电脉冲），以便文本在计算机中存储和通过通信网络的传递。常见的例子包括将拉丁字母表编码成摩斯电码和 ASCII。其中，ASCII 将字母、数字和其它符号编号，并用 7 比特的二进制来表示这个整数。通常会额外使用一个扩充的比特，以便于以 1 个字节的方式存储。在计算机技术发展的早期，如 ASCII（1963 年）和 EBCDIC（1964 年）这样的字符集逐渐成为标准。但这些字符集的局限很快就变得明显，于是人们开发了许多方法来扩展它们。对于支持包括东亚 CJK 字符家族在内的写作系统的要求能支持更大量的字符，并且需要一种系统而不是临时的方法实现这些字符的编码。
-  [Unicode](https://zh.wikipedia.org/zh-hans/Unicode) ：中文：万国码、国际码、统一码、单一码。是计算机科学领域里的一项业界标准。它对世界上大部分的文字系统进行了整理、编码，使得电脑可以用更为简单的方式来呈现和处理文字。
-  [ASCII](https://zh.wikipedia.org/wiki/ASCII) ：（American Standard Code for Information Interchange，美国信息交换标准代码）是基于拉丁字母的一套电脑编码系统。它主要用于显示现代英语，而其扩展版本延伸美国标准信息交换码则可以部分支持其他西欧语言，并等同于国际标准 ISO/IEC 646。美国信息交换标准代码是这套编码系统的传统命名，互联网号码分配局现在更倾向于使用它的新名字 US-ASCII[2]。美国信息交换标准代码是美国电气和电子工程师协会里程碑之一。
- Token：记号、标记。JS 里有效的输入元素都可以叫 Token。
-  [NBSP](https://zh.wikipedia.org/wiki/%E4%B8%8D%E6%8D%A2%E8%A1%8C%E7%A9%BA%E6%A0%BC) ：不换行空格（英语：no-break space，NBSP）是空格字符，用途是禁止自动换行。HTML 页面显示时会自动合并多个连续的空白字符（whitespace character），但该字符是禁止合并的，因此该字符也称作“硬空格”（hard space、fixed space）。Unicode 码点为：U+00A0 no-break space。
- [零宽空格](https://zh.wikipedia.org/zh-hans/%E9%9B%B6%E5%AE%BD%E7%A9%BA%E6%A0%BC)：（zero-width space, ZWSP）是一种不可打印的 Unicode 字符，用于可能需要换行处。在 HTML 页面中，零宽空格可以替代。但是在一些网页浏览器（例如 Internet Explorer 的版本 6 或以下）不支持零宽空格的功能。

## 参考

[ecma-262.pdf](https://www.ecma-international.org/publications-and-standards/standards/ecma-262/)

[unicode官网](https://home.unicode.org/)

[fileformat.info_unicode](https://www.fileformat.info/info/unicode/index.htm)

[JavaScript 浮点数陷阱及解法](https://github.com/camsong/blog/issues/9)

[npm double](https://www.npmjs.com/package/double-bits)

[字符编码笔记：ASCII，Unicode 和 UTF-8](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)