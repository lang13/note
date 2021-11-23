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
```

