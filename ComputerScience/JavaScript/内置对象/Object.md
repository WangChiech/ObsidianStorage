# 内置对象 Object

- `Object()`

## Object.prototype
**属性**
- Object.prototype.constructor：
    - 指向 Object 函数的引用

**方法**
- Object.prototype.hasOwnProperty：`hasOwnProperty(prop)`
    - prop 要测试的属性（字符串或 Symbol）
    - 返回 prop 是否是对象的**自有**属性
- Object.prototype.propertyIsEnumerable：`propertyIsEnumerable(prop)`
    - prop 要测试的属性（字符串或 Symbol）
    - 返回 prop 是否是对象**可枚举**的**自有**属性
- Object.prototype.isPrototypeOf：`isPrototypeOf(object)`
    - 返回对象是否位于 object 的原型链中
- Object.prototype.toString：`toString()`
    - 查询**运行时 this**的 Type（对象类型），返回 `"[object Type]"`
    - 若对象存在 `Symbol.toStringTag` 属性，则该属性值为 Type
    - 可将 运行时 this 指向 null、undefined，Type 分别为 Null、Undefined
- Object.prototype.toLocaleString：
- Object.prototype.valueOf：
    - 将**运行时 this**转换成对象，返回该对象