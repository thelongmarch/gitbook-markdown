## 框架搭建

1，2，3，4，5 路由相关

6，7，8 路由相关需要的一些依赖


1. @react-native-community/masked-view  

路由相关  遮罩层

yarn add @react-native-community/masked-view

npm install --save @react-native-community/masked-view

2. @react-navigation/bottom-tabs

路由相关  底部tab

3. @react-navigation/native

路由相关  路由本地主程序

4. @react-navigation/stack

路由相关 通过点击什么东西跳转其他页面 tack栈

5. react-native-gesture-handler

路由相关  路由跳转综合路由管理

6. react-native-reanimated

动画

7. react-native-safe-area-context

上下文

8. react-native-screens

屏幕相关

9. react-native-swiper

轮播图

10. react-native-vector-icons 

图标库

## 安装

```js

npm i -S  @react-native-community/masked-view @react-navigation/bottom-tabs @react-navigation/native @react-navigation/stack react-native-gesture-handler react-native-reanimated react-native-safe-area-context react-native-screens react-native-swiper react-native-vector-icons


npx react-native link

```

Tips:
```

1.使用React-native link的背景
并不是所有的 APP 都需要使用全部的原生功能，包含支持全部特性的代码会增大应用的体积。但我们仍然希望能让你简单地根据自己的需求添加需要的特性。
在这种思想下，我们把许多特性都发布成为互不相关的静态库。
大部分的库只需要拖进两个文件就可以使用了，偶尔你还需要几步额外的工作，但不会再有更多的事情要做了。
我们随着 React Native 发布的所有库都在仓库中的Libraries文件夹下。其中有一些是纯 Javascript 代码，你只需要去import它们就可以使用了。另外有一些库基于一些原生代码实现，你必须把这些文件添加到你的应用，否则应用会在你使用这些库的时候产生报错。

2.具体使用React-native link的步骤：

（1）.下载某个库到本地

npm install ******

（2）.链接某个库到项目中

react-native link *****

```
### 报错处理

当安装完依赖之后会有类似报错 unable to resolve module

1. clear watchman watches: watchman watch-del-all

2. delete node_modules:rm -rf node_modules and run yarn install

3. reset wetro's cache: yarn start --reset-cache

4. remove the cache: rm -rf /temp/metro-*

### rnc 需要安装的插件

react-native-snippets

react-native-css snippets

ReactJs 

ReactJs snippets







