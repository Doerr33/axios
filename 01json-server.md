---
sidebar: 'auto'
title: 01json-server快速创建服务器
date: 2022.03.10
tags:
 - Axios
categories: 
 - 10Axios
---

# json-server

[官方文档](https://github.com/typicode/json-server)

## 1.安装

```js
npm install -g json-server
```

## 2.项目的根目录下创建db.json

```json
{
  "posts": [
    { "id": 1, "title": "json-server", "author": "typicode" }
  ],
  "comments": [
    { "id": 1, "body": "some comment", "postId": 1 }
  ],
  "profile": { "name": "typicode" }
}
```

## 3.启动json服务

```js
json-server --watch db.json
```



