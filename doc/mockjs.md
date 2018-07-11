在前端范围使用 MockJS 时，它会对 XHR 对象进行覆盖，匹配到对应 URL 后直接返回对应数据，不会真正发出请求。

比如这个例子，覆盖 `alert`，参数为对象时不弹出，而是在 `console` 输出：

```js
var _alert = alert
alert = function(e){
    if(typeof e === 'object') {
        console.log(e)
    } else {
        _alert(e)
    }
}
```

而在实际开发时，也许我们会需要有真实的 HTTP 请求存在。
于是我把 Mock 放到了 DevServer 去做。

## 配置

可以在 `build/config.js` 中设置 api 根路径：

```js
module.exports = {
    apiRoot: '/api'    // api root path
}
```

## 添加 API 路由

`mock` 目录中的每个单独的文件都匹配一个路径，规则为根路径加文件名。`mock/city.js ` 对应的路径为`/api/city/`。可以在这个文件中添加该路径下的子路由。

```js
// 输出格式为数组
module.exports = [
    {
        url: '/',
        data: {
            'citys|1-10': {
                '310000': '上海市',
                '320000': '江苏省',
                '330000': '浙江省',
                '340000': '安徽省',
                '350000': '河南省'
            }
        }
    },
    {
        url: '/:id',
        method: 'delete',
        data(req) {
            const id = req.params.id
            // ...

            return {
                code: 200
            }
        }
    }
]
```

每一个路由可由 3 个字段组成：

* `url`：为 `/` 时匹配当前文件对应的路径（如：`/api/city/`），否则追加路径到当前文件对应路径的末尾（如：`/api/city/:id`）。可定义动态路由及正则等格式。
* `method`：缺省值为 `get`，可根据需要设置 `post`、`put`、`delete` 等。
* `data`：可设置为固定数据格式、mockjs 数据模版或函数。当设置为函数时，必须有返回值，通过 request 获取参数进行个性化的数据返回。
