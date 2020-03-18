## Getter

有时候我们需要从 store 中的 state 中派生出一些状态, 比如对列表进行过滤并记数

```js
computed: {
  doneTodosCount () {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

如果有多个组件需要用到此属性, 我们要复制这个函数, 或者抽取到一个共享函数然后在多处导入它

Vuex 允许我们在 store 中定义 getter(可以认为是 store 的计算属性). 就像计算属性一样. getter 的返回值会根据他的依赖被缓存起来, 且只有当它的依赖值发生了改变才会被重新计算.

Getter 接受 state 作为其第一个参数:

```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getter: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

## 通过属性访问

getter 会暴露 `store.getters` 对象, 可以以属性的形式访问这些值:

```js
store.getters.doneTodos // -> [{id:1, text: '...', done: true}]
```

Getter 也可以接受其他 getter 作为第二个参数

```js
getters: {
  // ...
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length
  }
}
```

```js
store.state.doneTodosCount // -> 1
```

我们可以很容易的在任何组件中使用它:

```js
computed: {
  doneTodosCount() {
    return this.$store.getters.doneTodosCount
  }
}
```

注意, getter 在通过属性访问时是作为 Vue 的响应式系统的一部分缓存其中的

## 通过方法访问

也可以通过让 getter 返回一个函数, 来实现给 getter 传参.

```js
getters: {
  // ...
  getTodoById: state => id => {
    return state.todos.find(todo => tido.id === id)
  }
}
```

```js
store.getters.getTodoById(2) // { id: 2, text: '...', done: false}
```

> getter 在通过方法访问时, 每次都会去进行调用, 不会缓存结果.

## mapGetters 辅助函数

`mapgetters` 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性:

```js
import { mapgetters } from 'vuex'

export default {
  // ...
  computed: {
    // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters(['doneTodosCount', 'anotherGetter'])
  }
}
```

如果想将一个 getter 属性另取一个名字, 使用对象形式

```js
mapGetters([(doneCount: 'doneTodosCount')])
```
