# 使用预处理器

在Webpack中，所有预处理器都需要应用相应的加载器。 `vue-loader`允许你使用其他Webpack加载器处理Vue组件的一部分。它将从语言块的`lang`属性自动推断出要使用的正确加载器。

### CSS

例如，让我们用SASS编译我们的`<style>`标签：

``` bash
npm install sass-loader node-sass --save-dev
```

``` html
<style lang="sass">
  /* 在这里写sass代码 */
</style>
```

在引擎盖下，`<style>`标签内的文本内容将首先由`sass-loader`编译，然后传递进行进一步处理。

#### sass-loader 警告

与其名称指示相反，*sass*-loader](https://github.com/jtangelder/sass-loader)默认解析 *SCSS*语法。如果实际上想要使用缩进的 *SASS*语法，则必须相应地为sass-loader配置vue-loader的选项。

```javascript
{
  test: /\.vue$/,
  loader: 'vue-loader',
  options: {
    loaders: {
      scss: 'vue-style-loader!css-loader!sass-loader' // <style lang="scss">
      sass: 'vue-style-loader!css-loader!sass-loader?indentedSyntax' // <style lang="sass">
    }
  }
}
```

有关如何配置vue-loader的详细信息，请参阅[高级加载程序配置](./ advanced.md)部分。

### JavaScript

默认情况下，Vue组件中的所有JavaScript都由babel-loader处理。 但你当然可以改变它：

``` bash
npm install coffee-loader --save-dev
```

``` html
<script lang="coffee">
  # 编写coffeescript!
</script>
```

### Templates

Processing templates is a little different, because most Webpack template loaders such as `pug-loader` return a template function instead of a compiled HTML string. Instead of using `pug-loader`, we can just install the original `pug`:

处理模板有点不同，因为大多数Webpack模板加载器（如`pug-loader`）返回一个模板函数，而不是一个编译的HTML字符串。我们可以安装原生的的`pug`来替代`pug-loader`：

``` bash
npm install pug --save-dev
```

``` html
<template lang="pug">
div
  h1 Hello world!
</template>
```

> **重要:** 如果你使用 `vue-loader@<8.2.0`，你还需要安装`template-html-loader`.

### 内联加载请求

你可以在`lang`属性上使用 [Webpack loader 请求](https://webpack.github.io/docs/loaders.html#introduction):

``` html
<style lang="sass?outputStyle=expanded">
  /* 在使用sass扩展输出 */
</style>
```

但是，请注意，这使你的Vue组件Webpack特定，并不兼容Browserify和[vueify](https://github.com/vuejs/vueify)。 **如果您打算将您的Vue组件作为可重复使用的第三方组件，请避免使用此语法**。
