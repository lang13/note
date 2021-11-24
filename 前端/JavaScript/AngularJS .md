##### 一个简单的AngularJs实例

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="https://cdn.staticfile.org/angular.js/1.4.6/angular.min.js"></script>
</head>
<body>

<div ng-app="">
  <p>名字 : <input type="text" ng-model="name"></p>
  <h1>Hello {{name}}</h1>
  <!-- 效果一样-->
  <h1>Hello <span ng-bind="name"></h1>
</div>

</body>
</html>
    
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="https://cdn.staticfile.org/angular.js/1.4.6/angular.min.js"></script> 
</head>
<body>
```

##### ng-model

> `ng-model` 指令可以为应用数据提供状态值(`invalid`, `dirty`, `touched`, `error`):

```html
<form ng-app="" name="myForm">
    Email:
    <input type="email" name="myAddress" ng-model="text">
    <span ng-show="myForm.myAddress.$error.email">不是一个合法的邮箱地址</span>
	{{ myForm.myAddress.$error.email }}
</form>
<!-- 当 input里面的不是email格式的时候，myForm.myAddress.$error.email的值为true，否则没有值-->
<p>在输入框中输入你的邮箱地址，如果不是一个合法的邮箱地址，会弹出提示信息。</p>
<p>编辑邮箱地址，查看状态的改变。</p>
<h1>状态</h1>
<p>Valid: {{myForm.myAddress.$valid}} (如果输入的值是合法的则为 true)。</p>
<p>Dirty: {{myForm.myAddress.$dirty}} (如果值改变则为 true)。</p>
<p>Touched: {{myForm.myAddress.$touched}} (如果通过触屏点击则为 true)。</p>
</body>
</html>
```

##### 过滤器

| 过滤器    | 描述                     |
| :-------- | :----------------------- |
| currency  | 格式化数字为货币格式。   |
| filter    | 从数组项中选择一个子集。 |
| lowercase | 格式化字符串为小写。     |
| orderBy   | 根据某个表达式排列数组。 |
| uppercase | 格式化字符串为大写。     |

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="//cdn.staticfile.org/angular.js/1.4.6/angular.min.js"></script>
</head>
<body>
<div ng-app="myApp" ng-controller="personCtrl">
	<p>姓名为 {{ lastName | uppercase }}</p>
</div>
<script>
	var app = angular.module("myApp", []);
    app.controller("personCtrl", function($scope)){
    	$scope.firstName = "John";
      	$scope.lastName = "Doe";
        $scope.fullName = function(){
        	return $scope.firstName + " " = $scope.lastName;
    	}
    }
</script>
</body>
</html>

<!-- 向指令添加过滤器-->
<div ng-app="myApp" ng-controller="namesCtrl">
<ul><!-- orderBy 根据country排序数组-->
  <li ng-repeat="x in names | orderBy:'country'">
    {{ x.name + ', ' + x.country }}
  </li>
</ul>
    <script>
    angular.module('myApp', []).controller('namesCtrl', function($scope) {
    	$scope.names = [
            {name:'Jani',country:'Norway'},
            {name:'Hege',country:'Sweden'},
            {name:'Kai',country:'Denmark'}
    	];
	});
    </script>
</div>

<!-- 自定义过滤器-->
<script>
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope) {
        $scope.msg = "Runoob";
    });
    app.filter('reverse', function() { //可以注入依赖
        return function(text) { // text为接收到的需要进行过滤的字符串
            return text.split("").reverse().join("");
        }
    });
</script>
```

##### select

> `ng-potions`实现下拉列表，注意 `x for x in names`语句

```html
<div ng-app="myApp" ng-controller="myCtrl">
 
<select ng-init="selectedName = names[0]" ng-model="selectedName" ng-options="x for x in names">
</select>
 
</div>
 
<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.names = ["Google", "Runoob", "Taobao"];
});
</script>
```

> `ng-repeat`实现下拉列表，这里的语句为`x in names`，并且`ng-repeat`遍历出来的是**字符串**而`ng-options`遍历出来的是**对象**

```html
<select>
	<option ng-repeat="x in names">{{x}}</option>
</select>
```

演示：`ng-options`绑定对象

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="https://cdn.staticfile.org/angular.js/1.4.6/angular.min.js"></script>
</head>
<body>

<div ng-app="myApp" ng-controller="myCtrl">

<p>选择网站:</p>
<!-- x.size 表示显示x.site，但是selectedSite绑定的是x对象-->
<select ng-model="selectedSite" ng-options="x.site for x in sites">
</select>

<h1>你选择的是: {{selectedSite.site}}</h1>
<p>网址为: {{selectedSite.url}}</p>

</div>

<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
   $scope.sites = [
	    {site : "Google", url : "http://www.google.com"},
	    {site : "Runoob", url : "http://www.runoob.com"},
	    {site : "Taobao", url : "http://www.taobao.com"}
	];
});
</script>

<p>该实例演示了使用 ng-options  指令来创建下拉列表，选中的值是一个对象。</p>
</body>
</html>
```

当数据源是key-value的格式时

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="https://cdn.staticfile.org/angular.js/1.4.6/angular.min.js"></script>
</head>
<body>

<div ng-app="myApp" ng-controller="myCtrl">

<p>选择的网站是:</p>
<!-- 此时的selectedSite是value(value是一个字符串，所以selectedSite也是一个字符串)-->
<select ng-model="selectedSite" ng-options="x for (x, y) in sites">
</select>

<h1>你选择的值是: {{selectedSite}}</h1>

</div>

<p>该实例演示了使用对象作为创建下拉列表。</p>

<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.sites = {
	    site01 : "Google",
	    site02 : "Runoob",
	    site03 : "Taobao"
	};
});
</script>

</body>
</html>
```

