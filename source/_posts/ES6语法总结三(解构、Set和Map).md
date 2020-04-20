---
title: ES6语法总结(三)
date: 2020-01-05 18:54:12
tags:
	- 前端
	- ES6
categories:
	- web前端
	
---



# ES6语法总结

## 解构

### 作用:

从对象或数组中获取信息、并将特定数据存入本地变量

### 对象解构：

- **简单的解构**:先从一段代码说起

  ```javascript
  let man = {
  age: "23",
  name: "zhoushuang"
  },
  age: "24",
  name = "zs";
  // 使用解构来分配不同的值
  ({ age, name } = man);//解构表达式
  console.log(age); // "23"
  console.log(name); // "zhoushuang"
  ```
<!--more-->
  在这个例子中 age与 name 属性在声明时被初始化，而两个同名变量也被声明并初始化为不同的值。接下来一行使用了解构表达式，通过读取 man对象来更改这两个变量的值。在这里必须用**圆括号包裹解构赋值语句**，这是因为暴露的花括号会被解析为代码块语句，而块语句不允许在赋值操作符左侧出现。圆括号标示了里面的花括号并不是块语句、而应该被解释为表达式，从而允许完成赋值操作。				

- **设置默认值**:同上代码中的赋值语句改成

  ```javascript
  ({ age, name,type:'handsome' } = man);//解构表达式+默认值
  ```

  在赋值的时候由于man对象中并没有名为type的属性所以会使用默认值同时要注意的是凡是在解构赋值在声明构造器中查找不到同名属性时都会变成undefined

- **赋值给本地变量名**：依旧是第一段代码的赋值语句改成

  ```javascript
  let { age: manage, name: manname='zs' } = man;//解构表达式+默认值+赋值给本地变量
  ```

  这样的话man里面的age和name属性分别对自定义的manage,manname进行了赋值操作， 该语法实际上与传统对象字面量语法相反，传统语法将名称放在冒号左边、值放在冒号右边；而在本例中，**名称在右边，需要进行值读取的位置被放在了左边。**

- **解构嵌套对象**:

  - **简单用法**：在日常开发中有时候会出现我们拿到的数据是一个多层嵌套的对象，而我们只需要获取其中的某些项的情况，这时自己写方法去遍历对象会比较麻烦，这个时候使用解构赋值会节省很多时间:

  ```javascript
  let man = {
  name: "zhoushuang",
  type: {
  habit: {
  sleep: true,
  eating: true
  },
  
  }
  };
  let { type: { habit }} = man;//嵌套解构赋值
  console.log(habit.sleep); // true
  console.log(habit.eating); // true
  ```
  在赋值结构中每当有一个冒号出现，就意味着冒号之前的标识符代表需要检查的位置，而冒号右侧则是赋值的目标。当冒号右侧存在花括号时，表示目标被嵌套在对象的更深一层中，因此`{ type: { habit }} = man`的运行顺序应该是:man->寻找type属性-> type属性需找habit属性->进行操作，每一个{}都可以简单的理解成又进行了一次简单解构赋值而这次的数据源就来自于冒号左边

  - 赋值给本地变量名**:同上一行代码中的赋值语句改成

    ```javascript
    let { type: { habit:manHabit }} = man;//嵌套解构赋值+赋值给本地变量
    ```

    其实和简单赋值的形式一样把habit整个对象都赋值给了本地变量manHabit不赘述

### 数组解构

总体来说数组解构赋值和对象解构赋值非常的相似，因此在此也只介绍数组解构的不同之处,其他情况参考上文。

- **简单解构赋值**:与对象相比语法不同’{}‘->'[]‘，并且在运行的时候不是查找同名属性而是直接按照参数的顺序进行赋值

  ```javascript
  let colors = [ "red", "green", "blue" ];
  let [ first, second ] = colors;
  console.log(first); // "red"
  console.log(second); // "green"
  
  ```

  在以上代码中能看到first和second分别获取了colors数组中的第一和第二个而这是根据参数各自的位置决定的，所以我们也可以使用占位符`，`来给获取指定的数据

  ```javascript
  let [,second ] = colors;
  console.log(second); // "green"
  ```

  

- 默认值:

  ```java
  let [ first, second= "pink" ] = colors;
  ```

- 参数交换:在ES6中可以运用解构赋值进行参数交换而无需中间变量

  ```javascript
  let a = 1,
  b = 2;
  [ a, b ] = [ b, a ];
  console.log(a); // 2
  console.log(b); // 1
  ```

- 嵌套解构:

  ```javascript
  let colors = [ "red", [ "green", "lightgreen" ], "blue" ];
  // 随后
  let [ firstColor, [ secondColor ] ] = colors;
  console.log(firstColor); // "red"
  console.log(secondColor); // "green"
  ```

  和对象嵌套时的原理一样,只不过在数组中不是根据属性名来查找而是根据参数的位置查找的,所以` [ firstColor, [ secondColor ] ] = colors;`也相当于进行了两次解构赋值操作,第一次`firstColor=red；[ secondColor ]= [ "green", "lightgreen" ]` ，第二次`secondColor =green`

需要注意的是在数组解构的时候如果有嵌套解构的情况但是对应的数据源只要不是可转成数组的值都会报错，如果可以转成数组会先将其转化成数组在进行赋值:

![](C:\Users\Administrator\Desktop\bolg\ES6\image\数组解构赋值.png)

- 剩余项赋值:因为数组中有arguments对象所以在数组解构中也可以使用

  ```javascript
  let colors = [ "red", "green", "blue" ];
  let [ firstColor, ...restColors ] = colors;
  console.log(firstColor); // "red"
  console.log(restColors.length); // 2
  console.log(restColors[0]); // "green"
  console.log(restColors[1]); // "blue"
  ```

