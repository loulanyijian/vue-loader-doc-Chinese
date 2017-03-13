# 代码风格

你可能已经想知道如何将你放在`*.vue`文件中的代码检查风格了，尽管它们不是JavaScript。我们将假设你正在使用[ESLint](http://eslint.org/)（如果你还没用，那你应该用了！）。

你还需要[eslint-plugin-html]（https://github.com/BenoitZugmeyer/eslint-plugin-html），它支持在 `*.vue` 文件中提取和删除JavaScript。

确保在ESLint配置中包含插件：

``` json
"plugins": [
  "html"
]
```

然后在命令行中：

``` bash
eslint --ext js,vue MyComponent.vue
```

另一个选项是使用[eslint-loader](https://github.com/MoOx/eslint-loader)，以便您的`* .vue`文件在开发过程中自动保存：

``` bash
npm install eslint eslint-loader --save-dev
```

``` js
// webpack.config.js
module.exports = {
  // ... 其余选项
  module: {
    loaders: [
      {
        test: /.vue$/,
        loader: 'vue!eslint'
      }
    ]
  }
}
```

注意，Webpack加载器链应用于 **first-first**。 确保在`vue`之前应用`eslint`，所以我们正在编译预编译源代码。

我们需要考虑的一件事是使用NPM包中提供的第三方`*.vue`组件。 在这种情况下，我们要使用`vue-loader`来处理第三方组件，但不想去掉它。 我们可以分离到Webpack的[preLoaders](https://webpack.github.io/docs/loaders.html#loader-order)：

``` js
// webpack.config.js
module.exports = {
  // ... 其余选项
  module: {
    // 只检测本地`*.vue`文件
    preLoaders: [
      {
        test: /.vue$/,
        loader: 'eslint',
        exclude: /node_modules/
      }
    ],
    // 针对所有的`*.vue`文件使用vue-loader
    loaders: [
      {
        test: /.vue$/,
        loader: 'vue'
      }
    ]
  }
}
```

For Webpack 2.x:

``` js
// webpack.config.js
module.exports = {
  // ... 其余选项
  module: {
    rules: [
      // 只检测本地`*.vue`文件
      {
        enforce: 'pre',
        test: /.vue$/,
        loader: 'eslint-loader',
        exclude: /node_modules/
      },
      // 针对所有的`*.vue`文件使用vue-loader
      {
        test: /.vue$/,
        loader: 'vue-loader'
      }
    ]
  }
}
```
