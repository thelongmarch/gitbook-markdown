### 小程序与普通网页开发的区别

​网页开发渲染线程和脚本线程是互斥的，这也是为什么长时间的脚本运行可能会导致页面失去响应，而在小程序中，二者是分开的，分别运行在不同的线程中。网页开发者可以使用到各种浏览器暴露出来的 DOM API，进行 DOM 选中和操作。而如上文所述，小程序的逻辑层和渲染层是分开的，逻辑层运行在 JSCore 中，并没有一个完整浏览器对象，因而缺少相关的DOM API和BOM API。这一区别导致了前端开发非常熟悉的一些库，例如 jQuery、 Zepto 等，在小程序中是无法运行的。同时 JSCore 的环境同 NodeJS 环境也是不尽相同，所以一些 NPM 的包在小程序中也是无法运行的。

​网页开发者需要面对的环境是各式各样的浏览器，PC 端需要面对 IE、Chrome、QQ浏览器等，在移动端需要面对Safari、Chrome以及 iOS、Android 系统中的各式 WebView 。而小程序开发过程中需要面对的是两大操作系统 iOS 和 Android 的微信客户端，以及用于辅助开发的小程序开发者工具，小程序中三大运行环境也是有所区别的，如表1-1所示。

### 小程序的运行环境

运行环境	               逻辑层	                   渲染层
iOS	                     JavaScriptCore         	WKWebView
安卓	                    V8              	chromium定制内核
小程序开发者工具	        NWJS	              Chrome WebView

### 小程序代码构成

1. .json 后缀的 JSON 配置文件
2. .wxml 后缀的 WXML 模板文件
3. .wxss 后缀的 WXSS 样式文件
4. .js 后缀的 JS 脚本逻辑文件


####  JSON 配置

项目的根目录有一个 app.json 和 project.config.json，此外在 pages/logs 目录下还有一个 logs.json

app.json 是当前小程序的全局配置，包括了小程序的所有页面路径、界面表现、网络超时时间、底部 tab 等。QuickStart 项目里边的 app.json 配置内容如下：

{
  "pages":[
    "pages/index/index",
    "pages/logs/logs"
  ],
  "window":{
    "backgroundTextStyle":"light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "WeChat",
    "navigationBarTextStyle":"black"
  }
}
我们简单说一下这个配置各个项的含义:

pages字段 —— 用于描述当前小程序所有页面路径，这是为了让微信客户端知道当前你的小程序页面定义在哪个目录。
window字段 —— 定义小程序所有页面的顶部背景颜色，文字颜色定义等。

### wxss

WXSS 在底层支持新的尺寸单位 rpx

### 小程序宿主环境

我们称微信客户端给小程序所提供的环境为宿主环境。小程序借助宿主环境提供的能力，可以完成许多普通网页无法完成的功能。

###　渲染层和逻辑层
首先，我们来简单了解下小程序的运行环境。小程序的运行环境分成渲染层和逻辑层，其中 WXML 模板和 WXSS 样式工作在渲染层，JS 脚本工作在逻辑层。

小程序的渲染层和逻辑层分别由2个线程管理：渲染层的界面使用了WebView 进行渲染；逻辑层采用JsCore线程运行JS脚本。一个小程序存在多个界面，所以渲染层存在多个WebView线程，这两个线程的通信会经由微信客户端（下文中也会采用Native来代指微信客户端）做中转，逻辑层发送网络请求也经由Native转发，小程序的通信模型下图所示。




## 小程序全局配置页面 app.json

```
{
  "pages":[
    "pages/index/index",
    "pages/logs/logs"
  ],
  "window":{   
    "navigationBarBackgroundColor": "#0094ff",
    "navigationBarTitleText": "我的应用",
    "navigationBarTextStyle":"white",
    "backgroundTextStyle":"light",
    "enablePullDownRefresh": true,
    "backgroundColor": "#d2357d"
  },
  "tabBar": {
    "list": [{
      "pagePath": "pagePath",
      "text": "text",
      "iconPath": "iconPath",
      "selectedIconPath": "selectedIconPath"
    }]
  },
  "style": "v2",
  "sitemapLocation": "sitemap.json"
}

```


