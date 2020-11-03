### [官网](http://imakewebthings.com/waypoints/)

#### 1.引用

解压压缩包后，将关键的js文件`jquery.waypoints.js`引入后即可

#### 2.使用

```js
//element: 被监测的，滚动的元素
//handler: 滚动时触发的方法
var waypoint = new Waypoint({
  element: document.getElementById('waypoint'),
  handler: function(direction) {
    //direction 是滚动的方向 down、up等
    //只有滚动到最上方的时候, direction才会变成up
    console.log('Scrolled to waypoint!')
  }
})

/*--------------具体实例--------------*/
//滚动监测
var waypoint = new Waypoint({
    element: document.getElementById('waypoint'),
    handler: function (direction) {
        if (direction == "down") {
            //把元素显示出来
            $("#toolbar").show(500);
        } else {
            //把元素隐藏
            $("#toolbar").hide(500);
        }
    }
})
```