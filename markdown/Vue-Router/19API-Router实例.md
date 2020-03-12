## Router 实例属性

- router.app
- 类型: Vue instance
  配置了 router 的 Vue 根实例

- router.mode
- 类型: string
  路由使用的模式

- router.currentRoute
- 类型 Route
  当前路由对应的路由信息对象

## Router 实例方法

### 全局导航守卫

- router.beforeEach
- router.beforeResolv
- router.afterResolve
- router.afterEach

  函数签名:

  ```js
  // 都返回一个移除已注册的守卫/钩子的函数。

  router.beforeEach((to, from, next) => {
    /* 必须调用 `next` */
  })

  router.beforeResolve((to, from, next) => {
    /* 必须调用 `next` */
  })

  router.afterEach((to, from) => {})
  ```

### 动态导航到一个新的 URL

- router.push
- router.replace
- router.go
- router.back
- router.forward

  函数签名:

  ```js
  router.push(location, onComplete?, onAbort?)
  router.push(location).then(onComplete).catch(onAbort)
  router.replace(location, onComplete?, onAbort?)
  router.replace(location).then(onComplete).catch(onAbort)
  router.go(n)
  router.back()
  router.forward()
  ```

### 返回目标位置或当前路由匹配的组件数组

- router.getMatchedComponents

  函数签名：

  ```js
  // 通常在服务端渲染的数据预加载时使用。
  const matchedComponents: Array<Component> = router.getMatchedComponents(location?)
  ```

### 解析目标位置

- router.resolve

  函数签名:

  ```js
  const resolved: {
    location: Location;
    route: Route;
    href: string;
  } = router.resolve(location, current?, append?)
  ```

  解析目标位置 (格式和 <router-link> 的 to prop 一样)

- current 是当前默认的路由 (通常你不需要改变它)
- append 允许你在 current 路由上附加路径 (如同 router-link)

### 动态添加更多的路由规则

- router.addRoutes

  函数签名:

  ```js
  // 动态添加更多的路由规则。参数必须是一个符合 routes 选项要求的数组。
  router.addRoutes((routes: Array<RouteConfig>))
  ```

### router.onReady

该方法把一个回调排队，在路由完成初始导航时调用，这意味着它可以解析所有的异步进入钩子和路由初始化相关联的异步组件。
这可以有效确保服务端渲染时服务端和客户端输出的一致。
第二个参数 errorCallback 只在 2.4+ 支持。它会在初始化路由解析运行出错 (比如解析一个异步组件失败) 时被调用。

函数签名:

```js
router.onReady(callback, [errorCallback])
```

### router.onError

注册一个回调，该回调会在路由导航过程中出错时被调用。注意被调用的错误必须是下列情形中的一种：

- 错误在一个路由守卫函数中被同步抛出；

- 错误在一个路由守卫函数中通过调用 next(err) 的方式异步捕获并处理；

- 渲染一个路由的过程中，需要尝试解析一个异步组件时发生错误。

函数签名:

```js
router.onError(callback)
```
