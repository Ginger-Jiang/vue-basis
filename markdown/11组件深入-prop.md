## Prop 的大小写

在 HTML 元素的属性中对于大小写是不敏感的, 所以浏览器会将所有大写解释为小写字符, 意味着使用 DOM 模板时, 驼峰命名的 prop 名需要使用等价的短横线分隔命名

```js
Vue.component('componentsName', {
  // 驼峰命名
  props: ['docTitle'],
  template: '<h1>{{ docTitle }}</h1>'
})
```

```html
<!-- 在 html 中是短横线分隔的 -->
<component-name doc-title="hello"></component-name>
```

## Prop 类型

props 可以是个数组

```js
props: ['title', 'name', 'content', 'gender']
```

很多时候我们都希望每个 prop 都有指定的值类型, 我们可以用对象的形式列出 prop, 如果传入的 prop 类型与定义不符, Vue 会在控制台进行提示

```js
props: {
  title: String,
  name: String,
  content: String,
  gender: Boolean
}
```

## 传递静态或动态 Prop

> 我们可以将任意数据类型的值传递给 prop

我们可以给 prop 传入一个静态值

```html
<my-component title="Ginger"></my-component>
```

我们也可以通过`v-bind`对 prop 进行动态赋值

```js
const info = {
  title: 'Ginger'
}
<my-component v-bind:title="info.title"></my-component>
```

## 单向数据流

所有的 prop 都使得父子组件的 prop 之间形成了一个**单向下行绑定**  
父级的 prop 的更新会向下流动到子组件中, 无法反过来  
因为每次父组件发生更新时, 子组件中所有 prop 都将会刷新为最新的值, 所以不应该在一个子组件内改变 prop

- 需要将 prop 作为子组件内部数据来使用

```js
props: ['initialCounter'],
data: function() {
  return {
    counter: this.initialCounter
  }
}
```

- 需要将传入的 prop 值进行转换

```js
props:['size'],
// 使用一个计算属性来处理
computed: {
  normalizedSize: function() {
    return this.size.trim().toLowerCase()
  }
}
```

## prop 验证

我们可以为组件的 prop 指定验证要求, 如果不满足要求, Vue 会在控制台发出警告  
为了定制 prop 的验证方式, 可以为 props 中的值提供一个验证需求的对象, 而不是一个字符串数组

> 验证失败 Vue 会在控制台显示警告

```js
Vue.component('my-component', {
  props: {
    propA: Number, // 基础类型验证
    propB: [String, Number], // 可以是多个类型
    propC: {
      // 必须传入的字符串
      type: String,
      require: true
    },
    propD: {
      // 默认100的数字
      type: Number,
      default: 100
    },
    propE: {
      // 带有默认的对象
      type: Object,
      default: function() {
        return { name: 'Ginger' }
      }
    },
    propF: {
      // 自定义验证函数
      validator: function(value) {
        return ['success', 'waring', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

### 类型检测

`type` 可以是下列原生构造函数中的一个:

- `String`
- `Number`
- `Boolean`
- `Array`
- `Object`
- `Date`
- `Function`
- `Symbol`

另外, `type` 也可以是一个自定义的构造函数, 并且通过 `instanceof` 来进行检查确认

```js
function Person(firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}

// 使用
Vue.component('my-component', {
  props: {
    author: Person
  }
})
```

### 非 Prop 的 Attribute

有些 attribute 通过自定义属性传向子组件, 但是在子组件内部并没有通过 props 接受, 这些 attribute 会被添加到这个组件的根元素上

### 替换/合并已有的 attribute

对于绝大多数 `attribute` 来说，从外部提供给组件的值会替换掉组件内部设置好的值。所以如果传入 `type="text` 就会替换掉 `type="date` `并把它破坏！庆幸的是，class` 和 `style` attribute 会稍微智能一些，即两边的值会被合并起来，从而得到最终的值：`form-control` `date-picker-theme-dark`。

### 禁用 attribute 继承

如果不希望组件的根元素继承 attribute, 可以在组件的选项中设置 `inheritAttrs: false`

```js
Vue.component('my-component', {
  inheritAttrs: false,
  ...
})
```

#### \$attrs 属性包含了传递给一个组件的 attribute 名和值

利用 `inheritAttrs: false` 和 `$attrs`，就可以手动决定这些 attribute 会被赋予哪个元素

> inheritAttrs: false 选项不会影响 style 和 class 的绑定。

```js
Vue.component('base-input', {
  inheritAttrs: false,
  props: ['label', 'value'],
  template: `
    <label>
      {{ label }}
      <input
        v-bind="$attrs"
        v-bind:value="value"
        v-on:input="$emit('input', $event.target.value)"
      >
    </label>
  `
})
```
