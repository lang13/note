### bootstrapValidator

#### 关闭后清除模态框数据

```js
$("#myModal").on('hidden.bs.modal', function () {
	$(this).removeData('bs.modal');
});
```

