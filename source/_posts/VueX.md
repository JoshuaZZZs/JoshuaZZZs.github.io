---
title: VueX
date: 2020-1-21 18:38:45
categories: 
	- Web前端
tags: 
    - Vue
    - VueX

---

# VueX 详解(理论部分)

## 一、VueX 的概念

### 1.参考官方文档： Vuex 是一个专门为 Vue.js 设计的应用程序状态管理模式。采用集中存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。简单来说这段话有以下几个关键点：

    1.状态管理模式
    2.相应的规则和可预测的方式

下面一一来介绍一下：

#### 1.状态管理模式

对此 vuex 的提供了一个官方案例来说明：

```
new Vue({
 // state
 data () {
   return {
     count: 0
   }
 },
 // view
 template: `
   <div>{{ count }}</div>
 `,
 // actions
 methods: {
   increment () {
     this.count++
   }
 }
})
```

    分析上例能看到事实上构成vuex总共有三个大部分：
    1.`data部分存放数据也就是驱动程序的数据源`-state
    2.`template部分存放的是html标签也就是视图`-view
    3.`methods部分存放相关的函数也就是驱动数据改变方式`-actions

这三个部分组合共同构成了一个状态自管理应用并以单向数据流的方式运行，用以保证数据的简洁即：
![单向数据流](VueX/flow.png "单向数据流")

#### 2.相应的规则和可预测的方式

明白了以上概念后这个也很好理解，状态管理模式下的数据的简洁行可能在某些应用场景中被破坏：
`1.多个视图依赖同一状态`
`2.多个不同的视图行为都需要更改同一状态`

- 对于问题 1.一般都是各组件多层嵌套，问题 2.一般都是父子组件传值或 bus 事件监听但是以上解决办法不但会导致代码冗长难以维护还有不必要的性能消耗问题。
- 此时有一个新思路来解决这个问题：把所有共享的状态单独抽离出来这样任意位置的视图都可以获取和更改对应状态，通过定义与隔离状态管理的概念并通过强制的规则来保证视图和状态的独立性，这样就比较完美的解决了这个问题，而这种以一个全局单例管理模式来管理的方式也是 v 时 uex 的基本设计思路
  基于此 vuex 制定出了相关的强制方法，以便让所有更改共享状态的行为简洁明了同时维护方便

## 二、VueX 的使用场景

vuex 是用来统一管理状态的但是我们该什么时候使用呢？一般来我们普遍意义上来说仅在中大型项目中会考虑使用，但是我觉着不太准确，在我的理解中是否使用 vuex 应当解决与项目中是否会遇到 vuex 方便解决的问题，它被设计出来解决什么就用它来解决什么，所以根据我不多的经验简单的罗列了一下使用场景：
`1.组件结构复杂多种交互：购物车，列表操作...`
`2.组件重复销毁与生成但需要留存数据：弹窗，...`
`3.多个触发方法，多个依赖视图：全局风格...`

## 三、vueX 核心概念 ###先看看官方文档中的图例

![流程](VueX/vuex.png "VueX")
有 3 个概念`State`，`Action`，`Mutaiton`再加上`Getter`和`Moudule`总计有 5 个,一一来看一下

#### 1.State

- `概念：`State 作为单例模式管理下的概念，以全局且唯一的对象方式而存在，作为唯一的数据源里仅能够存放需要共享的状态
- `在Vuex中的定义和使用：`

```
const Counter = {
 template: `<div>{{ count }}</div>`,
 computed: {
   count () {
     return this.$store.state.count
   }
 }
}
```

- `辅助函数mapState：`一般来说在 computed 中使用对象展开运算符将多个状态生成对应的计算属性

```
computed: {
localComputed () { /* ... */ },
// 使用对象展开运算符将此对象混入到外部对象中
...mapState({
  // ...
})
}
```

#### 2.Mutation

- `概念：`Mutation 在 Vuex 中是唯一能对 State 状态进行操作的属性，里面存放着各种对 State 中状态的各种操作，和 vue 中的 methods 很类似，可以对比着理解。

- `在Vuex中的定义与使用：`

  ```
  const store = new Vuex.Store({
  state: {
  count: 1
  },
  mutations: {
  increment (state) {
    // 变更状态
    state.count++
  }
  }
  })
  ```

`1.声明`:由此也能看到的是 mutation 中的各种方法都由事件类型(type)+回调函数(handler)组成，类似于方法名+函数体
`2.调用`:store.commit(Type 名)来调用

- `提交载荷(PayLoad)`:在我们调用 Mutation 对应函数时除了输入 Type 名称还可以在后面传入其他参数，可以使对象也可以是其他类型，类似于函数中的参数列表
- `对象风格提交`：在 commit 时直接把 Type 和荷载写在同一个对象中

```
 store.commit({
  type: 'increment',
  amount: 10
  })
```

`3.同步性：`在 Mutation 中的操作必须是同步的，为了保证对 state 状态的确定性修改
`4.辅助函数MapMutation:`一般来说对应在 Methods 中使用对象展开符将 Mutation 中的对应方法映射到局部组件中

```
import { mapMutations } from 'vuex'

export default {
 // ...
 methods: {
   ...mapMutations([
     'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

     // `mapMutations` 也支持载荷：
     'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
   ]),
   ...mapMutations({
     add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
   })
 }
}
```

#### 3.Action

- `概念`:Action 的性质和 Mutation 几乎一样，但区别在于 Action 中可以包含同，异步方法。同时 Action 并不能直接对 State 进行操作需要提交的到 Mutation，在由 Mutation 中的方法对 State 进行操作

- `在VueX中的定义与使用`

