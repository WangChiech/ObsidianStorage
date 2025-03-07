# 内置对象 Function

## 作为构造函数使用

语法

```
// 创建的函数在 全局作用域 执行

new Function([arg0, arg1, /* …, */ argN,] functionBody)

Function([arg0, arg1, /* …, */ argN,] functionBody)

```

## 作为对象使用

### 属性

**name**
```js
Object.getOwnPropertyDescriptor(Function, 'name')
// { value: 'Function', writable: false, enumerable: false, configurable: true }
```

**length**
```js
Object.getOwnPropertyDescriptor(Function, 'length')
// { value: 1, writable: false, enumerable: false, configurable: true }
```

length 存储函数期望的参数个数（形参个数），不包括默认参数与剩余参数，并且只包括第一个具有参数默认值之前或剩余参数之前的形参数量。

```
function fn (a, b, c = 4, d, ...e) {
    console.log(a, b, c, d , e)
}
console.log(fn.length) // 2
```

**prototype**

propertyDescriptors: 
`{ value: f, writable: false, enumerable: false, configurable: false }`

```
typeof Function.prototype // function

// 可调用，返回 undefined
(Function.prototype)()

// 不可 new
new (Function.prototype)() // Function.prototype is not a constructor
```


## Function.prototype

### 属性

**name**

`{ value: '', writable: false, enumerable: false, configurable: true }`

**length**

`{ value: 0, writable: false, enumerable: false, configurable: true }`

### 方法

#### call

**propertyDescriptors**

`{ value: f, writable: true, enumerable: false, configurable: true }`

**实现**

```
Function.prototype.customCall = function (context, ...args) {
  if (context == null) {
    context = window
  } else if (!['object', 'function'].includes(typeof context)) {
    context = Object(context)
  }
  context.fn = this
  const result = context.fn(...args)
  delete context.fn
  return result
}
// eval() + arguments 实现
Function.prototype.customCall1 = function (context) {
  if (context == null) {
    context = window
  } else if (!['object', 'function'].includes(typeof context)) {
    context = Object(context)
  }
  context.fn = this
  const args = []
  for (let i = 1; i < arguments.length; i++) {
    // 不采用 push(argments[i])，因 [undefined, null] + '' => ','
    args.push('arguments[' + i + ']')
  }
  // args 隐式转换为 string => arguments[1],arguments[2],...
  const result = eval('context.fn(' + args + ')')
  delete context.fn
  return result
}
```

#### apply

**propertyDescriptors**

`{ value: f, writable: true, enumerable: false, configurable: true }`

**实现**

```
Function.prototype.customApply = function (context, argsArrayLike) {
  if (context == null) {
    context = window
  } else if (!['object', 'function'].includes(typeof context)) {
    context = Object(context)
  }
  if (argsArrayLike == null) {
    argsArrayLike = []
  } else if (!['object', 'function'].includes(typeof argsArrayLike)) {
    throw new TypeError('CreateListFromArrayLike called on non-object')
  } else {
    argsArrayLike = Array.from(argsArrayLike)
  }
  context.fn = this
  const result = context.fn(...argsArrayLike)
  delete context.fn
  return result
}
```

#### bind

**propertyDescriptors**

`{ value: f, writable: true, enumerable: false, configurable: true }`

**实现**

```fn

```

#### toString

**propertyDescriptors**

`{ value: f, writable: true, enumerable: false, configurable: true }`

#### @@hasInstance

**propertyDescriptors**

`{ value: f, writable: false, enumerable: false, configurable: false }`

#### constructor

**propertyDescriptors**

`{ value: f, writable: true, enumerable: false, configurable: true }`

#### arguments

**propertyDescriptors**

`{ enumerable: false, configurable: true, get: f, set: f }`

#### caller

**propertyDescriptors**

`{ enumerable: false, configurable: true, get: f, set: f }`
