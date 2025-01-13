# BigInt

- `BigInt(value)`
    - value 可以是字符串（Number()方法可以转换为整数的字符串）或整数、布尔值
    - 返回 value 转为 bigint 类型的值

## 静态方法

- BigInt.asIntN：`asIntN(width, bigint)`
    - width 可存储整数的位数
    - bigint 要存储在指定位数上的整数
    - 返回 bigint 模（modulo）$2^{width}$ 作为**有符号整数的值**
- BigInt.asUintN：`asUintN(width, bigint)`
    - width 可存储整数的位数
    - bigint 要存储在指定位数上的整数
    - 返回 bigint 模（modulo）$2^{width}$ 作为**无符号整数的值**

## BigInt.prototype

- BigInt.prototype.toString：`toString([radix])`
    - radix 取值区间 [2, 36]，指定用于表示数值的基数
    - 返回指定 BigInt 对象的字符串
- BigInt.prototype.toLocaleString：`toLocaleString([locales][, options])`
    - 返回一个表示给定 BitInt 对象的字符串，该字符串格式因不同语言而不同
- BigInt.prototype.valueOf：`valueOf()`
    - 返回指定 BigInt 对象的**原始 BigInt 值**