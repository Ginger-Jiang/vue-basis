## State

Vuex 使用 **单一状态树** 用一个对象就包含了全部的应用层级状态. 至此它便作为一个唯一数据源而存在. 意味着, 每个应用将仅仅包含一个 store 实例. 单一状态树让我们能够直接地定位任一特定的状态片段, 在调试的过程中也能轻易地取得整个当前应用状态的快照.

### 在组件中获得 Vuex 状态

由于 Vuex 的状态储存是响应式的, 从 store 实例中读取状态的最简单的方法就是在计算属性中返回某个状态:

```js
const Counter - {
  template: '<div>{{ count }}</div>'
  computed: {
    count() {
      return store.state.count
    }
  }
}
```

每当 `store.state.count` 变化的时候, 都会重新求取计算属性, 并且触发更新相关联的 DOM

然而, 这种模式导致组件依赖全局状态单例. 在模块化的构建系统中, 在每个需要使用 state 的组件中需要频繁的导入, 并且在测试组件时需要模拟状态

Vuex 通过 `store` 选项, 提供了一种机制将状态从根组件注入到每一个子组件中

```js
const app = new Vuex({
  el: 'app',
  store,
  components: { Counter },
  template: `
  <div class='app'>
    <counter></counter>
  </div>`
})
```

通过在根实例中注册 `store` 选项, 该 store 实例会注入到根组件下的所有子组件中, 且子组件能通过 `this.$store` 访问到

```js
const Counter = {
  template: '<div> {{ count }} </div>',
  computed: {
    count() {
      return this.$store.state.count
    }
  }
}
```

### mapState 辅助函数

当一个组件需要获取多个状态的时候, 将这些状态都声明为计算属性会有些重复和冗余, 为了解决这个问题, 我们可以使用 `mapState` 辅助函数帮助我们生成计算属性

```js
// 在单独构建的版本中辅助函数为 Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 箭头函数更简洁
    count: state => state.count,

    // 传字符串参数 count 等同于 state => state.count
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState(state) {
      return state.count + this.localCount
    }
  })
}
```

当映射的计算属性名称与 state 的子节点名称相同时, 我们也可以给 `mapState` 传一个字符串数组

```js
// 映射 this.count 为 store.state.count
computed: mapState(['count'])
```

### 对象展开运算符

`mapState` 函数返回的是一个对象, 可以通过展开运算符将它与局部计算属性混合使用

```js
computed: {
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState(['count']),
  localComputed() {
    // ..
  }
}
```

### 组件仍保有局部状态

使用 Vuex 并不意味着需要将所有的状态放入 Vuex. 如果有些状态严格属于单个组件, 最好还是作为组件的局部状态.
