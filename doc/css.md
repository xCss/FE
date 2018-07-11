## 公共样式

样式重置、图标、字体、工具类等样式样式，各自单独一个文件。

```
🗁 less
  |--🗎 app.less
  |--🗎 base.less
  |--🗎 icons.less
  |--🗎 mixins.less
  |--🗎 reset.less
  |--🗎 utils.less
  `--🗎 variables.less
```

`app.less` 作为编译入口，引入 variables、mixins 和全部公共样式。

```less
@import "variables.less";
@import "mixins.less";
@import "reset.less";
@import "base.less";
@import "icons.less";
@import "utils.less";
```

在项目入口文件引入 `app.less`。

```js
// main.js
import './less/app.less'
```

## 组件样式

可以把各个组件相关的样式写入组件内 `<style>...</style>` 中，组件样式引入公共依赖进行调用。

```vue
<template>...</template>
<script>...</script>
<style lang="less">
@import "~less/variables";
@import "~less/mixins";
/*
  ...
*/
</style>
```

也可以在组件目录中，添加同名 `less` 文件，并在组件内引入。

```
🗁 components
  `--🗁 myComponent
  |  |--🗎 index.vue
  |  `--🗎 myComponent.less
```

```vue
<template>...</template>
<script>...</script>
<style lang="less">
@import "./myComponent";
</style>
```

组件功能和样式内聚，在删除一个组件功能的同时，样式也被删除，不需要考虑废弃的样式被打包。

## 变量管理

变量按照全局和类别，分模块编写，类别样式引用全局样式。

```less
// global
@primary-color: #20a0ff;

// btn
@btn-primary-background: #20a0ff;

// link
@link-color: #20a0ff;
@link-hover-color: darken(#20a0ff, 10%);
```

> 示范文件：[bootstrapV3 - variable.less](https://github.com/twbs/bootstrap/blob/master/less/variables.less)
