---
title: axios
tags: [axios]
date: 2020-8-7 20:10:00
categories: axios
---





<!--more-->

## Axios使用ajax

+ axios是什么 
  + http://www.axios-js.com/
  + axios是一套对ajax的封装的JS库，本质上用的还是XMLHttpRequest
  + 它用promise进行封装，有.then和.catch方法

+ 为什么用
  + 因为它只是网络请求的封装，相对来说要轻量
  + 它是基于promise封装的，所以用起来方便
  + 【jQuery的后期版本也加装了promise，但是jQuery还封装了许多别的功能】
+ axios不支持jsnop，请使用CORS(后端)进行跨域

### 1.get请求

+ params的参数会拼接到url上
+ 请求方式还有：put、delete等,需要你把参数写到url上，那么就要写params

```javascript
axios.get('https://autumnfish.cn/api/joke/list',{
            params:{ num:5 }//params的参数会拼接到url上
          })
          .then( res => {
            // 请求成功触发
            console.log(res)
          })
          .catch (error => {
            // 请求失败触发
            console.log('请求失败')
          })
```

### 2.post请求

```javascript
// 要注意：post请求传参数，直接在第二个参数里写就行了
          axios.post('https://autumnfish.cn/api/user/reg',{
            username: this.username
          })
          .then( res => {
            console.log(res)
          })
```

### 3.config配制模式

- 通过网址传递的参数都是写到params里
- 通过请求体传递的参数都是写到data里
- 用法：

```javascript
axios({
    method:'', //请求方式
    url:'', //请求路径
    data:'', //请求体传递的参数
    params:'', //url传递的参数
})
```

+ 示例

```javascript
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
    axios({
        method: 'get', //请求方式
        url: 'https://autumnfish.cn/api/joke/list', //请求路径
        data: '', //请求体传递的参数
        params: {
            num: 5
        }, //url传递的参数
    }).then(res => {
        console.log(res);
    })
</script>
```







## 拦截器

给所有的请求加请求头

拦截错误，直接阻止then，马上跳转到cath，一次性处理所有的错误



## 设置请求头

```javascript
axios({
    url:'xxx',
    headers:{
        xxx:xxx
    }
})
```



## ajax请求解决缓存问题

- ajax请求也有缓存
- 因为浏览器内部策略访问同一个网址它可能会缓存起来
- 解决思路：在网址后面拼接参数，参数值为随机数







## axios基地址配制与全局调用【比较low的用法】

- 把axios对象设置给 Vue.prototype.$axios 就可以了

- 在main.js有Vue构造函数，所以上面代码写在main.js里

- 这是利用：构造函数实例化出来的对象也可以访问原型对象里的属性

- ```js
  Vue.prototype.$axios = axios.create({
    // 配置副本的一些信息
    baseURL:'https://autumnfish.cn/'
  })
  ```

- 这代表创建一个新的axios对象，并使用以上作为基地址

- 基地址如果设置了，会自动帮你拼接，但是如果你发请求时就写好了全网址，就不帮你拼接了