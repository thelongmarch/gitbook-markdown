1. node_env

https://www.jianshu.com/p/db59535fb723

https://www.jianshu.com/p/c8f9c61c2f20

https://segmentfault.com/a/1190000016308995


2. 关于webpack.DefinePlugin

https://juejin.im/post/5868985461ff4b0057794959#comment

https://juejin.im/post/5c02378af265da61715e13d7

官方文档：

https://webpack.docschina.org/plugins/define-plugin/

注意，因为这个插件直接执行文本替换，给定的值必须包含字符串本身内的实际引号。通常，有两种方式来达到这个效果，使用 '"production"', 或者使用 JSON.stringify('production')。
