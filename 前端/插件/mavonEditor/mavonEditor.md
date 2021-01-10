### 引入依赖

```cmd
npm install mavon-editor --save
```

### 使用

> 在main.js中引入

```js
// 全局注册
// import with ES6
import Vue from 'vue'
import mavonEditor from 'mavon-editor'
import 'mavon-editor/dist/css/index.css'
// use
Vue.use(mavonEditor)
```

### Markdown转HTML功能

```html
<mavon-editor
            class="md"
            :value="content"
            :subfield="false"
            :defaultOpen="'preview'"
            :toolbarsFlag="false"
            :editable="false"
            :scrollStyle="true"
            :ishljs="true"
        ></mavon-editor>
<!--
    :value="content"：引入要转换的内容
    :subfield = "false"：开启单栏模式
    :defaultOpen = "'preview'"：默认展示预览区域
    :toolbarsFlag = "false"：关闭工具栏
    :editable="false"：不允许编辑
    scrollStyle="true"：开启滚动条样式(暂时仅支持chrome)
    :ishljs = "true"：开启代码高亮
-->
```

