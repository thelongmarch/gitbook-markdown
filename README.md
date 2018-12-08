# Introduction

## 1. Introduction

微信小程序 - 四级页（商品详情）

开发须知：

- 迭代周期 - 两周一次，开发时间约为周期的 1/3，需求评估0.5d 起
- 四级页（web、pc）暂时在李宁那边
- appid - wx681b1e78da02dd16
- gitlab 地址 - http://git.cnsuning.com/minip/MINIP
- svn 项目地址 - http://a.svndoc.cnsuning.com/svn/ScrumTeam/00_需求管理/四级页产品线
- 项目目录 - /suning/pages/fouth

各个部门对接人：

- UI - 王刚 - 17032367
- 测试 - 杨萍 - 14073978
- 产品 - 沈忱  - 18033666
- 后端 - 胡宇 - 16071758

## 2. Folder Structure
项目暂时性结构目录
```
.
├── activity                   # 营销活动
├── address                    # 地址选择（三级地址）
├── coupon                     # 领券模块
├── fragment                   # 代码片段
├── imageVerify                # 图形验证码
├── itemlist                   # 商品列表
├── js                         # js
  ├── cart.js                      # 购物车
  ├── common.js                    # 工具方法
  ├── detail.js                    # 商品详情（规格参数）
  ├── eval.js                      # 商品评论
  ├── oversea.js                   # 海外购
  ├── parser.js                    # 爆炸贴
  ├── promotion.js                 # 商品促销
  ├── requests.js                  # 请求封装
  ├── service.js                   # 商品服务（退货等）
  ├── shop.js                      # 店铺
  ├── sub.js                       # 通子码 （颜色簇、衣服才有通子码）
  └── vip.js                       # super会员
├── oversea                    # 海外购
├── package                    # 商品套餐
├── util                       # 工具类
├── base-fourth.less           # 普通商品样式
├── base.less                  # 通用样式
├── fourth-doc.js              # 迭代文档
├── fourth.js                  # 核心代码
├── fourth.json                # 配置文件
├── fourth.wxml                # html
├── fourth.wxss                # css
└── oversea.less               # less
```

## 3. Dependencies

### 3.1 IE 代理

http://it.cnsuning.com/pac.pac



### 3.2 Vscode

需要安装

- easy less - 用于编译 less
- eslint - 代码检查工具
- .editorconfig - 统一IDE风格
- Document This - JSDoc
- minapp - support 小程序
- koroFileHeader - file header

### 3.3 Git、Svn

git - 拉项目代码 

- 权限找禹立彬

- ssh 秘钥

```bash
ssh-keygen -t rsa -C "工号@git.cnsuning.com" -b 4096
```

- git

```bash
git config --global credential.helper store
git config --global user.name "姓名"
git config --global user.email "工号@git.cnsuing.com"
```



svn - 拉需求文档

- 权限找周亚萍

- http://a.svndoc.cnsuning.com/svn/ScrumTeam/00_需求管理/四级页产品线



### 3.4 Nodejs

苏宁源

```bash
$ npm config set registry http://snpm.cnsuning.com
// 更改全局变量
$ npm config set prefix "C:\Program Files\nodejs"
```

或者设置代理，用淘宝源

```bash
$ npm config set proxy=http://xzproxy.cnsuning.com:8080
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

PS: 若使用 NVM 切换 Nodejs 版本需要在 nvm 安装目录下的 settings.txt 添加

```
proxy: http://xzproxy.cnsuning.com:8080
```



### 3.5 小程序IDE

- 开发者权限找禹立彬

四级页自定义编译环境如下：

shop - 店铺id

productId - 商品id

![编译设置](C:/Users/18085949/Desktop/fourth-devlopment/images/wechat-fourth.png)



### 

## 4. Development

### 4.1 开发环境

开发分支 - dev

发布分支 - Dev_{上线日期}

环境切换 suning/config/environment.js

```js
var ENV = {
  sit: 'sit',
  pre: 'pre',
  prod: 'prod',
  prodFast: 'prodFast',
  xgpre: 'xgpre'
};
var env = 'prod';
```

需要什么环境就切换相应的 env



### 4.2 LESS编译

wxss编译流程：

1. 分别编译base-fourth.less和oversea.less
2. 执行npm run css编译生成forth.wxss

注：在海外购开发过程中可以直接编译oversea.less生成forth.wxss，简化编译流程。
但是该方法编译生成的样式只包含海外购样式且css优先级与正式编译结果有差异，只能用于本地调试，不能提交到版本库中。



### 4.3 图片URL

四级页用到的会全部放到 OSS 服务器上， 需要图片上传的请联系我

返回的图片url地址为 - https://oss.suning.com/vss/activity/wximg/fourth/ + imageUrl



## 5. Deployment

// TODO



## 6 . TODO List

// TODO



## 7. Change Log

// TODO



## 8. Notices

- css - wxParse
- wxs - wxscript 在ios上性能高于js两边，andriod上无区别
- 



## 9. Rebuild

四级页面（pc、wap、app内嵌、小程序）项目前端规范以及 pc 重构思路.

重构坑点:

产品应提供项目整体开发流程说明文档.

前端开发规范文档应包含如下：

- 功能需求 - **产品提供** 
- 开发规范 
- 接口说明 - **后端提供**
- 环境部署
- 交接事项