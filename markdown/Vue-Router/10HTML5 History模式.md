## HTML5 History 模式

`vur-router` 默认 hash 模式--使用 URL 的 hash 来模拟一个完整的 URL, 于是当 URL 改变时, 页面不会重新加载

如果不想要很丑的 hash, 我们可以用路由的 history 模式. 这种模式冲分利用 `history.pushState` API 来完成 URL 跳转而无须重新加载页面

```js
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

当你使用 history 模式时, URL 就像正常的 url. 不过 history 模式需要后台配置支持, 需要在后台配置如果 URL 匹配不到任何静态资源, 则应该返回同一个 `index.html` 页面, 这个页面就是你 app 依赖的页面

## 后端配置例子

Apache

```php
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
```

nginx

```nginx
location / {
  try_files $uri $uri/ /index.html;
}
```

Node.js

```js
const htpp = require('http')
const fs = require('fs')
const httpPort = 80
http
  .createServer((req, res) => {
    fs.readFile('index.html', 'utf-8', (err, content) => {
      if (err) {
        console.log('We cannot open "index.htm" file.')
      }
      res.writeHead(200, {
        'Content-Type': 'text/html; charser=ytf-8'
      })

      red.end(content)
    })
  })
  .listen(httpPort, () => {
    console.log('server listening on: http://localhost:%s', httpPort)
  })
```

基于 Node.js 的 Express，请考虑使用 connect-history-api-fallback 中间件

## 警告

这么做之后, 服务器就不再返回 404 错误页面, 因为对于所有路径都会返回 `index.html` 文件, 为了避免这种情况, 应该在 Vue 应用里面覆盖所有路由的情况下再给出一个 404 页面

```js
const router = new VueRouter({
  mode: 'history',
  routes: [
    {
      path: '*',
      component: NotFoundComponent
    }
  ]
})
```

或者, 如果使用的是 Node.js 服务器, 可以用服务端路由匹配到来的 URL, 并在没有匹配到路由的时候返回 404, 以实现回退
