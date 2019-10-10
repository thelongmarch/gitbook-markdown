## 介绍

npm 从5.2版开始，增加了 npx 命令。万一不能用，就要手动安装一下

```
$ npm install -g npx
```

### 调用项目安装的模块

npx 想要解决的主要问题，就是调用项目内部安装的模块。比如，项目内部安装了测试工具Mocha。

```
$ npm install  mocha -D
```
一般来说，调用 Mocha ，只能在项目脚本和 package.json 的 scripts 字段里面， 如果想在命令行下调用，必须像下面这样。

### 项目的根目录下执行
```
$ npm init -y
$ npm i mocha -D
$ node_modules/.bin/mocha --version

```


注意：亲测在cmd和vscode都不可以执行（可通过cd 到.bin目录下直接执行），不过可以在git终端中执行。

npx 就是想解决这个问题，让项目内部安装的模块用起来更方便，只要像下面这样调用就行了。

```
$ npx mocha --version

```
npx 的原理很简单，就是运行的时候，会到 node_modules/.bin 路径和环境变量 $PATH 里面，检查命令是否存在。

由于 npx 会检查环境变量 $PATH ，所以系统命令也可以调用。

```
# 等同于 ls
$ npx ls
```

注意:Bash 内置的命令不在 $PATH 里面，所以不能用。比如， cd 是 Bash 命令，因此就不能用 npx cd 。

### 避免全局安装模块

1. 除了调用项目内部模块，npx 还能避免全局安装的模块。比如， create-react-app 这个模块是全局安装，npx 可以运行它，而且不进行全局安装。

```
$ npx create-react-app my-react-app
```
上面代码运行时，npx 将 create-react-app 下载到一个临时目录，使用以后再删除。所以，以后再次执行上面的命令，会重新下载 create-react-app 。

2. 下载全局模块时，npx 允许指定版本。

```
$ npx uglify-js@3.1.0 main.js -o ./dist/main.js
```
上面代码指定使用 3.1.0 版本的 uglify-js 压缩脚本。

注意:只要 npx 后面的模块无法在本地发现，就会下载同名模块。比如，本地没有安装 http-server 模块，下面的命令会自动下载该模块，在当前目录启动一个 Web 服务。
```
$ npx http-server
```