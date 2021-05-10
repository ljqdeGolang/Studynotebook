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

### Chapter05  内容展示

#### 模板引擎

无逻辑模板引擎和嵌入逻辑的模板引擎。一般的模板引擎都介于两者之间，Go语言的模板引擎也是如此。

`{{ . }}`模板中的动作，模板引擎执行模板时，会用值去替换这个动作。`process()`函数负责触发模板引擎。

对模板进行语法分析：`ParseFiles()`函数，不论传入多少个模板，只会返回一个模板集合。执行模板：Execute()、ExecuteTemplate()

#### 动作

条件动作：

```html
{{ if  arg }}
	some content
{{ else }}
	other content
{{ end }}
```

迭代动作：可以对数组、切片、映射或者通道进行迭代，内部的{{ . }}被设为当前被迭代的元素

```
{{ range arrary}}
	Dot is set to the element {{ . }}
{{ else }}
	这里的内容将在.为nil时展示
{{ end }}
```

设置动作：允许用户在指定范围之内为（.）设置值。

```
{{ with arg }}
	Dot is set to arg
{{ else }}
	备选放案
{{ end }}
```

包含动作：允许用户在一个模板里面包含另一个模板，构建出嵌套模板。

```
{{ template "name" arg }} #arg 为想要传递给嵌套模板的数据
```

#### 参数、变量、管道

设置变量： $variable := value

管道（pipeline）:`{{ p1 | p2 | p3 }}`，允许用户将一个参数传递给下一个参数，各个参数之间用|分隔。

#### 函数

自定义模板函数：

- 创建为FucnMap的映射，键是函数名字，值是实际定义的函数。
- 将FuncMap与模板进行绑定。

```go1
template.FucnMap{}
template.New().Funcs()
```

#### 上下文感知

主要用于实现自动的防御编程，比如防御XSS（跨站脚本攻击）。

#### 嵌套模板

布局(layout)：web设计可以重复运用到多个页面的固定模式。

```
{{ define "name" }}

{{ end }}
```

通过块动作定义默认模板：允许用户定义一个模板立即进行使用

```
{{ block arg }}
	Dot is set to arg
{{ end }}
```

### Chapter06 存储数据

#### 内存存储

这里说的是指将数据存储在运行的应用中，并在运行的过程中使用这些数据。对于Go语言，常使用数组、切片、映射和结构来存储数据。

#### 文件存储

读取和写入CSV文件：官方库(encoding/csv)

```go
csv.NewWriter()
csv.Flush()
csv.NewReader()
```

encoding/gob包：用于管理由gob组成的流(stream)，这是一种在编码器与解码器之间进行交换的二进制数据。

```go
gob.NewEncoder()
gob.NewDecoder()
```

#### Go与SQL

数据库服务器，可以让其他程序通过客户端-服务器模型来访问数据。

客户端与数据库服务器进行连接，通过SQL对关系型数据库进行访问。

```go
//创建数据库用户
createuser -P -d name
//为用户创建数据库
createdb name 
//运行安装脚本，创建执行相关操作所需的表
psql -U username -f setup.sql -d dbname
```

连接数据库

```go
*sql.DB
sql.Open("数据库驱动名字","数据源名字")
//数据库驱动的导入
_ "github.com/lib/pq"
```

创建帖子

```go
// Create a new post
func (post *Post) Create() (err error) {
	statement := "insert into posts (content, author) values ($1, $2) returning id"
    //创建预处理语句
	stmt, err := Db.Prepare(statement)
	if err != nil {
		return
	}
	defer stmt.Close()
    //只返回一个row，Scan用于将值复制给程序提供的参数里面
	err = stmt.QueryRow(post.Content, post.Author).Scan(&post.Id)
	return
}
```

获取帖子

```go
// Get a single post
func GetPost(id int) (post Post, err error) {
	post = Post{}
	err = Db.QueryRow("select id, content, author from posts where id = $1", id).Scan(&post.Id, &post.Content, &post.Author)
	return
}
```

更新帖子

```go
// Update a post
func (post *Post) Update() (err error) {
	_, err = Db.Exec("update posts set content = $2, author = $3 where id = $1", post.Id, post.Content, post.Author)
	return
}
```

删除帖子

```go
// Delete a post
func (post *Post) Delete() (err error) {
	_, err = Db.Exec("delete from posts where id = $1", post.Id)
	return
}
```

#### Go与SQL的关系

将一项记录与其他记录关联起来：一对一关联、一对多关联、多对一关联、多对多关联

设置数据库

