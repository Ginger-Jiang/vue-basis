## el

- 类型 string | Element
- 限制: 只在用 `new` 创建实例时生效
- 详细
  提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标, 可以是 CSS 选择器, 也可以是一个 HTMLElement 实例
  在实例挂载之后, 元素可以用 `vm.$el` 访问
  如果在实例化时存在这个选项, 实例将立刻进入编译过程, 否则, 需要显式调用 `vm.$mount()` 手动开启编译

## template

- 类型 string
- 详细
  一个字符串模板作为 Vue 实例的标识使用。模板将会 替换 挂载的元素。挂载元素的内容都将被忽略，除非模板的内容有分发插槽。
  如果值以 # 开始，则它将被用作选择符，并使用匹配元素的 innerHTML 作为模板。常用的技巧是用 <script type="x-template"> 包含模板。

## render

- 类型 (createElement: () => VNode) => VNode
- 详细
  字符串模板的代替方案，允许你发挥 JavaScript 最大的编程能力。该渲染函数接收一个 createElement 方法作为第一个参数用来创建 VNode。
  如果组件是一个函数组件，渲染函数还会接收一个额外的 context 参数，为没有实例的函数组件提供上下文信息。

## renderError

- 类型 (createElement: () => VNode, error: Error) => VNode
- 详细:
  **只在开发者环境下工作。**
  当 render 函数遭遇错误时，提供另外一种渲染输出。其错误将会作为第二个参数传递到 renderError。这个功能配合 hot-reload 非常实用。
- 示例

  ```js
  new Vue({
    render(h) {
      throw new Error('oops')
    },
    renderError(h, err) {
      return h('pre', { style: { color: 'red' } }, err.stack)
    }
  }).$mount('#app')
  ```
