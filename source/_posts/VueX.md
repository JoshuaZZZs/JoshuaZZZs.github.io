---
title: VueX
date: 2020-1-21 18:38:45
categories: 
	- Webǰ��
tags: 
    - Vue
    - VueX
---

# VueX ���(���۲���)

## һ��VueX �ĸ���

### 1.�ο��ٷ��ĵ��� Vuex ��һ��ר��Ϊ Vue.js ��Ƶ�Ӧ�ó���״̬����ģʽ�����ü��д洢����Ӧ�õ����������״̬��������Ӧ�Ĺ���֤״̬��һ�ֿ�Ԥ��ķ�ʽ�����仯������˵��λ������¼����ؼ��㣺

    1.״̬����ģʽ
    2.��Ӧ�Ĺ���Ϳ�Ԥ��ķ�ʽ

����һһ������һ�£�

#### 1.״̬����ģʽ

�Դ� vuex ���ṩ��һ���ٷ�������˵����

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

    ���������ܿ�����ʵ�Ϲ���vuex�ܹ��������󲿷֣�
    1.`data���ִ������Ҳ�����������������Դ`-state
    2.`template���ִ�ŵ���html��ǩҲ������ͼ`-view
    3.`methods���ִ����صĺ���Ҳ�����������ݸı䷽ʽ`-actions

������������Ϲ�ͬ������һ��״̬�Թ���Ӧ�ò��Ե����������ķ�ʽ���У����Ա�֤���ݵļ�༴��
![����������](VueX/flow.png "����������")

#### 2.��Ӧ�Ĺ���Ϳ�Ԥ��ķ�ʽ
���������ϸ�������Ҳ�ܺ���⣬״̬����ģʽ�µ����ݵļ���п�����ĳЩӦ�ó����б��ƻ���
`1.�����ͼ����ͬһ״̬`
`2.�����ͬ����ͼ��Ϊ����Ҫ����ͬһ״̬`

- �������� 1.һ�㶼�Ǹ�������Ƕ�ף����� 2.һ�㶼�Ǹ��������ֵ�� bus �¼������������Ͻ���취�����ᵼ�´����߳�����ά�����в���Ҫ�������������⡣
- ��ʱ��һ����˼·�����������⣺�����й����״̬�������������������λ�õ���ͼ�����Ի�ȡ�͸��Ķ�Ӧ״̬��ͨ�����������״̬����ĸ��ͨ��ǿ�ƵĹ�������֤��ͼ��״̬�Ķ����ԣ������ͱȽ������Ľ����������⣬��������һ��ȫ�ֵ�������ģʽ������ķ�ʽҲ�� v ʱ uex �Ļ������˼·
  ���ڴ� vuex �ƶ�������ص�ǿ�Ʒ������Ա������и��Ĺ���״̬����Ϊ�������ͬʱά������

## ����VueX ��ʹ�ó���
vuex ������ͳһ����״̬�ĵ������Ǹ�ʲôʱ��ʹ���أ�һ���������ձ���������˵�����д�����Ŀ�лῼ��ʹ�ã������Ҿ��Ų�̫׼ȷ�����ҵ�������Ƿ�ʹ�� vuex Ӧ���������Ŀ���Ƿ������ vuex �����������⣬������Ƴ������ʲô�����������ʲô�����Ը����Ҳ���ľ���򵥵�������һ��ʹ�ó�����
`1.����ṹ���Ӷ��ֽ��������ﳵ���б����...`
`2.����ظ����������ɵ���Ҫ�������ݣ�������...`
`3.����������������������ͼ��ȫ�ַ��...`

## ����vueX ���ĸ��� ###�ȿ����ٷ��ĵ��е�ͼ��
![����](VueX/vuex.png "VueX")
�� 3 ������`State`��`Action`��`Mutaiton`�ټ���`Getter`��`Moudule`�ܼ��� 5 ��,һһ����һ��

#### 1.State

- `���`State ��Ϊ����ģʽ�����µĸ����ȫ����Ψһ�Ķ���ʽ�����ڣ���ΪΨһ������Դ����ܹ������Ҫ�����״̬
- `��Vuex�еĶ����ʹ�ã�`

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

- `��������mapState��`һ����˵�� computed ��ʹ�ö���չ������������״̬���ɶ�Ӧ�ļ�������

