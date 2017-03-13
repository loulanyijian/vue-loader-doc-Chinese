# 先进的Loader配置

有时候你可能想：

1. 将自定义加载程序字符串应用于语言，而不是让vue-loader推断它;

2. 覆盖默认语言的内置加载程序配置;

3. 使用自定义加载程序预处理或后处理特定语言块。

为此，为`vue-loader`指定`loaders`选项：

> 注意，`preLoaders`和`postLoaders`只在 >=10.3.0中支持

### Webpack 2.x

``` js
module.exports = {
  // 其余选项...
  module: {
    // module.rules与1.x中的module.loaders相同
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          //`loaders`将覆盖默认加载器。
          //以下配置将导致所有<script>标签没有“lang”
          //属性将通过coffee-loader加载
          loaders: {
            js: 'coffee-loader'
          },

          //`preLoaders`附加在默认加载器之前。
          //你可以使用它来预处理语言块 - 一种常见的用法
          // case将是构建时间i18n。
          preLoaders: {
            js: '/path/to/custom/loader'
          },

          //`postLoaders`附加在默认加载器之后。
          //
          // - 对于`html`，默认加载器返回的结果
          //   将编译的JavaScript渲染函数代码。
          //
          // - 对于`css`，结果将由vue-style-loader返回
          //   这在大多数情况下不是特别有用。 使用postcss
          //   插件将是一个更好的选择。
          postLoaders: {
            html: 'babel-loader'
          }
        }
      }
    ]
  }
}
```

### Webpack 1.x

``` js
// webpack.config.js
module.exports = {
  // 其余配置项...
  module: {
    loaders: [
      {
        test: /\.vue$/,
        loader: 'vue'
      }
    ]
  },
  // vue-loader 配置
  vue: {
    loaders: {
      // 与上面相同的配置
    }
  }
}
```

高级加载器配置的更实际用法是[将组件内部的CSS提取到单个文件中](./extract-css.md)。
