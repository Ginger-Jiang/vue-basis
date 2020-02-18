## 全局配置

`Vue.config` 是一个对象, 包含 Vue 的全局配置.

### slient

- 类型: boolean
- 默认: false
- 用法:

```js
// 取消 Vue 所有的日志与警告
Vue.config.slient = true
```

### optionMergeStrategies

- 类型: { [key: string]: Function }
- 默认: {}
- 用法:

```js
// 自定义合并策略的选项。
Vue.config.optionMergeStrategies._my_option = function(parent, child, vm) {
  return child + 1
}

const Profile = Vue.extend({
  _my_option: 1
})
// Profile.options._my_option = 2
```

> 合并策略选项分别接收在父实例和子实例上定义的该选项的值作为第一个和第二个参数，Vue 实例上下文被作为第三个参数传入。

### errorHandle

- 类型: Function
- 默认值: undefined
- 用法:

```js
Vue.config.errorHandle = function(err, vm, info) {
  // handle error
  // info 是 Vue 特定的错误信息, 比如错误所在的生命周期钩子
}
```

> 指定组件的渲染和观察期间未捕获错误的处理函数。这个处理函数被调用时，可获取错误信息和 Vue 实例。

### warnHandle

- 类型: Function
- 默认值: undefined
- 用法:

```js
Vue.config.warnHandle = function(msg, vm, trace) {
  // trace 是组件的继承关系追踪
}
```

### ignoredElements

- 类型 Array<string | RegExp>
- 默认值: {}
- 用法:

```js
Vue.config.ignoredElements = [
  'my-custom-web-component',
  'another-web-component',
  // 用一个 `RegExp` 忽略所有“ion-”开头的元素
  // 仅在 2.5+ 支持
  /^ion-/
]
```

> 须使 Vue 忽略在 Vue 之外的自定义元素 (e.g. 使用了 Web Components APIs)。否则，它会假设你忘记注册全局组件或者拼错了组件名称，从而抛出一个关于 Unknown custom element 的警告。

### keyCodes

- 类型: { [key: string]: number | Array<number> }
- 默认值: {}
- 用法:

```js
// 给 v-on 自定义键位别名。
Vue.config.keyCodes = {
  v: 86,
  f1: 112,
  // camelCase 不可用
  mediaPlayPause: 179,
  // 取而代之的事 kebab-case 且用双引号括起来
  'media-play-pause': 179,
  up: [38, 87]
}
```

```html
<input @media-play-pause="method" />
```

### productionTip

- 类型: boolean
- 默认值: true
- 用法:
  设置为 false 以阻止 vue 在启动时生成生产提示
