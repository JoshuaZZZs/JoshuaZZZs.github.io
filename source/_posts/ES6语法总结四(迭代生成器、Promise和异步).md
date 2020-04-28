---
title: ES6语法总结(四)
date: 2020-01-20 18:54:12
tags:
	- 前端
	- ES6
categories:
	- web前端
	
---



# ES6语法总结

## Promise与异步

### Promise诞生的原因

前端和后端各种多样化交互才使各种程序拥有了使用的实际价值，而前后端交互绝大部分都属于异步编程，所以了解异步是对于前端来说是异常重要的一步，接下来先看一下传统的异步

- **传统的异步**：

  - 事件模型

  ```javascript
  let button = document.getElementById("btn");
      button.onclick = function(event) {
      console.log("Clicked");
  }
  ```
<!--more-->
  在这段代码中就是事件模型的一种，给id为`btn`的dom元素绑定了一个点击方法，但是它只会被编译而不会被执行，只有当该元素被点击之后才会js引擎才会把click的方法加入到作业队列的末尾，而所有类似这种绑定特定事件只有在特定条件下才会执行的模式叫做事件模式，这也是异步的一种。

  - 问题：对于这种模式来说当需要简单的交互时简单好用但是又两个问题
    - 每一个事件模型都需要追踪事件对象，有一百个点击事件就要监控一百个对象很麻烦
    - 未必能够完成绑定因为也可能绑定事件的时候dom还不存在
  - 回调模式

  ```javascript
  readFile("example.txt", function(err, contents) {
      if (err) {
     		 throw err;
     	 }
     		console.log(contents);
     });
  console.log("Hi!");
  ```

  `readFile()` 函数用于读取磁盘中的文件（由第一个参数指定），并在读取完毕后执行回调函数（即第二个参数）。如果存在错误，回调函数的 err 参数会是一个错误对象；否则 contents 参数就会以字符串形式包含文件内容。使用回调函数模式， `readFile()` 会立即开始执行，并在开始读取磁盘时暂停。这意味着`console.log("Hi!") `会在 `readFile()` 被调用后立即进行输出，要早于`console.log(contents) `的打印操作。当 `readFile() `结束操作后，它会将回调函数以及相关参数作为一个新的作业添加到作业队列的尾部。在之前的作业全部结束后，该作业才会执行,在这种情况下如果要进行异步串联就容易得多，但是会出现**回调地狱**的情况

```javascript
method1(function(err, result) {
       if (err) {
         throw err
       	 }
       method2(function(err, result) {
         if (err) {
           throw err
         }
         method3(function(err, result) {
           if (err) {
             throw err
           }
           method4(function(err, result) {
             if (err) {
               throw err
             }
             method5(result)
           })
         })
       })
     })
```

​	是不是看了就很想死...不但代码复杂高度耦合而且当你需要两个异步并行操作或者想同时开始两个异步的时候就	特别麻烦了，所以才有了Promise

- ### Promise

  #### Promise 生命周期

  ![1585030576405](ES6语法总结四(迭代生成器、Promise和异步)/Promise生命周期.png)

  在这里把几个意思都解释一下：

  1. 程序整体状态：
     - unsettled(未决):就是挂起状态，但是这个状态是用来描述程序的整体状况
     - settled(已决):程序已经执行完成
     - resolve(决议):推进为程序执行状态为已决
  2. 程序具体状态:
     - pending(挂起):异步程序尚未结束，等待结果中
     - fulfilled(完成):异步程序结束，操作完成
     - rejected(拒绝):异步程序结束，操作失败
  3. 推进状态操作函数
     - resolve():调用resolve()更改程序状态为已完成
     - reject():调用resolve()更改程序状态为已失败
  4. 状态完成函数
     - fulfillment handler:成功决议之后调用的函数
     - rejection handler :失败之后调用的函数

  在以上描述中描述程序具体状态的值会被设置在Promise对象的`PromiseState`属性中但是这个属性没有暴露所以不能访问，因此才需要`.then()`方法来对Promise的不同情况进行想要的操作

  **then()方法**：

  ​	在Promise状态为settled的时候会被调用，接收两个可选参数（都是回调函数的形式）,第一个是成功的回调函数，第二个是失败的回调函数，在失败的时候会默认传递一个包含有错误信息的对象进去。

  **catch()方法**:

  ​	 在这里的`promise.catch(function...)`<=>`promise.then(null,function..)`,默认传递rejection handler

​			**当没有写入都有的错误都会静默发生所以最好给promise都写一个默认的日志**

#### 创建Promise

##### 创建未决Promise

​	使用 Promise 构造器来创建。此构造器接受单个参数：执行器(executor)函数，包含初始化 Promise 的代码。该执行器会被传递两个名为 resolve()与 reject() 的函数作为参数

```javascript
let promise = new Promise(function(resolve, reject) {
    console.log("啊啊啊");
    resolve();
    if(err){
       reject();
	}
});
console.log("哦哦哦");
//啊啊啊
//哦哦哦
```

​	构造器会在promise实例化的时候立刻执行。然后才会发生而里面 resolve() 或 reject() 在执行器内部被调用	      时，一个作业被添加到作业队列中，以便决议（ resolve ）这个 Promise 。这也称为作业调度( job scheduling )，要注意一下被调度的作业会被放到队列的末尾最后执行

