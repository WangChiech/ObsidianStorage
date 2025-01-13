# Number

- `Number(value)`
    - 返回将 value 强制转换为的**数字原始值**，不能转换则返回 NaN
    - bigint 类型被转换为数字

## 静态属性
- Number.EPSILON：$2^{-52}$，表示 1 与大于 1 的最小浮点数之间的差值
- Number.MAX_SAFE_INTEGER：$(2^{53}-1)，最大安全整数
- Number.MIN_SAFE_INTEGER：$(-2^{53}-1)，最小安全整数
- Number.MAX_VALUE：$2^{1024}-1$，可表示的最大数值
- Number.MIN_VALUE：$2^{-1074}$，最小正数值（最接近 0 的数）
- Number.NaN：数字值 NaN
- Number.NEGATIVE_INFINITY：负无穷，与全局属性 Infinity 负值相同
- Number.POSITIVE_INFINITY：正无穷大，与全局属性 Infinity 相同

## 静态方法
- Number.parseInt：`parseInt(string[, radix])`
    - string 要被解析的值，会被**强制转化为字符串**，字符串开头的空白会被忽略
    - radix 取值区间 [2, 36]，表示 string 的**基数**，
    - 返回从 string 中解析出的整数，若 radix 不属于 [2, 36]，或第一个非空白字符不能转为数字，则返回 NaN
- Number.parseFloat：`parseFloat(string)`
    - string 要被解析的值，会被**强制转化为字符串**，字符串开头的空白会被忽略
    - 返回从 string 解析得到的浮点数，若第一个非空白字符不能被转为数字，则返回 NaN
- Number.isFinite：`isFinite(value)`
    - value 要测试有限性的值
    - 返回 value 的有限性，value 为非数字、NaN、Infinity时返回 false，否则返回 true
- Number.isInteger：`isInteger(value)`
    - 返回 value 是否为整数，非 number 类型、NaN、Infinity、number 类型非整数返回 false
- Number.isSafeInteger：`isSafeInteger(value)`
    - 返回 value 值是否为**安全整数**，是则返回 true
    - 安全整数区间 [$-(2^{53}-1)$, $2^{53}-1$]
- Number.isNaN：`isNaN(value)`
    - 返回 value 值是否为**一个值为 NaN 的数字**，是则返回 true
    - 不会将 value 强制转为数字

## Number.prototype

- Number.prototype.toFixed：`toFixed([digits])`
    - digits 指定有**几位小数**，默认为 0，取值区间 [0, 100]
    - 返回定点表示法表示指定数字的（舍入为最接近的数字）**字符串**
- Number.prototype.toPrecision：`toPrecision([precision])`
    - precision 指定有效位数的**整数**，省略则返回 `num.toString()`
    - 返回以定点表示法或指数表示法表示的 precision 个有效数字的（舍入为最接近的数字）**字符串**
- Number.prototype.toExponential：`toExponential([fractionDigits])`
    - fractionDigits 指定小数点后有几位数字，取值区间 [0, 100]
    - 返回指数表示法（小数点前 1 位数字）表示的 fractionDigits 位小数的（舍入为最接近的数字）的**字符串**
- Number.prototype.toString：`toString([radix])`
    - radix 表示指定数字值的**基数**，默认 10，取值区间 [2, 36]
    - 返回表示指定数字值的**字符串**
- Number.prototype.toLocaleString：`toLocaleString(locales, options)` [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString)
    - 返回一个指定语言环境下的**表示字符串**
- Number.prototypr.valueOf：`valueOf()`
    - 返回 Number 对象的**原始值数字**
## 参考

[数字类型-js_info](https://zh.javascript.info/number)