# Promise 实现

现代 JavaScript 中广泛使用的 Promise 规范是基于 Promises/A+ 的。这是对 Promises/A、Promises/B 和 Promises/D 的综合改进。Promises/A+ 保留了它们的核心设计理念，并进一步规范了 Promise 的行为。

- Promises/A 是基础的 Promise 规范，定义了基本的 then() 方法和 Promise 状态转换。
- Promises/B 扩展了 Promise 的链式调用和错误传播机制，使异步操作更加流畅。
- Promises/D 引入了异步回调机制，确保回调函数在事件循环的下一轮执行，避免同步调用带来的问题。
- Promises/A+ 是目前最完整、最规范的 Promise 规范，它规定了 Promise 的标准实现，包括如何处理异步回调、链式调用、错误传播等。

- 一个类，本质上是一个**代码执行器**，将代码执行相关信息维护到实例化的对象上
- 对象实例内部存储了状态、结果(成功/失败)、成功的回调队列、失败的回调队列
- 对象实例上有 `then() catch() finally()` 方法

```
const PENDING = 'PENDING'
const FULFILLED = 'FULFILLED'
const REJECTED = 'REJECTED'

class myPromise {
  status = PENDING
  value = null
  reason = null
  successCallbackList = []
  failCallbackList = []
  constructor(ctor) {
    try {
      ctor(this.resolve.bind(this), this.reject.bind(this))
    } catch (err) {
      this.reject(err)
    }
  }
  resolve(v) {
    if (this.status !== PENDING) return
    this.value = v
    this.status = FULFILLED
    while (this.successCallbackList.length) this.successCallbackList.shift()()
  }
  reject(v) {
    if (this.status !== PENDING) return
    this.reason = v
    this.status = REJECTED
    while (this.failCallbackList.length) this.failCallbackList.shift()()
  }
  then(successCallback, failCallback) {
    const thenRPromise = new myPromise((resolve, reject) => {
      successCallback = typeof successCallback !== 'function'
        ? () => this.value: successCallback
      failCallback = typeof failCallback !== 'function'
        ? () => { throw this.reason } : failCallback
      // status -> FULFILLED
      if (this.status === FULFILLED) {
        setTimeout(() => {
          try {
            const res = successCallback(this.value)
            handleThenRes(res, thenRPromise, resolve, reject)
          } catch (err) {
            reject(err)
          }
        })
      }
      // status -> REJECTED
      if (this.status === REJECTED) {
        setTimeout(() => {
          try {
            const res = failCallback(this.reason)
            handleThenRes(res, thenRPromise, resolve, reject)
          } catch (err) {
            reject(err)
          }
        })
      }
      // status -> PENDING
      if (this.status === PENDING) {
        this.successCallbackList.push(() => setTimeout(() => {
          try {
            const res = successCallback(this.value)
            handleThenRes(res, thenRPromise, resolve, reject)
          } catch (err) {
            reject(err)
          }
        }))
        this.failCallbackList.push(() => setTimeout(() => {
          try {
            const res = failCallback(this.reason)
            handleThenRes(res, thenRPromise, resolve, reject)
          } catch (err) {
            reject(err)
          }
        }))
      }
    })
    return thenRPromise
  }
}
function handleThenRes(res, thenRPromise, resolve, reject) {
  if (res === thenRPromise) throw 'find promise chain cycle'
  if (res instanceof myPromise) {
    res.then(resolve, reject)
  } else {
    resolve(res)
  }
}
```

## 应用
**手动触发**

sleep、halk函数
```
const engine = (cb) => {
  let _resolve;
  new Promise((resolve, reject) => {
    _resolve = resolve;
  }).then(res => {
    cb()
  })

  return {
    start: () => {
      _resolve()
    }
  }
}

let e = engine(() => {
  console.log("engine")
})

e.start()
```

**链式执行**

100 个 promise，10 个先执行，每 resolve 一个，加一个进去，形成 stream
```
const promiseArrGenerator = (num) => new Array(num).fill(0)
  .map((item, index) => () => new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(index)
    }, Math.random() * 1000)
  })
)

let arr = promiseArrGenerator(100);

// arr.map((fn) => {
//     fn().then(console.log)
// })
// Promise.all(arr.map(fn => fn())).then(res => console.log(res))

// 设计一个 promise Chain 链式调用

const promiseChain = (arr) => {
  arr.reduce((proChain, pro) => proChain.then(res => {
    ~res && console.log(res);
      return pro()
  }), Promise.resolve(-1))
}

promiseChain(arr)
```