> 希望大家能一起参与到团队文档的建设中来。

## 克隆仓库

```bash
$ git clone git@github.com:HYFE/Guide.git
```

## 创建文档

文档均为 `Markdown` 格式，如果是一个新的分类内容可以直接放到仓库根目录，否则放入分类目录或新建目录。

如果需要添加图片，把图片放入 `img` 目录，不管文档在什么位置，引入路径均为 `/img/xxx.ext`。

```markdown
![](/img/image.jpg)
```

## 设置导航

编辑 `_sidebar.md` 为你新添加的文档按层级设置链接。

```markdown
- 规范
    - [CSS/LESS](guide/less)
    - [JavaScript](guide/js)
    - [Vue](guide/vue)
- 指南
    - [统一命名](dev/naming)
    - [样式管理](dev/css)
    - [组件管理](dev/components)
    - [图标集成](dev/icon)
    - [axios集成与使用](dev/axios)
    - [MockJs配置](dev/mockjs)
    - [Vue测试介绍](dev/vue-test)
- [贡献](contribution)
```

## 本地预览

如果你想在本地预览文档，可以安装一个简单的 WebServer，如：[anywhere](https://github.com/JacksonTian/anywhere)。

```bash
# 全局安装 anywhere
$ npm install anywhere -g

# 在需要开启服务器的目录执行
$ anywhere
```

## 在线编辑

如果嫌以上步骤太麻烦，你也可以在[仓库](https://github.com/HYFE/Guide)内在线添加和编辑文档。
