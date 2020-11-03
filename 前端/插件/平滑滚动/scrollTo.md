### [官网](https://github.com/flesler/jquery.scrollTo)

#### 1.引用

cdn的方式引用

```html
<script src="//cdn.jsdelivr.net/npm/jquery.scrollto@2.1.2/jquery.scrollTo.min.js"></script>
```

#### 2.使用

```js
//全屏滚动，滚动到最顶端，滚动的间隔时间为500
$(window).scrollTo(0,500);

$(element).scrollTo(target[,duration][,settings]);
```

### *element*

This must be a scrollable element, to scroll the whole window use `$(window)`.

### *target*

This defines the position to where `element` must be scrolled. The plugin supports all these formats:

- A number with a fixed position: `250`
- A string with a fixed position with px: `"250px"`
- A string with a percentage (of container's size): `"50%"`
- A string with a relative step: `"+=50px"`
- An object with `left` and `top` containining any of the aforementioned: `{left:250, top:"50px"}`
- The string `"max"` to scroll to the end.
- A string selector that will be relative to the element to scroll: `".section:eq(2)"`
- A DOM element, probably a child of the element to scroll: `document.getElementById("top")`
- A jQuery object with a DOM element: `$("#top")`

### *settings*

The `duration` parameter is a shortcut to the setting with the same name. These are the supported settings:

- **axis**: The axes to animate: `xy` (default), `x`, `y`, `yx`
- **interrupt**: If `true` will cancel the animation if the user scrolls. Default is `false`
- **limit**: If `true` the plugin will not scroll beyond the container's size. Default is `true`
- **margin**: If `true`, subtracts the margin and border of the `target` element. Default is `false`
- **offset**: Added to the final position, can be a number or an object with `left` and `top`
- **over**: Adds a % of the `target` dimensions: `{left:0.5, top:0.5}`
- **queue**: If `true` will scroll one `axis` and then the other. Default is `false`
- **onAfter(target, settings)**: A callback triggered when the animation ends (jQuery's `complete()`)
- **onAfterFirst(target, settings)**: A callback triggered after the first axis scrolls when queueing