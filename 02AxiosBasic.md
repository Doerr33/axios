---
sidebar: 'auto'
title: 02Axios基础
date: 2022.03.13
tags:
 - Axios
categories: 
 - 10Axios
---

# Axios

## 1.简介

Axios 是一个基于 [Promise](https://so.csdn.net/so/search?q=Promise&spm=1001.2101.3001.7020) 的 HTTP 客户端，拥有以下特性：

- 支持 Promise API
- 能够拦截请求和响应
- 能够转换请求和响应数据
- 客户端支持防御 CSRF 攻击
- 同时支持浏览器和Node.js环境
- 能够取消请求及自动转换JSON数据

在浏览器端 Axios 支持大多数主流的浏览器， 比如 Chrome、Firefox、[Safari](https://so.csdn.net/so/search?q=Safari&spm=1001.2101.3001.7020) 和IE11。此外，Axios 还拥有自己的生态：

![img](https://gitee.com/lingisme9/images1/raw/master/img/20220313070912.png)

## 2.安装和引用

```js
import axios from 'axios';
//npm安装方法
$ npm install axios --save
//bower安装方法
$ bower install axios
//使用 cdn
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

Vue中使用:

```js
import axios from 'axios'
Vue.prototype.$http = axios
```

使用实例：

```js
methods: {
  postData () {
    this.$http({
      method: 'post',
      url: '/user',
      data: {
        name: 'xiaoming',
        info: '12'
      }
   })
}
```

## 3.基本API

### 1.GET/POST

- 格式：get/post (url, config)
  1. url：链接地址
  2. config：
     (1) data：请求的数据
     (2) headers：请求头信息

- post默认方式：Content-Type: application/json

```js
axios.post(url[, data, config])：发post请求
```

```js
//发送post请求
axios.post('/user',{
    firstName: 'simon',
    lastName:'li'
}).then(res=>console.log(res))
    .catch(err=>console.log(err));
```

- GET默认方式：Content-Type: application/x-www-form-urlencoded

```js
axios.get(url[, config])： 发get请求
```

```js
// 为给定 ID 的 user 创建请求
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// 可选地，上面的请求可以这样做
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

### 2.PUT/DELETE

```
axios.put(url[, data, config])：发put请求
```

```js
axios.put('/XXX',data,{
   headers:{}
 })
   .then(response => {
     console.log('/posts get', response.data)
   })
  .catch(error => {
    console.log(error)
  })

```

```js
axios.delete(url[, config]): 发delete请求
```

```js
axios.delete('/XXX',{
    params: {
        id: 1
    }
})
    .then(response => {
    console.log('/posts get', response.data)
})
    .catch(error => {
    console.log(error)
})
```

### 3.处理并发请求的助手函数

> 处理并发请求的助手函数
>
> axios.all(iterable)
> axios.spread(callback)

```js
function getUserAccount() {
    return $http.get('/user/12345');
}
function getUserPermissions() {
    return $http.get('/user/12345/permissions');
}
axios.all([getUserAccount(), getUserPermissions()])
    .then($http.spread(function (acct, perms) {
    //两个请求现已完成
}));

```

### 4.axios(config)

> axios(config)可通过设置一些属性来发送请求
>
> axios(url[, config])：可以只指定url发get请求

```js
//语法：
axios({
    url,			//地址
    method:GET|POST|PUT|DELETE, //请求方式
    data:post		//传递的数据
    params:get		//传递的数据|参数query存在里面
    headers：		//请求头信息
})


(2) axios.request(config): 等同于axios(config)
```



```js
// 发送 POST 请求
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});
//axios(url[, config])
// 发送 GET 请求（默认的方法）
axios('/user/12345');
```

### 5.axios.request

```js
axios.request({
  url: '/xxxx',
  method: 'post',
  data: {"title": "aaa", "author": "xxx"}
})
  .then(response => {
    console.log('/posts post', response.data)
  })
  .catch(error => {
    console.log(error)
  })

```



## 4.API别名

请求方法别名
为了方便起见，已经为所有支持的请求方法提供了别名。

```js
axios.request（config）
axios.get（url, [config]）
axios.delete（url, [config]）
axios.head（url, [config]）
axios.post（url, [data], [config]）
axios.put（url, [data], [config]）
axios.patch（url, [data], [config]）
```

**注意：**
当使用别名方法时，不需要在config中指定url，method和data属性。

## 5.创建实例axios.create

> 可用axios,create([config])来创建一个实例，并设置相关属性

```js
axios.create（[config]）

var instance = axios.create({
    baseURL: 'https://some-domain.com/api/',
    timeout: 1000,
    headers: {'X-Custom-Header': 'foobar'}
});
```

**注意：axios.create（）创建的对象不具备取消请求，批量发送多个请求的功能**



## 6.请求配置

这些是创建请求时可以用的配置选项。**只有 url 是必需的**。如果没有指定 method，请求将默认使用 get 方法。

```js
{
  //`url`是用于请求的服务器 URL
  url: '/user',

      
      
  //`method`是创建请求时使用的方法
  method: 'get', // 默认是 get

      
      
  //`baseURL`将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  //它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: 'https://some-domain.com/api/',

      
      
  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法
  // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
  transformRequest: [function (data) {
    //对data进行任意转换处理
    return data;
  }],

      
      
  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对 data 进行任意转换处理
    return data;
  }],
      
      
      
  // `headers` 是即将被发送的自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

      
      
  // `params` 是即将与请求一起发送的 URL 参数
  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
  params: {
    ID: 12345
  },

  // `paramsSerializer` 是一个负责 `params` 序列化的函数
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },
      
      

  // `data` 是作为请求主体被发送的数据
  // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
  data: {
    firstName: 'Fred'
  },

      
      
  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
  // 如果请求话费了超过 `timeout` 的时间，请求将被中断
  timeout: 1000,

      
      
  // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // 默认的

      
      
  // `adapter` 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },

      
      
  // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

      
      
  // `responseType` 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  responseType: 'json', // 默认的

      
      
  // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default

      
      
  // `xsrfHeaderName` 是承载 xsrf token 的值的 HTTP 头的名称
  xsrfHeaderName: 'X-XSRF-TOKEN', // 默认的

      
      
  // `onUploadProgress` 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

      
      
  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

      
      
  // `maxContentLength` 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

      
      
  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise 。如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status >= 200 && status < 300; // 默认的
  },

      
      
  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // 默认的

      
      
  // `httpAgent` 和 `httpsAgent` 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
      
      
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

      
      
  // 'proxy' 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: : {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```

## 7.响应结构

```js
{
  // `data` 由服务器提供的响应
  data: {},
  // `status` 来自服务器响应的 HTTP 状态码
  status: 200,
  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',
  // `headers` 服务器响应的头
  headers: {},
  // `config` 是为请求提供的配置信息
  config: {}
}
使用 then 时，你将接收下面这样的响应：
axios.get('/user/12345')
  .then(function(response) {
    console.log(response.data);
    console.log(response.status);
    console.log(response.statusText);
    console.log(response.headers);
    console.log(response.config);
  });
```

在使用 catch 时，或传递`rejection callback`作为 then 的第二个参数时，响应可以通过 error 对象可被使用，正如在错误处理这一节所讲。

## 8.配置的默认值/defaults

### 1.全局默认值

```js
(1) axios.defaults.xxx: 请求的默认全局配置
```

```js
axios.defaults.baseURL = 'https://api.example.com';//指定请求的基本地址
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

### 2.自定义实例默认值

```js
// 创建实例时设置配置的默认值
var instance = axios.create({
  baseURL: 'https://api.example.com'
});

// 在实例已创建后修改默认值
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
```

### 3.配置的优先顺序

配置会以一个优先顺序进行合并。**这个顺序是：在 lib/defaults.js 找到的库的默认值，然后是实例的 defaults 属性，最后是请求的 config 参数**。后者将优先于前者。这里是一个例子：

```js
// 使用由库提供的配置的默认值来创建实例
// 此时超时配置的默认值是 `0`
var instance = axios.create();

// 覆写库的超时默认值
// 现在，在超时前，所有请求都会等待 2.5 秒
instance.defaults.timeout = 2500;

// 为已知需要花费很长时间的请求覆写超时设置
instance.get('/longRequest', {
  timeout: 5000
});
```



## 9.[HTTP](https://so.csdn.net/so/search?q=HTTP&spm=1001.2101.3001.7020)拦截器的设计与实现

### 1.[拦截器](https://so.csdn.net/so/search?q=拦截器&spm=1001.2101.3001.7020)简介

对于大多数SPA应用程序来说，通常会使用 token 进行用户的身份认证。这就要求在认证通过后，我们需要在每个请求上都携带认证信息。针对这个需求，**为了避免每个请求单独处理，我们可以通过封装统一的 request 函数来为每个请求统一添加 token 信息。**



但后期如果需要为某些 GET 请求设置缓存时间或控制某些请求的调用频率的话，我们就需要不断修改 request 函数来扩展对应的功能。此时，**如果考虑对响应进行统一处理的话，我们的request 函数会变得越来越庞大，也越来越难维护**。对于这个问题，Axios 为我们提供了解决方案——拦截器。


Axios 是一个基于 Promise 的 HTTP [客户端](https://so.csdn.net/so/search?q=客户端&spm=1001.2101.3001.7020)，而 HTTP 协议是基于请求和响应：

![img](https://gitee.com/lingisme9/images1/raw/master/img/20220313070912.png)



所以 Axios 提供了请求拦截器和响应拦截器来分别处理请求和响应，它们的作用如下

- 请求拦截器：该类拦截器的**作用是在请求发送前统一执行某些操作，比如在请求头中添加 token 字段。**
- 响应拦截器：该类拦截器的**作用是在接收到服务器响应后统一执行某些操作，比如发现状态响应码为401时，自动跳转到登录页。**

在 Axios 中设置拦截器很简单，通过 axios.interceptors.request 和 axios.interceptors.response 对象提供的 use 方法，就可以分别设置请求和响应拦截器：

```js
// 添加请求拦截器 
axios.interceptors.request.use(function (config){ config.headers.token = 'added by interceptor' return config })

// 添加响应拦截器
axios.interceptors.response.use(function(data){ data.data = data.data + ' - modified by interceptors' return data })
```

那么拦截器是如何工作的呢？ 在看具体的代码之前，我们先分析它的设计思路。 Axios 的作用是用于发送 HTTP 请求， **而请求拦截器和响应拦截器的本质都是实现一个特定功能的函数。**

> 我们可以**按照功能任务把发送 HTTP 请求拆解成为不同类型的子任务**，比如有用于处理请求配置对象的子任务，用于发送 HTTP 请求的子任务和用于处理响应对象的子任务。**当我们按照指定的顺序来执行这些子任务时，就是一次完整的 HTTP 请求。**

接下来我们从 **任务注册、任务编排和任务调度** 三方面来分析 Axios 拦截器的实现。

### 2.任务注册

通过前面拦截器的使用示例，我们已经知道如何注册请求拦截器和响应拦截器，**其中请求拦截器用于处理请求配置对象的子任务，而响应拦截器于处理响应对象的子任务**。要搞清楚任务是如何注册的，就需要了解 axios 和 axios.interceptors 对象。

```js
// lib/axios.js 
function createInstance(defaultConfig){
    var context = new Axios(defaultConfig);
    var instance = bind(Axios.prototype.request,context); 

    //Copy axios.prototype to instace 
    utils.extend(instance,Axios.prototype,context); 
    //Copy context to instace 
    utils.extend(instance,context) 
    return instance } 

// Creact the default instance to be exported 
var axios = createdInstance(defaults);
```

在 Axios 的源码中，我们找到了 axios 对象的定义，很明显默认的 axios 实例是通过 cerateInstance 方法创建的，该方法最终返回的是 Axios.prototype.request 函数对象。同时，我们发现了 Axios 的构造函数：

```js
// lie/core/Axios.js
function Axios(instanceConfig){
    this.defaults = instanceConfig;
    this.insterceptors = {
        request:new InterceptorManager(),
        response:new InterceptorManager()
    }
}
```

在构造函数中，**我们找到了 axios.insterceptors 对象的定义，也知道了 interceptors.request 和 interceptors.response 对象都是 InterceptorManager 类的实例**。因此接下来，进一步分析 InsterceptorManager 构造函数及相关 use 方法就知道任务是如何注册的：

```js
// lib/core/InterstorManager.js
function InterstorManager(){
    this.handlers = [];
}
InterstorManager.prototype.use = function use(fulfilled,rejected){
    this.handlers.push({
        fulfilled:fulfilled,
        rejected:rejected
    })
    //返回当前的索引，用于移除已注册的拦截器
    return this.handlers.length - 1;
}
```

通过观察 use 方法，我们可知注册的拦截器都会被保存到 InsterceptorManager 对象 handlers 属性中。下面我们用一张图来总结下 Axios 对象与 InterceptorManager 对象内部结构与关系

![img](https://gitee.com/lingisme9/images1/raw/master/img/20220313070912.png)



### 3.任务编排

现在我们已经知道如何注册拦截器任务。但仅仅注册任务是不够，我们还需要对已注册的任务进行编排，这样才能确保任务的执行顺序。**这里我们把完成一次完整的 HTTP 请求分为处理请求配置对象、发起 HTTP 请求和处理响应对象3个阶段。**、

接下来我们来看一下 Axios 如何发请求的：

```js
axios({
    url:'/hello',
    method:'get',
}).then(res => {
    console.log('axios res',res)
    console.log('axios res.data',res.data)
})
```

通过前面的分析，我们已经知道 axios 对象对应的是 Axios.prototype.request 函数对象，该函数的具体实现如下：

```js
// lib/core/Axios.js
Axios.prototype.request = function request(config){
    config = mergeConfig(this.defaults,config);
    
    // 省略过部分代码
    var chain = [dispatchRequest,undefined];
    var pormise = Pormise.response(config);
    
    // 任务编排
    this.interceptors.request.forEach(function unshiftRequestInterceptors(interceptor){
        chain.unshift(interceptor.fulfilled,interceptor.rejcted);
    });
    
    this.interceptors.response.forEach(function pushResponseInterceptors(interceptor){
        chain.push(interceptor.fulfilled,interceptor.rejected);
    });
    
    // 任务调度
    while (chain.length){
        promise = promise.then(chain.shift(),chain.shift());
    }
    
    return promise
}
```

任务编排的代码比较简单，我们来看一下任务编排前和任务编排后的对比图：

![img](https://gitee.com/lingisme9/images1/raw/master/img/20220313070912.png)

### 4.任务调度

任务编排完成后，要发起 HTTP 请求，我们还需要按编排后的顺序执行任务调度。 在 Axios中具体的调度方式很简单，具体如下图所示：

```js
// lib/core/Axios.js
Axios.prototype.request = function request(config){
    //省略部分代码
    var promise = Promise.resolve(config);
    while (chain.length){
        promise = promise.then(chain.shift(),chain.shift());
    }
}
```

因为 chain 是数组，所以通过 while 语句我们就可以不断取出设置的任务，人为组装成 Promise 调用链从而实现任务调度，对应的处理流程如下图所示：

![img](https://gitee.com/lingisme9/images1/raw/master/img/20220313070912.png)

### 5.完整流程

下面我们来回顾一下 Axios 拦截器完整的使用流程：

```js
// 添加请求拦截器--处理请求配置对象
    axios.interceptors.request.use(function(config) {
        config.header.token = 'added by interceptor';
        return config
    })
// 添加响应拦截器--处理响应对象
    axios.interceptors.response.use(function(data){
        data.data = data.data + '-modified by interceptor';
        return data
    })   
    
     axios({
         url:'/hello',
         method:'get',
     }) .then(res =>{
         console.log('axios res.data', res.data)
     }])
```

介绍完 Axios 的拦截器，我们来总结一下它的优点。Axios 通过提供拦截器机制，让开发者可以很容易在请求的生命周期内自定义不同的处理逻辑。此外，也可以通过拦截器机制来灵活地扩展 Axios 的功能，比如 Axios 生态中列举的 axios-response-logger 和 axios-debug-log 这两个库。


参考 Axios 拦截器的涉及模型，我们就可以抽出以下通用的任务处理模型：

![img](https://gitee.com/lingisme9/images1/raw/master/img/20220313070912.png)

## 10.HTTP 适配器的涉及与实现

### 1.默认 HTTP 适配器

Axios 同时支持浏览器和 Node.js 环境，对于浏览器环境来说，我们可以通过 XMLHttpRequest 或 fetch API 来发送 HTTP 请求，而对于 Node.js 环境来说，我们可以通过 Node.js 内置的 http 或 https 模块来发送 HTTP 请求。

为了支持不同的环境，Axios 引入了适配器。在 HTTP 拦截器设计部分，我们看到了一个 dispatchRequest 方法，该方法用于发送 HTTP 请求，它的具体实现如下所示：

```js
// lib/core/dispatchRequest.js
module.exports = function dispatchRequest(config){
    // 省略部分代码
    var adapter = config.adapter || defaults.adapter;

    return adapter(config).then(function onAdapterResolution(response){
        return response;
    },function onAdapterRejection(reason){
        return Promise.reject(reason)
    })
}
```

通过查看以上的 dispatchRequest 方法，我们可知 Axios 支持自定义适配器，同时也提供了默认的适配器。对于大多数场景，我们并不需要自定义适配器，而是直接使用默认的适配器。因此，默认的适配器就会包含浏览器和Node.js环境的适配代码，其具体的适配逻辑如下所示：

```js
// lib/defaults.js
var defaults = {
    adapter:getDefaultAdapter(),
    xsrfCookieName: 'XSRF-TOKEN',
    xsrfHeaderName: 'X-XSRF-TOKEN',
    //...
}
function getDefaultAdapter(){
    var adapter;
    if(typeof XMLHttpRequest !== 'undefined'){
        // For browsers use XHR adapter
        adapter = require('./adapters/xhr')
    } else if (typeof process !== 'undefined' && Object.prototype.toString.call(process) === '[object process]'){
        //For node use HTTP adapter
        adapter = require('./adapter/http');
    }
    return adapter
}
```

在 getDefaultAdapter 方法中，首先通过平台中特定的对象来区分不同的平台，然后再导入不同的适配器，具体的代码比较简单，这里就不展开介绍了。

### 2.自定义适配器

其实除了默认的适配器外，我们还可以自定义适配器呢？这里我们可以参考 Axios 提供的示例：

```js
var settle = require('./../core/settle');
module.exports = function myAdapter(config) {
  // 当前时机点：
  //  - config配置对象已经与默认的请求配置合并
  //  - 请求转换器已经运行
  //  - 请求拦截器已经运行
  
  // 使用提供的config配置对象发起请求
  // 根据响应对象处理Promise的状态
  return new Promise(function(resolve, reject) {
    var response = {
      data: responseData,
      status: request.status,
      statusText: request.statusText,
      headers: responseHeaders,
      config: config,
      request: request
    };
 
    settle(resolve, reject, response);
 
    // 此后:
    //  - 响应转换器将会运行
    //  - 响应拦截器将会运行
  });
}
```

**在以上示例中，我们主要关注转换器、拦截器的运行时机点和适配器的基本要求。比如当调用自定义适配器之后，需要返回 Promise 对象。**这是因为 Axios 内部是通过 Promise 链式调用来完成请求调度，不清楚的小伙伴可以重新阅读 “拦截器的设计与实现” 部分的内容。

现在我们已经知道如何自定义适配器了，那么自定义适配器有什么用呢？在 Axios 生态中，[axios-mock-adapter](https://github.com/ctimmerm/axios-mock-adapter) 这个库，该库通过自定义适配器，让开发者可以轻松地模拟请求。对应的使用示例如下所示：

```js
var axios = require("axios");
var MockAdapter = require("axios-mock-adapter");
 
// 在默认的Axios实例上设置mock适配器
var mock = new MockAdapter(axios);
 
// 模拟 GET /users 请求
mock.onGet("/users").reply(200, {
  users: [{ id: 1, name: "John Smith" }],
});
 
axios.get("/users").then(function (response) {
  console.log(response.data);
});
```

### 3.整体处理流程

总结一下 Axios 使用请求拦截器和响应拦截器后，请求的处理流程：

![img](https://gitee.com/lingisme9/images1/raw/master/img/20220313070913.png)

## 11.取消请求

```js
axios.Cancel(): 用于创建取消请求的错误对象
axios.CancelToken(): 用于创建取消请求的token对象
axios.isCancel(): 是否是一个取消请求的错误
```

+ 基本流程

1. 配置 cancelToken 对象
2. 保存用于取消请求的 cancel 函数
3. 在后面特定时机调用 cancel 函数取消请求
4. 在错误回调中判断如果 error 是cancel ，做相应处理

+ Vue实例

```js
<template>
  <div class="myCanacle">
    <button @click="get1">发送请求1</button><br>
    <button @click="get2">发送请求2</button><br>
    <button @click="cancelReq">取消请求</button><br>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    data () {
      return {
        cancel: null // 声明一个用于保存取消函数的变量
      }
    },
    methods: {
      // 请求1
      get1 () {
        var _this = this
        axios({
          url: 'http://localhost:4000/products1',
          // 配置 cancelToken 对象 
          cancelToken: new axios.CancelToken((c) => {
            // 保存取消函数, 用于之后可能需要取消当前请求
            _this.cancel = c
          })
        }).then(
          response => {
            _this.cancel = null
            console.log('请求1成功了', response.data)
          }
        ).catch(error => {
            _this.cancel = null
            console.log('请求1失败了', error.message, error)
          })
      },
      // 请求2
      get2 () {
        var _this = this
        axios({
          url: 'http://localhost:4000/products1',
          cancelToken: new axios.CancelToken((c) => {
            // 保存取消函数, 用于之后可能需要取消当前请求
            _this.cancel = c
          })
        }).then(
          response => {
            _this.cancel = null
            console.log('请求2成功了', response.data)
          }
        ).catch(error => {
          _this.cancel = null
          console.log('请求2失败了', error.message, error)
        })
      },
      // 取消请求
      cancelReq() {
        if (typeof this.cancel === 'function') {
          this.cancel('强制取消请求')
        } else {
          console.log('没有可取消的请求')
        }
      }
    }
  }
</script>

<style>
  .myCanacle{

  }
</style>


```

![在这里插入图片描述](https://gitee.com/lingisme9/images1/raw/master/img/20220313070913.png)



(2) 配合拦截器实现上一个接口还未响应 下一个接口开始请求，把上一个接口取消

```js
<template>
  <div class="myCanacle">
    <button @click="get1">发送请求1</button><br>
    <button @click="get2">发送请求2</button><br>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    data () {
      return {
        cancel: null // 声明一个用于保存取消函数的变量
      }
    },
    mounted () {
      var _this = this
      // 添加请求拦截器
      axios.interceptors.request.use(config => {
        // 在准备发请求前, 取消未完成的请求
        if (typeof _this.cancel === 'function') {
          _this.cancel('取消请求')
        }
        // 添加一个cancelToken的配置
        config.cancelToken = new axios.CancelToken((c) => { // c是用于取消当前请求的函数
          // 保存取消函数, 用于之后可能需要取消当前请求
          _this.cancel = c
        })
        return config
      })

      // 添加响应拦截器
      axios.interceptors.response.use(
        response => {
          _this.cancel = null
          return response
        },
        error => {
          if (axios.isCancel(error)) {// 取消请求的错误
            console.log('请求取消的错误', error.message) // 做相应处理
            return new Promise(() => {})
          } else { // 请求出错了
            _this.cancel = null
            // 将错误向下传递
            return Promise.reject(error)
          }
        }
      )
    },
    methods: {
      // 请求1
      get1 () {
        axios({
          url: 'http://localhost:4000/products1'
        }).then(
          response => {
            console.log('请求1成功了', response.data)
          }
        ).catch(error => {
            console.log('请求1失败了', error.message, error)
          })
      },
      // 请求2
      get2 () {
        axios({
          url: 'http://localhost:4000/products1'
        }).then(
          response => {
            console.log('请求2成功了', response.data)
          }
        ).catch(error => {
          console.log('请求2失败了', error.message, error)
        })
      },
      // 取消请求
      cancelReq() {
        if (typeof this.cancel === 'function') {
          this.cancel('强制取消请求')
        } else {
          console.log('没有可取消的请求')
        }
      }
    }
  }
</script>

<style>
  .myCanacle{

  }
</style>


```

效果图（我连续重复点击两次：发送1，发送2）：

![在这里插入图片描述](https://gitee.com/lingisme9/images1/raw/master/img/20220313070913.png)



![在这里插入图片描述](https://gitee.com/lingisme9/images1/raw/master/img/20220313070913.png)

## 13.错误处理

```js
axios.get('/user/12345')
    .catch(function (error) {
    if (error.response) {
        // 请求已发出，但服务器响应的状态码不在 2xx 范围内
        console.log(error.response.data);
        console.log(error.response.status);
        console.log(error.response.headers);
    } else {
        // Something happened in setting up the request that triggered an Error
        console.log('Error', error.message);
    }
    console.log(error.config);
});
```

可以使用 validateStatus 配置选项定义一个自定义 HTTP 状态码的错误范围。

```js
axios.get('/user/12345', {
  validateStatus: function (status) {
    return status < 500; // 状态码在大于或等于500时才会 reject
  }
})
```

## 14.CSRF防御

### 1.CSRF 简介

**跨站请求伪造**（Cross-site request forgery），**通常缩写为 CSRF 或者 XSRF**，是一种挟制用户在当前已登录的 Web 应用程序上执行非本意的操作的攻击方法。

跨站请求攻击，简单的来说，是攻击者通过一些技术手段欺骗用户的浏览器去访问一个自己曾经认证过的网站并运行一些操作（如发邮件，发消息，甚至财产操作如转账和购买商品）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去运行。

![img](https://gitee.com/lingisme9/images1/raw/master/img/20220313070913.png)



在上图中攻击者利用了 Web 中用户身份验证的一个漏洞：简单的身份验证只能保证请求发自某个用户的浏览器，却不能保证请求本身是用户自愿发出的。既然存在以上的漏洞，那么我们应该怎么进行防御呢？接下来我们来介绍一些常见的 CSRF 防御措施。



### 2.CSRF 防御措施

+ 检查 Referer字段

HTTP 头中有一个 Referer 字段，这个字段用以标明请求来源于那个地址，在处理敏感数据请求时，通常来说，Referer 字段应和请求的地址位于同一域名下。

以示例中商城操作为例，Referer 字段地址通常应该是商城所在的网页地址，应该也位于 www.semlinker.com 之下。而如果是 CSRF 攻击传来的请求，Referer 字段会是包含恶意网址的地址，不会位于 www.semlinker.com 之下，这时候服务器就能识别出恶意的访问。

这种办法简单易行，仅需要在关键访问处增加一步校验。但这种办法也有其局限性，因其完全依赖浏览器发送正确的 Referer 字段。虽然 HTTP 协议对此字段的内容有明确的规定，但并无法保证来访的浏览器的具体实现，亦无法保证浏览器没有安全漏洞影响到此字段。并且也存在攻击者攻击某些浏览器，篡改其 Referer 字段的可能。


+ 同步表单 CSRF 校验

CSRF 攻击之所以能够成功，是因为服务器无法区分正常请求和攻击请求。针对这个问题我们可以要求所有的用户请求都携带一个 CSRF 攻击者无法获取到的 token。对于 CSRF 示例图中的表单攻击，我们可以使用 同步表单 CSRF 校验 的防御措施。

同步表单 CSRF 校验就是在返回页面时将 token 渲染到页面上，在 form 表单提交时候通过隐藏域或者作为查询参数把 CSRF token 提交到服务器。比如，在同步渲染页面时，在表单请求中增加一个 _csrf 的查询参数，这样当用户在提交这个表单的时候就会将 CSRF token 提交上来：

```html
<form method="POST" action="/upload?_csrf={{由服务端生成}}" enctype="multipart/form-data"> 
    用户名: <input name="name" /> 
    选择头像: <input name="file" type="file" /> 
    <button type="submit">提交</button>
</form>
```

### 3.双重 Cookie 防御

双重 Cookie 防御 就是将 token 设置在 Cookie 中，在提交（POST、PUT、PATCH\DELETE）等请求时提交 Cookie ，并通过请求头或请求体带上 Cookie 中已设置的 token，服务端接收到请求后，再进行校验。

下面以 JQuery 为例，来看一下如何设置 CSRF token :

```js
let csrfToken = Cookies.get('csrfToken');

function csrfSafeMethod(method){
    // 以下HTTP方法不需要进行CSRF防护
    return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method))
}

