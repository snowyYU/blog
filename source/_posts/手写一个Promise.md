---
title: 手写一个Promise
date: 2020-08-19 20:36:17
tags: [promise]
---

尝试实现一个符合规范的 promise，含大量注释和新得～

<!--more-->

```javascript
const { count } = require('console')

const promisePractice = new Promise((resolve, reject) => {
  setTimeout(() => {
    return resolve(12)
  }, 4000)
})
promisePractice
  .then((data) => {
    console.log(data)
  })
  .finally((data) => {
    console.log(data)
  })

console.log(Promise)

// 区分 resolve() 和 reject() 方法传入的值是 promise 和 then() 方法的返回值 是 promise 的情况

const PENDING = 'PENDING'
const FULLFILLED = 'FULLFILLED'
const REJECTED = 'REJECTED'

class PromiseClass {
  constructor(fun) {
    // 状态初始化
    this._status = PENDING
    // value
    this._value = undefined
    // 添加成功回调函数队列
    this._fullfilledQueues = []
    // 添加失败回调函数队列
    this._rejectedQueues = []
    if (typeof v !== 'function') throw new Error('must accept a function as a parameter')
    try {
      fun(this._resolve.bind(this), this._reject.bind(this))
    } catch (error) {
      this._reject(error)
    }
  }
  _resolve(val) {
    const run = () => {
      if (this._status !== PENDING) return
      // 依次执行成功队列中的函数，并清空队列
      const runFullfilled = (value) => {
        let cb
        while ((cb = this._fullfilledQueues.shift())) {
          cb(val)
        }
      }
      // 依次执行失败队列中的函数，并清空队列
      const runrejected = (error) => {
        let cb
        while ((cb = this._rejectedQueues.shift())) {
          cb(error)
        }
      }

      if (val instanceof PromiseClass) {
        val.then(
          (value) => {
            this._value = value
            this._status = FULLFILLED
            runFullfilled(value)
          },
          (err) => {
            this._value = err
            this._status = REJECTED
            runrejected(err)
          }
        )
      } else {
        this._value = val
        this._status = FULLFILLED
        runFullfilled(val)
      }
    }

    // setTimeout(run,0)
    setTimeout(() => run(), 0)
  }
  _reject(err) {
    if (this._status !== PENDING) return

    this._status = REJECTED
    this._value = err
    const run = () => {
      let cb
      while ((cb = this._rejectedQueues.shift())) {
        cb(err)
      }
    }
    // setTimeout(run,0)
    setTimeout(() => run(), 0)
  }
  // then 方法必定返回一个 promise
  then(onFullfilled, onRejected) {
    const { _status, _value } = this
    return new PromiseClass((onFullfilledNext, onRejectedNext) => {
      // 封装成功时的回调函数
      let fullfilled = (value) => {
        try {
          if (typeof onFullfilled !== 'function') {
            onFullfilledNext(value)
          } else {
            let res = onFullfilled(value)
            if (res instanceof PromiseClass) {
              // 如果当前回调函数返回MyPromise对象，必须等待其状态改变后在执行下一个回调
              res.then(onFullfilledNext, onRejectedNext)
            } else {
              // 否则会将返回结果直接作为参数，传入下一个then的Fulfilled回调函数，并立即执行下一个then的Fulfilled回调函数
              onFullfilledNext(res)
            }
          }
        } catch (e) {
          onRejectedNext(e)
        }
      }
      // 封装失败时的回调函数
      let rejected = (err) => {
        try {
          if (typeof onRejected !== 'function') {
            onRejectedNext(err)
          } else {
            let res = onRejected(err)
            if (res instanceof PromiseClass) {
              // 如果当前回调函数返回Promise对象，必须等待其状态改变后在执行下一个回调
              res.then(onFullfilledNext, onRejectedNext)
            } else {
              // 否则会将返回结果直接作为参数，传入下一个then的rejected回调函数，并立即执行下一个then的rejected回调函数
              onRejectedNext(res)
            }
          }
        } catch (e) {
          onRejectedNext(e)
        }
      }
      switch (_status) {
        case PENDING:
          this._fullfilledQueues.push(onFullfilled)
          this._rejectedQueues.push(onRejected)
          break

        case FULLFILLED:
          fullfilled(_value)
          break

        case REJECTED:
          rejected(_value)
          break
      }
    })
  }
  catch(reject) {
    return this.then(null, reject)
  }
  static resolve(val) {
    if (val instanceof PromiseClass) {
      return val
    } else {
      return new PromiseClass((resolve) => {
        resolve(val)
      })
    }
  }
  static reject(err) {
    return new PromiseClass((resolve, reject) => {
      reject(err)
    })
  }
  static all(list) {
    return new PromiseClass((resolve, reject) => {
      const values = []
      count = 0
      for (let [index, promise] of list.entires()) {
        // 确保数组中的各项是 PromiseClass的实例，先调用 PromiseClass.resolve
        this.resolve(promise).then(
          (res) => {
            values[index] = res
            count++
            // 所有状态都为 fullfilled 时返回的PromiseClass状态变成 fullfilled
            if (count === list.length) resolve(values)
          },
          (err) => {
            // 有一个被rejected时返回的MyPromise状态就变成rejected
            reject(err)
          }
        )
      }
    })
  }
  static race(list) {
    return new PromiseClass((resolve, reject) => {
      for (let p of list) {
        this.resolve(p).then(
          (res) => {
            resolve(res)
          },
          (err) => {
            reject(err)
          }
        )
      }
    })
  }
  finally(cb) {
    return this.then(
      (value) => PromiseClass.resolve(cb()).then(() => value),
      (reason) =>
        PromiseClass.resolve(cb()).then(() => {
          throw reason
        })
    )
  }
}

```