```
const store = new Vuex.Store({
 state: {
   count: 0
 },
 mutations: {
   increment (state) {
     state.count++
   }
 },
 actions: {
   increment (context) {
     context.commit('increment')
   }
 }
})
```

`1.声明：`与 Mutation 相同
`2.调用(分发)：`context.dispatch(Type 名)
`3.提交荷载：`与 Mutation 相同
`4.对象风格提交：`与 Mutation 相同

- `异步性`：在 Action 的异步方法完成后返回的是一个 Promise 对象，若要实现对 State 的操作必须在其中加入 commit 同步方法
- `辅助函数MapAction：`与 Mutation 相同
  ####4.Getter
- `概念：`当需要对 Store 对象中 state 的部分属性进行某些操作之后生成的排成属性存放在 getter 里面，类似于 Computed。

```
   computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

- `在Vuex中的定义与使用`
  `1.声明:`名称+提交荷载+回调函数
  `2.调用：`
  `1通过属性访问`：Getter 会暴露一个 store.getters 对象通过该对象.对应属性的形式访问
  `store.getters.doneTodos`
  `2.通过方法访问：`让 getters 返回一个函数并在调用时传入相应参数通过 getters 中对应的属性间接的对 store 中的某些属性进行操作

```
     getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
```

- `MapperGetter辅助函数：`用对象展开符将 computed 属性进行映射，此时若要重命名需要用对象的形式重新接收

```
import { mapGetters } from 'vuex'

export default {
 // ...
 computed: {
 // 使用对象展开运算符将 getter 混入 computed 对象中
   ...mapGetters([
     'doneTodosCount',
     'anotherGetter',
     // ...
   ])
 }
}
```

#### 5.Moudule

- `概念：`当应用过于复杂时 store 对象会变得异常的臃肿，为了解决这个问题就需要使用到 Vuex 中的 Moudule 属性(模块)，用于对复杂的 store 进行切割，使之分割成一个独立的模块，每个模块拥有独立的 state,Mutation,等同时模块内部也能够相互嵌套，类似于 vue 中的组件 components 的概念

```
 const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})
store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

- `Moudule内的局部状态：`在不同某块中获取的默认对象作用域也仅限于模块内部，类似于不同组件的 this
  `1.局部状态下的Mutation和state:`使用方式和全局下没有区别，
  `2.局部状态下的action和getters：`在局部状态下接收的参数中的第三个是局部状态的根节点
- `Module命名空间：`
  `内容：`通过在 moudule 中添加 namespace:true 来生成对应名称的模块，当模块被注册后模块内的所有内容都会调整命名（vue 拆分出组件后加入 name:'xx'会让对应所有内容路径改变）
- `带命名空间的Module与内外的访问`
  `1.Module访问全局内容`
  `1.1.`使用全局的 state 与 getter，在局部状态中接收第三第四个参数分别代表全局状态，全局 getters，也会通过对应 context 或 store 传入方法
  `1.2`使用全局的 mutation 和 action，在局部方法中将其作为第三个参数并传入对应的 dispatch 或 commit
  `1.3`局部注册全局 action:将 root:true 作为定义放在 handler 中

```
 modules: {
  foo: {
   namespaced: true,
   getters: {
     // 在这个模块的 getter 中，`getters` 被局部化了
     // 你可以使用 getter 的第四个参数来调用 `rootGetters`
     someGetter (state, getters, rootState, rootGetters) {
       getters.someOtherGetter // -> 'foo/someOtherGetter'
       rootGetters.someOtherGetter // -> 'someOtherGetter'
     },
     someOtherGetter: state => { ... }
   },

   actions: {
     // 在这个模块中， dispatch 和 commit 也被局部化了
     // 他们可以接受 `root` 属性以访问根 dispatch 或 commit
     someAction ({ dispatch, commit, getters, rootGetters }) {
       getters.someGetter // -> 'foo/someGetter'
       rootGetters.someGetter // -> 'someGetter'

       dispatch('someOtherAction') // -> 'foo/someOtherAction'
       dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

       commit('someMutation') // -> 'foo/someMutation'
       commit('someMutation', null, { root: true }) // -> 'someMutation'
     },
     someOtherAction (ctx, payload) { ... }
   }
 }
}
```

`2.外部访问Moudule局部内容：`
`1.通过辅助函数访问：`
`2.通过createNamespacedHelpers访问：`通过引入 vue 中的 createNamespacedHelpers，返回的是一个对象其中会包含给定明明看见的辅助函数类似于组件应用后获取引入组件的内容

```
import { createNamespacedHelpers } from 'vuex'

const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')

export default {
 computed: {
   // 在 `some/nested/module` 中查找
   ...mapState({
     a: state => state.a,
     b: state => state.b
   })
 },
 methods: {
   // 在 `some/nested/module` 中查找
   ...mapActions([
     'foo',
     'bar'
   ])
 }
}
```

## VueX 引申内容

### 严格模式

- `概念：`在严格模式下会深度监测所有状态，凡是引起状态改变的动作不是来自于 mutation 都会报错
- `启用：`在 store 创建时传入`strict：true
- `注意事项：`
  `严格模式下的表单处理：`当用 input 去绑定 vuex 中的状态时由于改变不是有 mutation 处理的所以会报错，要解决此这个问题一般来说 给 input 绑定 change 事件，在对应函数中使用 commit 提交
  或者绑定一个计算属性使用 get 方法获取状态使用 set 方法更改状态`

#### 在理解了 VueX 相应的概念后，接下来就应该加入实战了，后面有时间的话还会补充一篇关于 VueX 的使用详情，敬请期待