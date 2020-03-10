## `<router-link>`

`<router-link>` 组件支持用户在具有路由功能的应用中导航, 通过 `to` 属性指定目标地址, 默认渲染程带有正确连接的 `<a>` 标签, 可以通过配置 `tag` 属性生成别的标签. 另外, 当目标路由成功激活时, 连接元素自动设置一个表示激活的 CSS 类名

`<router-link>` 比起写死的 `<a href="...">` 会好一些

- 无论是 HTML5 history 模式还是 hash 模式, 它的表现行为一致, 所以, 当要切换路由模式, 或者在 IE9 降级使用 hash 模式, 无需做任何变动
- 在 HTML5 history 模式下, `router-link` 会守卫点击事件, 让浏览器不在重新加载页面
- 在 HTML5 history 模式下使用 `base` 选项之后, 所有的 `to` 属性都不需要写了

## v-slot

`router-link` 通过一个作用域插槽暴露底层的定制能力. 这是一个更高阶的 API, 主要面向库坐着, 但也可以为开发者提供便利, 多数情况用一个类型 NavLInk 这样的组件里

在使用 `v-slot` API 时, 需要向 `router-link` 传入一个单独的子元素. 否则 `router-link` 将会把子元素包裹在一个 `span` 元素内

```html
<router-link
  to="/about"
  v-slot="{ href, route, navigate, isActive, isExactActive}"
>
  <NavLink :active="isActive" :href="href" @click="navigate">
    {{ toute.fullPath }}
  </NavLink>
</router-link>
```

- href: 解析后的 URL, 将会作为一个 `a` 元素的 `href` attribute
- route: 解析后的规范化的地址
- navigate: 触发导航的函数
- isActive: 如果需要应用激活的 class 则为 true 允许应用一个任意的 class
- isExactActive: 如果需要应用精确激活的 class 则为 true, 允许应用一个任意的 class

示例: 将激活的 class 应用在外层元素
有时候我们可能想把激活的 class 应用到一个外部元素而不是 `<a>` 标签本身, 这时可以在一个 `router-link` 中包裹该元素并使用 `v-slot` property 来创建链接

```html
<router-link
  to="/foo"
  v-slot="{href, route, navigate, isActive, isExactActive}"
>
  <li
    :class="[isActive && 'router-link-active', isExactActive && 'router-link-exact-active']"
  >
    <a :href="href" @click="navigate">{{route.fullPath}}</a>
  </li>
</router-link>
```

> 如果给 a 元素添加了 target=\_blank, 则@click="navigate" 处理器会被忽略

## Props

### to

- 类型: string | Loaction
- required
  表示目标路由的链接, 当被点击后, 内部会立刻把 `to` 的值传到 `router.push()` 所以这个值可以是一个字符串或者是描述目标位置的对象

```html
<!-- 字符串 -->
<router-link to="home">Home</router-link>
<!-- 渲染结果 -->
<a href="home">Home</a>

<!-- 使用 v-bind 的 JS 表达式 -->
<router-link v-bind:to="'home'">Home</router-link>

<!-- 不写 v-bind 也可以，就像绑定别的属性一样 -->
<router-link :to="'home'">Home</router-link>

<!-- 同上 -->
<router-link :to="{ path: 'home' }">Home</router-link>

<!-- 命名的路由 -->
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

<!-- 带查询参数，下面的结果为 /register?plan=private -->
<router-link :to="{ path: 'register', query: { plan: 'private' }}"
  >Register</router-link
>
```

### replace

- 类型: boolean
- 默认值: false
  设置 replace 属性的话, 当点击时, 会调用 route.replace() 而不是 route.push(), 于是导航后不会留下 history 记录

```html
<router-link replace :to="{path: '/abc'}"></router-link>
```

### append

- 类型: boolean
- 默认值: false
  设置 append 属性后, 则在当前路径前添加基路径, 例如, 从 `/a` 导航到一个相对路径 `b`, 如果没有配置 append, 则路径为 `/b` 如果配置了, 则为 `/a/b`

```html
<router-link :to="{path: '/b'}" append></router-link>
```

## tag

- 类型: string
- 默认值: 'a'
  有时候想要 `<router-link>` 渲染成某种标签, 比如 `<li>`. 于是我们使用 `tag` prop 类指定何种标签, 同样它还会监听点击, 触发导航

```html
<router-link :to="{path: '/a'}" tag="li"></router-link>
```

### active-class

- 类型: string
- 默认值: "router-link-active"
  设置链接激活时使用的 CSS 类名. 默认值可以通过路由的构造选项 `linkActiveClass` 来全局配置

### exact

- 类型: boolean
- 默认值: false
  是否激活默认类名的依据是包含匹配. 举个例子，如果当前的路径是 /a 开头的，那么 <router-link to="/a"> 也会被设置 CSS 类名。

  按照这个规则，每个路由都会激活 <router-link to="/">！想要链接使用“精确匹配模式”，则使用 exact 属性：

  ```html
  <!-- 这个链接只会在地址为 / 的时候被激活 -->
  <router-link to="/" exact></router-link>
  ```

### event

- 类型: string
- 默认值: "router-link-exact-active"
  配置当链接被精确匹配的时候应该激活的 class. 注意默认值也是可以通过路由构造函数选项 `linkExactActiveClass` 进行全局配置的

## `<router-view>`

`<router-view>` 组件是一个 functional 组件，渲染路径匹配到的视图组件。`<router-view>` 渲染的组件还可以内嵌自己的 `<router-view>`，根据嵌套路径，渲染嵌套组件。

其他属性 (非 router-view 使用的属性) 都直接传给渲染的组件， 很多时候，每个路由的数据都是包含在路由参数中。

因为它也是个组件，所以可以配合 `<transition>` 和 `<keep-alive>` 使用。如果两个结合一起用，要确保在内层使用 `<keep-alive>`：

```html
<transition>
  <keep-alive>
    <router-view></router-view>
  </keep-alive>
</transition>
```
