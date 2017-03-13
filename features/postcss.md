# PostCSS

`vue-loader`处理的任何CSS输出都通过PostCSS进行管道传输以进行作用域CSS重写。您还可以向过程添加自定义PostCSS插件，例如[autoprefixer](https://github.com/postcss/autoprefixer) 或 [CSSNext](http://cssnext.io/)。

## 使用配置文件

从11.0开始`vue-loader`支持自动加载`postcss-loader`支持的相同PostCss配置文件：

- `postcss.config.js`
- `.postcssrc`
- `postcss` field in `package.json`

使用配置文件允许你在`postcss-loader`处理的普通CSS文件和`*.vue`文件中的CSS之间共享相同的配置，建议如此使用。

## 内联选项

或者，您可以使用vue-loader的postcss选项为* .vue文件指定postcss配置。

在Webpack 1.x中的实例用法:

``` js
// webpack.config.js
module.exports = {
  // other configs...
  vue: {
    // 使用自定义postcss插件
    postcss: [require('postcss-cssnext')()]
  }
}
```

For Webpack 2.x:

``` js
// webpack.config.js
module.exports = {
  // 其余选项...
  module: {
    // module.rules与1.x中的module.loaders相同
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        // vue-loader 配置项在这里
        options: {
          // ...
          postcss: [require('postcss-cssnext')()]
        }
      }
    ]
  }
}
```

除了提供一个插件数组，`postcss`选项也接受：

- 返回一个插件数组的函数;

- 包含要传递到PostCSS处理器的选项的对象。 这在使用依赖于自定义解析器/字符串的PostCSS项目时非常有用：

  ``` js
  postcss: {
    plugins: [...], // 组件列表
    options: {
      parser: sugarss // 使用sugarss解析器
    }
  }
  ```