```
computed: {
localComputed () { /* ... */ },
// ʹ�ö���չ����������˶�����뵽�ⲿ������
...mapState({
  // ...
})
}
```

#### 2.Mutation

- `���`Mutation �� Vuex ����Ψһ�ܶ� State ״̬���в��������ԣ��������Ÿ��ֶ� State ��״̬�ĸ��ֲ������� vue �е� methods �����ƣ����ԶԱ�����⡣
- `��Vuex�еĶ�����ʹ�ã�`
  ```
  const store = new Vuex.Store({
  state: {
  count: 1
  },
  mutations: {
  increment (state) {
    // ���״̬
    state.count++
  }
  }
  })
  ```

`1.����`:�ɴ�Ҳ�ܿ������� mutation �еĸ��ַ��������¼�����(type)+�ص�����(handler)��ɣ������ڷ�����+������
`2.����`:store.commit(Type ��)������

- `�ύ�غ�(PayLoad)`:�����ǵ��� Mutation ��Ӧ����ʱ�������� Type ���ƻ������ں��洫����������������ʹ����Ҳ�������������ͣ������ں����еĲ����б�
- `�������ύ`���� commit ʱֱ�Ӱ� Type �ͺ���д��ͬһ��������

```
 store.commit({
  type: 'increment',
  amount: 10
  })
```

`3.ͬ���ԣ�`�� Mutation �еĲ���������ͬ���ģ�Ϊ�˱�֤�� state ״̬��ȷ�����޸�
`4.��������MapMutation:`һ����˵��Ӧ�� Methods ��ʹ�ö���չ������ Mutation �еĶ�Ӧ����ӳ�䵽�ֲ������

```
import { mapMutations } from 'vuex'

export default {
 // ...
 methods: {
   ...mapMutations([
     'increment', // �� `this.increment()` ӳ��Ϊ `this.$store.commit('increment')`

     // `mapMutations` Ҳ֧���غɣ�
     'incrementBy' // �� `this.incrementBy(amount)` ӳ��Ϊ `this.$store.commit('incrementBy', amount)`
   ]),
   ...mapMutations({
     add: 'increment' // �� `this.add()` ӳ��Ϊ `this.$store.commit('increment')`
   })
 }
}
```

#### 3.Action

- `����`:Action �����ʺ� Mutation ����һ�������������� Action �п��԰���ͬ���첽������ͬʱ Action ������ֱ�Ӷ� State ���в�����Ҫ�ύ�ĵ� Mutation������ Mutation �еķ����� State ���в���

- `��VueX�еĶ�����ʹ��`

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

`1.������`�� Mutation ��ͬ
`2.����(�ַ�)��`context.dispatch(Type ��)
`3.�ύ���أ�`�� Mutation ��ͬ
`4.�������ύ��`�� Mutation ��ͬ

- `�첽��`���� Action ���첽������ɺ󷵻ص���һ�� Promise ������Ҫʵ�ֶ� State �Ĳ������������м��� commit ͬ������
- `��������MapAction��`�� Mutation ��ͬ
  ####4.Getter
- `���`����Ҫ�� Store ������ state �Ĳ������Խ���ĳЩ����֮�����ɵ��ų����Դ���� getter ���棬������ Computed��

```
   computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

- `��Vuex�еĶ�����ʹ��`
  `1.����:`����+�ύ����+�ص�����
  `2.���ã�`
  `1ͨ�����Է���`��Getter �ᱩ¶һ�� store.getters ����ͨ���ö���.��Ӧ���Ե���ʽ����
  `store.getters.doneTodos`
  `2.ͨ���������ʣ�`�� getters ����һ���������ڵ���ʱ������Ӧ����ͨ�� getters �ж�Ӧ�����Լ�ӵĶ� store �е�ĳЩ���Խ��в���

```
     getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
```

- `MapperGetter����������`�ö���չ������ computed ���Խ���ӳ�䣬��ʱ��Ҫ��������Ҫ�ö������ʽ���½���

```
import { mapGetters } from 'vuex'

