### 概述

使用 koa 编写 web 应用，通过组合不同的 generator，可以免除重复繁琐的回调函数嵌套，并极大地提升错误处理的效率。一个Koa应用就是一个对象，包含了一个middleware数组，这个数组由一组Generator函数组成。这些函数负责对HTTP请求进行各种加工，比如生成缓存、指定代理、请求重定向等等。这些中间件函数基于 request 请求以一个类似于栈的结构组成并依次执行。

### 安装node

Koa2的开发，Node.js是有要求的，它要求Node.js版本高于V7.6。因为node.js 7.6版本开始完全支持async/await,不需要再加flag，所以才能完全你支持我们的Koa2。

查看版本

```js 
    node -v 
```
1. windwos升级node

直接下载新的安装包，重新安装，覆盖原来的安装位置。

安装位置查找方法，在命令行里输入:

```js
where node

```

2. mac 升级node

```
sudo npm install -g n
sudo n stable

```
### 搭建环境

1. 新建项目目录

```
cd code  //进入code文件夹
mkdir koa2 //创建koa2文件夹
cd koa2  //进入koa2文件夹

```
2. 初始化

```
npm init -y
```

3. 安装koa

```
npm install --save koa
```

3. 在文件夹跟目录下新建index.js文件，输入下面代

```js
const Koa = require('koa')
const app = new Koa()
 
app.use( async ( ctx ) => {
  ctx.body = 'hello koa2'
})
 
app.listen(3000)
console.log('start')

```
4. node index.js 启动

### get和post请求

1. get请求

获得GET请求的方式有两种，一种是从request中获得，一种是一直从上下文中获得。获得的格式也有两种：query和querystring。

query和querystring区别：

query：返回的是格式化好的参数对象。
querystring：返回的是请求字符串。


（1）在koa2中GET请求通过request接收，但是接受的方法有两种：query和querystring。

（2）直接从ctx中获取Get请求：除了在ctx.request中获取Get请求外，还可以直接在ctx中得到GET请求。ctx中也分为query和querystring。

```js
const Koa = require('koa');
const app = new Koa();
app.use(async(ctx)=>{
    let url =ctx.url;
 
    //从request中获取GET请求
    let request =ctx.request;
    let req_query = request.query;
    let req_querystring = request.querystring;
 
    //从上下文中直接获取
    let ctx_query = ctx.query;
    let ctx_querystring = ctx.querystring;
 
    ctx.body={
        url,
        req_query,
        req_querystring,
        ctx_query,
        ctx_querystring
    }
 
});
 
app.listen(3000,()=>{
    console.log('[demo] server is starting at port 3000');
});

```