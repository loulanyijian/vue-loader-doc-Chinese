# CSS作用域

当一个`<style>`标签具有`scoped`属性时，它的CSS将只应用于当前组件的元素。这类似于Shadow DOM中的样式封装。 它有一些注意事项，但不需要任何polyfills。它通过使用PostCSS来实现以下转换：

``` html
<style scoped>
.example {
  color: red;
}
</style>

<template>
  <div class="example">hi</div>
</template>
```

编译成如下代码：

``` html
<style>
.example[_v-f3f3eg9] {
  color: red;
}
</style>

<template>
  <div class="example" _v-f3f3eg9>hi</div>
</template>
```

#### 注意

1. 你可以在一个相同的组件里面同时包含作用域与非作用域的样式

  ``` html
  <style>
  /* 全局样式 */
  </style>

  <style scoped>
  /* 本地样式 */
  </style>
  ```

2. 一个子组件的根节点会被父组件的作用域css和该子组件的作用域css同时影响。

3. 部分不受作用域样式的影响。

4. **作用域样式不能消除class**的需要。由于浏览器呈现各种CSS选择器的方式，`p {color：red}`在作用域时（即与属性选择器组合时）会慢很多倍。 如果你使用类或id，例如`.example {color：red}`，那么你几乎可以消除性能损失。 [这里是一个试验场](http://stevesouders.com/efws/css-selectors/csscreate.php)，你可以在那里自己测试差异。

5. **在递归组件中小心使用后代选择器!** 对于带有选择器`.a .b`的CSS规则，如果匹配`.a`的元素包含一个递归子组件，那么所有`.b' 子组件将由规则匹配。