export default {
 // ...
 computed: {
 // ʹ�ö���չ��������� getter ���� computed ������
   ...mapGetters([
     'doneTodosCount',
     'anotherGetter',
     // ...
   ])
 }
}
```

#### 5.Moudule

- `���`��Ӧ�ù��ڸ���ʱ store ��������쳣��ӷ�ף�Ϊ�˽������������Ҫʹ�õ� Vuex �е� Moudule ����(ģ��)�����ڶԸ��ӵ� store �����иʹ֮�ָ��һ��������ģ�飬ÿ��ģ��ӵ�ж����� state,Mutation,��ͬʱģ���ڲ�Ҳ�ܹ��໥Ƕ�ף������� vue �е���� components �ĸ���

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
store.state.a // -> moduleA ��״̬
store.state.b // -> moduleB ��״̬
```

- `Moudule�ڵľֲ�״̬��`�ڲ�ͬĳ���л�ȡ��Ĭ�϶���������Ҳ������ģ���ڲ��������ڲ�ͬ����� this
  `1.�ֲ�״̬�µ�Mutation��state:`ʹ�÷�ʽ��ȫ����û������
  `2.�ֲ�״̬�µ�action��getters��`�ھֲ�״̬�½��յĲ����еĵ������Ǿֲ�״̬�ĸ��ڵ�
- `Module�����ռ䣺`
  `���ݣ�`ͨ���� moudule ����� namespace:true �����ɶ�Ӧ���Ƶ�ģ�飬��ģ�鱻ע���ģ���ڵ��������ݶ������������vue ��ֳ��������� name:'xx'���ö�Ӧ��������·���ı䣩
- `�������ռ��Module������ķ���`
  `1.Module����ȫ������`
  `1.1.`ʹ��ȫ�ֵ� state �� getter���ھֲ�״̬�н��յ������ĸ������ֱ����ȫ��״̬��ȫ�� getters��Ҳ��ͨ����Ӧ context �� store ���뷽��
  `1.2`ʹ��ȫ�ֵ� mutation �� action���ھֲ������н�����Ϊ�����������������Ӧ�� dispatch �� commit
  `1.3`�ֲ�ע��ȫ�� action:�� root:true ��Ϊ������� handler ��

```
 modules: {
  foo: {
   namespaced: true,
   getters: {
     // �����ģ��� getter �У�`getters` ���ֲ�����
     // �����ʹ�� getter �ĵ��ĸ����������� `rootGetters`
     someGetter (state, getters, rootState, rootGetters) {
       getters.someOtherGetter // -> 'foo/someOtherGetter'
       rootGetters.someOtherGetter // -> 'someOtherGetter'
     },
     someOtherGetter: state => { ... }
   },

   actions: {
     // �����ģ���У� dispatch �� commit Ҳ���ֲ�����
     // ���ǿ��Խ��� `root` �����Է��ʸ� dispatch �� commit
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

`2.�ⲿ����Moudule�ֲ����ݣ�`
`1.ͨ�������������ʣ�`
`2.ͨ��createNamespacedHelpers���ʣ�`ͨ������ vue �е� createNamespacedHelpers�����ص���һ���������л�����������������ĸ����������������Ӧ�ú��ȡ�������������

```
import { createNamespacedHelpers } from 'vuex'

const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')

export default {
 computed: {
   // �� `some/nested/module` �в���
   ...mapState({
     a: state => state.a,
     b: state => state.b
   })
 },
 methods: {
   // �� `some/nested/module` �в���
   ...mapActions([
     'foo',
     'bar'
   ])
 }
}
```

## VueX ��������

### �ϸ�ģʽ

- `���`���ϸ�ģʽ�»���ȼ������״̬����������״̬�ı�Ķ������������� mutation ���ᱨ��
- `���ã�`�� store ����ʱ����`strict��true
- `ע�����`
  `�ϸ�ģʽ�µı�����`���� input ȥ�� vuex �е�״̬ʱ���ڸı䲻���� mutation ��������Իᱨ��Ҫ������������һ����˵ �� input �� change �¼����ڶ�Ӧ������ʹ�� commit �ύ
  ���߰�һ����������ʹ�� get ������ȡ״̬ʹ�� set ��������״̬`

#### ������� VueX ��Ӧ�ĸ���󣬽�������Ӧ�ü���ʵս�ˣ�������ʱ��Ļ����Ჹ��һƪ���� VueX ��ʹ�����飬�����ڴ�
