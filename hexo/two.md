## 来源

视频地址：https://v.youku.com/v_show/id_XMzkyODMyNjcyOA==.html?spm=a2h0j.11185381.listitem_page1.5~A&&f=51971728

博客网址：http://damienzhong.com

github：https://github.com/zhujiangsky

## git之ssh生成

1. 目录：C:\Users\ccz

git中可以通过命令查看是否存在ssh
```js
cd ~/.ssh 
```

>ssh-keygen命令用于为“ssh”生成、管理和转换认证密钥，它支持RSA和DSA两种认证密钥.

>ssh-keygen(选项)
```
-b：指定密钥长度； 
-e：读取openssh的私钥或者公钥文件； 
-C：添加注释； 
-f：指定用来保存密钥的文件名； 
-i：读取未加密的ssh-v2兼容的私钥/公钥文件，然后在标准输出设备上显示openssh兼容的私钥/公钥； 
-l：显示公钥文件的指纹数据； 
-N：提供一个新密语； 
-P：提供（旧）密语；
-q：静默模式； 
-t：指定要创建的密钥类型。
```

2. 生成github ssh key（一直回车）:

```
ssh-keygen -t rsa   -C "2454424641@qq.com"
            ~~密钥类型   ~~~~ 备注
```
此命令前建议先 cd ~/.ssh 进入ssh目录,那么生成的ssh都在这个目录，方便管理。
3. 登录github。打开setting->SSH keys，点击右上角 New SSH key，把生成好的公钥id_rsa.pub放进 key输入框中，再为当前的key起一个title来区分每个key

4. 测试是否连接成功
(1) 本地配置config
```
git config --global user.name 'thelongmarch'
git config --global user.email '2454424641@qq.com'

```
（2）输入ssh -T git@github.com，测试添加ssh是否成功。如果看到Hi后面是你的用户名，就说明成功了.





