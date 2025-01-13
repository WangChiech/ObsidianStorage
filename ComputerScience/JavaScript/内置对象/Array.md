# String

- `String(thing)`
    - thing 为任何要转换为字符串的内容
    - 返回将参数强制转为字符串原始类型的值
    - Symbol 值会被转成 `"Symbol(description)"`

## 静态方法
- String.fromCharCode：`fromCharCode(num1[, num2][, /*...*/ numN])`
    - numN 为 [0, 0xFFFF] 之间的 **UTF16 码元**，超出会被截断为 16 位
    - 返回长度为 N 的指定 UTF16 码元组成的字符串
- String.fromCodePoint：`fromCodePoint(num1[, num2][, /*...*/ numN])`
    - numN 为 [0, 0x10FFFF] 之间的 **Unicode 码点**，此区间外的值报错
    - 返回指定码点对应字符组成的字符串
**模板标签函数**
- String.raw：````raw`templateString```` | `raw(objOfRaw, ...substitutions)`
    - templateString 模板字符串，内部解析为 objOfRaw substitutions，作为参数传递给 raw 函数
    - 返回模板字符串的原始字符串内容，即不对反斜杠 \ 和特殊字符进行转义
    - ```javascript [手写 raw 函数]
      function customRaw(strings, ...values) {
        let result = "";
        for (let i = 0; i < strings.length; i++) {
            result += strings[i] + (values[i] || "");
        }
        return result;
      }

      let customRawString = customRaw`Hello\nWorld`;
      console.log(customRawString); // 输出: Hello\nWorld

      ```

## String.prototype
### 属性
- length：UTF-16 码元的个数

### 方法
#### 在字符串获取单个字符
- String.prototype.at：`at(index)`
    - 返回 index 处**码元字符**
    - 没有返回 `undefined`
    - index 为负数 => index + length
- String.prototype.charAt：`charAt(index)`
    - index 区间 [0, length - 1]
    - 返回 index 处**码元字符**，没有返回 `''`
- String.prototype.charCodeAt：`charCodeAt(index)`
    - 返回 index 处**码元值**
    - 没有返回 `NaN`
    - index 区间 [0, len - 1]
- String.prototype.codePointAt：`codePonitAt(index)` [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/codePointAt)
    - index 区间 [0, len - 1]
    - 返回**码元index**处的**unicode码点**(index处为前导代理时)或**后尾代理码元**（index处为后尾代理时），没有返回 `undefined`
    - index 为 UTF16 码元的索引

#### 在字符串中获取的一部分
- String.prototype.substring：`substring(indexStart[, indexEnd])`
    - indexStart 默认为 0；小于 0 => 0
    - indexEnd 默认为 length； 大于 length => length
    - indexStart > indexEnd => 两个值互换
    - 截取区间 [indexStart, indexEnd)
    - 返回一个包含提取的字符串片段的新字符串
- String.prototype.slice：`slice(indexStart[, indexEnd])`
    - indexStart 默认为 0；小于 0 => max(indexStart + str.length, 0)
    - indexEnd 默认为 length；小于 0 => max(indexEnd + str.length, 0)；大于 length => length
    - 截取区间 [indexStart, indexEnd)
    - 返回一个包含提取的字符串片段的新字符串

#### 在字符串中搜索str的索引
- String.prototype.indexOf：`indexOf(searchString[, position])`
    - 返回查找的字符串 searchString 第一次出现的索引，如果没有找到，则返回 -1
    - 搜索范围 [position, length)
    - position 默认为 0， position < 0 => 0
- String.prototype.lastIndexOf：`lastIndexOf(searchString[, position])`
    - 如果找到了 searchString，则返回最后一次出现的索引，否则返回 -1
    - 查找区间 [0, position]
    - position 默认为 +Infinity，position >length -1 => length -1；position < 0 => 0
- String.prototype.search：`search(regexp)`
    - regexp 为正则或具有  `Symbol.search` 方法的对象
    - 字符串匹配 regexp，如果匹配成功，则返回正则表达式在字符串中首次匹配的索引；否则，返回 -1
    - str.search(obj) 结果为 obj[Symbol.search](str) 的返回值

#### 在字符串中搜索str是否存在
- String.prototype.startsWith：`startsWith(searchString[, position])`
    - searchString 省略时为 `'undefined'`
    - position 默认为 0；小于0 => 0
    - 区间字符串截取范围 [position, length)
    - 返回**区间字符串**开头是否包含 searchString，包含则返回 true，否则返回 false
- String.prototype.endsWith：`endsWith(searchString[, endPosition])`
    - endPosition 默认为 `str.length`；大于 `str.length` => `str.length`
    - 区间字符串截取范围 [0, endPosition)
    - 返回**区间字符串**末尾是否包含 searchString，包含则返回 true，否则返回 false
- String.prototype.includes：`includes(searchString[, position])`
    - 如果在给定的字符串中找了要搜索的字符串，则返回 true，否则返回 false
    - 搜索范围 [position, str.length)
    - position 默认为 0，小于 0 => 0

#### 在字符串基础上做特定字符转换
- String.prototype.toUpperCase：`toUpperCase()`
    - 返回一个新的字符串，表示转换为大写的调用字符串
- String.prototype.toLowserCase：`toLowserCase()`
    - 返回一个新的字符串，表示转换为小写的调用字符串
- String.prototype.toLocaleUpperCase：`toLocaleUpperCase([locales])`
    - 返回一个新的字符串，表示调用字符串根据特定区域设置的大小写映射规则转换得到的大写形式
- String.prototype.toLocaleLowerCase：`toLocaleLowerCase([locales])`
    - 返回一个新的字符串，表示调用字符串根据特定区域设置的大小写映射规则转换得到的小写形式
- String.prototype.replace：`replace(pattern, replacement)`
    - pattern 为字符串或具有 `Symbol.replace` 方法的对象（正则具有`Symbol.replace` 方法）
    - replacement 为字符串或函数，为函数时，最终替换为函数调用（pattern为参数）的返回值
    - 返回一个新的字符串，其中一个、多个或所有的匹配项都被指定的替换项替换
    - 默认替换第一个匹配。若为正则并有 g 标志，则替换全部匹配
- String.prototype.replaceAll：`replaceAll(pattern, replacement)`
    - pattern 为字符串或具有 `Symbol.replace` 方法的对象（正则）
    - replacement 为字符串或函数，为函数时，最终替换为函数调用（pattern为参数）的返回值
    - 返回一个新字符串，其中所有匹配 pattern 的部分都被替换为 replacement
    - pattern 为正则时，必须设置 g 标志，否则报错

#### 在字符串基础上做增删
- String.prototype.padStart：`padStart(targetLength[, padString])`
    - targetLength 返回新字符串的长度，默认为 length；小于等于 length 时 => length
    - padString 默认为 `' '/* u+0020 */`；过长截断时，使用左侧部分
    - 返回在开头填充 padString 直到达到给定的 targetLength 所形成的 String
- String.prototype.padEnd：`padEnd(targetLength[, padString])`
    - targetLength 返回新字符串的长度，默认为 length；小于等于 length 时 => length
    - padString 默认为 `' '/* u+0020 */`；过长截断时，使用左侧部分
    - 返回在当前 str 末尾填充 padString 直到达到给定的 targetLength 所形成的 String
- String.prototype.repeat：`repeat(count)`
    - count 为 [0, +Infinity] 之间的整数
    - 返回包含指定字符串指定数量副本的新字符串
- String.prototype.concat：`concat(str1[, str2][, /*...*/ strN])`
    - 返回与所传的 n 个字符串拼接后的**新字符串**
- String.prototype.trim：`trim()`
    - 返回一个去除开头、结尾空白字符的新字符串
- String.prototype.trimStart：`trimStart() // 等同于 trimLeft()`
    - 返回一个去除开头空白字符的字符串
