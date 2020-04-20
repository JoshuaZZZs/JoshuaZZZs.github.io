---
title: ES6语法总结(二)
date: 2019-11-21 18:54:12
tags:
	- 前端
	- ES6
categories:
	- web前端
	
---



# ES6语法总结

## 对象(Object)

- **属性初始化器速记**：在ES6中当对象字面量中的属性只有名称时， JS 引擎会在周边作用域查找同名变量。若找到，该变量的值将会被赋给对象字面量的同名属性。所以当属性和值相同时可以只写一个名称例：

  ```javascript
  function createPerson(name, age) {
      return {
          name,
          age
      };
  }
  
  ```
<!--more-->
- **方法简写**:在ES6为字面量定义函数时必须指定一个名称并用完整的函数定义来为对象添加方法，在ES6中也将其做了优化

  ```javascript
  var person = {
  name: "zhoushaung",
  sayName: function(){//es5 
  console.log(this.name);
  }
  };
  sayName(){//es6 
  console.log(this.name);
  }
  
  ```

- **可计算的属性名**:对象实例能使用“需计算的属性名”，只要用方括号表示法来代替小数点表示法即可,在ES6中的计算属性名是对象字面量语法的一部分所以可以直接用变量或字符串字作为属性名，同时在[]也可以使用表达式

  ```javascript
  var suffix = " man";
  var person = {
  [suffix+"name"]: "zhoushuang",
  [suffix+"gender"]: "male"
  };
  console.log(person["man name"]); // "zhoushuang"
  console.log(person["mane gender"]); //male
  ```

  

- **新增方法(Object.is(),Object.assign())**：

  - Object.is:用来弥补相等运算符的不足(`==`和`===`)，在一般相等运算符无法判定值相同类型不同的情况，在严格相等运算符中无法判定为 +0 与 -0 相等， NaN === NaN 等情况，而Object.is会判定两个数据的类型和值是否相等例

    ```javascript
    console.log(+0 == -0); // true
    console.log(+0 === -0); // true
    console.log(Object.is(+0, -0)); // false
    console.log(NaN == NaN); // false
    console.log(NaN === NaN); // false
    console.log(Object.is(NaN, NaN)); // true
    console.log(1 == 1); // true
    console.log(1 == "1"); // true
    console.log(1 === 1); // true
    console.log(1 === "1"); // false
    console.log(Object.is(5, 5)); // true
    console.log(Object.is(5, "5")); // false
    
    ```

    

  - Object.assign:用来对两个对象进行混入操作，该方法接受任意数量的参数并且会按照参数列表中的顺序来依次接收它们的属性，也就是说当有同名属性时可能会出现后来的参数覆盖前面的情况

    ```javascript
    var obj1 = {};
    Object.assign(obj1,
    {
    type: "shuaib",
    name: "zhoushuang"
    },
    {
    type: "handsome"
    }
    );
    console.log(obj1.type); // "handsome"
    console.log(obj1.name); // "zhoushuang"
    
    ```

- **重复的对象字面量属性**:当同一对象存在重复属性时，排在后面的属性的值会成为该属性的实际值而不会报错

- **可枚举属性的顺序(Object.getOwnPropertyNames())**：该方法会根据以下规则返回对象的属性：

  1. 所有的数字类型键，按升序排列。
  2. 所有的字符串类型键，按被添加到对象的顺序排列。
  3. 所有的符号类型（详见第六章）键，也按添加顺序排列。

```javascript
var obj = {a: 1,0: 1,c: 1,2: 1,b: 1,1: 1};
obj.d = 1;
console.log(Object.getOwnPropertyNames(obj).join("")); // "012acbd"
```

- **原型**：

  - 新增和修改原型：Object.setPrototypeOf()允许你修改任意指定对象的原型。它接受两个参数：需要被修改原型的对象，以及将会成为前者原型的对象, Object.getPrototypeOf() 方法从任意指定对象中获取其原型.

  - super调用原型的函数:

    ```javascript
    let person = {
    getGreeting() {
    return "Hello";
    }
    };
    let dog = {
    getGreeting() {
    return "Woof";
    }
    };
    let friend = {
    getGreeting() {
    return super.getGreeting() + ", hi!";
    }
    };
    // 将原型设置为 person
    Object.setPrototypeOf(friend, person);
    console.log(friend.getGreeting()); // "Hello, hi!"
    console.log(Object.getPrototypeOf(friend) === person); // true
    // 将原型设置为 dog
    Object.setPrototypeOf(friend, dog);
    console.log(friend.getGreeting()); // "Woof, hi!"
    console.log(Object.getPrototypeOf(friend) === dog); // true
    ```

    有点类似于java对父类方法的重写,在有多重继承的情况下会比较好使一点

## 数组

- **创建数组方法**：

  - Array.of():) 方法总会创建一个包含所有传入参数的数组，而不管参数的数量与类型

    ```javascript
    let items = Array.of(1, 2);
    console.log(items.length); // 2
    console.log(items[0]); // 1
    console.log(items[1]); // 2
    items = Array.of(2);
    console.log(items.length); // 1
    console.log(items[0]); // 2
    items = Array.of("2");
    console.log(items.length); // 1
    console.log(items[0]); 
    ```

    

  - Array.from():多用于类数组对象转化成为数组或者转化前进行额外操作时使用，接收三个参数

    1. 参数对象(arguments)：传入要进行转化的初始对象
    2. 操作方法(function)：如需额外加工，传入加工具体步骤
    3. 映射(object):如果function已经被定义或者需要进入额外对象方法，在此传入方法所在对象，此时的this就会指向被传入的对象

  ```javascript
  let helper = {
  diff: 1,
  add(value) {
  return value + this.diff;
  }
  };
  function translate() {
  return Array.from(arguments, helper.add, helper);
  }
  let numbers = translate(1, 2, 3);
  console.log(numbers); // 2,3,4
  ```

- **其他方法**:

  - **find()和findIndex()**:这两俩函数非常像所以把他们放在一起讨论，

    - 相同点：
      1. 都是在数组中查找符合要求的值
      2. 均接受两个参数：一个回调函数和一个可选值用于指定回调函数内部的 this 。该回调函数可接收三个参数：数组的某个元素、该元素对应的索引位置、以及该数组自身
      3. 均会在回调函数第一次返回 true 时停止查找
    - 区别
      1. find() 方法返回匹配的值
      2. findIndex() 方法则会返回匹配位置的索引

    ```javascript
    let numbers = [25, 30, 35, 40, 45];
    console.log(numbers.find(n => n > 33)); // 35
    console.log(numbers.findIndex(n => n > 33)); 
    ```

  - **fill() **:

    - 作用:用于填充数组中的一个或者多个参数
    - 语法:fill(number,firstIndex,lastIndex)：
      1. number：进行替换操作的数据，
      2. firstIndex，lastIndex:进行替换操作的初始位置和结束位置
    - 注:后两个参数是该数组的下标,当其为负是值为Array.length+firstIndex/lastIndex

  - **copyWithin()**:

    - 作用：和fill一样用于填充替换数组,但这个方法可以在数组内部复制自身元素
    - 语法:copyWithin(target,start,end):
      1. start:粘贴的起始位置
      2. target:从第几个位置作为粘贴项
      3. end:被覆盖元素的个数

```javascript
let numbers = [1, 2, 3, 4];
// 从索引 2 的位置开始粘贴
// 从数组索引 0 的位置开始复制数据
// 在遇到索引 1 时停止复制
numbers.copyWithin(2, 0, 1);
console.log(numbers.toString()); // 1,2,1,4
```

