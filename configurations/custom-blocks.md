# 自定义代码块

> 需要 10.2.0+

您可以在`*.vue`文件中定义自定义语言块。 自定义块的内容将由在`vue-loader'选项的`loaders`对象中指定的加载器处理，然后由组件模块require。 配置类似于[先进的Loader配置](../ configurations / advanced.md)中描述的配置，除了匹配使用标记名称而不是`lang`属性。

如果找到一个自定义块的匹配加载器，它将被处理; 否则将简单地忽略自定义块。

## 例子

以下是将所有`<docs>`自定义块提取到单个文档文件的示例：

#### component.vue

``` html
<docs>
## 这是一个示例组件.
</docs>

<template>
  <h2 class="red">{{msg}}</h2>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Hello from Component A!'
    }
  }
}
</script>

<style>
comp-a h2 {
  color: #f00;
}
</style>
```

#### webpack.config.js

``` js
// Webpack 2.x
var ExtractTextPlugin = require("extract-text-webpack-plugin")

module.exports = {
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue',
        options: {
          loaders: {
            // 将所有<docs>内容提取为原始文本
            'docs': ExtractTextPlugin.extract('raw-loader'),
          }
        }
      }
    ],
    plugins: [
      // 将所有文档输出到单个文件
      new ExtractTextPlugin('docs.md')
    ]
  }
}
```
