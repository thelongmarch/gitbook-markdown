### 使用

//main.js

import Axios from "axios"

Vue.prototype.$axios = Axios
//添加全局配置
Axios.defaults.baseUrl = 'http://www.wwtliu.com'
Axios.defaults.headers.common['Authorization'] = AUTH_TOKEN
Axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencodeed'



//vue

```js
//get 方式
this.$axios.get("http://www.wwtliu.com/sxtstu/blueberrypai/getChengpinDetails.php")
.then(res=>{
    console.log(res);
})
.catch(error=>{
    console.log(error);
})


//参数
//方式一
this.$axios.get("http://www.wwtliu.com/sxtstu/blueberrypai/getChengpinDetails.php?count=10")
.then(res=>{
    console.log(res);
})
.catch(error=>{
    console.log(error);
})

//方式二
this.$axios.get("http://www.wwtliu.com/sxtstu/blueberrypai/getChengpinDetails.php?",{
    params:{
        count:10
    }
})
.then(res=>{
    console.log(res);
})
.catch(error=>{
    console.log(error);
})

//通过请求获取远程图片
this.$axios({
    method:'get',
    url:'http://bit.ly/2mTM3Ny',
    responseType:'stream'
})
.then(function(response{
    response.data.pipe(fs.createWriteStream('aaa.jpg'))
})

```


//post
```js

this.$axios.post("http://localhost:3000/selectuser",qs.stringfy({
    user:'hello'
}))
.then(res=>{
    console.log(res);
})
.catch(error=>{
    console.log(error);
})

//发起post请求
this.$axios({
    method:'post',
    url:'http://bit.ly/2mTM3Ny',
    data:{
        firstName:'redd'
    }
});

```