```sql
drop table posts cascade;
drop table comments;

create table posts (
  id      serial primary key,
  content text,
  author  varchar(255)
);

create table comments (
  id      serial primary key,
  content text,
  author  varchar(255),
  post_id integer references posts(id)
);
```

#### Go与关系映射器

Sqlx：能够很好地兼容database/sql

Gorm

## 实战演练

### Chapter 07 Go web 服务

#### web 服务简介

web服务就是一个向其他软件程序提供服务的程序。

#### 基于SOAP的web服务简介

SOAP是一种协议，用于交换XML里面的结构化数据。

#### 基于REST的web服务简介

将动作转换为资源、将动作转换为资源的属性

#### 通过Go分析和创建XML

分析XML：创建用于存储XML数据的结构、使用encoding/xml里的xml.Unmarshal将XML数据解封到结构里。

```go
//结构标签
`xml:"post"`
//处理体积较小的XML
type Post struct {
	XMLName xml.Name `xml:"post"`
	Id      string   `xml:"id,attr"`
	Content string   `xml:"content"`
	Author  Author   `xml:"author"`
	Xml     string   `xml:",innerxml"`
}

type Author struct {
	Id   string `xml:"id,attr"`
	Name string `xml:",chardata"`
}

func main() {
	xmlFile, err := os.Open("post.xml")
	if err != nil {
		fmt.Println("Error opening XML file:", err)
		return
	}
	defer xmlFile.Close()
	xmlData, err := ioutil.ReadAll(xmlFile)
	if err != nil {
		fmt.Println("Error reading XML data:", err)
		return
	}

	var post Post
	xml.Unmarshal(xmlData, &post)
	fmt.Println(post)
}
//手动解码XML，流式传输的XML文件
func main() {
	xmlFile, err := os.Open("post.xml")
	if err != nil {
		fmt.Println("Error opening XML file:", err)
		return
	}
	defer xmlFile.Close()

	decoder := xml.NewDecoder(xmlFile)
	for {
		t, err := decoder.Token()
		if err == io.EOF {
			break
		}
		if err != nil {
			fmt.Println("Error decoding XML into tokens:", err)
			return
		}

		switch se := t.(type) {
		case xml.StartElement:
			if se.Name.Local == "comment" {
				var comment Comment
				decoder.DecodeElement(&comment, &se)
				fmt.Println(comment)
			}
		}
	}
}
```

#### 通过Go分析和创建JSON

分析JSON：Unmarshal()和Decoder()

创建JSON：Marshal()和Encoder()

#### 创建Go Web服务



### Chapter 08 应用测试

#### 使用Go进行单元测试

测试文件与被测试源代码文件位于同一个包内

`go test -v -cover`，可以获知测试用例对代码的覆盖率。

函数`t.Skip()`将跳过测试用例，命令`go test -v -short`将跳过长时间测试用例

以并行的方式运行测试：函数`t.Parallel()`，命令`go test -v -parallel 3`：并行执行三个测试用例。

基准测试：函数`func Benchmarkxxx(b *testing.B)`，命令`go test -v bench .`：运行所有基准测试

`-run`可用来忽略功能测试`func Testxxx(t *testing.T)`

#### 使用Go进行HTTP测试

用到的包：`testing/httptest`

```go
//预设与拆卸
func TestMain(m *testing.M) {
	setUp()
	code := m.Run()
	tearDown()
	os.Exit(code)
}
```

#### 测试替身与依赖注入

测试替身是一种让单元测试用例变为更为独立的的常用手法。

依赖注入是实现测试替身的一种设计方法。

#### 第三方Go测试库

Gocheck：导入包`gopkg.in/check.v1`

`Check()、Asset()`可以验证结果的值

Ginkgo：BDD(行为驱动开发)风格的测试框架

### Chapter 09 发挥Go的并发优势

并发：是同一时间内处理多个任务，并行：多个任务同时启动并执行。

#### goroutine

goroutine并不是线程，它只是对线程的多路复用。

等待goroutine：使用`sync`包等待组(WaitGroup)机制

- 声明一个等待组
- 使用Add方法为等待组的计数器设置值
- 当一个goroutine完成他的工作时，使用Done方法对等待组的计数器执行减一操作
- 调用Wait方法，该方法将一直阻塞，知道等待组计数器变为0

#### 通道

使用通道在不同的goroutine中通信。

```go
//声明创建
ch := make(chan int)
//将1放入通道
ch <- 1
//从通道中取值
i := <- ch
//只能发送的通道
ch := make(chan <- string)
//只能接受的通道
//只能接受说明一旦接受了值后，它就变成了这个类型的值了？可以这样理解吗？
ch := make(<-chan string)
```

通过通道实现同步

