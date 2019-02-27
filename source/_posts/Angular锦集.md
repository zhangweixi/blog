---
title: Angular锦集
tags: []
originContent: "### angularjs上传图片\n```javascript\n$scope.upload = function(){\n\n\tvar form = new FormData();\n\t//添加图片\n\tvar file = document.getElementById('eleId');\n\tform.append('file',file);\n\n\t//添加其他参数\n\tform.append('name','zhangweixi');\n\tvar url = \"http://www.xx.com/upload\";\n\t$http({\n\t\tmethod:\"post\",\n\t\turl:url,\n\t\tdata:form,\n\t\theaders:{\"Content-Type\",undefined},\n\t\ttransformRequest:angular.identify\n\t}).success(function(res){\n\n\t\t//do something\n\t});\n\n}\n\n```"
categories: []
toc: false
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