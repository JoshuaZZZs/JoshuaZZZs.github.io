---
title: ES6语法总结(一)
date: 2019-10-13 19:21:50
tags:
	- 前端
	- ES6
categories:
	- web前端
---



# ES6法语总结

## 块级绑定（*let ，var，const*）

### 传统var的问题（*变量提升，块级作用域*）

var是用来声明各类型变量的关键字，但存在一个问题--**变量提升**（*在当前作用域内使用var关键词声明的变量都会提升到当前作用域的顶部*）例:

```javascript
function getValue(condition) {
if (condition) {
var value = "blue";
// 其他代码
return value;
} else {
// value 在此处可访问，值为 undefined
return null;
}
// value 在此处可访问，值为 undefined
}
```

在这里可以很明显的知道value虽然定义在了if为真后的语句块中但事实上在整个函数内部都能访问的到，这段代码实际上相当于

<!--more-->

```javascript
function getValue(condition) {

var value;
if (condition) {
value = "blue"
```

这就可能会产生一些意想不到的问题了，因此ES6中增加了一个**块级作用域**的概念（*就是让所声明的变量在指定块的作用域外无法被访问。块级作用域（词法作用域）在如下情况被创建：*
1. *在一个函数内部*
2. *在一个代码块（由一对花括号{}包裹）内部）* 

### let与const

#### 共同特性：



- 块级声明：两者均为块级声明即仅在当前定义语句行开始生效，以let为例：

- ```javascript
function getValue(condition) {
  if (condition) {
  //console.log(value)报错

   let value = "blue";

  //console.log(value) blue
eturn value;
  } else {
  
  return null;
}
  
  }
  ```

<!--more-->

- 禁止重复声明：在同一作用域内不能重复声明一个参数，否则会报错。

- 暂时性死区：使用let与const声明的变量会被存放到暂时性死区中，只有js引擎执行到该语句时声明语句才会把它移出，在此之前的任何对死区内变量进行的操作都会发生运行时异常（runtime error）

- 全局块级绑定：当在全局作用域上使用 var 时，它会创建一个新的全局变量，并成为全局对象（在浏览器中是 window ）的一个属性。这可能会无意中覆盖原本自带的属性例：

  ![](C:\Users\Administrator\Desktop\bolg\ES6\image\全局块级绑定1.png)

  RegExp原本是全局自带的函数 但是用var声明同名全局变量后，原本的RegExp就被新值代替了，这样就无法使用原本自带的正则函数，而let与const的全局绑定的时候会创建一个全新变量而屏蔽自带的同名变量，这样避免了全局属性被污染例：

  ![](C:\Users\Administrator\Desktop\bolg\ES6\image\全局块级绑定2.png)

需要注意的是在这种情况下使用自定义的变量直接变量名即可而需要使用全局自带方法时需要用window.

#### const特性

const关键字用于声明一个不可修改的常量，一般情况而言对于const声明的变量进行任何修改都会报错，除此之外还有一种情况下不会报错：**对象或数组等引用类型中对于属性值的修改**，只要不是对整个类型进行操作都是合法的例：

![](ES6\image\const声明.png)

## 函数

### 参数对象

- ES5的默认参数值问题：先看一个例子：

- ```javascript
  function getRequest(url, timeout, timeout) {
  timeout = timeout || 2000;
  allback = callback || function() {};
  // 函数的剩余部分
  }
  ```

 在这个函数中我们用||运算符给timeout，timeout都赋予了默认值，但是当timeout为0时由于0的布尔值为false所以会被设为2000但有时候其实是需要赋值为0的，所以在我们为函数赋默认值时有时候会有出现传0或‘’参数被覆盖的情况，解决这种情况可以用typeof===‘undefined’解决但是增加了很多代码量，所以es6中对这种情况进行了改进

- ES6中参数对象--(**具名对象**)
  - **语法**:`function makeRequest(url, timeout = 2000, callback = function() {}) }`ES6中一般在参数列表声明时赋与默认值，在这种情况下只有两种函数才会有默认值

1. ​		未传入第二三个对象
2. 传入undfined(这里的null是一个数据类型，因此传入null不会赋予默认值)
  
   - **消除对arguments对象的影响**：在ES5中在函数对与参数的任意操作都会影响到arguments对象里面的值，只有在ES5严格模式下才会消除影响而只要被启用了ES6规则都可以消除影响
   
   - **传递默认表达式**：在设置具名参数默认值的时候不仅可以使用基本参数类型还可以使用表达式，即
   
   - ```javascript
     function getValue() {
     return 5;
     }
     function add(first=1, second = getValue()) {
     return first + second;
     }
     console.log(add(1, 1)); // 2
     console.log(add(1)); // 6
     console.log(add(1)); // 7
     ```
   
   - 在这种情况下我们就能够动态的给具名参数赋值，也因此可以**默认值复用**即我们除了可以使用表达式赋值还能够使用前面的参数赋值`function add(first=1, second=first)`，但需要注意我们使用表达式的时候一定本那个忘记()否则只是传递了引用而非结果，前面的参数也不能复用后面参数的默认值
   
   - **暂时性死区**：在let和const中存在暂时性死区同样的，在声明具名参数的时候也存在暂时性死区，在赋值表达式的时候说了前面的参数无法复用后面的，原因就在此，以上面的函数为例，在js执行的时候参数的赋值方式是：`let first=1;let second=getValue()`,所以由于let与const的暂时性死区也造成了具名参数的暂时性死区

- ES6中参数对象--(**具名对象**)

  - 剩余参数对象

    - 语法：有...与紧跟的具名参数组成例

      ```javascript
      function pick(object, ...keys) {
      let result = Object.create(null);
      for (let i = 0, len = keys.length; i < len; i++) {
      result[keys[i]] = object[keys[i]];
      }
      return result;
      }
      ```

      keys就是一个剩余参数对象，他会包含object之后的所有参数对象

    - 使用的时候要注意两个点1.不能再剩余参数之后再声明参数，2.不能再settr字面量属性中使用

-   拓展运算符(...)
  
- 拓展运算符会便利参数对象所有可枚举的属性，拷贝到当前对象中，对象，数组。字符串等均可使用
  
- 箭头函数(=>)
  - 语法：`（）=>{函数体}`,当只有一个参数或者之有一条执行语句时可以省略括号，当有多个参数，无参数，函数体有多条语句时必须用`（）=>{}`的格式
  - 特点：
    - **没有this绑定**:箭头函数体内部的this与调用它的父级指向一致
    - **没有arguments绑定**：函数体内无法使用arguments访问它本身的参数对象，但是可以访问父级的arguments对象
    - **具名参数和不具名参数**:在箭头函数中都能正常使用

- 尾调用优化：

  - 语法：在函数的最后一句调用某函数的第一句(也可以是自己)例：

    ```javascript
    function Something() {
    return SomethingElse(); // 尾调用
    }
    ```

  - 作用：会清除当前函数的栈帧并重复使用以此提高性能在ES6严格模式中有以下情况会启用尾调用：

    1. 尾调用不能引用当前栈帧中的变量（意味着该函数不能是闭包）；
    2. 进行尾调用的函数在尾调用返回结果后不能做额外操作；
    3. 尾调用的结果作为当前函数的返回值。

  - 应用：一般是应用在递归函数，或者有复杂计算过程的函数时效果明显，

