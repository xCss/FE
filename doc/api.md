> 跟随社区的脚步，HTTP 库从 VueResource 换为 axios。

项目中 `src/api/ajax.js` 文件中封装了 axios 的常用配置和一些工具函数。

## 默认配置

根据需要自行添加或修改。

```js
{
    baseURL: '/api',
    responseType: 'json'
}
```

针对 `post` 和 `put` 请求把 `Content-Type` 设为 `application/x-www-form-urlencoded`。

```js
const FORM_CONTENT_TYPE = 'application/x-www-form-urlencoded'
axios.defaults.headers.post['Content-Type'] = FORM_CONTENT_TYPE
axios.defaults.headers.put['Content-Type'] = FORM_CONTENT_TYPE
```

## 工具函数

* `GET(url[, params])`
* `POST(url, body)`
* `PUT(url, body)`
* `DELETE (url[, params])`

```js
import { GET, POST, PUT, DELETE } from './api/ajax'

GET('/url').then(data => console.log(data))
POST('/url', params).then(data => console.log(data))
PUT('/url', params).then(data => console.log(data))
DELETE('/url').then(data => console.log(data))
```

> `data` 即为 `response.data`。

## 工具类

对常用 REST 请求格式的封装。以一个实体 User 为例，基础 URL 为 `/api/user`，则：

 * 查询 **GET** `/api/user/:id`
 * 添加 **POST** `/api/user`
 * 修改 **PUT** `/api/user/:id`
 * 删除 **DELETE** `/api/user/:id`

**export default class Api**

* `constructor(url)`
* `add(model)`
* `update(id, model)`
* `delete(id[, params])`
* `one(id[, params])`
* `all([params])`

```js
// CityApi.js
import Ajax, { GET } from './ajax'

// 通过 Ajax 类实例化拥有基础的 QRUD 能力
const CityApi = new Ajax('/city')

// 如果需要额外的个性化请求，借助 GET、POST、PUT、DELETE 工具方法添加
CityApi.paging = params => GET('/city2', params)

export default CityApi
```

## 调用

```js
// actions.js
import CityApi from 'api/city'

export default {
    getCitys(store) {
        CityApi.all().then(data => {
            store.commit('citys', data.citys)
        })
    }
}
```

```vue
// componet
<template>...</template>
<script>
import CityApi from 'api/city'

export default {
    data() {
        return {
            cityData: []
        }
    },
    created() {
        CityApi.all().then(data => {
            this.cityData = data.citys
        })
    }
}
</script>
```

## 多级资源请求

路径模版约定为：`/api/资源名/:资源id/子资源名/:子资源id`。

假如我们要请求用户的团队信息，则：

* 添加 **POST** `/api/user/:userId/team`
* 查询 **GET** `/api/user/:userId/team`
* 查询单个 **GET** `/api/user/:userId/team/:teamId`
* 修改 **PUT** `/api/user/:userId/team/:teamId`
* 删除 **DELETE** `/api/user/:userId/team/:teamId`

```js
// teamApi.js
import Ajax from './ajax'

const TeamApi = new Api('/user/:userId/team')

export default TeamApi
```

```js
// 调用
import TeamApi from 'api/teamApi'

// 添加
TeamApi.add(userId, team)
// 查询用户所有团队
TeamApi.all(userId)
// 查询用户某个团队
TeamApi.one(userId, teamId)
// 修改
TeamApi.update(userId, teamId, team)
// 删除
TeamApi.delete(userId, teamId)
```

总结来说：

* `Ajax` 基础类只需要传一个 URL pattern，就可以实例化符合 RESTful 规范的接口调用。
* 所有 URL pattern 上的占位符按顺序传入，`body` 或 `params` 永远作为最后一个参数传入。
