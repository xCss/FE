目前在 Vue 的开发实践中一般有两种测试：单元测试（Unit）和端到端测试（E2E）。

## 单元测试

> 单元测试，是指对软件中的最小可测试单元进行检查和验证。

在 Vue 中，以一个组件为最小可测试单元，也就是最小的可测试功能模块。在测试运行时，将会把一个独立单元置入与程序其他部分相隔离的情况下测试。

一个简单的 Vue 单元测试用例：

```js
import Vue from 'vue'
import Hello from 'src/components/Hello'

describe('Hello.vue', () => {
  it('should render correct contents', () => {
    const vm = new Vue({
      el: document.createElement('div'),
      render: (h) => h(Hello)
    })
    expect(vm.$el.querySelector('.hello h1').textContent)
      .to.equal('Welcome to Your Vue.js App')
  })
})
```

在单元测试环境中，被测试组件将与其他 Vue 组件和模块（如：vuex、vueRouter等）隔离。这意味着如果一个组件依赖其他组件或模块，会难以达到单元测试的条件。

**期望：**

> 很多组件的渲染输出由它的props决定。事实上，如果一个组件的渲染输出完全取决于它的props，那么它会让测试变得简单，就好像断言不同参数的纯函数的返回值。        *--[编写可被测试的组件](https://vuefe.cn/guide/unit-testing.html#编写可被测试的组件)*

组件编写保持单一的原则，尽量保证每一个文件的代码行数不要超过 100 行，也尽量保证组件可独立的运行，便于编写单元测试。

## 端到端测试

![Vue-e2e-test-示例](https://segmentfault.com/img/remote/1460000006769113)

图片来自 [搭建自己的前端自动化测试脚手架](https://segmentfault.com/a/1190000005991670?share_user=1030000002791361)

只需要敲一条命名，就能启动浏览器窗口，按照你预定的动作对页面模拟操作，在整个测试周期结束后显示测试结果。这才是我们理想中的自动化测试啊！

这是目前集成的示例项目中的测试用例：

```js
module.exports = {
    'default e2e tests': function (browser) {
        // doc: http://nightwatchjs.org/api
        const devServer = browser.globals.devServerURL

        browser.url(devServer)
            .waitForElementVisible('#app', 10000)
            .waitForElementVisible('button', 2000)
            .click('button')
            .pause(3000)

        browser.expect.element('p').text.to.match(/共[ ]?[1-9][ ]?条数据/)
        browser.expect.element('ul li').to.be.visible
        browser.end()
    }
}
```

所做的工作有：

1. 启动浏览器，打开测试服务器网址
2. 等待 10s，直到 `#app` 为可见状态
3. 等待 2s，直到 `button` 为可见状态
4. 模拟点击 `button`
5. 暂停 3s
6. 期望 `p` 标签的内容匹配 `/共[ ]?[1-9][ ]?条数据/`
7. 期望 `ul li` 为可见状态
8. 结束会话，如果没有下一个测试用例就结束测试

如此便可一劳永逸，而你需要做的就是写测试！[NightWatch API](http://nightwatchjs.org/api)

### 弊端

有时候虽然 e2e 没有通过，并不代表程序是有问题的。

由于 Vue 应用节点多为组件动态构建或异步载入，在测试运行时可能不能正确获取到状态的变更，所以测试中会加入许多延迟、暂停类的等待语句。尽管如此也不能保证 100% 测试通过。

缩短网络请求时间也许是最简单的解决办法了，只需要服务器和运行测试都在本地进行，甚至一台机器的同一端口。
