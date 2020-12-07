### thymeleaf

#### 代码提示和引用

```html
<!DOCTYPE html SYSTEM "http://www.thymeleaf.org/dtd/xhtml1-strict-thymeleaf-spring4-4.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org">
```

#### 使用

##### 条件表达式

```html
<div class="ui basic left pointing label" th:classappend="${type.id == typeId}?'teal'" th:text="${type.sum}">  20   
</div>
```

##### 循环

```html
<div th:each="type : ${types}"></div>
```

##### 日期格式化

```html
<span th:text="${#dates.format(user.date, 'yyyy-MM-dd')}">4564546</span>  
```

##### 内嵌

```html
<div class="item" th:inline="text">
    <i class="eye icon"></i>
    [[${blog.views}]]
</div>
```

##### 超链接

```html
<a th:href="@{/(current=${current}+1, typeId=${typeId})}">下一页</a>
<!--相当于-->
<a href="/?current=2&typeId=1">下一页</a>
```

##### if多条件判断

```html
<a th:if="${cmenu.text!= '角色管理' && cmenu.text!= '系统菜单'}">系统管理</a>
```

##### checked

```html
<input type="radio" th:checked="true">
```

