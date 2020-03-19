## Mutation

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation. Vuex 中的 mutation 非常类似于事件:每个 mutation 都有一个字符串的事件类型和一个回调函数. 这个回调函数就是我们实际进行状态更改的地方, 并且他会接受 state 作为第一个参数

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment(state) {
      // 变更状态
      state.count++
    }
  }
})
```

不能直接调用一个 mutation handler. 这个选项更像是事件注册: "当触发一个类型为 `increment` 的 mutation 时, 调用此函数. "要唤醒一个 mutation handler, 需要已相应的 type 调用 `store.commit` 方法:

```js
store.commit('increment')
```

## 提交载荷 (Payload)

可以向 `store.commit` 传入额外的参数, 即 mutation 的载荷 (Payload)

```js
mutations: {
  increment: (state, n) => {
    state.count += n
  }
}
```

```js
store.commit('increment', 10)
```

在大多数情况下, 载荷应该是一个对象, 这样可以包含多个字段并且记录的 mutation 会更易读:

```js
mutations: {
  increment: (state, payload) {
    state.count += payload.amount
  }
}
```

```js
store.commit('increment', { amount: 10 })
```

## 对象风格的提交方式

提交 mutation 的另一种方式是直接使用包含 type 属性的对象

```js
store.commit({
  type: 'increment',
  amount: 10
})
```

当使用对象风格的提交方式, 整个对象都作为载荷传给 mutation 函数, 因此 handler 保持不变

```js
mutations: {
  increment: (state, payload) {
    state.count += payload.amount
  }
}
```

## Mutation 需遵守 Vue 的响应规则

既然 Vuex 的 store 中的状态是响应式的, 那么当我们变更状态时, 监视状态的 Vue 组件也会自动更新. 这也意味着 Vuex 中的 mutation 也需要与使用 Vuex 一样遵守一些注意事项

1. 最好提前在 store 中初始化好所有的所需属性
2. 当需要在对象上添加新属性时, 应该:

- 使用 Vue.set(obj, 'newProp', 123)
- 以新对象替换老对象. 例如, 利用对象展开运算符
  ```js
  state.obj = { ...state.obj, newProp: 123 }
  ```

## 使用常亮代替 Mutation 事件类型

使用常亮替代 mutation 事件类型在各种 Flux 实现中是很常见的模式. 这样可以使 linter 之类的工具发挥作用, 同时把这些常亮放在单独的文件中可以让代码合作者对整个 app 包含的 mutation 一目了然

```js
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
```

```js
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.store({
  state: { ... },
  mutations: {
    // 可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION](state) {
      //  mutate state
    }
    }
  }
})
```

## Mutation 必须是同步函数

一条重要的原则就是 mutation 必须是同步函数

```js
mutations: {
  someMutation(state) {
    api.callAsyncMethod(()=> {
      state.count++
    })
  }
}
```

现在想象，我们正在 debug 一个 app 并且观察 devtool 中的 mutation 日志。每一条 mutation 被记录，devtools 都需要捕捉到前一状态和后一状态的快照。然而，在上面的例子中 mutation 中的异步函数中的回调让这不可能完成：因为当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用——实质上任何在回调函数中进行的状态的改变都是不可追踪的。

## 在组件中提交 Mutation

可以在组件中使用 `this.$store.commit('xxx')` 提交 mutation, 或者使用 `mapMutation` 辅助函数将组件中的 methods 映射为 `store.commit` 调用

```js
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations(['increment', 'incrementby']),
    ...mapMutations([(add: 'increment')])
  }
}
```