## 混合解构(对象+数组)

这里其实就是把数组解构和对象解构的语法组合起来，只要注意逻辑和语法就行了，混合结构对于JSON格式的数据获取很有帮助

```javascript
let node = {
type: "Identifier",
name: "foo",
loc: {
start: {
line: 1,
column: 1
},
end: {
line: 1,
column: 4
}
},
range: [0, 3]
};
let {
loc: { start },
range: [ startIndex ]
} = node;
console.log(start.line); // 1
console.log(start.column); // 1
console.log(startIndex); // 0
```

## Set和Map

### Set(无重复值的有序列表)

- **创建set并添加内容**：

  使用new来创建，add方法添加项目

  ```javascript
  let set = new Set();
  set.add(5);
  set.add("5");
  console.log(set.size); // 2
  ```

  Set 不会使用强制类型转换来判断值是否重复。这意味着 Set 可以同时包含数值 5 与 字符串 "5" ，将它们都作为相对独立的项,因为这里使用的是Object.is() 方法，来判断两个值是否相等，唯一的例外是 +0 与 -0 在 Set 中被判断为是相等的）。你还可以向 Set 添加多个对象，它们也不会被合并为同一项

- **Set.has()**：用来搜索目标set中是否存在某一项，并返回结果(true/false)

- **Set.delete()**:用来删除目标set中的某一项，并返回结果(true/false)

- **Set.clear()**:用来删除目标set中的某一项，无返回

- **Set 转换为数组**：将数组转换为 Set 相当容易，因为可以将数组传递给 Set 构造器；而使用扩展运算符也能简
  单地将 Set 转换回数组

  ```javascript
  let set = new Set([1, 2, 3, 3, 3, 4, 5]),
  array = [...set];
  console.log(array); // [1,2,3,4,5]
  ```

- Weak Set：

  在以上转化数组的时候仔细观察就能发现，使用set转换数组时只是将set中的内容复制给了一个array而已，原set的内容中依旧还存在，对象存储在 Set 的一个实例中时，实际上相当于把对象存储在变量中。只要对于 Set 实例的引用仍然存在，所存储的对象就无法被垃圾回收机制回收，从而无法释放内存，所以为了解决这个问题也就引入了Weak Set

  - 创建 Weak Set:

    ```javascript
    let set = new WeakSet(),
    key = {};
    // 将对象加入 set
    set.add(key);
    console.log(set.has(key)); // true
    set.delete(key);
    console.log(set.has(key)); // false
    ```

    Weak Set 使用 WeakSet 构造器来创建，同时也包含了 add() 、 has()和 delete(),没有clear，该类型只允许存储对象弱引用，而不能存储基本类型的值。对象的弱引用在它自己成为该对象的唯一引用时，不会阻止垃圾回收，

  - Set 类型之间的关键差异：

    - 对于 WeakSet 的实例，若调用 add() 方法时传入了非对象的参数，就会抛出错误（has() 或 delete() 则会在传入了非对象的参数时返回 false ）；
    2. Weak Set 不可迭代，因此不能被用在 for-of 循环中；
    - Weak Set 无法暴露出任何迭代器（例如 keys() 与 values() 方法），因此没有任何编程手段可用于判断 Weak Set 的内容；
    4. Weak Set 没有 forEach() 方法；
    5. Weak Set 没有 size 属性。

### Map(键值对有序列表)

Map中的数据以键值对的形式存在,而键和值都可以是任意类型。键的比较使用的是Object.is() ，因此你能将 5 与 "5" 同时作为键，因为它们类型不同。这与使用对象属性作为键的方式（指的是用对象来模拟 Map ）截然不同，因为对象的属性会被强制转换为字符串。

- **Map方法**：
  - get(),set():调用 set() 方法并给它传递一个键与一个关联的值，来给 Map 添加项；此后使用键
    名来调用 get() 方法便能提取对应的值
  - has(key) ：判断指定的键是否存在于 Map 中；
  - delete(key) ：移除 Map 中的键以及对应的值；
  - clear() ：移除 Map 中所有的键与值。

- **Weak Map**:

  WeakMap 类型是键值对的无序列表，其中键必须是非空的对象，值则允许是任意类型。 WeakMap 的接口与 Map 的非常相似，都使用 set() 与 get() 方法来分别添加与提取数据

  - **Weak Map 的初始化**
    为了初始化 Weak Map ，需要把一个由数组构成的数组传递给 WeakMap 构造器。就像正规Map 构造器那样，每个内部数组都应当有两个项，第一项是作为键的非空的对象，第二项则是对应的值（任意类型）

  ```javascript
  let key1 = {},
  key2 = {},
  map = new WeakMap([[key1, "Hello"], [key2, 42]]);
  console.log(map.has(key1)); // true
  console.log(map.get(key1)); // "Hello"
  console.log(map.has(key2)); // true
  console.log(map.get(key2)); // 42
  ```
  - Weak Map 的方法

    has() 方法用于判断指定的键是否存在 delete() 方法则用于移除一个特定的键值对。

- Weak Map 的用法与局限性

  如果你只想使用对象类型的键。最好的选择就是 Weak Map 。因为它能确保额外数据在不再可用后被销毁，从而能优化内存使用并规避内存泄漏。但是 Weak Map 只为它们的内容提供了很小的可见度，因此不能使用 forEach() 方法、size 属性或 clear() 方法来管理其中的项。而在除此之外的其他情况，正常 Map是更好的选择，只是要注意对内存的使用。

