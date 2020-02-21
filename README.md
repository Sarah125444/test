###Cookie

保留无状态协议又要解决给客户端保留登录状态的功能就使用cookie这个功能

Cookie技术就是通过在请求和响应报文中写入Cookie信息来控制客户端的状态

Cookie 会根据从

==服务端发送的响应报文==

内的一个叫做Set-cookie的首部字段信息

==通知客户端保存Cookie==

当下次客户端再往该服务器发送请求的时候，

客户端会自动在请求报文中加入Cookie值发出去

服务端发现客户端发送过来的cookie后，回去检查究竟是从哪一个客户端发送过来的连接请求，

然后对比服务器的记录，最后得到之前的状态信息

####Cookie

```js
Cookie: status=enable 状态=已授权

Cookie 会告知 `服务器`，如果客户端想要获的HTTP状态管理这项业务，

那么客户端会在发送请求报告的时候，在里面加上 服务器之前给的cookie
```



如果接收到了多个cookie,可以以多个cookie的形式发送

管理服务器与客户端之前**状态**的cookie

==工作机制是用户识别和状态管理==

web网站为了管理用户的状态会通过**web浏览器**，把一些数据临时的写入**用户的计算机**内

```js
						   将只有两个人之间的数据发给用户计算机
web网站 =================================================================>  用户的计算机
```



当用户访问该web网站的时候，可通过通信方式取回之前存放的cookie

```js
					    	问浏览器取回之前存放的cookie
用户的计算机 ===================== =====================>  web网站
```

调用cookie的时候，可以验证Cookie的有效期，以及发送方的域，路径，协议等信息，

```js
用户计算机 和 web网站 两个的cookie进行对比， 看下是否一致，如果一致，才允许取回
```

所以正规发布的Cookie内的数据不会因为来自其他web站点和攻击者的攻击而泄露



为cookie服务的首部字段

Set-Cookie: 开始状态管理使用的Cookie信息

Cookie: 服务器收到的cookie信息

- Set-Cookie 

  ```js
  Set-Cookie: status=enable; expires=Tue, 05 Jul 2011 07:26:31 GMT; => path=/; 
  domain=.hackr.jp;
  
  Cookie:状态信息存储
  enable:授予权利或方法
  expires：有效期
  domain：域名
  
  
  ```

  

当服务器准备开始管理客户端的状态时，会事先告知各种信息

NAME=VALUE 赋予Cookie的名称和其值 ==必须填写==

```js
在数据库或者服务端进行设置的东西，这个就是 name = value  userid=1
res.cookie('userid', user.id)
// Set-Cookie: userid=1; Path=/
```

#### expires属性



expires=DATE Cookie的有效期（如果不明确指定则默认为浏览器关闭前为止）==一般不用设置==

- Expires指定浏览器可发送Cookie的有效期
- 省略expires属性的时候，有效期是仅仅在浏览器会话（Session）时间段内
- 通常限于浏览器应用程序被关之前

```js
expires本身就是有效期的意思，有效期后面是日期
expires=Tue, 05 Jul 2011 07:26:31 GMT
```

一旦   ==Cookie 这个东西从服务器端发送至客户端，服务器端就不存在显式的删除Cookie的方法==

```js
意思是一旦Cookie这个里面放着各种信息的信封一发出，那么发出的人就再也没办法拦截处理这个信封了
```

挽救办法是：

==cookie是有expires（有效期）的==，只要有效期一过，那么收件人手中的cookie信息就没有效果了

此时，服务器端会生成新的cookie，那么以前旧的cookie会被弃用覆盖



#### path (没啥用)

path=PATH  将服务器上的文件目录作为Cookie的适用对象(若不指定则默认为文档所在的文件目录) 

```js
Path=/ 这个的意思是用 当前文档的目录来 作为Path参数
```





#### domain（不要指定）

domain=域名 作为cookie的适用对象的域名 （若不指定一般都是 创建cookie的服务器的域名 => localhost）

```js
domain本身就是域名的意思 domain=.hackr.jp
```

通过这个属性设置的domian, 可以做到与结尾保持一致

```js
当设置 example.com 域名之后， 除了 example.com 的域名，
www.example.com
www1.example.com
www3.example.com
等这些只要结尾一致的域名等都可以发送cookie
```

==其实不指定domain属性更安全==



#### Secure（安全设置）

Secure  仅在HTTPS安全通信的时候才会发送Cookie

用于限制web网站 仅在 HTTPS 安全连接的时候，才可以发送Cookie

```js
Set-Cookie：name=value; secure

https://www.example.com/
http: //www.example.com/  
只有在https的时候 web网站才会给客户端发送cookie,客户端才会给web网站返回cookie
```

如果省略就任何时候都会发送和返回cookie



#### HttpOnly(重要)

HttpOnly 加以限制，使Cookie不能被javascript脚本访问

作用是让Javascript脚本无法获取Cookie

目的是为了防止跨站脚本攻击（Cross-site-scripting, XSS) 对cookie信息的窃取

```js
Set-cookie: name=value; HtppOnly
这个设置之后，通常在 web页面还可以对Cookie进行读取操作
但是使用javascript的ducument.cookie 就无法读取 设置了这个属性后的cookie的内容了
因此，也就无法在XSS中利用javascript劫持cookie
```



#### 浏览器的cookie中心

![浏览器的cookie](/Users/fuzhiqun/Desktop/扫码点餐/扫码点餐文档/浏览器的cookie.png)

- Storage:贮藏所

  存储各种东西的地方

- Local storage:本地存储  

  只要你不主动去浏览器手动清空缓存，那么这个地方的东西就会一直在这里

- Session storage:对话期存储

  每当清空浏览器，都会自动清空

  就跟你去超市存了东西，存东西的时间是在你在超市的事件，离开超市就会把东西取出来，柜子就清空了

- Cookies:**信息记录程序**-状态管理

  帮助客户端保存状态
