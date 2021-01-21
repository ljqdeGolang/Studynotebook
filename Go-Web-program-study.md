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