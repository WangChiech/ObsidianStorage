# 内置对象 Symbol

- `Symbol(description)`
    - description 一个字符串，用来表示 symbol 的描述
    - 返回一个唯一的 symbol 类型的值

## 静态属性

- Symbol.asyncIterator：
    - 指定一个默认异步迭代器，可用于`for await...of`
- Symbol.hasInstance：
    - `class` 类函数内部若含有 `Symbol.hasInstance`，则在使用 `instanceof` 时，首先调用此方法。[instanceof](https://zh.javascript.info/instanceof)
- Symbol.isConcatSpreadable：
- Symbol.iterator：
- Symbol.match：
- Symbol.matchAll：
- Symbol.replace：
- Symbol.search：
- Symbol.species：
    - 在类中使用，用于覆盖子类中的默认构造器(用类 A 扩展内建类 Array，A 实例的 map 等方法返回的新实例对象由子类 A 的构造器生成，若想使用其他构造器则使用 `Symbol.species`)
    - ```js
      class mArray extends Array {}
      class mArray1 extends Array {
        static get [Symbol.species] () {
          return Array
        }
      }
      const arr = new mArray
      const arr1 = new mArray1
      const subArr = arr.map(item => item)
      const subArr1 = arr1.map(item => item)

      console.log(arr.constructor === mArray) // true
      console.log(arr1.constructor === mArray1) // true
      console.log(subArr.constructor === mArray) // true
      console.log(subArr1.constructor === mArray1) // false
      console.log(subArr1.constructor === Array) // true
    ```
- Symbol.split：
- Symbol.toPrimitive：
- Symbol.toStringTag：自定义 `Object.prototype.toString()` 的行为
- Symbol.unscopables：

## 静态方法

- Symbol.for：`for(key)`
    - key 一个字符串，作为**全局 symbol 注册表**中与某 symbol 关联的键（同时也会作为该 symbol 的描述）
    - 返回由给定 key 找到的 symbol，否则返回新创建的 symbol
- Symbol.keyFor：`keyFor(sym)`
    - sym 必选参数，需要查找键值的某个 Symbol
    - 返回**全局注册表**中找到的 sym 所对应的 key 值（字符串），没找到则返回 undefined

## Symbol.prototype

**属性**
- Symbol.prototype.description：
    - 只读属性
    - 返回 Symbol 对象的可选描述的字符串
**方法**
- Symbol.prototype[Symbol.toPrimitive]：`[Symbol.toPrimitive](hint)`
    - 返回指定 Symbol 值的**原始值**（configurable: true）
- Symbol.prototype.toString：`toString()`
    - 返回当前 symbol 的字符串表示
- Symbol.prototyp.valueOf：`valueOf`
    - 返回当前 symbol 值的**原始值**

