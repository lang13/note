### [Markdown编辑器](https://pandao.github.io/editor.md/)

#### 1.引用插件

> 将下载好的压缩文件加压，并将其中的文件复制到项目中的`editorMd`文件夹中

![image-20201102195103872](D:\environment\note\static\image\image-20201102195103872.png)

#### 2.集成代码

```html
<!--引入css文件-->
<link rel="stylesheet" href="../../static/lib/editorMd/css/editormd.min.css">
<!--引入js文件-->
<script src="../../static/lib/editorMd/editormd.min.js"></script>

<!--textarea部分-->
<div id="md-content">
	<textarea name="content" style="display:none;">[TOC]

                #### Disabled options

                - TeX (Based on KaTeX);
                - Emoji;
                - Task lists;
                - HTML tags decode;
                - Flowchart and Sequence Diagram;

                #### Editor.md directory

                    editor.md/
                            lib/
                            css/
                            scss/
                            tests/
                            fonts/
                            images/
                            plugins/
                            examples/
                            languages/
                            editormd.js
                            ...

                ```html
                &lt;!-- English --&gt;
                &lt;script src="../dist/js/languages/en.js"&gt;&lt;/script&gt;

                &lt;!-- 繁體中文 --&gt;
                &lt;script src="../dist/js/languages/zh-tw.js"&gt;&lt;/script&gt;
                ```
	</textarea>
</div>

<!--初始化Markdown插件-->
<script>
    var contentEditor;
    $(function () {
        // 开启表情功能, 不建议开启连接已挂
        editormd.emoji = {
            path: "http://www.emoji-cheat-sheet.com/graphics/emojis/",
            ext: ".png"
        };
        
        
        contentEditor = editormd("md-content", {
            width: "100%",
            height: 640,
            //开启表情功能
            emoji: true,
            syncScrolling: "single",
            path: "../../static/lib/editorMd/lib/"
        });
    });
</script>
```

