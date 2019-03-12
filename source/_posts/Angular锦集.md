---
title: Angular锦集
tags: []
categories: []
toc: false
description: |-
  整理angularjs得使用过程中遇到的问题<br/>
  1.angularjs使用ajax的方式上传文件
date: 2019-02-27 15:28:18
---

### angularjs上传图片
```javascript
$scope.upload = function(){

	var form = new FormData();
	//添加图片
	var file = document.getElementById('eleId');
	form.append('file',file);

	//添加其他参数
	form.append('name','zhangweixi');
	var url = "http://www.xx.com/upload";
	$http({
		method:"post",
		url:url,
		data:form,
		headers:{"Content-Type",undefined},
		transformRequest:angular.identify
	}).success(function(res){

		//do something
	});

}

```