---
title: AjAx
tags: [ajax]
date: 2020-8-20 17:47:00
categories: ajax
hide: false
---





js原生ajax，JQ的ajax

<!--more-->

## 原生Ajax技术

在网页不跳转的情况下向服务器请求数据

`测试接口请戳我`👉 <a href='https://github.com/AutumnFish/testApi'>大佬提供的测试接口</a>

### 原理

```javascript
 设置请求报文三个部分

//(1).实例化ajax对象
let xhr = new XMLHttpRequest();

//(2).设置请求方法和地址
xhr.open('post', 'http://www.tuling123.com/openapi/api');

//(3).设置请求头（post请求才需要设置）
xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');

//(4).发送请求 ： 参数格式  'key=value' 
xhr.send('key=8b2116b8ddb94b6681fbbef3ee9bbbce&info=你吃饭了?');

//(5).注册回调函数
xhr.onload = function () {
    console.log(xhr.responseText);
};    
```



### get请求

```javascript
<script>
    //(1)创建xhr对象
    let xhr = new XMLHttpRequest();
    //(2)设置请求方法和地址
    xhr.open('get','接口?'+'参数key=参数value');
	//xhr.open('get','https://autumnfish.cn/api/hero/simple?name=赵信');
    //(3)发送请求
    xhr.send();
    //(4)注册响应事件
    xhr.onload =function(){
        console.log(xhr.responseText);  
    };
</script>
```

### post请求

```javascript
<script>
    //(1)创建xhr
    let xhr = new XMLHttpRequest();
    //(2)设置请求方法和地址
    xhr.open('post', 'https://autumnfish.cn/api/user/register');

    //(3)设置请求头（只有post才需要，为固定格式）
    xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');

    //(4)发送ajax请求,
	xhr.send('参数key='+参数value);
    //xhr.send('username=' + username);
    //(5)注册响应事件
    xhr.onload = function () {
        $('.info').text(xhr.responseText);
    }

});
</script>

```



### get与post的区别

+ post需要单独设置请求头
+ get在url后面发：      url?key=value 
+ post在send()方法里面发：  xhr.send('key=value')



### 原生Ajax规范写法

xhr.onload 最好写在 xhr.send 之前

+  xhr.open 方法的第三个参数 false 是表示同步，true表示异步  默认true。

```text
如果同步的情况下，send写在onload之前。
onload 监听响应完成，注册的时候仅仅是监听，
服务器响应后才会执行里面的代码,此时才可以拿到send请求回来的响应体responseText，
就好像 onload注册事件告诉send：你出门的时候告诉我一声,帮我带个responseText回来，
提前给send打个招呼，所以等send发出请求以后，send才会帮你待带responseText回来，
```

而如果是异步任务，所有的异步任务都会在队列池中等待同步任务走完后执行，所以没有影响【**当线程中没有执行任何同步代码的前提下才会执行异步代码**】

异步任务中，send方法是同步的，send里面的代码是异步的，所以还是会等onload同步走完，才会进行这个异步操作

#### onreadystatechange事件

```javascript
onreadystatechange
/* 
相当于ajax的onload，区别在于浏览器兼容性不同

onreadystatechange ： 浏览器兼容性更好
    会调用多次
    需要判断 xhr.readyState==4, 才能获取服务器响应数据
    
onload 👉 旧版本浏览器不支持

*/
```





### 原生Ajax传递多参数

用 & 连接参数

```javascript
//get
xhr.open('get', 'http://www.tuling123.com/openapi/api?key1=2162602fd87240a8b7bba7431ffd379b&key12=额');

//post
xhr.send('key=2162602fd87240a8b7bba7431ffd379b&info=不需要吗？');
```

## JQuery使用ajax

$.ajax()

```javascript
$.ajax({
    //接口地址
    url: 'http://www.tuling123.com/openapi/api',
    //请求类型，可选get
    type: 'post',
    //检测json格式,如果不是json则不执行回调函数了
    dataType:'json',
    //参数
    data: {
        key: '2162602fd87240a8b7bba7431ffd379b',
        info: '请告诉我，我的女朋友在哪里？'
    },
    //回调函数
    success: function (backdata) {
        //backdata,存放返回的内容，会自动转换成js对象
    }
});
```



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

### axios之get请求

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

### axios之post请求

```javascript
// 要注意：post请求传参数，直接在第二个参数里写就行了
          axios.post('https://autumnfish.cn/api/user/reg',{
            username: this.username
          })
          .then( res => {
            console.log(res)
          })
```

### axios之config配制模式使用

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







## [拓展]服务器响应JSON格式数据

1. JSON是一种数据格式，本质是字符串。

2. JSON作用：解决跨平台问题。

`服务器一般返回json对象，不能直接使用需要转成js对象`

+ JSON 转 JS ； JSON.parse(json对象)
+ JS转JSON ： JSON.stringify(js对象)

```javascript
//1. json数组必须使用[]包起来
//json里面的内容必须要使用双引号包起来
let jsonArr = '["10","20","30"]';

//2. json对象必须要使用{}包起来
let jsonObj = '{"name":"张三","age":"18"}';
```





## 跨域

+ 什么是跨域
  + ajax请求地址 与 当前页面地址 不同源
  + 只有ajax请求才会有跨域的问题

+ 同源于不同源
  + 同源：协议名、ip地址、端口号 完全一致 (三码合一)
  + 不同源：ajax地址 与 页面地址，协议名 或  ip地址  或  端口号不一致

+ 为什么有同源策略

  + 如果你的ajax地址与页面地址不同源，浏览器则认为你是向两个服务器发送数据。可能会存在安全问题，所以进行限制。


### 如何解决跨域

#### CORS技术

  + 跨域资源共享（目前主流）【仅需后端操作】
  + 后台响应数据的时候设置一个跨域响应头，就可以了
  + axios不支持jsnop，请使用CORS(后端)进行跨域

```javascript
res.setHeader('Access-Control-Allow-Origin', '*');
```

#### jsonp技术

+ 利用 script 标签的 src 来发送请求（以前用的方法，浏览器漏洞）
+ 使用jsonp只能解决get请求的跨域，因为script标签中的src请求是get请求。
+ post请求的跨域,需要在服务器进行设置

```javascript
<script>
    function success(backData){
        console.log(backData);
        console.log(JSON.parse(backData));
    };
</script>
/*
用 script 标签发送接口，并在参数后面跟一个 callback
callback 是与后端约定的参数即可， success 是我们定义的函数
后端响应数据后 返回 success('数据') 给我们，浏览器就会执行这个函数
*/
<script src="http://127.0.0.1:3000/hero/info?id=3&callback=success"></script>

// nodejs 后端   res.end( req.query.callback( json数据 ) );
res.end(`${req.query.callback}('{"name":"寒冰"}')`);
```

#### jQuery的jsonp方法

```javascript
<script src="https://cdn.jsdelivr.net/npm/jquery@1.12.4/dist/jquery.min.js"></script>
<script>
    /* jq的jsonp底层原理
    (1)动态给head标签里面 新增一个script标签，利用script标签的src属性来发送请求
    (2)自动给jsonp请求添加一个额外的参数 ： 'callback=success'
    (3)服务器响应返回之后，jq又会自动的移除script标签
    */
    $.ajax({
        url:'http://127.0.0.1:3000/hero/info',
        type:'get',
        dataType:'jsonp',//type改成jsonp即可
        data:{
            id:3
        },
        success: function(backData){
            console.log(backData);

        }
    });
</script>
```

