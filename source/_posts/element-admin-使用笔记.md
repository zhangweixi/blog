---
title: element-admin 使用笔记
tags:
  - JS
categories:
  - JS
toc: false
description: 使用element-admin的项目经验
date: 2021-04-19 15:26:26
---

## 一、API接口统一格式

后台提供的API接口返回格式应该满足如下格式才可

```javascript
{
   code: 20000, // 50008 50012 50014 都会造成重新登录
   message: '提示消息',
   data: {}
}
```

<!--more-->

在授权后的请求中，将把token放在header的X-token中

```
header.X-Token = 'your token'
```

## 二、登录接口

请求

```javascript
{
    username: 'account',
    password: 'password',
}
```

返回

```javascript
{
    code: 20000,
    message: 'success',
    data: {
        token: 'your token'
    }
}
```

## 三、用户信息接口

```javascript
{
    code: 20000,
    message: 'success',
    data: {
        name: 'name',
        avatar: '头像'
    }
}
```

## 四、代理请求的时候一直pending

<a href="https://blog.csdn.net/yuse6262/article/details/107393394" target="_blank">详细文档"https://blog.csdn.net/yuse6262/article/details/107393394"</a>

**超时时候**

```javascript
proxy: {
    "/api": {
        target: 'http://127.0.0.1:8000',
        changeOrigin: true,
    }
}
```

**解决超时**

```javascript
proxy: {
    "/api": {
        target: 'http://127.0.0.1:8000',
        changeOrigin: true,
        // 由于vue中使用了body-parser 导致http中的body被序列化两次，从而使得配置代理后后端无法获取body中的数据
        onProxyReq: function(proxyReq, req, res, options) {
            if (req.body) {
                const reg = new RegExp('application/json') 
                if (reg.test(proxyReq.getHeader('Content-Type'))) {
                    const bodyData = JSON.stringify(req.body) 
                    proxyReq.setHeader('Content-Length', Buffer.byteLength(bodyData))
                    // stream the content
                    proxyReq.write(bodyData)
                }
            }
        }
    }
}
```