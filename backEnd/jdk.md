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