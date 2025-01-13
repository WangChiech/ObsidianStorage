
## Grammar

### Grammar Tree vs Priority


>优先级是用表达式生成树的方式实现的，如 `1 + 2 * 3` 这样简单的四则运算，实际上是将 `+` 的右面变成了一个新的节点，从使用者的角度来看，这个叫运算符的优先级，从语言的实现了定义上来看，这个叫树的结构形成的语法

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241150610.png)

>运算优先级最高的基本运算符是 Member 运算符和 new 运算符，

**Expression**

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241208999.png)

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241206706.png)

>函数名加模板字符串的形式设计自己的小语言时比较方便，jsx 考虑过用此实现


>new Foo() 与 New Foo 的优先级不一样，new Foo 相当于 `new (Foo的返回函数())`

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241221802.png)

>Member expressions 返回的是一个 Reference 类型

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241243624.png)

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241255454.png)

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241251708.png)


### Left hand side & Right hand side

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241253444.png)

>Left Handside 运行时必须是 Reference

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241258232.png)

>同一个值是没有办法先前自增再后自增的

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241312243.png)

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241313458.png)

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241324748.png)

>对于 IFFE ，建议采用 `void function (i) {}(i)` 代替 `(function (i) {})(i)`

>`typeof null === 'object'` 是设计上的失误，源于 C++ 中 null 是一种空指针


![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241343618.png)

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241344045.png)

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241346030.png)

![[Pasted image 20230824134909.png]]

>逻辑运算有短路逻辑

>运行时角度看，JavaScript 中加法有两种，一种是 Number 类型的加法，第二种是 String 类型的加法；乘法只有一种。

>运算符有多种，每种运算符基本都可以接受所有类型的数据，这就涉及到**类型转换**

## Runtime

### Type Convertion

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241541298.png)

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241604716.png)

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241550159.png)

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241558734.png)

![](https://cdn.jsdelivr.net/gh/wangchiech/image_store/img/202308241601547.png)

 
**StringToNumber**

```js
function convertStringToNumber(string, x) {
	if (arguments.length < 2) {
		x = 10
	}

	var chars = string.split('')
	var number = 0
	
	var i = 0
	while (i < chars.length && chars[i] !== '.') {
		number = number * x
		// charCodeAt 等效
		number += chars[i].codePointAt(0) - '0'.codePointAt(0)
		i++
	}
		
	if (chars[i] === '.') {
		i++
	}
	
	var fraction = 1
	while (i < chars.length) {
		fraction = fraction / x
		// charCodeAt 等效
		number += (chars[i].codePointAt(0) - '0'.codePointAt(0)) * fraction
		i++
	}
	
	return number
}

convertStringToNumber('10.02')
```

```js
function convertNumberToString(number, x = 10) {
	var integer = Math.floor(number)
	var fraction = number - integer
	var string = ''
	
	while (integer > 0) {
		string = integer % x + string
		integer = Math.floor(integer / x)
	}

	return string
}

console.log(convertNumberToString(7, 2))
```
### Reference


## 参考名词：

- LeftHandSideExpression：ECMA-262.pdf 201 页 12.3
- UpdateExpression：ECMA-262.pdf 178 页 11.9.1
-  [IIFE](https://zh.wikipedia.org/wiki/%E7%AB%8B%E5%8D%B3%E8%B0%83%E7%94%A8%E5%87%BD%E6%95%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F) ：Immediately-invoked Function Expressions 立即执行的函数表达式

## 参考

[Number 二进制存储 - jsfiddle](https://jsfiddle.net/plh8qeor/19/)
