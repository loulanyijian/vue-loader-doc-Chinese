# 生产环境构建

在构建我们的生产bundle时有两件事情要做：

1.压缩我们的应用代码;
2.使用[Vue.js指南中描述的设置](https://vuejs.org/guide/deployment.html)删除Vue.js源代码中的所有警告。

这里有一个示例的配置：

``` js
// webpack.config.js
module.exports = {
  // ... 其余配置
  plugins: [
    // 忽略所有Vue.js警告代码
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"'
      }
    }),
    // 利用dead-code压缩
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      }
    }),
    // 根据发生次数优化模块ID
    new webpack.optimize.OccurenceOrderPlugin()
  ]
}
```

显然我们不想在开发过程中使用这个配置，所以有几种方法来实现：

1.基于环境变量动态构建配置对象;

2.或者，使用两个单独的Webpack配置文件，一个用于开发，一个用于生产。 并且可能在第三个文件中共享一些常见的选项，如[vue-hackernews-2.0](https://github.com/vuejs/vue-hackernews-2.0)所示。

只要它达到目标，它真的取决于你。