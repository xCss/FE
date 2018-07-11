## 命名

组件文件命名与引用使用 `CamelCase` 规则，首字母小写，其余单词首字母大写。

```
app.vue
navMenu.vue
```

```js
import navMenu from 'components/navMenu'

export default {
    components: {
        navMenu
    }
}
```

模版内组件遵循 `自定义元素规范`：全小写，使用 `-` 分隔单词。

```vue
<app-header></app-header>
<user-list></user-list>
<range-slider></range-slider>
```

## name

组件必须包含 `name` 属性。这在你使用 VueDevtools 时能很方便查找组件。

```js
export default {
    name: 'myComponent'
}
```

## 顺序

三个区块的书写顺序按关注度排列。

```vue
<template></template>
<script></script>
<style></style>
```

实例属性顺序书写建议：

```js
export default {
    name,
    // 扩展
    mixins,
    extends,
    // 子级
    components,
    // 属性
    props,
    propsData,
    data,
    computed,
    // 函数
    watch,
    methods,
    // 生命周期
    beforeCreate,
    created,
    beforeMount,
    mounted,
    beforeUpdate,
    updated,
    activated,
    deactivated,
    beforeDestroy,
    destroyed
}
```

引用按类别分组：

```js
// bad
import comp1 from 'comp1'
import Api from 'Api'
import comp2 from 'comp2'
import util from 'util'

// good
// -> 组件
import comp1 from 'comp1'
import comp2 from 'comp2'
// -> 库
import util from 'util'
// -> 服务
import Api from 'Api'
```

## 模板

对于可简写的指令使用简写。

```vue
<!-- bad -->
<div v-bind:class="className" v-bind:html="hello"></div>
<input v-on:change="onChange">

<!-- good -->
<div :class="className" :html="hello"></div>
<input @change="onChange">
```

属性过多时，换行。

```vue
<!-- bad -->
<my-component v-if="show" :comp-id="id" :data-list="dataList" :map-options="mapOptions" @on-change="onChange"></my-component>

<!-- good -->
<my-component v-if="show"
    :comp-id="id"
    :data-list="dataList"
    :map-options="mapOptions"
    @on-change="onChange"></my-component>
```

无 `slot` 的组件可以自闭合。

```vue
<my-loding :show="show"/>
```

## Props

永远对 `props` 进行验证：

* 提供默认值
* 使用 `type` 属性校验类型
* 使用 `props` 之前先检查该 `prop` 是否存在

```vue
<template>
  <ul v-if="data">
    <li v-for="item in data">{{ item }}</li>
  </ul>
</template>
<script>
export default {
    props: {
        data: Array,
        msg: {
            type: String,
            default: 'hello'
        }
    },
}
</script>
```

> 验证组件 props 可以保证你的组件永远是可用的（防御性编程）。即使其他开发者并未按照你预想的方法使用时也不会出错。

组件 `props` 保持原子化。即：组件的每一个属性单独使用一个 `props`，并且使用函数或是原始类型（字符串、数字、布尔值）的值。

```vue
<!-- 推荐 -->
<range-slider
  :values="[10, 20]"
  min="0"
  max="100"
  step="5"
  :on-slide="updateInputs"
  :on-end="updateResults">
</range-slider>

<!-- 避免 -->
<range-slider :config="complexConfigObject"></range-slider>
```

## 事件

* 避免与原生事件名一致
* 事件名使用连字符分隔多个单词
* 事件名要有意义，一个基本原则为：**什么事已发生**。如：`show`、`row-edit`、`item-selected`，动词或形容词结尾。
* 事件所绑定的函数，表示有意义的操作，遵循函数命名规则：`do-something`。

```vue
<child @item-selected="selectedChildItem"></child>
```

## 减少状态

* 如果一个数据可以由另一个 `state` 变换得到，那么这个数据就不是一个 `state`。只需要写一个变换的处理函数，或者是表达式、计算属性。
* 不要重复自己。如果你的 `state` 是一个数组，而模版最外层是渲染这个数组，那么你需要做的事是把渲染的项作为一个组件，只接受一个单级对象形式的数据，由外部决定这个组件的展示次数。
* 如果一个数据是固定的，不会变化的常量，那么这个数据就如同 HTML 固定的站点标题一样，写死或作为全局配置属性等，不属于 `state`。
* 如果一个数据需要从外部得到，它应该属于 `props`。
* 如果组件和兄弟组件拥有相同的 `state`，那么这个 `state` 应该放到更高的层级中，使用 `props` 传递到两个组件中。

