### http


URL ： 统一资源定位符

http://www.jikexueyuan.com:80/course?id=21302

http: 协议，约定好的通信内容的格式，双方根据约定好的格式来理解彼此

www.jekexueyuan.com: 主机名称 ，请求的服务器（DNS域名解析），相当于楼号

80： 端口，相当于房间号，Http默认使用80端口，所以80时也可以省略

主机+端口 ：确定了唯一的通信通道，可以在此通道上完成通信过程

/course ：资源路径,指明请求当前WEB服务上的什么资源，服务端按此部分内容决定处理行为

?id=21302 : 查询参数，也称query string


### 响应

HTTP/1.1 200 ok

HTTP/1.1 表示服务器支持的协议版本

200 表示响应状态码，200表示成功

其他常见状态码

301,302 重定向，后面会跟一个Location头，指明跳转位置

304 从浏览器缓存加载

403 权限不够，拒绝访问

404 资源未找到

500 服务器内部错误，通常是后端程序发生错误

Content-Type : text/html,imge/png, application/json 等等，表明返回类型

Content-length: 响应Boyd长度

发生重定向时包含的头，这时候一般没有Body

Location:http://localhost/other/resource


### 安装 

1. npm init -y
2. npm i express -S

## 使用

