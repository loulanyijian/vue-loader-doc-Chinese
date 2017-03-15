# 资源url处理

默认情况下，`vue-loader`使用[css-loader](https://github.com/webpack/css-loader) 和Vue模板编译器自动处理你的样式和模板文件。
在这个编译过程中，所有的资源URL，例如 img src="...", `background：url（...）`和CSS`@import`**被解析为模块依赖**。

例如，url（./ image.png）将被翻译为require（'./ image.png'）和

``` html
<img src="../image.png">
```

将会被编译成：

``` js
createElement('img', { attrs: { src: require('../image.png') }})
```

因为`.png`不是JavaScript文件，你需要配置Webpack使用[file-loader](https://github.com/webpack/file-loader)或[url-loader](https：// github.com/webpack/url-loader)来处理它们。用`vue-cli`脚手架的项目也为你配置了这个。

所有这些的好处是：

1.`file-loader`允许你指定复制和放置资源文件的位置，以及如何使用版本哈希对其进行命名以获得更好的缓存。此外，这也意味着**你可以放置图像旁边你的`*.vue`文件，并使用相对路径基于文件夹结构，而不是担心部署URL**。使用正确的配置，Webpack将自动重写文件路径到绑定的输出中的正确的URL。

2.`url-loader`允许你有条件地将文件内联为base-64数据URL，如果它们小于给定的阈值。这可以减少对琐碎文件的HTTP请求的数量。如果文件大于阈值，它会自动回退到`file-loader`。