## this

在组件上下文，`this` 指向了组件实例。因此当你切换到了不同的上下文时，要确保 `this` 仍然指向组件。
使用箭头函数能有效规避这个问题。

```js
export default {
    data() {
        return {
            msg: 'hello'
        }
    },
    created() {
        // 避免
        setTimeout(function() {
            // this 指向 window
            console.log(this.msg)
        }, 100)

        // 避免
        const self = this
        setTimeout(function() {
            console.log(self.msg)
        }, 100)

        // 推荐
        setTimeout(() => {
            console.log(this.msg)
        }, 100)
    }
}
```

## 避免操作DOM

遵循数据驱动，由 `state` 映射 DOM 变化，而非手动切换。

**避免：**

```vue
<template>
    <ul>
        <li v-for="item in data" @item-click="highlightItem">{{ item }}</li>
    </ul>
</template>
<script>
export default {
    data() {
        return {
            data: [1, 2, 3]
        }
    },
    methods: {
        highlightItem(e) {
            e.target.parentNode.querySelector('li.active').classList.remove('active')
            e.target.classList.add('active')
        }
    }
}
</script>
```

**推荐：**

```vue
<template>
    <ul>
        <li v-for="(item, i) in data"
            @item-click="highlightItem(i)"
            :class="{ active: highlightIndex === i }">{{ item }}</li>
    </ul>
</template>
<script>
export default {
    data() {
        return {
            data: [1, 2, 3],
            highlightIndex: 0
        }
    },
    methods: {
        highlightItem(index) {
            this.highlightIndex = index
        }
    }
}
</script>
```

避免使用 `this.$parent`、`this.$children`、`this.$refs`：

* 组件必须相互保持独立，Vue 组件也是。如果组件需要访问其父层或下级的上下文就违反了该原则。
* 如果一个组件需要访问其父层或下级组件的上下文，那么该组件将不能在其它上下文中复用。
* 当遇到 `props` 和 `events` 难以实现的功能时，通过 `this.$refs` 来实现。
* 当需要操作 DOM 无法通过指令来做的时候可使用 `this.$ref` 而不是 `JQuery`、`document.getElement*`、`document.querySelector`。

!> 组件的属性和事件必须足够的给大多数的组件使用。思考从组件设计上规避以上问题，或者可以采用[指令](https://cn.vuejs.org/v2/guide/custom-directive.html)封装 DOM 操作。

## 样式作用域

采用 [CSS Modules](https://github.com/vuejs/vue-loader/blob/master/docs/zh-cn/features/css-modules.md) 方案，规则：`[name]-[local]__[hash:base64:6]`。

```
// app.vue
.header  ->  .app-header__1Sn5qI_0
```

使用 CSS Modules 的文件名中必须包含模块名。

```bash
# bad
🗁 tree
  |--🗎 index.vue
  |--🗎 item.vue

# good
🗁 tree
  |--🗎 index.js
  |--🗎 tree.vue
  |--🗎 treeItem.vue
```

> 一个名称为 `treeItem.vue` 中的类名可以生成 `.treeItem-className__xxxx` 的格式，但是 `item.vue` 只能生成 `.item-className__xxxx`。前者更具有语义。

样式类名不使用连接符 `-`，而是使用驼峰命名：

```vue
<template>
    <div>
        <!-- bad, 变量可包含的特殊符号只能是 _ 或 $ -->
        <p :class="$style.text-red">hello</p>

        <!-- good -->
        <p :class="$style.textRed">world</p>
    </div>
</template>
<style module>
.text-red,
.textRed {
    color: red
}
</style>
```

样式名，1~3 个单词为佳，不要与文件名中的单词一致。我们可以把文件名比作 `module`，那么样式名可取的类别可以是 `position`、`element`、`block`、`name` 等。
你不需要担心 `a.vue` 中的 `.header` 会与 `b.vue` 中的 `.header` 冲突。

绑定多个类名时，使用 `Array` 语法：

```vue
<header :class="[$style.header, $style.fixed]"></header>
```

绑定动态类时使用 `Object` 语法：

```vue
<header :class="{ [$style.show]: show }"></header>
```

固定类和动态类同时存在于一个元素时，使用 `Array` + `Object` 语法：

```vue
<header :class="[$style.header, $style.fixed, { [$style.show]: show }]"></header>
```

!> 对于公共组件不建议使用 CSS Modules。因为每次编译后的类名 base64 值都不一样，这会导致样式难以被局部覆写。
