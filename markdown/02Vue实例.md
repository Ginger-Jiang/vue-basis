## 创建一个 Vue 实例

Vue 实例通过 `new` 一个 `Vue` 函数得到  
在创建一个 `Vue` 实例的时候, 我们可以通过传入一个**选项对象**来构建我们需要的行为.

```js
var vm = new Vue({
  // 选项
})
```

## 数据与方法

当一个 `Vue` 实例被创建时, 它会将 `data` 对象中所有的属性加入到 `Vue` 的响应式系统中, 当他们发生改变时, 视图将会产生响应, 进行更新

> data 中数据只有在实例创建时就已存在的属性才是响应式的

```js
// 数据对象
var data = { name: 'jack' }

// 将数据对象加入到 Vue 实例中
var vm = new Vue({
  el: 'app'
  data: data
})

// 获取实例上的属性 返回源数据中对应的字段
vm.name == data.a // true

// 设置属性也会影响到原始数据
vm.name = 'Ginger'
data.a // Ginger
```

`Vue` 实例暴露了一系列内置实例属性与方法, 他们都有\***\*\$\*\*** 前缀

```js
vm.$data === data // true
vm.$el === document.getElementById('app') // true
```

## 实例生命周期

`Vue` 实例在创建时, 需要经过一系列的初始化过程, 在这个过程中, 会运行一些叫做**生命周期钩子**的函数, 这些钩子函数为用户提供了在实例创建过程中添加自己代码的机会

> 生命周期钩子中的`this`上下文指向调用他的`vue`实例
> 不要使用箭头函数

### 生命周期钩子

- beforeCreate 实例创建前
- created 实例创建后
- beforeMount 挂载前
- Mounted 挂载完毕
- beforeupdate 实例数据更新前
- updated 数据更新完毕
- beforeDestroy 实例销毁前
- destroyed 实例销毁完毕

### 生命周期图示

![生命周期](../image/vue生命周期.png)
