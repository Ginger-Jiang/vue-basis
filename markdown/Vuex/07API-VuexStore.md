## Vuex.Store

```js
import Vuex from 'vuex'
const store = new Vuex.Store({ ...options })
```

## Vuex.Store 构造器选项

### state

- 类型: `Object | Function`
  Vuex store 实例的根 state 对象

  如果传入返回一个对象的函数, 其返回的对象会被用作根 state.

### mutations

- 类型: `{ [type: string ]: Function }`
  在 store 上注册 mutation, 处理函数总是接受 `state` 作为第一个参数(如果定义在模块中, 则为模块的局部状态), `payload`作为第二个参数

### sctions

- 类型: `{ [type: string]: Function }`
  在 store 上注册 action. 处理函数总是接受 `context` 作为第一个参数, `payload` 作为第二个参数

  `context` 对象包含以下属性:

  ```js
  {
    state, // 等同于 `store.state`，若在模块中则为局部状态
      rootState, // 等同于 `store.state`，只存在于模块中
      commit, // 等同于 `store.commit`
      dispatch, // 等同于 `store.dispatch`
      getters, // 等同于 `store.getters`
      rootGetters // 等同于 `store.getters`，只存在于模块中
  }
  ```

### getters

- 类型: `{ [type: string ]: Function}`

  在 store 上注册 getter, getter 方法接受以下参数:

  ```js
  state, getters
  ```

  当定义在一个模块粒时会特别一些:

  ```js
  state, // 如果在模块中定义则为模块的局部状态
    getters, // 等同于 store.getters
    rootState, // 等同于 store.state
    rootGetters // 所有 getters
  ```

  注册的 getter 为暴露为 getters

### modules

- 类型: `Object`
  包含了子模块的对象, 会被合并到 store
  ```js
  {
    key: {
      state,
      namespaced?,
      mutations,
      actions?,
      getters?,
      modules?
    }
  }
  ```
  与根模块的选项一样, 每个模块也包含 `state` 和 `mutations` 选项. 模块的状态使用 `key` 关联到 store 的根状态. 模块的 mutation 和 getter 只会接受 module 的局部状态作为第一个参数, 而不是根状态, 并且模块的 action 的 `context.state` 同样指向局部状态

### pulgins

- 类型: `Array<Function>`
  一个数组, 包含应用在 store 上的插件方法. 这些插件直接接受 store 作为唯一参数, 可以监听 mutation 或者提交 mutation

### strict

- 类型: `Boolean`
- 默认值: `false`
  使 Vuex store 进入严格模式, 在严格模式下, 任何 mutation 处理函数以外修改 Vuex state 都会抛出错误

### devtools

- 类型: `Boolean`
  为某个特定的 Vuex 实例打开或关闭 devtools. 对于传入 false 的实例来说 Vuex store 不会订阅到 devtools 插件
  ```js
  {
    devtools: false
  }
  ```

## Vuex.Store 实例属性

### state

- 类型: `Object`
  根状态, 只读

### getters

- 类型: `Object`
  暴露出注册的 getter, 只读

## Vuex.Store 实例方法

### commit

- `commit(type: string, payload?:any, options?: Object)`
- `commit(mutation: Object, options?: Object)`

  提交 mutation, `options` 里可以有 `root: true`, 它允许在命名空间模块里提交根的 mutation

### dispatch

- `dispatch(type: string, payload?:any, options?: Pbject)`
- `dispatch(action: Object, options?: Object)`

  分发 action. `options` 里可以有 `root: true`, 它允许在命名空间模块里分发根的 action. 返回一个解析所有被触发的 action 处理器的 Promise

### replaceState

- `replaceState(state: Object)`

  替换 store 的根状态, 仅用状态合并或时光旅行调试

### wathc

- `watch(fn: Function, callback: Funtion, options?: Object): Function`

  响应式的侦听 `fn` 的返回值, 当值改变时调用回调函数. `fn` 接受 store 的 state 作为第一个参数, 其 getter 作为第二个参数, 最后接受一个可选对象参数表示 Vue 的`vm.$watch`方法的参数

  要停止侦听, 调用此方法返回的函数即可停止侦听

### subscribe

- `subscribe(handler: Function):Function`

  订阅 store 的 mutation. `hanlder` 会在每个 mutation 完成后调用, 接受 mutation 和经过 mutation 后的状态作为参数

  ```js
  store.subscribe((mutation, state) => {
    console.log(mutation.type)
    console.log(mutation.paylaod)
  })
  ```

  要停止订阅, 调用此方法返回的函数可停止订阅

### subscribeAction

- `subscribeAction(handler: Function): Function`

  订阅 store 的 action。handler 会在每个 action 分发的时候调用并接收 action 描述和当前的 store 的 state 这两个参数：

  ```js
  store.subscribeAction((action, state) => {
    console.log(action.type)
    console.log(action.payload)
  })
  ```

  要停止订阅，调用此方法返回的函数即可停止订阅。

  从 3.1.0 起，subscribeAction 也可以指定订阅处理函数的被调用时机应该在一个 action 分发之前还是之后 (默认行为是之前)：

  ```js
  store.subscribeAction({
    before: (action, state) => {
      console.log(`before action ${action.type}`)
    },
    after: (action, state) => {
      console.log(`after action ${action.type}`)
    }
  })
  ```

### registerModule

- `registerModule(path: string | Array<string>, module: Module, options?: Object)`

  注册一个动态模块

  options 可以包含 `preserveState: true` 以允许保留之前的 state。用于服务端渲染。

### unregisterModule

- `unregisterModule(path: string | Array<string>)`

  卸载一个动态模块。

### hotUpdate

- `hotUpdate(newOptions: Object)`

  热替换新的 action 和 mutation。详细介绍

## 组件绑定的辅助函数

### mapState

- `mapState(namespace?: string, map: Array<string> | Object<string | Function>): Object`

  为组件创建计算属性以返回 Vuex store 中的状态

  第一个参数是可选的, 可以是一个命名空间字符串

  对象形式的第二个参数的成员可以是一个函数

### mapGetters

- `mapGetters(namespace?: string, map: Array<string> | Object<string>): Object`

  为组件创建计算属性以返回 getter 的返回值

  第一个参数是可选的, 可以是一个命名空间字符串

### mapActions

- `mapActions(namespace?: string, map: Array<string> | Object<string | function>): Object`

  创建组件方法分发 action

  第一个参数是可选的, 可以是一个命名空间字符串

  对象形式的第二个参数的成员可以是一个函数 `function(dispatch: function, ...args: any[])`

### mapMutations

- `mapMutations(namespace?: string, map: Array<string> | Object<string | function>): Object`

  创建组件方法提交的 mutation

  第一个参数是可选的, 可以是一个命名空间字符串

  对象形式的第二个参数的成员可以是一个函数 `function(commit: function, ...args: any[])`

### createNamespacedHelpers

- `createNamespacedHelpers(namespace: string): Object`

  创建基于命名空间的组件绑定辅助函数。其返回一个包含 mapState、mapGetters、mapActions 和 mapMutations 的对象。它们都已经绑定在了给定的命名空间上
