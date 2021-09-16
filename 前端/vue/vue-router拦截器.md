### 拦截器

#### 1.新建拦截器实例

```js
import axios from 'axios';
import store from '@/store';
import router from '@/router';

// 名字
let tokenHttp = axios.create({
    headers: {
        'content-type': 'application/x-www-form-urlencoded'
    }
});

// 设置默认请求 url
tokenHttp.defaults.baseURL = 'http://localhost'

// 请求拦截器
tokenHttp.interceptors.request.use(
    config => {
        const token = store.getters.getToken
        if (token != null) {
            config.headers.Authorization = token;
        }
        return config;
    },
    error => {
        return Promise.reject(error)
    }
)

// 响应拦截器
tokenHttp.interceptors.response.use(
    response => {
        // 正常响应
        return response
    },
        // 错误响应
    error => {
        let response = error.response;
        if (response) {
            switch (response.status) {
                case 401:
                    store.commit("setToken", null);
                    router.replace({
                        path: '/login',
                    })
            }
        }
        return Promise.reject(error) // 返回接口返回的错误信息
    }
)

// 导出tokenHttp
export default tokenHttp;
```

#### 2.main.js中引入 tokenHttp

```js
import Vue from 'vue'
import tokenHttp from "@/api/tokenHttp";

Vue.prototype.$tokenHttp = tokenHttp
# 这样就可以通过 this.$tokenHttp 的方式调用 tokenHttp实例

new Vue({
    el: '#app',
    router,
    store,
    render: h => h(App)
})
```

#### 3.使用tokenHttp

```js
 getData() {
      // 获取 blogs
      # 使用 tokenHttp
      this.$tokenHttp.get("/admin/listBlogs", {
        params: {
          current: this.current,
          size: this.size,
          title: this.title,
          typeId: this.typeId,
          isPublished: this.isPublished
        }
      }).then((response) => {
        // console.log(response);
        this.articles = response.data.extend.blogs;
        this.current = response.data.extend.current;
        this.size = response.data.extend.size;
        this.total = response.data.extend.total;
        this.pages = response.data.extend.pages;
      }).catch(error => {
        console.log(error)
      })
    }
```