##### 创建已决Promise

​	Promise.resolve()和Promise.reject()的使用方式一样的所以只列举一个，另外一个照搬就行了

```javascript
let promise = Promise.resolve(42);
    promise.then(function(value) {
    console.log(value); // 42
});	
```

​	接受单个参数并返回一个处于完成态的 Promise 。没有任何作业调度会发生，并且需要向 Promise 添加一个或更多的完成处理函数来提取这个参数值。

##### 执行器错误

​	如果在执行器内部抛出了错误， Promise 的拒绝处理函数就会被调用，如果没有错误就会静默

#####全局的 Promise 拒绝处理

​	Promise有一个问题就是报错静默：当没有指定报错处理时发生的所有错误都不会被暴露和抛出而且你没有什么直观的办法去判断Promise的处理结果，而且你也不知道一个Promise对象什么时候会被处理

 ```javascript
let rejected = Promise.reject(42);

// 一段时间后……
	rejected.catch(function(value) {
// rejected 被处理
	console.log(value);
});

 ```

​	但是不用担心浏览器已经有了解决方案：

​	**浏览器的拒绝处理**：

当有有未处理的拒绝 时window 对象会触发两个事件：			

> `unhandledrejection` ：当一个 Promise 被拒绝、而在事件循环的一个轮次中没有任何拒绝处理函数被调用，该事件就会被触发；
> `rejectionHandled` ：若一个 Promise 被拒绝、并在事件循环的一个轮次之后再有拒绝处理函数被调用，该事件就会被触发。

以上两个浏览器事件的处理函数会接收到含下列属性的一个对象：

> type ： 事件的名称（ "unhandledrejection" 或 "rejectionhandled" ）；
> promise ：被拒绝的 Promise 对象；
> reason ： Promise 中的拒绝值（拒绝原因）。

#### 串联 Promise(Promise链)

```javascript
let p1 = new Promise(function(resolve, reject) {
	resolve(42);
});
p1.then(function(value) {
	console.log(value);
	}).then(function() {
	console.log("Finished");
});

```

对 p1.then() 的调用返回了第二个 Promise ，又在这之上调用了 then() 。仅当第一个Promise 已被决议后，第二个 then() 的完成处理函数才会被调用,每一次的加入作业队列时都会新生成一个promise

- **捕获错误**
  
  - Promise 链允许捕获上一个 Promise 的完成或拒绝处理函数中发生的错误
  
  ```javascript
  let p1 = new Promise(function(resolve, reject) {
  	resolve(42);
  });
  p1.then(function(value) {
  	throw new Error("Boom!");
  }).catch(function(error) {
  	console.log(error.message); // "Boom!"
  });
  ```
  
  在以上代码中， p1 的完成处理函数抛出了一个错误，链式调用指向了第二个 Promise 上的catch() 方法，能通过此拒绝处理函数接收前面的错误，前面如果是拒绝函数的话也是一样的。

- 在 Promise 链中返回值

  Promise 链的另一重要方面是能从一个 Promise 传递数据给下一个 Promise 的能力。传递给
  执行器中的 resolve() 处理函数的参数，会被传递给对应 Promise 的完成处理函数，

  ```javascript
  let p1 = new Promise(function(resolve, reject) {
  	resolve(42);
  });
  p1.then(function(value) {
  		console.log(value); // "42"
  		return value + 1;
  	}).then(function(value) {
  		console.log(value); // "43"
  });
  
  ```

  p1 的完成处理函数在被执行时返回了 value + 1 。由于 value 的值为 42 （来自执行
  器），此完成处理函数就返回了 43 。这个值随后被传递给第二个 Promise 的完成处理函数，

- 在 Promise 链中返回 Promise

  ```javascript
  let p1 = new Promise(function(resolve, reject) {
  	resolve(42);
  });
  let p2 = new Promise(function(resolve, reject) {
  	resolve(43);
  });
  p1.then(function(value) {
  		// 第一个完成处理函数
          console.log(value); // 42
          return p2;
  	}).then(function(value) {
  	// 第二个完成处理函数
  	console.log(value); // 43
  });
  ```

  在以上例子中如果p2执行出错的话会导致第二个完成处理函数永不被调用，这是因为在promise链中每当一个promise完成后下一个promise才会被加入作业队列，并且同时会很创建一个新的promise对象获取上一个对象的数据，  因此这以上代码中存在四个promise对象也就是

  ```javascript
  let p1 = new Promise(function(resolve, reject) {
  resolve(42);//第一个
  });
  let p2 = new Promise(function(resolve, reject) {
  resolve(43);//第二个
  });
  let p3 = p1.then(function(value) {
  // 第一个完成处理函数
  console.log(value); // 42
  return p2;
  });//第三个
  	p3.then(function(value) {
  // 第二个完成处理函数
  console.log(value); // 43
  });//第四个对象
  ```

  所以当p2报错时p3应当调用的是then()方法的第二个参数也就是拒绝函数而永不调用成功函数

#### 响应多个 Promise

- **Promise.all()**：
- **Promise.race() 方法**：