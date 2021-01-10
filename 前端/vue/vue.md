
# 1.HTML
```html
<!--label标签的使用
    必选属性for,值为需要绑定的标签的id
-->
<input type="checkbox" id="runoob">
<label for="runoob">Runoob</label>
```

# 2.css
```css
1.font-szie: 0px: 用于消除子元素的间隙
```

# 3.vue
## vue网络编程部分
 ```html
<!--    引入axios的代码-->
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
 ```
* vue的生命周期
<img src="../../static/image/vue生命周期.png" alt="vue生命周期" style="zoom:50%;" />

beforeCreate
> 在实例初始化之后，数据观测(data observe)和event/watcher配置之前调用
>
> **此时`data`和`methods`里面的内容还未被初始化**

created
> 实例已经创建完成后被调用。在这一步，实例已完成一下配置：数据观测(data observe)，
> 属性和方法的运算，watcher/event事件的回调。但是挂载阶段还没开始，$el属性目前不可见
>
> **此时`data`和`methods`里面的内容已经被初始化**

beforeMount
> 在挂载开始前被调用：相关的render函数被调用
>
> **此时页面中的元素还没有被替换依旧显示`{{ msg }}`，此时的模板还未被挂载只在内存中渲染**

Mounted
> el被新创建的vm.$el替代，并挂载到实例上去之后调用该钩子
>
> **此时页面中的元素已经被替换显示出真实想要的数据，Mounted是实例创建过程中的最后一个钩子函数**

beforeUpdate
> 数据更新前调用，发生在虚拟Dom重新渲染和打补丁之前。你可以在这个钩子中进一步的更改状态，这不会触发附件的重渲染效果
>
> **此时`data`里面的数据更新了，但还未渲染到`页面`上，所以页面数据和data数据不一致**

updated
> 由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用此钩子函数
>> 当这个钩子被调用时，组件DOM已经更新，所以现在可以执行依赖于新的DOM的操作。然而大多数情况下，应该避免在此期间更改状态
因为可能导致更新无限循环。

beforeDestroy
> 实例销毁之前调用。在这一步，实例仍然完全可以调用

destroyed
> Vue实例销毁后调用。调用后，Vue实例指示的所有东西都会解绑，所有的事件监听器都会被移除，所有的子实例也会销毁。改钩子在服务器渲染期间不被调用

![vue钩子函数](../../static/image/vue钩子函数.png)
* v-show和v-if控制的标签的显示与否，而v-bind:class则是控制标签的class属性
* v-bind和v-model很像，但v-model只在表单元素内使用，在表单元素外使用无效，v-model是双向绑定的；v-bind可以绑定参数或者样式，但不是双向绑定。
在自定义组件中可用于绑定参数

