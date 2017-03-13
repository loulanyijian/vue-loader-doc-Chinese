# 测试

> [webpack vue-cli模板](https://github.com/vuejs-templates/webpack)为你提供预配置的单元测试和e2e测试设置。

当测试`*.vue`文件时，我们不能使用普通的基于CommonJS的测试运行器，因为它不知道如何处理`*.vue`文件。相反，我们仍然使用Webpack + vue-loader来打包我们的测试文件。建议的设置是使用[Karma](http://karma-runner.github.io/0.13/index.html)和[karma-webpack](https://github.com/webpack/karma-webpack)。

Karma是一个测试运行器，启动浏览器并为您运行测试。您可以选择要测试的浏览器以及要使用的测试框架（例如Mocha或Jasmine）。下面是一个使用[Jasmine](http://jasmine.github.io/edge/introduction.html)测试框架在[PhantomJS](http://phantomjs.org/)中运行测试的Karma配置示例：

``` bash
npm install\
  karma karma-webpack\
  karma-jasmine jasmine-core\
  karma-phantomjs-launcher phantomjs\
  --save-dev
```

``` js
//我们只需要使用完全相同的webpack配置
//但是，请记住删除原始条目，因为我们在测试期间不需要它
var webpackConfig = require('./webpack.config.js')
delete webpackConfig.entry

// karma.conf.js
module.exports = function (config) {
  config.set({
    browsers: ['PhantomJS'],
    frameworks: ['jasmine'],
    // 这是我们所有测试文件的入口
    files: ['test/index.js'],
    // 我们将把入口文件传递给webpack进行打包。
    preprocessors: {
      'test/index.js': ['webpack']
    },
    // 使用webpack配置
    webpack: webpackConfig,
    // 删除没用得文本
    webpackMiddleware: {
      noInfo: true
    },
    singleRun: true
  })
}
```

在`test/index.js`入口文件中添加：

``` js
// test/index.js
// 要求所有测试文件使用特殊的Webpack功能
// https://webpack.github.io/docs/context.html#require-context
var testsContext = require.context('.', true, /\.spec$/)
testsContext.keys().forEach(testsContext)
```

这个入口文件只简单的require同一文件夹`.spec.js`结尾的所有其他文件。现在我们可以写一些测试：

``` js
// test/component-a.spec.js
var Vue = require('vue')
var ComponentA = require('../../src/components/a.vue')

describe('a.vue', function () {

  // 断言 JavaScript 条件
  it('should have correct message', function () {
    expect(ComponentA.data().msg).toBe('Hello from Component A!')
  })

  // 通过实际渲染该组件来断言渲染结果
  it('should render correct message', function () {
    var vm = new Vue({
      template: '<div><test></test></div>',
      components: {
        'test': ComponentA
      }
    }).$mount()
    expect(vm.$el.querySelector('h2.red').textContent).toBe('Hello from Component A!')
  })
})
```

要运行测试，需要添加如下的NPM脚本：

``` js
// package.json
...
"scripts": {
  ...
  "test": "karma start karma.conf.js"
}
...
```

Finally, run:

``` bash
npm test
```

重申一遍，[webpack vue-cli模板](https://github.com/vuejs-templates/webpack)包含一个关于测试的完整工作示例。
