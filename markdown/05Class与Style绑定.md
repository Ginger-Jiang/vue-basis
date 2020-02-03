## 绑定 HTML Class

Vue 对于 class 和 style 上使用`v-bind`做了专门的增强, 表达式结果的类型除了字符串之外, 还可以是对象或数组

### 对象语法

我们可以传给`v-bind:class`一个对象, 以动态的切换 class.

```vue
<template>
  <div id="app">
    <!-- 传递一个对象 -->
    <div :class="{ box: showBox, box1: showBox }"></div>
    <!-- class 与 绑定对象共存 -->
    <div class="box" :class="{ box1: showBox }"></div>
    <!-- 接受一个对象 -->
    <div :class="classObj"></div>
    <!-- 接受一个计算属性 -->
    <div :class="isShowBox"></div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      showBox: true,
      classObj: {
        box: true,
        box1: false
      }
    }
  },
  computed: {
    isShowBox() {
      return {
        box: true,
        box1: true
      }
    }
  }
}
</script>

<style>
.box {
  border: 1px solid red;
  width: 100px;
  height: 100px;
}
.box1 {
  width: 60px;
  height: 60px;
  background-color: pink;
}
</style>
```

### 数组语法

我们还可以把一个数组传给 `v-bind:class`, 应用一个 class 列表

```vue
<template>
  <div id="app">
    <!-- 接受一个数组 -->
    <div :class="clsasArr"></div>
    <!-- 使用表达式 -->
    <div :class="[...clsasArr]"></div>
    <!-- 使用三元表达式 -->
    <div :class="[showBox ? 'box' : '']"></div>
    <!-- 对象与数组同时使用 -->
    <div :class="[{ box: showBox }, 'box1']"></div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      showBox: true,
      clsasArr: ['box', 'box1']
    }
  }
}
</script>

<style>
.box {
  border: 1px solid red;
}
.box1 {
  width: 60px;
  height: 60px;
  background-color: pink;
}
</style>
```

### 用于组件上

当我们在一个自定义组件上使用 `class` 属性时, 这些 class 将被添加到该组件的根元素上, 这个元素上已存在的 class 不会被覆盖

## 绑定内联样式

### 对象语法

```vue
<template>
  <div id="app">
    <!-- 内联样式 -->
    <div
      class="box"
      :style="{ width: width + 'px', height: height + 'px' }"
    ></div>
    <!-- 直接传入对象 -->
    <div :style="style">box</div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      width: 100,
      height: 100,
      style: {
        backgroundColor: 'pink',
        color: '#000'
      }
    }
  }
}
</script>

<style>
div.box {
  border: 1px solid red;
}
</style>
```

### 数组语法

`v-bind:style` 的数组语法可以将多个样式对象应用到同一个元素上

```vue
<template>
  <div id="app">
    <div :style="arr">box</div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      arr: [{ color: "red" }, { border: "1px solid red" }]
    };
  }
};
</script>

<style>

```

### 自动添加前缀

当 `v-bind:style` 使用需要添加浏览器引擎前缀的 CSS 属性时，如 transform，Vue.js 会自动侦测并添加相应的前缀。

### 多重值

可以为 style 绑定中的属性提供一个包含多个值的数组，常用于提供多个带前缀的值，例如：  
这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 display: flex。

```html
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
