### [官文](http://tscanlin.github.io/tocbot/)

#### 1.引用css和js

> 将插件里的压缩包解压，并且将压缩包里面`dist`目录的文件复制到工程中的`lib`文件夹后即可
>
> 然后引入css和js，简单的时候目录生成工具时。只需要引入tocbot.css和tocbot.js即可

![](D:\environment\note\static\image\image-20201103164409840.png)

#### 2.代码使用

> 必须确保每个目录都带有`id`属性

初始化插件

```html
<script>
    tocbot.init({
        // 目录生成后放在哪个区域 [.js-toc 一般使用<ol></ol>标签]
        tocSelector: '.js-toc',
        // 需要生成目录的元素是哪个
        contentSelector: '.js-toc-content',
        // 哪些级别的标题需要生成目录(最多三级)
        headingSelector: 'h1, h2, h3',
        // For headings inside relative or absolute positioned containers within content.
        hasInnerContainers: true,
    });
</script>
```

