### Vuex

Vuex 是一个专为 Vue.js 应用程序开发的 **状态管理模式**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

> 用于存储用户的**token**等信息

#### 1.npm安装Vuex

```shell
npm install vuex --save
```

#### 2.在入口Main.js中引用Vuex

```js
//应用Vuex
import Vuex from "vuex"
Vue.use(Vuex)
```

#### 3.判断用户是否登录，验证Token之类的

* 使用到路由钩子函数**beforeEach**路由跳转前调用的方法，（真跳转前）

> **此方法卸载入口函数Main里面**

```js
//sessionStorage 用于临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据
router.beforeEach((to, from, next) => {  
  //属性：key-value，两个都是String属性
  //生命周期：
  //生命周期为当前窗口或标签页，一旦窗口或标签页被永久关闭了，那么所有通过sessionStorage存储的数据也就被清空了。
  let token = sessionStorage.getItem("token");
    
  //token验证, 一般来说是在后台验证
  if (to.path == "/logout") {  //注销验证
    sessionStorage.clear();
    //跳转回登录页面
    next({path: "/login"})
  } else if (to.path == "/login") {
    if (token != null) { //token验证
      next("/main");
    }
  } else if (token == null) { //token验证
    next("/login")
  }
  next();
});
```

#### 4.新增文件夹store放Vuex的配置文件index.js

```js
import vue from "vue"
import Vuex from "vuex"
import user from "./modules/user"

vue.use(Vuex)

export default new Vuex.Store({
  modules: {
    user
  }
});
```

#### 5.在store文件夹下面新建文件夹modules放入user模块，类似于java中的类

> state、getters、mutations、actions是关键字
>
> > state：包含了store中存储的各个状态
> >
> > getters：类似于Vue中的计算属性，用于获取state中的数据
> >
> > mutations：用于改变state中的属性的方法（同步执行的）
> >
> > actions：用于异步执行mutations中的方法

```js
const user = {
  //定义一个类似于数据库的常量
  state: sessionStorage.getItem("state") ? JSON.parse(sessionStorage.getItem("state")) : {
    user: {
      username: "",
      token: ""
    }
  },
  getters: {
    //这是个计算属性
    getUser(state) {
      return state.user
    }
  },
//这是个同步执行的方法
  mutations: {
    updateUser(state, user) {
      state.user = user
    }
  },

//异步执行的方法
  actions: {
    asyncUpdateUser(context, user) {
      context.commit("updateUser", user);
    }
  }
};
export default user;
```

