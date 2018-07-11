## 可复用组件

每个可复用组件（无状态、UI、端对端）在 `components` 中建立单独的文件目录，所有与该组件相关的资源均放入该目录中。

```
🗁 components
  `--🗁 tree
  |  |--🗎 index.js
  |  |--🗎 tree.vue
  |  `--🗎 treeItem.vue
```

组件目录以 `index.js` 为组件入口，导出可被调用的组件。

```js
// index.js
import tree from './tree.vue'
import treeItem from './treeItem.vue'

export default tree
export { treeItem }
```

然后就可以这样调用一个组件：

```js
import tree, { treeItem } from 'components/tree'

export default {
    components: {
        tree,
        treeItem
    }
}
```

## 视图/容器组件

放入 `view` 目录，以目录提现模块或路由层级，`index.vue` 为组件入口。

```
🗁 view
  `--🗁 project
  |  |--🗎 index.vue
  |  `--🗎 projectDetails.vue
```
