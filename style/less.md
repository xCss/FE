## 命名

1. `class` 名称中只能出现小写字符和破折号 `-` （不是下划线，也不是驼峰命名法）。破折号应当用于相关 `class` 的命名（类似于命名空间）。
2. 避免过度任意的简写。`.btn` 代表 `button`，但是 `.s` 不能表达任何意思。
3. `class` 名称应当尽可能短，并且意义明确。
4. 基于最近的父 `class` 或基本（`base`） `class` 作为新 `class` 的前缀。

示例：`.tabs`、`.tabs-item`、`.tabs-item-text`

## 缩进

2 个空格，而非TAB。可通过更改编辑器配置或安装 **Editorconfig** 插件应用项目中的配置文件。

## 空格

选择器和 `{` 之间

```css
.selector {
}
```

`:` 和属性值之间

```css
line-height: 1.5;
```

属性值中的 `,` 之后

```css
box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;
```

`>`、`+`、`~` 选择器的两边

```less
.selector {

  & > a {

  }

  & ~ .icon {

  }

  & + .footer {

  }
}
```


## 选择器声明

多选择器之间换行

```css
.selector-1,
.selector-2,
.selector-3 {
}
```

样式块之间留空行

```css
.selector-1 {

}

/* 这里空行 */
.selector-2 {

}
```

单条属性的样式块之间可不空行

```css
.pull-left { float: left; }
.pull-right { float: right; }
```


## 顺序

推荐的属性书写顺序，但不强制要求。

1. Positioning: 定位属性
2. Box model: 盒子模型
3. Typographic: 文本属性
4. Visual: 视觉属性
5. Other: 其他

```css
.classname {
  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  /* Box-model */
  display: block;
  float: right;
  width: 100px;
  height: 100px;
  padding: 20px;

  /* Typography */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  /* Visual */
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  /* Other */
  opacity: 1;
}
```

## 嵌套

减少嵌套层级。嵌套应由命名空间体现，而非 `class` 嵌套。

```less
// ×
.list {
  .item {
    .text {

    }
  }
}

// ×
.list {
  .list-item {
    .lsit-item-text {

    }
  }
}

// √
.list {

  &-item {
  }

  &-item-text {

  }

  &-item-link {

  }
}
```

!> 滥用嵌套会让生成后的 CSS 代码异常恐怖：`.a .b .c .d .e ... { }`。

嵌套时思考以下几个方面：

* 编译 CSS 越少越好。
* 选择器越少越好。
* 编译文件越小越好。
* 基于组件的 CSS 更好。
* 不太具体的 CSS 更好。

使用伪类嵌套感觉像是嵌套 CSS 的唯一适用方法。

```less
.btn {
  &:hover,
  &:focus { }

  &:active { }
}
```

