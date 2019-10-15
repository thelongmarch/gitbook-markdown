### 环境变量配置（1.8）

1. 我的电脑-->属性

2. 建JAVA_HOME系统变量

变量名：JAVA_HOME ，

变量值：C:\Program Files\Java\jdk1.8.0_171(这里填你自己选择的安装路径！！！)

3. 新建CLASSPATH变量

变量名：CLASSPATH , 

变量值： .;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar（注意最前面有一点）。

4. 保存

5. 测试

cmd中，分别输入"java -version"、"javac"


### 配置git提交名字和邮箱

```
git config --global user.name "姓名中文" 
git config --global user.email "姓名全拼小写@jsti.com"

```

查看
```
git config -l
```
1. 个人

user.email=themarch@163.com

user.name=thelongmarch



2. 公司

user.email=jsti_ccz@jsti.com
user.name=陈长征

