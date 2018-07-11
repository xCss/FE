在开发中不可避免会遇到文件上传类的业务，如果后端还没有写好相关接口，那就需要我们在开发环境中模拟了。

## 配置

**`build/config.js`**

```js
{
    apiRoot: '/api',
    uploadUrl: '/upload',      // 上传接口
    uploadDir: '../upload/'    // 文件保存目录
}
```

上传接口会拼接 `apiRoot` 路径，以上配置后前端调用接口为 `/api/upload`，上传成功后，文件保存在项目根目录 `upload` 文件夹中。
