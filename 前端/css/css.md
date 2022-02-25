### 1. 定位 position

#### 1. 静态定位

1. HTML的默认值，静态定位就是没有定位，遵循正常的文档流对象
2. 静态定位的元素不会受到top，bottom，right，left的影响

```css
.test {
	position: static;
}    
```

 #### 2. 决对定位

1. absolute:元素会脱离文档流，如果设置偏移量，会影响其他元素的位置定位；脱离文档流最直观的例子便是，当子类设置为绝对定位的时候，父类元素是不会被子类撑开的
2. 绝对定位元素的Width由块内元素自己决定，不设置Width的情况下宽度刚好能容下内容
3. 在父元素没有设置绝对定位或者相对定位的情况下，元素相对于根元素定位
4. 在父元素设置有相对定位或者绝对定位的情况下，元素相对于最近的非static父元素进行定位

```css
.test {
    position: absolute;
}
```

#### 3. 相对定位

 	1. 相对定位是相对于自身位置的定位，当设置偏移量的时候会相对于原本的位置发生偏移
 	2. 设置成相对定位后，自身仍然处于文档流当中，元素的宽和高不发生改变，自身设置偏移量也不会影响其他元素
 	3. 最外层设置相对定位的时候，如果没有设置Width那么元素的宽度为整个浏览器的宽度

```css
.test {
    position: relative;
}
```

#### 4. fixed定位

1. 元素的位置相对于浏览器窗口的位置
2. 即使窗口滚动，它也不会发生改变
3. fixed定位脱离文档流，所以fixed定位并不会占用空间

```css
.test {
	position: fixed;
    right: 30px;
    bottom: 50px;
}    
```

#### 5.sticky 定位

1. 粘性定位的位置依赖于用户的滚动，在滚动过程中在`position: relative`和`position: fixed`定位之间进行切换
2. 元素定位在跨越特定阈值之前为`relative`定位，跨越定义的阈值之后为`fixed`定位；这里的阈值由top、bottom、left、right定义

```css
.test {
	position: sticky;
    top: 0;
}
```

### 布局 display

#### 1. 块级元素

1. 块级元素独占一行
2. 块级元素能设置宽高

```css
.test {
    display: block;
}
```

#### 2. 行内元素

1. 行内元素不会换行
2. 行内元素没有宽高

```css
.test {
    display: inline;
}
```

#### 3. 行内块元素

1. 同时具有块级元素和行内元素的特性
2. 有宽高、不独占一行

```css
.test {
    display: inline-block;
}
```

#### 4. [弹性布局](https://www.runoob.com/w3cnote/flex-grammar.html)

1. 父元素独占一行
2. 子元素类似于float的增强版
3. 具体属性则看Chrome浏览器可视化配置（可实现元素的上下对齐）

```css
.test {
    display: flex;
}
```

