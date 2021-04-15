#                                         Go Web 编程

## Go与web应用

### Chapter 1 Go与web应用

#### HTTP与HTML简述

HTML是一种标记语言，使用标记标签来描述网

`<!DOCTYPE html>`为HTML5的通用申明

http报文的请求行里的请求方法，GET,PUT,HEAD,POST,DELETE等。安全的请求方法除post,put,delete外都是，幂等的请求方法除post以外都是。

URI:统一资源标识符，URL：统一资源定位符。

URI一般格式：`<方案名称>:<分层部分>[ ? <查询参数>] [ # <片段>]`

#### web应用概念

web应用（稍微狭隘的定义）：1、这个程序必须向发送命令请求的客户端返回HTML，而客户端会向用户展示渲染后的HTML。

2、这个程序在向客户端传送数据时必须使用HTTP协议。

web应用的组成部分：处理器、模板引擎

Go语言要求可执行程序必须位于main包中，web应用也不例外。

### Chapter 2 Chitchat 论坛

#### 应用设计

请求使用这个格式：http://<服务器名><处理器名>?<参数>。

#### 数据模型

这里论坛包含User,Session,Thread,Post四种数据结构，将被映射到数据库里。

#### web应用的核心：请求的接受与处理

多路复用器（net/http包中默认由NewServeMux函数来创建）：1、其可以将针对根URL的请求重新定向到指定处理器中。（由handleFunc函数完成）2、还可以为静态文件提供服务。（handle函数完成）

处理器函数：为客户端生成HTML文件。其是一种接受ResponseWriter和Request指针作为参数的Go函数。

#### 使用模板生成HTML响应

Template模板中，ParseFiles函数对模板文件进行语法分析，Must函数会向用户返回相应的错误报告

#### 连接数据库

创建数据库表的setup.sql文件，要用psql工具。 代表数据库连接池的*sql.DB。

连接Thread结构和threads表，创建能够在结构体和数据库之间互动的函数就行了。

建立一个或多个数据结构（模型）来间接访问数据库

#### 起动服务器

## Web应用的基本组成部分

### Chapter03  接受请求

#### 使用Go构建服务器 

Go的net/http既有服务器功能又有客户端功能。

通过HTTPS提供服务，实际上就是将HTTP通信放在SSL之上，`server.ListenAndServerTLS("cert.pem","key.pem")`，其中第一个参数是SSL证书，第二个为服务器私钥。

#### 处理器和处理器函数

DefaultServeMux是一个特殊的处理器，它唯一要做的就是根据请求的URL将请求重定向到不同的处理器。

一个处理器就是一个拥有ServeHTTP方法的接口，拥有以下签名：`ServeHTTP(http.ResponseWriter, *http.Request)`，用http.Handle来将处理器绑定至URL的具体方法。

处理器函数与上面的ServeHTTP方法一样拥有相同的签名，用http.HandleFunc来转换以及绑定，实际上处理器函数就是创建处理器的一种便利方法。

日志记录、安全检查和错误处理这样的操作通常被称为横切关注点。

由于`ServeMux`无法使用变量实现URL模式匹配，所以可以使用其他多路复用器，比如：`HttpRouter`。

使用HTTP/2，使用这条命令`http2.ConfigureServer(&server, &http2.Server{})`就可以启动。

### Chapter04 处理请求

#### 请求与响应

1、Request结构主要由以下部分组成：

URL字段；Header字段；Body字段；Form字段、PostForm字段、MultipartForm字段。

2、请求URL

#### Go与HTML表单

用户在表单中输入的数据会以键值对的形式记录在请求的主体中。

1、Form字段

用户将URL、主体又或者以上两者记录的数据提取到Request结构的From、PostForm、MultipartForm字段里。

2、PostForm字段，只会包含键的表单值，且只支持application/x-www-form-urlencoded编码。

3、MultipartForm字段，支持multipart/form-data编码的表单数据

4、文件，可以用FormFile方法。

5、处理带有JSON主体的POST请求。

#### ResponseWriter

其拥有三个方法：Write、WriteHeader（设置HTTP响应的返回状态码）、Header（修改首部）。

#### Cookie

会话cookie、持久cookie.

没有设置Expires字段的cookie就是会话cookie，在浏览器关闭时就会自动被移除。

1、将cookie发送至浏览器，一是用Cookie结构自带的string方法，在用Set方法修改响应首部的“Set-Cookie”；二是用http.SetCookie()方法。

2、从浏览器获取cookie，三种方法：response.Header["cookies"];response.Cookie()；response.Cookies().

3、使用cookie实现闪现消息，用新cookie来代替旧cookie，新的具有瞬间消失性。



