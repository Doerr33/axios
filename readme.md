---
sidebar: 'auto'
title: 03Axios源码分析
date: 2022.03.13
tags:
 - Axios
categories: 
 - 10Axios
---

# Axios源码分析

 ## 1.整体分析

<img src="https://gitee.com/lingisme9/images1/raw/master/img/20220315084609.png" alt="image-20220315084609757" style="zoom:150%;" />

### 1.index.js

![image-20220315085007137](https://gitee.com/lingisme9/images1/raw/master/img/20220315085007.png)

### 2.xhr.js

![image-20220315085149798](https://gitee.com/lingisme9/images1/raw/master/img/20220315085149.png)

![image-20220315085255350](https://gitee.com/lingisme9/images1/raw/master/img/20220315085255.png)

### 3.cancel.js

![image-20220315085344348](https://gitee.com/lingisme9/images1/raw/master/img/20220315085344.png)

![image-20220315085501143](https://gitee.com/lingisme9/images1/raw/master/img/20220315085501.png)

### 4.拦截器管理器

![image-20220315085853071](https://gitee.com/lingisme9/images1/raw/master/img/20220315085853.png)

### 5.axios.js

![image-20220315090018045](https://gitee.com/lingisme9/images1/raw/master/img/20220315090018.png)

### 6.默认配置文件

![image-20220315090055188](https://gitee.com/lingisme9/images1/raw/master/img/20220315090055.png)

### 7.重点执行流程

axios.js暴露axios=> Axios(request)=>dispathRequest.js

![image-20220315091009850](https://gitee.com/lingisme9/images1/raw/master/img/20220315091009.png)

## 2.axios和Axios的关系

<img src="https://gitee.com/lingisme9/images1/raw/master/img/20220315092704.png" alt="image-20220315092704106" style="zoom:150%;" />

### 1.分析

分析axios的产生

> 由createInstance返回Instance

![image-20220315093257118](https://gitee.com/lingisme9/images1/raw/master/img/20220315093257.png)

> instance的产生，是bind产生的一个新函数，新函数和原来函数的关系为，新函数里面用call调用旧函数，指定函数的this，即是context

![image-20220315093414541](https://gitee.com/lingisme9/images1/raw/master/img/20220315093414.png)

> 分析Axios的原型

![image-20220315093620044](https://gitee.com/lingisme9/images1/raw/master/img/20220315093620.png)

> context为Axios的实例

![image-20220315094004548](https://gitee.com/lingisme9/images1/raw/master/img/20220315094004.png)

> 关键步骤之后instance添加了请求方法和属性

![image-20220315094131786](https://gitee.com/lingisme9/images1/raw/master/img/20220315094131.png)

![image-20220315094234864](https://gitee.com/lingisme9/images1/raw/master/img/20220315094234.png)

> context上有什么

![image-20220315095710799](https://gitee.com/lingisme9/images1/raw/master/img/20220315095710.png)

### 2.验证

![image-20220315100046644](https://gitee.com/lingisme9/images1/raw/master/img/20220315100046.png)

![image-20220315100306154](https://gitee.com/lingisme9/images1/raw/master/img/20220315100306.png)

![image-20220315100323362](https://gitee.com/lingisme9/images1/raw/master/img/20220315100323.png)

![image-20220315100414262](https://gitee.com/lingisme9/images1/raw/master/img/20220315100414.png)

![image-20220315100425088](https://gitee.com/lingisme9/images1/raw/master/img/20220315100425.png)

## 3.instance和axios的区别

![image-20220315102644907](https://gitee.com/lingisme9/images1/raw/master/img/20220315102645.png)

instance实际上等价于Axios的实例，axios创建后添加了很多方法

![image-20220315102607948](https://gitee.com/lingisme9/images1/raw/master/img/20220315102608.png)

### 1.axios.create

![image-20220315100934804](https://gitee.com/lingisme9/images1/raw/master/img/20220315100935.png)

> mergeConfig:合并配置

### 2.验证

![image-20220315102758060](https://gitee.com/lingisme9/images1/raw/master/img/20220315102758.png)

![image-20220315102841071](https://gitee.com/lingisme9/images1/raw/master/img/20220315102841.png)

## 4.axios执行流程图分析

<img src="https://gitee.com/lingisme9/images1/raw/master/img/20220315102926.png" alt="image-20220315102926549" style="zoom:150%;" />

## 5.整体流程中最重要的步骤

<img src="https://gitee.com/lingisme9/images1/raw/master/img/20220315103352.png" alt="image-20220315103352504" style="zoom:150%;" />

![image-20220315103438904](https://gitee.com/lingisme9/images1/raw/master/img/20220315103439.png)



## 6.request串联流程分析

![image-20220315104834724](https://gitee.com/lingisme9/images1/raw/master/img/20220315104834.png)

> 注意underfined的作用,防止promise链位置串位

![image-20220315103646837](https://gitee.com/lingisme9/images1/raw/master/img/20220315103646.png)

> 任务编排

![image-20220315103939225](https://gitee.com/lingisme9/images1/raw/master/img/20220315103939.png)

![image-20220315104016276](https://gitee.com/lingisme9/images1/raw/master/img/20220315104016.png)

![image-20220315104118433](https://gitee.com/lingisme9/images1/raw/master/img/20220315104118.png)

![image-20220315104800770](https://gitee.com/lingisme9/images1/raw/master/img/20220315104800.png)

## 7.dispatchRequest转换数据

![image-20220315105742444](https://gitee.com/lingisme9/images1/raw/master/img/20220315105742.png)

> 转换请求数据

![image-20220315105534061](https://gitee.com/lingisme9/images1/raw/master/img/20220315105534.png)

> 转换响应数据

![image-20220315105923016](https://gitee.com/lingisme9/images1/raw/master/img/20220315105923.png)

![image-20220315110018535](https://gitee.com/lingisme9/images1/raw/master/img/20220315110018.png)

## 8.xhrAdapter发送请求

![image-20220315111049845](https://gitee.com/lingisme9/images1/raw/master/img/20220315111049.png)

## 9.取消请求

 **1 .当配置了cancelToken对象时, 保存cancel函数**

​      创建一个用于将来中断请求的cancelPromise

​      并定义了一个用于取消请求的cancel函数

​      将cancel函数传递出来

 **2 .调用cancel()取消请求**

​      执行cancel函数, 传入错误信息message

​      内部会让cancelPromise变为成功, 且成功的值为一个Cancel对象

​      在cancelPromise的成功回调中中断请求, 并让发请求的proimse失败, 失败的reason为Cacel对象

![image-20220315111407873](https://gitee.com/lingisme9/images1/raw/master/img/20220315111407.png)

![image-20220315111704310](https://gitee.com/lingisme9/images1/raw/master/img/20220315111704.png)

![image-20220315111826379](https://gitee.com/lingisme9/images1/raw/master/img/20220315111826.png)

















































