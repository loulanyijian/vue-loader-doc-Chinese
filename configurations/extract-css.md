# 将CSS提取到单个文件中

将所有Vue组件中的所有已处理的CSS解压缩到单个CSS文件中示例配置：

### Webpack 2.x

``` bash
npm install extract-text-webpack-plugin@2.x --save-dev
```

``` js
// webpack.config.js
var ExtractTextPlugin = require("extract-text-webpack-plugin")

module.exports = {
  // 其余配置...
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          loaders: {
            css: ExtractTextPlugin.extract({
              use: 'css-loader',
              fallback: 'vue-style-loader' // <- 这是vue-loader的dep，所以如果使用npm3，不需要显式安装
            })
          }
        }
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin("style.css")
  ]
}
```

### Webpack 1.x

``` bash
npm install extract-text-webpack-plugin --save-dev
```

``` js
// webpack.config.js
var ExtractTextPlugin = require("extract-text-webpack-plugin")

module.exports = {
  // 其余配置...
  module: {
    loaders: [
      {
        test: /\.vue$/,
        loader: 'vue'
      },
    ]
  },
  vue: {
    loaders: {
      css: ExtractTextPlugin.extract("css"),
      // 你还可以包括<style lang ="less">或其他语言
      less: ExtractTextPlugin.extract("css!less")
    }
  },
  plugins: [
    new ExtractTextPlugin("style.css")
  ]
}
```