$.ajaxSetup({
    beforeSend:function(xhr,settings){
        if(!csrfSafeMethod(settings.type) && !this.crossDomain){
            xhr.setRequestHeader('x-csrf-token',csrfToken)
        }
    }
})
```

### 4.Axios CSRF 防御

Axios 提供了 xsrfCookieName 和 xsrfHeaderName 两个属性来分别设置 CSRF 的 Cookie 名称和 HTTP 请求头的名称，它们的默认值如下所示：

```js
// lib/defauls.js
var defauls = {
    adapter:getDefaultAdapter(),
    //省略部分代码
    xsrfCookieName:'XSRF-TOKEN',
    xsrfHeaderName:'X-XSRF-TOKEN',
}
```

前面我们已知在不同的平台中，Axios 使用不同的适配器来发送 HTTP 请求，这里我们以浏览器平台为例，来看一下 Axios 如何防御 CSRF 攻击：

````js
// lib/adapters/xhr.js
module.exports = function xhrAdapter(config) {
  return new Promise(function dispatchXhrRequest(resolve, reject) {
    var requestHeaders = config.headers;
    
    var request = new XMLHttpRequest();
    // 省略部分代码
    
    // 添加xsrf头部
    if (utils.isStandardBrowserEnv()) {
      var xsrfValue = (config.withCredentials || isURLSameOrigin(fullPath)) && config.xsrfCookieName ?
        cookies.read(config.xsrfCookieName) :
        undefined;
 
      if (xsrfValue) {
        requestHeaders[config.xsrfHeaderName] = xsrfValue;
      }
    }
 
    request.send(requestData);
  });
};
````

Axios 内部是使用 双重 Cookie 防御 的方案来防御 CSRF 攻击，其实 Axios 项目还有一些值得我们借鉴的地方，比如 CancelToken 的设计、异常处理机制等