### 环境搭建

1. npm init -y 默认初始化值
2. npm i -g babel-cli
3. 本地安装babel-preset-es2015 和 babel-cli


```js
npm i  babel-preset-es2015 babel-cli -D
```
4. .babelrc文件

```js
{
    "presets":[
        "es2015"
    ],
    "plugins": []
}

```

4. babel src/index.js -o dist/index.js

5. 优化：

```js
"scripts": {
    "build": "babel src/index.js -o dist/index.js"
  }

```

### 新的声明方式

1. var：它是variable的简写，可以理解成变量的意思。
2. let：它在英文中是“让”的意思，也可以理解为一种声明的意思。
3. const：它在英文中也是常量的意思，在ES6也是用来声明常量的，常量你可以简单理解为不变的量。


### 解构赋值

- 数组的解构赋值：

1. ** 简单的数组解构：**

以前，为变量赋值，我们只能直接指定值。比如下面的代码：
```js
let a=0;
let b=1;
let c=2;
```

而现在我们可以用数组解构的方式来进行赋值。

```js
letl  [a,b,c]=[1,2,3];
```

上面的代码表示，可以从数组中提取值，按照位置的对象关系对变量赋值。

2. ** 数组模式和赋值模式统一：**

可以简单的理解为等号左边和等号右边的形式要统一，如果不统一解构将失败。

```js
let [a,[b,c],d]=[1,[2,3],4];
```
如果等号两边形式不一样，很可能获得undefined或者直接报错。

3. 解构的默认值：

解构赋值是允许你使用默认值的，先看一个最简单的默认是的例子。

let [foo = true] =[];
console.log(foo); //控制台打印出true
上边的例子数组中只有一个值，可能你会多少有些疑惑，我们就来个多个值的数组，并给他一些默认值。

let [a,b="JSPang"]=['技术胖']
console.log(a+b); //控制台显示“技术胖JSPang”
现在我们对默认值有所了解，需要注意的是undefined和null的区别。

let [a,b="JSPang"]=['技术胖',undefined];
console.log(a+b); //控制台显示“技术胖JSPang”
undefined相当于什么都没有，b是默认值。

let [a,b="JSPang"]=['技术胖',null];
console.log(a+b); //控制台显示“技术胖null”
null相当于有值，但值为null。所以b并没有取默认值，而是解构成了null。