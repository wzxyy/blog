##  http原理了解

1. http网络协议原理 : 约定 客户端(浏览器) 与 服务端(后台) 数据交换格式。

   + 客户端数据必须是  请求报文
     + 请求行 ： 请求方法和地址

     + 请求头 ： 数据格式（浏览器告诉服务器，我发你的是什么数据）

     + 请求体 ： 请求参数

   + 服务端数据必须是  响应报文

     + 响应行 ： 请求状态码
       + 200 ： 成功 
       + 400/3/4 ： 失败 (浏览器的锅)
       + 500 ： 失败 （服务器内部错误）

     + 响应头 ： 数据格式（服务器告诉浏览器，我给你的数据格式）

     + 响应体 ： 服务器响应数据

