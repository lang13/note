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

> **此方法写在入口函数Main里面**

```js
import router from './router'

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

#### 5.在store文件夹下面新建文件夹modules放入user（模块名）模块，类似于java中的类

> state、getters、mutations、actions是关键字
>
> > state：包含了store中存储的各个状态
> >
> > getters：类似于Vue中的计算属性，用于获取state中的数据（即使是不同的模块，getters里的方法的名字也不能相同）
> >
> > mutations：用于改变state中的属性的方法（同步执行的）
> >
> > actions：用于异步执行mutations中的方法

user.js

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

#### 6.读写数据

```js
# 如果读取不到, 则在 state 后方加入 js 的名字
读取数据：{{ $store.getters.getUser.username }} {{ this.$store.state.user }}
写入数据：
//dispatch调用的是actions里面的方法
this.$store.dispatch("asyncUpdateUser", {
	username: this.form.username,
    token: "this is token"
})      
```

#### 7.什么时候使用Vuex

> **什么样的应用场景下需要 vuex ？**如果不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的。确实是如此——如果你的应用够简单，那最好不要使用 Vuex。一个简单的 global event bus 就足够所需了。但是，如果需要构建是一个中大型单页应用，很可能会考虑如何更好地在组件外部管理状态，Vuex 将会成为自然而然的选择。
>
> vuex 一般用于中大型 web 单页应用中对应用的状态进行管理，对于一些组件间关系较为简单的小型应用，使用 vuex 的必要性不是很大，因为完全可以用组件 prop 属性或者事件来完成父子组件之间的通信，**vuex 更多地用于解决跨组件通信以及作为数据中心集中式存储数据。**
>
> **使用 vuex 解决跨组件通信问题**
> 跨组件通信一般指非父子组件间的通信，父子组件的通信一般可以通过以下方式：
> **1、通过 prop 属性实现父组件向子组件传递数据**
> **2、通过在子组件中触发事件实现向父组件传递数据**
> 非父子组件之间的通信一般通过一个空的 Vue 实例作为 中转站，也可以称之为 事件中心、event bus。