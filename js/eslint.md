###  eslint

1. 初始化
```
yarn init -y 

```

2. 安装eslint

```
yarn add -D eslint

```
3. 查看eslint命令

```
npx eslint

```
等价于

```
node ./node_modules/eslint/bin/eslint.js
```

4. 初始化

```
npm eslint --init

```

运行 eslint --init 之后，.eslintrc 文件会在你的文件夹中自动创建

5. 使用

（1）新建src目录，在src创建index.js文件，如下

```
let a = 123;
```
根目录：npx eslint ./src/index.js

会有如下提示：
```
D:\2020project\eslint-demo\src\index.js
  1:5  error  'a' is assigned a value but never used  no-unused-vars

✖ 1 problem (1 error, 0 warnings)
```
（2）如果是两个文件或者多个文件

```
npx eslint ./src/index.js  ./src/index2.js

```
```
npx eslint ./src/*.js
```

6. 优化

package.json

```
"scripts": {
    "eslint": "eslint ./src/*.js"
  },
```
```
yarn eslint
或
npm run eslint
```