```go
package main

import "fmt"
import "time"

func printNumbers(w chan bool) {
	for i := 0; i < 10; i++ {
		time.Sleep(1 * time.Microsecond)
		fmt.Printf("%d ", i)
	}
	w <- true
}

func printLetters(w chan bool) {
	for i := 'A'; i < 'A'+10; i++ {
		time.Sleep(1 * time.Microsecond)
		fmt.Printf("%c ", i)
	}
	w <- true
}

func main() {
	w1, w2 := make(chan bool), make(chan bool)
	go printNumbers(w1)
	go printLetters(w2)
	<-w1
	<-w2
}
```

通过通道实现消息传递

从多个通道中选择：`select`关键字

`close`关闭通道并不是必须的，并不会导致通道的技能完全停止，通常用来告知接受者该通道将不会再接受任何值。尝试向一个已关闭的通道发送信息将会引发一个panic，尝试关闭一个已经关闭的通道也是如此，尝试从一个已关闭的通道中取值总是会得到有个与同道类型相对应的零值，因此从已关闭的通道取值并不会导致`goroutine`被阻塞。

#### 在web应用中使用并发

举例：对图片进行马赛克处理的web应用，这是整体对图片进行扫描和载入瓷砖图片，效率较低。

采用并发版马赛克图片生成web应用，将图片四等分，同时对四张字图片进行处理，将处理完的图片合成一张即可。

为了避免并发程序中，对共享资源同时进行修改访问引起的竞争条件，于是引入了互斥`mutex`

### Go的部署

web应用是部署在服务器上，，然后通过终端用户设备上的浏览器等客户端对其进行访问，而

#### 将应用部署到独立的服务器

`IaaS `：基础设施即服务，供应商向用户提供包括计算、存储以及网络在内的基本计算能力。

`PaaS`：平台即服务，供应商会让用户用过他们提供的工具，将应用部署到云端的基础设施上

`Saas`：软件即服务，供应商会向用户提供应用服务。

通过init守护进程管理web服务，如使用`Upstart`来持续地运行Web服务。

#### 将应用部署到Heroku

其定义的应用：就是由其支持的某一种编程语言编写的一系列源码，以及与这些源代码想关联的依赖关系。

- 修改代码从环境变量中获得端口号，`os.Getenv("PORT")`
- 使用godep引入依赖关系，`godep save`命令
- 定义`Procfile`文件`web： ws-h`

#### 将应用部署到Google App Engine

实际上，用户编写的将不再是一个独立的应用，而是部署在GAE上的包。

需要下载Google为我们提供的SDK工具

- 修改代码，使用Google提供的库
- 创建app.yaml文件
- 创建GAE应用
- 将应用代码推送到GAE平台

#### 将应用部署到Docker

Docker就是在容器中构建、发布和运行应用的一个开放平台。实质上就是一种管理容器的软件。

容器和虚拟机得不同之处在于，虚拟机模拟的是包括操作系统在内的整个计算机系统，而容器只提供操作系统级别的虚拟化，并将计算机资源划分给多个独立的用户空间实例使用。容器对资源的需求比虚拟机少的多。

Docker的概念与组件

- Docker守护进程
- Docker容器
- Docker镜像

Docker化一个Go Web 应用：

不需要对代码进行任何修改，只要使用Docker并进行相应的配置就好了。

- 创建Dockerfile文件

  ```dockerfile
  # Start from a Debian image with the latest version of Go installed
  # and a workspace (GOPATH) configured at /go.
  FROM golang
  
  # Copy the local package files to the container's workspace.
  ADD . /go/src/github.com/sausheong/ws-d
  
  WORKDIR /go/src/github.com/sausheong/ws-d
  
  # Build the ws-d command inside the container.
  # (You may fetch or manage dependencies here,
  # either manually or with a tool like "godep".)
  RUN go get github.com/lib/pq
  RUN go install github.com/sausheong/ws-d
  
  # Run the ws-d command by default when the container starts.
  ENTRYPOINT /go/bin/ws-d
  
  # Document that the service listens on port 8080.
  EXPOSE 8080
  ```

- 使用Dockerfile构建Docker镜像，`docker build -t ws-d`
- 根据Docker镜像创建出Docker容器，`docker run --publish 80:8080 --name simple_web_service --rm ws-d`
- 在云端创建Docker宿主
- 连接远程Docker宿主
- 在远程宿主中构建Docker镜像
- 在远程宿主中启动Doecker容器

Docker机器是一个命令行接口，它允许用户在本地以及云端创建公开或者私有的Docker宿主。

云端上创建宿主，最为一种轻松的办法：使用Digital Ocean。

`docker-machine create --driver digitalocean-access-token <tokenwsd`