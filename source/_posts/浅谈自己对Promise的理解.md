---
title: 浅谈自己对Promise的理解
date: 2017-08-03 17:54:05
tags: [js,promise]
categores: js
---

一直以来虽然写了不少promise，总感觉有点一知半解，现在花些时间整理下。

# 前记 #
自己最早是从一篇介绍promise的文章看到的，记得文章中说promise可以使代码更整洁，避免陷入重重的回调当中。之后真正写是从angularjs中的$http服务中开始的，嗯，其中的写法很多，我一直用success和error来加载回调函数，现在看起来应该是then和catch的实现(或者是只用then方法即then中含有两个方法参数)，还有bootstrap UI for Angular组件中modal框部分，$modal.open().result返回的便是一个promise对象，在上面写关闭modal框后实现的方法

等等，现在又接触到angular2的observer了，

# 相关链接 #

>[promise小小书](http://liubin.org/promises-book/#promises-overview)

# Promise #

## 创建 ##

创建promise对象的流程如下所示。

1. new Promise(fn) 返回一个promise对象

2. 在fn 中指定异步等处理

3. 处理结果正常的话，调用resolve(处理结果值)

4. 处理结果错误的话，调用reject(Error对象)

举个书上的栗子，一看便懂

    function getURL(URL) {
        return new Promise(function (resolve, reject) {
            var req = new XMLHttpRequest();
            req.open('GET', URL, true);
            req.onload = function () {
                if (req.status === 200) {
                    resolve(req.responseText);
                } else {
                    reject(new Error(req.statusText));
                }
            };
            req.onerror = function () {
                reject(new Error(req.statusText));
            };
            req.send();
        });
    }
    // 运行示例
    var URL = "http://httpbin.org/get";
    getURL(URL).then(function onFulfilled(value){
        console.log(value);
    }).catch(function onRejected(error){
        console.error(error);
    });

首先，返回resolve就代表着接下来then的回调函数会调用，(也意味着catch中的回调函数不会执行了)

这有个angular2的例子，是个resolve很好的应用

    //返回的promise中的泛型，为返回data的格式
    queryLoans(query:any):Promise<{count:number,items:Loan[]}>{
        return this.myHttp.get({
          api:this.myHttp.api.loanList,
          query:query
        }).toPromise()
          .then((res)=>{
            let data={
              count:0,
              items:[]
            };
            if(res.ok){
              console.log(res);
              let result=res.json();
              if(result.status===200){
                data.count=result.body.paginator.totalCount;
                for(let l of result.body.records){
                  let loan=new Loan();
                  loan.borrowApplyId=l.borrowApplyId;
                  loan.memberId=l.memberId;
                  loan.companyName=l.companyName;
                  loan.applyAmount=parseFloat(l.applyAmount);
                  loan.approveAmount=parseFloat(l.approveAmount);
                  loan.productId=l.productId;
                  loan.borrowHowLong=l.borrowHowlong;
                  loan.repaymentWay=l.repaymentWay;
                  loan.cardId=l.cardId;
                  loan.cardNo=l.cardNo;
                  loan.isContract=!!parseFloat(l.isContract);
                  loan.status=parseFloat(l.status);
                  loan.createTime=l.createTime;
                  loan.auditOneTime=l.auditOneTime;
                  loan.auditOneBy=l.auditOneBy;
                  data.items.push(loan);
                }
              }
            }
            return Promise.resolve(data);
          });
    }


angular2使用typescript来写的，ts可以说是强类型了，这个方法是用来获取接口的数据，并对接收的数据进行了一定的格式化，最后把格式化后的数据放在promise中的resolve中返回，angular2中这种和数据打交道的方法一般都放置在服务中，组件作为服务的最大消费者来调用它。



## Promise的状态 ##

实例化的promise对象只有三种状态，成功，失败，初始化状态

* "has-resolution" - Fulfilled
resolve(成功)时。此时会调用 onFulfilled

* "has-rejection" - Rejected
reject(失败)时。此时会调用 onRejected

* "unresolved" - Pending
既不是resolve也不是reject的状态。也就是promise对象刚被创建后的初始化状态等

## Promise.all ##

Promise.all 接收一个 promise对象的数组作为参数，当这个数组里的所有promise对象全部变为resolve或reject状态的时候，它才会去调用 .then 方法。

    Promise.all([promise1,promise2],...).then(res=>{})

res接收各个结果组成的数组，

个人觉得这个会应用于接口间顺序的请求渲染，比如编辑页面的初始渲染可能会依赖于一些字典接口查询后的结果。或者对多个接口数据请求后的统一处理。

## Promise.race ##

Promise.race 也同样接收一个promise对象的数组作为参数，区别是Promise.race只要有一个promise对象进入 FulFilled 或者 Rejected 状态的话，就会继续进行后面的处理

>这样的话，之后的then或catch接收的参数就只有最快进入FulFilled 或 Rejected 状态的promise返回的数据了