- String.prototype.trimEnd：`trimEnd() // 等同于 trimRight()`
    - 返回一个去除末尾空白字符的字符串

#### 分割成数组
- String.prototype.split：`split(separator[, limit])`
    - separator 可以是 undefined，一个字符串，或者一个具有 Symbol.split 方法的对象（正则）
    - limit 返回的字符串数组中的字符串最大个数
    - 返回在给定字符串中出现 separator 的每一个点上进行分割而成的字符串数组

#### 在字符串中提取匹配项
- String.prototype.match：`match(regexp)` [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)
    - regexp 为正则或具有  `Symbol.match` 方法的对象
    - 返回 调用 `Symbol.match` 方法的返回值（数组）
    - 当使用 g 标志全局匹配时，返回的结果为**匹配的字符数组**
    - 当没有使用 g 标志时，值返回第一个完整匹配，但会讲其捕获组相关信息添加到 结果数组的属性上
- String.prototype.matchAll：`matchAll(regexp)`
    - regexp 为正则或具有  `Symbol.matchAll` 方法的对象
    - regexp 若为正则，则必须设置 g 标志，否则报错
    - 返回一个匹配结果的可迭代迭代器对象（它不可重新开始）
    - 每个匹配结果都是一个数组，其形状与 RegExp.prototype.exec() 的返回值相同 
    - res.next().value => `[0: 'matched str', index: 'matched str index' groups: undefined, input: 'input str']`

#### 字符串规范化
- String.prototype.isWellFormed：`isWellFormed`
    - 如果字符串不包含单独代理项，返回 true，否则返回 false
- String.prototype.toWellFormed：`toWellFormed()`
    - 返回一个将**单独代理项**替换为**Unicode字符`U+FFFD`**的新字符串
- String.prototype.normalize：`normalize([form])` [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/normalize)
    - form 默认为 'NFC'，取值为 "NFC"、"NFD"、"NFKC" 或 "NFKD" 其中之一，用于指定 Unicode 标准化形式
    - 返回一个包含给定字符串的 Unicode 标准化形式的字符串
    - 在 Unicode 中，如果两个**码位序列**表示相同的抽象字符（具有相同的视觉外观），则这两个序列具有规范等价性

- String.prototype.toString：`toString()`
    - 返回字符串原始值
- String.prototype.valueOf：`valueOf()`
    - 返回字符串原始值

- String.prototype[Symbol.iterator]：``
    - 返回一个新的可迭代迭代器对象，它以字符串值中的 Unicode 码位生成单独的字符串
    - 此方法使得字符串可迭代，如`for...of`、`...str`迭代语法会自动调用该方法获取迭代器进行循环

- String.prototype.localeCompare：`localeCompare(compareString[, locales][, options])` [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare)
    - 如果引用字符串（referenceStr）存在于比较字符串（compareString）之前则为负数；如果引用字符串存在于比较字符串之后则为正数；相等的时候返回 0。
