# CSS Modules

> 需要 ^9.8.0 以上版本

[CSS Modules](https://github.com/css-modules/css-modules)是一个流行的模块化和组合CSS的系统。 `vue-loader`提供了将css Modules作为css作用域的一流集成。

### 用法

只需要在你的`<style>`上添加  `module` 属性：

``` html
<style module>
.red {
  color: red;
}
.bold {
  font-weight: bold;
}
</style>
```

这将打开`css-loader`的CSS Modules模式，并且将生成的类标识符对象注入到组件中作为名为`$ style`的计算属性。 你可以在模板中使用它与动态类绑定：

``` html
<template>
  <p :class="$style.red">
    这里应当是红色的
  </p>
</template>
```

因为它是一个计算属性，它也适用于class的object/array语法：

``` html
<template>
  <div>
    <p :class="{ [$style.red]: isRed }">
      我是红色的？
    </p>
    <p :class="[$style.red, $style.bold]">
      红色的、加粗的
    </p>
  </div>
</template>
```

你也可以从JavaScript访问它：

``` html
<script>
export default {
  created () {
    console.log(this.$style.red)
    // -> "_1VyoJ-uZOjlOxP7jWUy19_0"
    // 基于filename和className生成的标识符。
  }
}
</script>
```

参考[CSS Modules规格](https://github.com/css-modules/css-modules)了解模式详细信息，如[全局异常](https://github.com/css-modules/css-modules #exceptions)和[composition](https://github.com/css-modules/css-modules#composition)。

### 自定义注入名称

你可以在一个`* .vue`组件中有多个`<style>`标签。为了避免注入的样式彼此覆盖，可以通过给`module`属性一个值来定制注入的计算属性的名称：

``` html
<style module="a">
  /* 标识符注入为 a */
</style>

<style module="b">
  /* 标识符注入为 b */
</style>
```

### 配置 `css-loader` 查询

CSS Modules 通过[css-loader](https://github.com/webpack/css-loader)进行处理。 使用`<style module>`，`css-loader`的默认查询是：

``` js
{
  modules: true,
  importLoaders: true,
  localIdentName: '[hash:base64]'
}
```

您可以使用vue-loader的`cssModules`选项为`css-loader`提供其他查询选项：

``` js
// webpack 1
vue: {
  cssModules: {
    // overwrite local ident name
    localIdentName: '[path][name]---[local]---[hash:base64:5]',
    // enable camelCase
    camelCase: true
  }
}

// webpack 2
module: {
  rules: [
    {
      test: '\.vue$',
      loader: 'vue-loader',
      options: {
        cssModules: {
          localIdentName: '[path][name]---[local]---[hash:base64:5]',
          camelCase: true
        }
      }
    }
  ]
}
```
