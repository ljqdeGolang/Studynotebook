#                       Go Web 编程

## HTTP与HTML简述

HTML是一种标记语言，使用标记标签来描述网

`<!DOCTYPE html>`为HTML5的通用申明

http报文的请求行里的请求方法，GET,PUT,HEAD,POST,DELETE等。安全的请求方法除post,put,delete外都是，幂等的请求方法除post以外都是。

URI:统一资源标识符，URL：统一资源定位符。

URI一般格式：`<方案名称>:<分层部分>[ ? <查询参数>] [ # <片段>]`

## Go与web应用

web应用（稍微狭隘的定义）：1、这个程序必须向发送命令请求的客户端返回HTML，而客户端会向用户展示渲染后的HTML。

2、这个程序在向客户端传送数据时必须使用HTTP协议。

web应用的组成部分：处理器、模板引擎

Go语言要求可执行程序必须位于main包中，web应用也不例外。

### web应用的核心：请求的接受与处理

多路复用器（net/http包中默认由NewServeMux函数来创建）：1、其可以将针对根URL的请求重新定向到处理器函数中。（由handleFunc函数完成）2、还可以为静态文件提供服务。（handle函数完成）

处理器函数：为客户端生成HTML文件。其是一种接受ResponseWriter和Request指针作为参数的Go函数。

### 使用模板生成HTML响应

Template模板中，ParseFiles函数对模板文件进行语法分析，Must函数会向用户返回相应的错误报告

### 连接数据库

创建数据库表的setup.sql文件，要用psql工具。 代表数据库连接池的sql.DB。

建立一个或多个数据结构（模型）来间接访问数据库

## Go工具处理请求

Request结构主要由以下部分组成：

URL字段；Header字段；Body字段；Form字段、PostForm字段、MultipartForm字段