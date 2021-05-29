#### 1. 引入axios

```shell
 npm install axios
 # qs 是用于参数格式化的
 npm install qs 
```

#### 2. 全局配置

> 在`main.js`中加入

```js
Vue.prototype.$axios = axios
Vue.prototype.$qs = qs
```

#### 3. 使用

- get请求

```js
getData() {					# 全局配置的url 使用vuex 
      this.$axios.get(this.$store.state.common.httpUrl + ":8001/blogs/getByTitle", {
        params: { # 参数
          title: this.title,
        }
      })
          .then((response) => {
            this.total = response.data.extend.total
            this.blogs = response.data.extend.blogs
            console.log(this.blogs)
          })
    }
```

- post请求

```js
this.$axios.post('/user', {
    firstName: 'Fred',        // 参数 firstName
    lastName: 'Flintstone'    // 参数 lastName
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

