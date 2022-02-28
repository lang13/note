### 1. [选择器](https://blog.csdn.net/MrYushiwen/article/details/112968411)

- 组合选择器

```js
$("div,.one") //选取所有<div>和所有class="one"的元素
```

- 特殊例子

```js
var indexs = [];
// 选择input中，type为checkbox且name为sceCheck且有checked属性的元素
$("input:checkbox[name='sceCheck']:checked").each(function(i){
    indexs[i] = $(this).val();
})
```

### 2. 捕获和设置

#### 1. html()

```html
<p id="id">这是段落中的 <b>粗体</b> 文本。</p>
```

```js
// 设置或返回所选元素的内容，包括HTML标签
var html = $("#id").html(); // html = 这是段落中的 <b>粗体</b> 文本。
```

#### 2. text()

```html
<p id="id">这是段落中的 <b>粗体</b> 文本。</p>
```

```js
// 设置或获取所选元素的文本内容
var text = $("#id").text(); // text =  这是段落中的 粗体 文本。
```

#### 3.val()

```html
<input type="text" id="id" value="value值">
```

```js
// 设置或者获取表单字段的value值
var val = $("#id").val(); // val = value值
```

#### 4.attr()

```html
<a href="www.runoob.com" id="id">菜鸟教程</a>
```

```js
// 设置或获取属性值
var attr = $("#id").attr("href"); // attr = www.runoob.com
// 设置一个值
$("#id").attr("href", "www.runoob.com");
// 设置多个值
$("#id").attr({
    "href" : "www.runoob.com",
    "title" : "菜鸟教程"
})
```

### 3. CSS

#### 1. 添加、删除、切换CSS类

addClass()、removeClass()、toggleClass()

> 本质是操作html元素中class的值

#### 添加CSS属性

> 本质是操作html元素中的style属性

```js
$("#id").css("display", "inline-block");
```

