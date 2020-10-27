### semantic

#### 1.环境配置

##### 1.直接导包配置

**semantic-2.4.1.min.css**：要修改谷歌的字体库。需要将**<u>字体库</u>**下载下来

```css
semantic-2.4.1.min.css
@import url(fonts.css);/*!
 * # Semantic UI 2.4.0 - Reset
 * http://github.com/semantic-org/semantic-ui/
 *
 *
 * Released under the MIT license
 * http://opensource.org/licenses/MIT
 *
 */
```

##### 2.HTML中引入

```html
<link rel="stylesheet" type="text/css" href="../static/css/semantic-2.4.1.min.css">
<script src="../static/js/jquery-3.1.1.min.js"></script>
<script src="../static/js/semantic-2.4.1.min.js"></script>
```

#### 2.编码

##### 1.ui segment

创建一个分段。

分段：用于创建一组相关联的内容。

```html
<div class="ui raised segment">
  <p>this is test in segment</p>
</div>
```

![0a8ee8601c9d519006689c2f5f4638d](D:\environment\note\static\image\0a8ee8601c9d519006689c2f5f4638d.png)

