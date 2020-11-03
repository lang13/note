### [官网](http://davidshimjs.github.io/qrcodejs/)

#### 1.引用插件

直接解压，复制文件夹中的js文件即可。只有一个js文件

#### 2.代码

基础使用，div中必须有`id`属性

```html
<div id="qrcode"></div>
<script type="text/javascript">
	new QRCode(document.getElementById("qrcode"), "http://jindo.dev.naver.com/collie");
</script>
```

进阶使用

```js
//第一个参数是需要承载生成二维码后的标签的id
var qrcode = new QRCode("test", {
    text: "http://jindo.dev.naver.com/collie",
    width: 128,
    height: 128,
    colorDark : "#000000",
    colorLight : "#ffffff",
    correctLevel : QRCode.CorrectLevel.H
});
```