> 参考：[Nesting in Sass and Less](http://markdotto.com/2015/07/20/css-nesting/)

## 覆写样式

假如要局部覆盖一个 `class` 为 `modal` 的样式，推荐以下两种方法：

1.基于当前模块添加新的 `class`

```html
<div class="modal module-modal"></div>
```

```less
.module-modal.modal {

}
```

2.基于临近的父级模块 `class` 作为选择器父级

```html
<div class="admin-wrap">
  <div class="modal"></div>
</div>
```

```less
.admin-wrap {
  .modal {

  }
}
```

如果是全局性样式覆写，直接使用原类名即可。

## 数值

属性值为小数时，省略整数部分的 `0`

```less
.link {
  &:hover {
    opacity: .8;
  }
}
```

## 长度

长度为 `0` 时，省略单位

```css
padding: 0 10px;
```

## 状态

使用有意义的类名定义的不同状态下的样式，便于代码中直接切换 `class`，而非显示设置样式。

```less
.alert {
  opacity: 0;
  transition: opacity .3s;

  &.active {
    opacity: 1;
  }
}
```

切换显示状态只需要：

```js
// ✓
alertEl.classList.toggle('active')
```

## 变量

使用连接符 `-` 定义变量，与选择器命名规则一致

```less
@font-size-base: 14px;
@line-height-base: 1.5;
@text-color: #333;
```

组件的变量尽量引用基础变量或基于基础变量计算。越少的变量源，就意味着我们只需要改变少数变量就可以生成新的 `theme`。

```less
// base
@primary-color: #20a0ff;
@font-size-base: 14px;

// header
@header-link-color: @primary-color;
@header-link-hover-color: lighten(@primary-color, 20%);
@header-font-size: ceil((@font-size-base * 1.25));
```

一切你认为会发生变化的值都可设为变量，可以参考 [bootstrap](https://github.com/twbs/bootstrap/blob/master/less/variables.less) 的拆分粒度。

## Mixin

如果 `mixin` 名称不是一个需要使用的 `className`，加上括号，否则即使不被调用也会输出到 CSS 中

```less
// ✗
.text-2x {
  font-size: 2em;
}

h3 {
  .text-2x;
}

// ✓
.text-2x() {
  font-size: 2em;
}

h3 {
  .text-2x();
}
```

带括号调用的 `mixin` 不会编译为 `class`。从这个角度看，你可以定义任意可复用的、可提高编码效率的 `mixin`。
但你需考虑这个 `mixin` 是否可以定义为 `class`，以达到页面级的复用。

```less
// 1: 定义 mixin，代码级的复用
.abs-left() {
  position: absolute;
  top: 0;
  left: 0;
}

.selector {
  // css...
  .abs-left();
}
```

```html
<div class="selector"></div>
```

使用时只需要给节点添加一个 `class`，`my-mixin`只存在与代码运行时，不会被编译到 `css` 文件中。


```less
// 2: 定义 class，页面级的复用
.abs-left {
  position: absolute;
  top: 0;
  left: 0;
}

.selector {
  // css...
}
```

```html
<div class="selector abs-left"></div>
```

使用时，需要给节点添加多个 `class`，`my-mixin` 会被编译到 `css` 文件中。


根据代码的可复用性酌情选择方案，我们也不希望看到这样的页面内引用。

```html
<div class="class-1 class-2 class-3 class-n">...</div>
```

## 引用

不全局引用所有样式，只引用基础或公共样式。

组件引用样式有两种方式：

1. 使用 `less` 文件编写样式，把组件样式关联到对应的 Vue 组件内

```html
<template>

</template>
<script>

</script>
<style lang="less">
@import '~less/tabs.less';
</style>
```

2. 在组件内编写样式，引用需要的 `varible` 或 `mixin`

```html
<template>...</template>
<script>...</script>
<style lang="less">
@import '~less/varible.less';

.tabs {

}
</style>
```

## 编辑器

工欲善其事，必先利其器。去了解你的编辑器配置，以及使用可为你带来编码效率的插件。

### EditorConfig

```
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 4
insert_final_newline = true
trim_trailing_whitespace = true

[*.{css,less,json}]
indent_size = 2
```

### CSScomb

```json
{
    "remove-empty-rulesets": true,
    "always-semicolon": true,
    "color-case": "lower",
    "block-indent": "  ",
    "color-shorthand": true,
    "element-case": "lower",
    "eof-newline": true,
    "leading-zero": false,
    "quotes": "double",
    "space-before-colon": "",
    "space-after-colon": " ",
    "space-before-combinator": " ",
    "space-after-combinator": " ",
    "space-between-declarations": "\n",
    "space-before-opening-brace": " ",
    "space-after-opening-brace": "\n",
    "space-after-selector-delimiter": "\n",
    "space-before-selector-delimiter": "",
    "space-before-closing-brace": "\n",
    "strip-spaces": true,
    "tab-size": true,
    "unitless-zero": true,
    "vendor-prefix-align": true
}
```

## 其他

未免与个人编码习惯冲突，没有作太多的代码级约束。可以参考一些好的实践逐渐养成良好的编码习惯。

* http://codeguide.bootcss.com/
* https://developer.mozilla.org/zh-CN/docs/Web/CSS/Shorthand_properties
* http://www.runopencode.com/index.php/how-we-code/css-and-less-coding-standards
* https://github.com/ecomfe/spec/blob/master/css-style-guide.md
* https://www.drupal.org/node/1887862
