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
```js
let [foo = true] =[];
console.log(foo); //控制台打印出true
```
上边的例子数组中只有一个值，可能你会多少有些疑惑，我们就来个多个值的数组，并给他一些默认值。
```js
let [a,b="JSPang"]=['技术胖']
console.log(a+b); //控制台显示“技术胖JSPang”
```
现在我们对默认值有所了解，需要注意的是undefined和null的区别。
```js
let [a,b="JSPang"]=['技术胖',undefined];
console.log(a+b); //控制台显示“技术胖JSPang”
```
undefined相当于什么都没有，b是默认值。
```js
let [a,b="JSPang"]=['技术胖',null];
console.log(a+b); //控制台显示“技术胖null”
```
null相当于有值，但值为null。所以b并没有取默认值，而是解构成了null。

- 对象的解构赋值

1. 解构不仅可以用于数组，还可以用于对象。
```js
let {foo,bar} = {foo:'JSPang',bar:'技术胖'};
console.log(foo+bar); //控制台打印出了“JSPang技术胖”
```
注意：对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

2, 圆括号的使用

如果在解构之前就定义了变量，这时候你再解构会出现问题。下面是错误的代码，编译会报错。
```js
let foo;
{foo} ={foo:'JSPang'};
console.log(foo);
```
要解决报错，使程序正常，我们这时候只要在解构的语句外边加一个圆括号就可以了。
```js
let foo;
({foo} ={foo:'JSPang'});
console.log(foo); //控制台输出jspang
```
- 字符串解构

字符串也可以解构，这是因为，此时字符串被转换成了一个类似数组的对象。
```js
const [a,b,c,d,e,f]="JSPang";
console.log(a);
console.log(b);
console.log(c);
console.log(d);
console.log(e);
console.log(f);
```

### 第4节：扩展运算符和rest运算符

1. 对象扩展运算符（…）：

当编写一个方法时，我们允许它传入的参数是不确定的。这时候可以使用对象扩展运算符来作参数，看一个简单的列子：
```js
function jspang(...arg){
    console.log(arg[0]);
    console.log(arg[1]);
    console.log(arg[2]);
    console.log(arg[3]);
 
}
jspang(1,2,3);
```
这时我们看到控制台输出了 1,2,3，undefined，这说明是可以传入多个值，并且就算方法中引用多了也不会报错。

扩展运算符的用处：

我们先用一个例子说明，我们声明两个数组arr1和arr2，然后我们把arr1赋值给arr2，然后我们改变arr2的值，你会发现arr1的值也改变了，因为我们这是对内存堆栈的引用，而不是真正的赋值。
```js
let arr1=['www','jspang','com'];
let arr2=arr1;
console.log(arr2);
arr2.push('shengHongYu');
console.log(arr1);
控制台输出：

["www", "jspang", "com"]
["www", "jspang", "com", "shengHongYu"]
```
这是我们不想看到的，可以利用对象扩展运算符简单的解决这个问题，现在我们对代码进行改造。
```js
let arr1=['www','jspang','com'];
//let arr2=arr1;
let arr2=[...arr1];
console.log(arr2);
arr2.push('shengHongYu');
console.log(arr2);
console.log(arr1);
```
现在控制台预览时，你可以看到我们的arr1并没有改变，简单的扩展运算符就解决了这个问题。

rest运算符

如果你已经很好的掌握了对象扩展运算符，那么理解rest运算符并不困难，它们有很多相似之处，甚至很多时候你不用特意去区分。它也用…（三个点）来表示，我们先来看一个例子。
```js
function jspang(first,...arg){
    console.log(arg.length);
}
 
jspang(0,1,2,3,4,5,6,7);
```
这时候控制台打印出了7，说明我们arg里有7个数组元素，这就是rest运算符的最简单用法。

如何循环输出rest运算符

这里我们用for…of循环来进行打印出arg的值，我们这里只是简单使用一下，以后我们会专门讲解for…of循环。
```js
function jspang(first,...arg){
    for(let val of arg){
        console.log(val);
    }
}
 
jspang(0,1,2,3,4,5,6,7);
```
for…of的循环可以避免我们开拓内存空间，增加代码运行效率，所以建议大家在以后的工作中使用for…of循环。有的小伙伴会说了，反正最后要转换成ES5，没有什么差别，但是至少从代码量上我们少打了一些单词，这就是开发效率的提高。