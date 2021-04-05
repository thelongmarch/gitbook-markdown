## 云开发

云开发是微信团队联合腾讯云提供的原生 Serverless  云服务，致力于帮助更多的开发者快速实现小程序业务的开发，快速迭代。

### 云开发能力介绍

- 存储 ：在小程序端直接上传/下载云端文件，可视化管理
- 云函数：在云端运行的代码，微信私有天然鉴权，开发者只需要编写自身业务逻辑代码
- 云数据库：一个既可在小程序前端操作，也能在云函数中读写的 JSON 数据库
- 音视频服务：提供互通高品质实时音视频通话服务，支持互动白板，美颜滤镜，高清视频通话等，基于云开发快速接入
- 智能图像服务：集成智能鉴黄、人脸识别、人脸核身等AI视觉能力，基于云开发快速接入



### 小程序云开发实战

视频地址：

https://www.bilibili.com/video/BV1pE411C7Ca?p=4&spm_id_from=pageDriver

配套笔记：
https://www.kancloud.cn/java-qiushi/yunkaifa


1. 初始化一个空项目，不勾选云开发

2. 删除log文件夹，app.json中删除log路由

3. page/index/index.js  

```js
Page({
 
})

```
4. 在project.config.json 文件中添加 

```js

"cloudfunctionRoot":"cloud",

```
然后再根目录下新建cloud文件夹，文件夹会有一个云的标志。

5. 初始化云功能

```js 
// app.js
App({
  onLaunch() {
      //云开发环境初始化
      wx.cloud.init({
        env:'yun-env-3gc0139s7817845b'
      })
  }
  
})

```