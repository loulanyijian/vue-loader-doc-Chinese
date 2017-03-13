# Vue组件规范

一个`*.vue`文件是一种自定义的文件格式，使用类似于HTML的语法来描述一个Vue组件。每个`*.vue`文件由三种类型的顶级语言块组成：`<template>`、`<script>`和`<style>`，以及可选的其他自定义块：

``` html
<template>
  <div class="example">{{ msg }}</div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>

<style>
.example {
  color: red;
}
</style>

<custom1>
  这可以是组件的示例文档。
</custom1>
```

`vue-loader`会解析文件，提取每一个语言块，如果有必要的话通过其它的加载器来传送，最后将它们组装到一个CommonJS模块，其`module.exports`是一个Vue.js组件选项对象。

`vue-loader`支持使用非默认的语言，像CSS预处理器和编译到HTML的模板语言，通过指定语言块的`lang`属性就行。例如，你可以为你组件的样式使用SASS，如下所示：

``` html
<style lang="sass">
  /* 写SASS! */
</style>
```

可以在[使用预处理](../configurations/pre-processors.md)中查看更多细节。

### 语言块

#### `<template>`

- 默认语言: `html`。

- 每个`*.vue`文件同时最多只能包含一个`<template>`块。

- 内容将作为字符串提取，用来编译Vue组件的`template`选项。

#### `<script>`

- 默认语言: `js`(如果检测到`babel-loader`或者`buble-loader`，会自动支持ES2015)。

- 每个`*.vue`文件同时最多只能包含一个 `<script>`块。

- script在类似CommonJS的环境中执行（就像通过Webpack处理的正常`.js`模块），这意味着你可以'require()'其他依赖。通过ES2015的支持，你也可以使用`import`和`export`语法。

- script必须导出Vue组件选项对象。虽然也支持由`Vue.extend()`创建的扩展构造函数，但最好还是导出一个简单对象。

#### `<style>`

- 默认语言: `css`.

- 可以在一个`*.vue`文件支持多个`<style>`标签

- 一个`<style>`标签可以有`scoped` 或 `module` 属性（可参考[CSS作用域](../features/scoped-css.md)和[CSS Modules](../features/css-modules.md）来帮助封装当前组件的样式。具有不同封装模式的多个`<style>`标签可以在同一组件中混合。

- 默认情况下，内容会被`style-loader`提取，并像一个实际的`<style>`标签一样动态插入到文档的`<head>`中。也可以[配置webpack来将所有组件中的所有样式提取到一个单独的css文件中](../configurations/extract-css.md)。

### 自定义模块

> 只有vue-loader 10.2.0+支持

如果有项目特殊需要的话，在一个`*.vue`文件中包含自定义添加的模块，例如一个`<docs>`模块。`vue-loader`会使用这个标签名称来看看哪个webpack loader会支持这块内容。webpack loader 应当在`vue-loader`的`loaders`模块定义。

要查看细节, 瞅瞅 [Custom Blocks](../configurations/custom-blocks.md)。

### 资源引入

如果你将你的`*.vue`组件引入到多个文件中，你可以使用`src`属性来为代码块引入一个额外的文件。

``` html
<template src="./template.html"></template>
<style src="./style.css"></style>
<script src="./script.js"></script>
```
注意，src的导入遵循相同的路径解析规则到CommonJS`require（）`调用，这意味着相对路径需要以`。/`开头，您可以直接从已安装的NPM包导入资源，例如：

``` html
<!-- 从一个已经安装的npm包中引入 "todomvc-app-css" -->
<style src="todomvc-app-css/index.css">
```

`src` 导入也可以使用自定义块，例如：

``` html
<unit-test src="./unit-test.js">
</unit-test>
```

### 语法高亮

目前支持[Sublime Text](https://github.com/vuejs/vue-syntax-highlight),[Atom](https://atom.io/packages/language-vue)，[Vim](https://github.com/posva/vim-vue)，[Visual Studio](https://marketplace.visualstudio.com/items/liuji-jim.vue)，[Brackets](https：// github.com/pandao/brackets-vue)和[JetBrains产品](https://plugins.jetbrains.com/plugin/8057)(WebStorm，PhpStorm等)。 对其他编辑器/IDE的贡献高度赞赏！ 如果你不在Vue组件中使用任何预处理器，你也可以通过在编辑器中将`*.vue`文件视为HTML来使用。

### 建议

在每个语法块中，你将使用所使用的语言的注释语法（HTML，CSS，JavaScript，Jade等）。对于顶级注释，使用HTML注释语法：`<!-在此处注释内容->`
