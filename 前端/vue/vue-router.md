### vue-router

#### 1.概述

Vue Router 是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。包含的功能有：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于 Vue.js 过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的 CSS class 的链接
- HTML5 历史模式或 hash 模式，在 IE9 中自动降级
- 自定义的滚动条行为

#### 2.安装

```sh
#新建vue项目时，可顺便安装
npm install vue-router --save-dev
```

#### 3.在入口Main.js中引用

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter);
```

#### 4.配置文件

**根目录新建router文件夹放router配置文件index.js**

```js
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

export default new Router({
  routes: [
    {
      //登录页
      path: '/login',
      name: 'Login',
      component: Login
    },
    {
      path: "*",
      name: "NotFound",
      component: NotFound
    },
    {
      //首页
      path: '/main',
      name: 'Main',
      component: Main,
      //首页的嵌套路由
      children: [
        {
          path: '/user/profile/:id',
          name: 'UserProfile',
          component: UserProfile
        },
        {
          path: '/user/list',
          name: 'UserList',
          component: UserList,
          props: true
        },
        {
          //重定向的url
          path: "/goMain", //明面上的url
          //重定向回到去的url
          redirect: "/main"  //实际上的url
        }
      ]
    }
  ]
})

```

#### 5.常用方法

```js
//跳转页面
this.$router.push("/main")

this.$router.push({
    name: "UserList",
    params: {
        id: 2
    }
})
```

