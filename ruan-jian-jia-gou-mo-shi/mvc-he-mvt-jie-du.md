# MVC和MVT

## MVC解读

* M：Model，模型，和数据库交互
* V：View，视图，负责产生HTML页面
* C：Controller，控制器，接受请求，进行处理，与M和V进行交互，返回应答。

## MVC示意图

![MVC&#x793A;&#x610F;&#x56FE;](../.gitbook/assets/image%20%2829%29.png)

1. 用户点击注册按钮，将要注册的信息发送给网站服务器
2. Controller控制器接收到用户的注册信息，Controller会告诉Model层将用户的注册信息保存到数据库
3. Model层将用户的注册信息保存到数据库
4. 数据保存之后将保存的结果返回给Model模型
5. Model层将保存的结果返回给Controller控制器
6. Controller控制器收到保存的结果之后告诉View视图，View视图产生一个html页面
7. View将产生的HTML页面的内容给了Controller控制器
8. Controller将HTML页面的内容返回给浏览器
9. 浏览器接收到服务器Controller返回的HTML页面进行解析展示

## MVT解读

* M：Model，模型，和MVC的M功能相同，和数据库进行交互
* V：View，视图，和MVC的C功能相同，接收请求，进行处理，与M和T进行交互，返回应答
* T：Template，模板，和MVC的V功能相同，产生HTML页面

## MVT示意图

![MVT&#x793A;&#x610F;&#x56FE;](../.gitbook/assets/image%20%2826%29.png)

1. 用户点击注册按钮，将要注册的内容发送给网站服务器
2. View视图，接收到用户的注册数据，View告诉Model将用户的注册信息保存到数据库
3. Model层将用户的注册数据保存到数据库中
4. 数据库将保存的结果返回给Model
5. Model将保存的结果给View视图
6. View视图告诉Template模板去产生一个HTML页面
7. Template生成HTML页面内容返回给View视图
8. View将HTML页面内容返回给浏览器
9. 浏览器拿到View返回的HTML页面内容进行解析和展示