### window

```
   "window":{   
     //设置导航栏相关

    "navigationBarBackgroundColor": "#0094ff",//导航栏背景颜色
    "navigationBarTitleText": "我的应用",//导航栏标题文字内容
    "navigationBarTextStyle":"white",//导航栏标题颜色，仅支持 black / white

    //设置下拉相关属性
    
    "enablePullDownRefresh": true,   //是否开启全局的下拉刷新。
    "backgroundTextStyle":"light",   // 下拉 loading 的样式，仅支持 dark / light	
    "backgroundColor": "#d2357d"    //下拉刷新的背景颜色
  }

```
### tabbar

```
  "tabBar": {
    "list": [
      {
        "pagePath": "pages/index/index",  //页面路径，必须在 pages 中先定义
        "text": "首页",                   //tab 上按钮文字
        "iconPath": "icon/_home.png",     
        // 图片路径，icon 大小限制为 40kb，建议尺寸为 81px * 81px，不支持网络图片。当 position 为 top 时，不显示 icon。
        "selectedIconPath": "icon/home.png"
        // 选中时的图片路径，icon 大小限制为 40kb，建议尺寸为 81px * 81px，不支持网络图片。当 position 为 top 时，不显示 icon。
      },
      {
        "pagePath": "pages/img/img",
        "text": "图片",
        "iconPath": "icon/_img.png",
        "selectedIconPath": "icon/img.png"
      },
      {
        "pagePath": "pages/mine/mine",
        "text": "我的",
        "iconPath": "icon/_my.png",
        "selectedIconPath": "icon/my.png"
      },
      {
        "pagePath": "pages/search/search",
        "text": "搜索",
        "iconPath": "icon/_search.png",
        "selectedIconPath": "icon/search.png"
      }
    ],
    "color": "#95a5a6", //tab 上的文字默认颜色，仅支持十六进制颜色
    "selectedColor": "#8e44ad",//tab 上的文字选中时的颜色，仅支持十六进制颜色
    "backgroundColor": "#ecf0f1",//ab 的背景色，仅支持十六进制颜色
    "position":"bottom"//tabBar 的位置，仅支持 bottom / top
  },
```

### rpx

1. 设计稿750px
750 px = 750rpx
1px = 1rpx
2. 设计稿375px
375 px = 750rpx
1px = 2rpx
3. 存在一个设计稿414或者未知page
3.1 设计稿page  存在一个元素 宽度 100px
3.2 拿以上的需求去实现 不同宽度的页面适配

page px = 750 rpx
1 px = 750 rpx /page
100px = 750 rpx *100 /page

### image

默认图片大小  320px  240px

1 scaleToFill 默认值 缩放模式，不保持纵横比缩放图片，使图片的宽高完全拉伸至填满 image 元素

<image mode="scaleToFill" src="https://ae01.alicdn.com/kf/H7bc7ad347620440e835350e037387086m.jpg"></image>

2 aspectFit 保持宽高比  确保图片的长边  显示出来  页面轮播

<image mode="aspectFit" src="https://ae01.alicdn.com/kf/H7bc7ad347620440e835350e037387086m.jpg"></image>

3 aspectFill 保持纵横比缩放图片，只保证图片的 短边 能完全显示出来  少用

<image mode="aspectFill" src="https://ae01.alicdn.com/kf/H7bc7ad347620440e835350e037387086m.jpg"></image>

4 widthFix 以前web的图片的 宽度指定了之后 高度  会自己按比例来调整  常用

<image mode="widthFix" src="https://ae01.alicdn.com/kf/H7bc7ad347620440e835350e037387086m.jpg"></image>

5 bottom...   类似以前的background-position 


lazy-load  会自己判断 当图片出现在  视口  上下 三屏的高度  之内的时候  自己开始加载图片

### swiper

swiper  默认width：100%  height:150px   高度无法实现由内容撑开


原图：1125 x 352

swiper  宽度 / swiper 高度  = 原图的宽度 /原图的高度

swiper 高度  = swiper 宽度 * 原图的高度  / 原图的宽度


height:  100vw*352 /1125



