## Doctype

使用 HTML5 doctype。

```html
<!DOCTYPE html>
```

## 字符编码

明确声明为通用的 UTF-8 编码。

```html
<meta charset="UTF-8">
```

## 渲染模式

针对国内浏览器环境设置 IE 兼容模式和极速渲染模式。

```html
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
<meta name="renderer" content="webkit">
```

## 样式、脚本引用

HTML5 规范中，不需要为 CSS 和 JavaScript 指定 `type` 属性，因为 `text/css` 和 `text/javascript` 是它们的默认值。

```html
<!-- 外部 CSS -->
<link rel="stylesheet" href="your.css">
<!-- 页面 CSS -->
<style>
    /* ... */
</style>

<!-- 外部 js -->
<script src="your.js"></script>
<!-- 页面 js -->
<script>
    // ...
</script>
```

## 标签闭合

省略自闭合标签尾部的 `/`。

```html
<!-- × -->
<img src="logo.jpg" alt="logo"/>

<!-- √ -->
<img src="logo.jpg" alt="logo">
```

## 布尔属性

HTML5 规范中布尔型属性可以在声明时不赋值。

```html
<input type="text" disabled readonly>

<input type="checkbox" value="1" checked>
```

## 减少层级

不要有额外的冗余的结构。

```html
<!-- 如果可以这样 -->
<h1></h1>

<!-- 就不要这样 -->
<div>
    <h1></h1>
</div>
```

可以并列，就不要嵌套。

```html
<!-- 如果可以这样 -->
<dl>
    <dt></dt>
    <dd></dd>
    <dd></dd>
</dl>

<!-- 就不要这样 -->
<dl>
    <dt></dt>
    <dd>
        <div></div>
        <div></div>
    </dd>
</dl>
```

## 语义化

* 由内容类型决定标签，使用 `header`、`footer`、`section`、`article`、`nav`、`aside`、`ul`、`ol`、`dl` 等替代漫天的 `div`。
* `table` 不用于布局，但在展示明确的表格型数据时还是首选。
* 在资源型的内容上加入描述文案，比如给 `img` 添加 `alt` 属性等。

## 按钮

如果使用 `button` 定义按钮，请明确设置 `type` 属性。因为不同的浏览器默认类型是不一致的。

```html
<button type="button">Click</button>
```

## 实体字符

以实体代替与 HTML 语法相同的字符，避免解析错误。

字符 | 名称 | 实体
---|---|---
" | 引号 | `&quot;`
& | 与 | `&amp;`
< | 小于 | `&lt;`
> | 大于 | `&gt;`
  | 空格 | `&nbsp;`
