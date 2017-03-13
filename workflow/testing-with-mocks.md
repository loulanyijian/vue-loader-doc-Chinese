＃使用Mocks测试

在现实世界的应用程序中，我们的组件很可能具有外部依赖性。当为组件编写单元测试时，如果我们可以模拟这些外部依赖性，以使我们的测试仅依赖于被测试组件的行为，这将是理想的。

`vue-loader`提供了一个特性，它允许你使用[inject-loader](https://github.com/plasticine/inject-loader)向`*.vue`组件注入任意依赖项。一般的想法是，不是直接导入组件模块，我们使用`inject-loader`为该模块创建一个“模块工厂”函数。当这个函数被一个mock对象调用时，它返回一个带有mocks的模块的实例。

假设我们有一个这样的组件：

``` html
<!-- example.vue -->
<template>
  <div class="msg">{{ msg }}</div>
</template>

<script>
// 这个依赖需要被mocked
import SomeService from '../service'

export default {
  data () {
    return {
      msg: SomeService.msg
    }
  }
}
</script>
```

下面是如何使用mock导入它：

> 注意：inject-loader@3.x目前不稳定。

``` bash
npm install inject-loader@^2.0.0 --save-dev
```

``` js
// example.spec.js
const ExampleInjector = require('!!vue?inject!./example.vue')
```

注意，crazy require string - 我们在这里使用一些内联的[webpack加载器请求](https://webpack.github.io/docs/loaders.html)。 快速解释：

- `!!`在开始时意味着“禁止所有加载器从全局配置”;
- `vue？inject！`意思是“使用`vue`加载器，并传递`？inject`查询。 这告诉`vue-loader`在依赖注入模式下编译组件。

返回的“ExampleInjector”是一个工厂函数，可以调用它来创建`example.vue`模块的实例：

``` js
const ExampleWithMocks = ExampleInjector({
  // mock it
  '../service': {
    msg: 'Hello from a mocked service!'
  }
})
```

最后，我们可以像平常一样测试组件：

``` js
it('should render', () => {
  const vm = new Vue({
    template: '<div><test></test></div>',
    components: {
      'test': ExampleWithMocks
    }
  }).$mount()
  expect(vm.$el.querySelector('.msg').textContent).toBe('Hello from a mocked service!')
})
```
