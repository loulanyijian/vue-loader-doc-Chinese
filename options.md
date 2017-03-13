# 参考选项

## Webpack 1 & 2的不同使用之处

对 Webpack 2: 将选项直接传递到加载器规则。

``` js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          // vue-loader options
        }
      }
    ]
  }
}
```

对 Webpack 1.x: 在你的webpack配置文件中添加一个根 `vue` 模块。

``` js
module.exports = {
  // ...
  vue: {
    // vue-loader options
  }
}
```

### loaders

- type: `{ [lang: string]: string }`

  指定Webpack加载器以覆盖用于`*.vue`文件中的语言块的默认加载器的对象。该键对应于语言块的`lang`属性，如果指定的话。 每种类型的默认`lang'是：

  - `<template>`: `html`
  - `<script>`: `js`
  - `<style>`: `css`

  例如，使用`babel-loader`和`eslint-loader`来处理所有`<script>`块：

  ``` js
  // Webpack 2.x config
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          loaders: {
            js: 'babel-loader!eslint-loader'
          }
        }
      }
    ]
  }
  ```

### 预处理

- type: `{ [lang: string]: string }`
- 仅在 >=10.3.0 被支持

  配置格式与“loaders”相同，但是“preLoaders”应用于默认加载器之前的相应语言块。 您可以使用它来预处理语言块 - 一个常见的用例是构建时间i18n。

### postLoaders

- type: `{ [lang: string]: string }`
- 仅在 >=10.3.0 被支持

  配置格式与`loaders`相同，但是`postLoaders`应用于默认加载器之后。 您可以使用它来后处理语言块。 但是请注意，这有点复杂：

  - 对于`html`，默认加载器返回的结果将是编译的JavaScript渲染函数代码。

  - 对于`css`，结果将由`vue-style-loader`返回，这在大多数情况下不是特别有用。 使用postcss插件将是一个更好的选择。

### postcss

- type: `Array` 或 `Function` 或 `Object`

  指定要应用于`* .vue`文件中的CSS的自定义PostCSS插件。 如果使用函数，函数将使用相同的加载器上下文调用，并应返回一个插件数组。

  ``` js
  // ...
  vue: {
    // 注意：不要在`loaders`下嵌入`postcss`选项
    postcss: [require('postcss-cssnext')()],
    loaders: {
      // ...
    }
  }
  ```

  此选项也可以是包含要传递到PostCSS处理器的选项的对象。 这在使用依赖于自定义解析器/字符串的PostCSS项目时非常有用：

  ``` js
  postcss: {
    plugins: [...], // list of plugins
    options: {
      parser: sugarss // use sugarss parser
    }
  }
  ```

### cssSourceMap

- type: `Boolean`
- default: `true`

  是否为CSS启用源映射。禁用这个设置可以在`css-loader'避免一些相对路径相关的错误，并使构建更快一点。

  注意，如果主要的Webpack配置中没有出现`devtool`选项，则自动设置为`false`。

### esModule

- type: `Boolean`
- default: `undefined`

  是否提交esModule兼容代码。默认情况下，vue-loader将以commonjs格式提交默认导出，如module.exports = .....当esModule设置为true时，默认导出将转换为导出。exports = ....用于与转换器互操作而不是Babel，如TypeScript。

### preserveWhitespace

- type: `Boolean`
- default: `true`

  如果设置为`false`，模板中HTML标记之间的空格将被忽略。

### transformToRequire

- type: `{ [tag: string]: string | Array<string> }`
- default: `{ img: 'src', image: 'xlink:href' }`

  在模板编译期间，编译器可以将某些属性（如`src`URL）转换为`require`调用，以便目标资源可以由Webpack处理。 默认配置转换`<img>`标签上的`src`属性和SVG的`<image>`标签上的`xlink：href`属性。

### buble

- type: `Object`
- default: `{}`

  配置`buble-loader`（如果存在）的选项，以及模板渲染函数的buble编译。

  > 版本说明：在版本9.x中，模板表达式通过现在删除的`templateBuble`选项单独配置。

  模板渲染函数编译支持一个特殊的变换`stripWith`（默认启用），它删除生成的渲染函数中的`with`用法，使它们兼容严格模式。

  示例配置：

  ``` js
  // webpack 1
  vue: {
    buble: {
      //启用对象展开运算符
      //注意：你需要自己提供Object.assign polyfill！
      objectAssign: 'Object.assign',

      // turn off the `with` removal
      transforms: {
        stripWith: false
      }
    }
  }

  // webpack 2
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          buble: {
            // same options
          }
        }
      }
    ]
  }
  ```
