## HTML学习笔记

### 标签

`<html></html>`定义html文件

`<body></body>` 定义文档主体

`<h1></h1>` 标题标签

`<p></p>`段落标签

`<a href="">`链接标签、href是属性

`<img loading= src= width= height= >`图片标签、名称与尺寸属性

`<br/>`换行标签，/是结束标签可不写

`<hr>`水平线标签

`<!-- 这是一个注释 -->`注释写法

| 标签         | 描述                               |
| :----------- | :--------------------------------- |
| `<b>`        | 定义粗体文本                       |
| `<em>`       | 定义着重文字                       |
| `<i>`        | 定义斜体字                         |
| `<small>`    | 定义小号字                         |
| `<strong>`   | 定义加重语气                       |
| `<sub>`      | 定义下标字                         |
| `<sup>`      | 定义上标字                         |
| `<ins>`      | 定义插入字                         |
| `<del>`      | 定义删除字                         |
| `<code>`     | 定义计算机代码                     |
| `<var>`      | 定义变量                           |
| `<pre>`      | 定义预格式文本                     |
| `<abbr>`     | 定义缩写                           |
| `<bdo>`      | 定义文字方向                       |
| `<table>`    | 定义表格                           |
| `<th>`       | 定义表格的表头                     |
| `<tr>`       | 定义表格的行                       |
| `<td>`       | 定义表格的单元                     |
| `<div>`      | 定义了文档区域，块级               |
| `<span>`     | 用来组合文档中的行内元素，内联元素 |
| `<script>`   | 定义了客户端脚本                   |
| `<noscript>` | 定义了不支持脚本浏览器输出的文本   |



### 组成部分

####     元素

- HTML 元素以**开始标签**起始
- HTML 元素以**结束标签**终止
- **元素的内容**是开始标签与结束标签之间的内容
- 某些 HTML 元素具有**空内容（empty content）**
- 空元素**在开始标签中进行关闭**（以开始标签的结束而结束）
- 大多数 HTML 元素可拥有**属性**

####    属性

- HTML 元素可以设置**属性**
- 属性可以在元素中添加**附加信息**
- 属性一般描述于**开始标签**
- 属性总是以名称/值对的形式出现，**比如：name="value"**。

####   链接

HTML 使用超级链接与网络上的另一个文档相连。几乎可以在所有的网页中找到链接。点击链接可以从一张页面跳转到另一张页面。

`<a href="url">链接文本</a>` *链接文本"* 不必一定是文本。图片或其他 HTML 元素都可以成为链接

target 属性，定义被链接的文档在何处显示 `<a href="http://www.runoob.com/" target="_blank">访问菜鸟教程!</a>`在空白处显示

id 属性，可用于创建在一个HTML文档书签标记。书签是不以任何特殊的方式显示，在HTML文档中是不显示的，所以对于读者来说是隐藏的。

在HTML文档中插入ID:

`<a id="tips">有用的提示部分</a>`

在HTML文档中创建一个链接到"有用的提示部分(id="tips"）"：

`<a href="#tips">访问有用的提示部分</a>`

或者，从另一个页面创建一个链接到"有用的提示部分(id="tips"）"：

`<a href="https://www.runoob.com/html/html-links.html#tips">访问有用的提示部分</a>`

```html
<a rel="noopener noreferrer">意思是不会打开其他的网站，因为恶意病毒可能会修改你的浏览器空白页地址。
```

####   头部

```html
<title> 标签定义了不同文档的标题。

<title> 在 HTML/XHTML 文档中是必须的。

<title> 元素:
    - 定义了浏览器工具栏的标题
    - 当网页添加到收藏夹时，显示在收藏夹中的标题
    - 显示在搜索引擎结果页面的标题
<base> 标签描述了基本的链接地址/链接目标，该标签作为HTML文档中所有的链接标签的默认链接
<link> 标签定义了文档与外部资源之间的关系。
<style>定义了HTML文档的样式文件引用地址
<meta> 描述了一些基本的元数据
```

####   图像

src源属性：图像的url地址

Alt属性用来为图像定义一串预备的可替换的文本。

height（高度） 与 width（宽度）属性用于设置图像的高度与宽度。

####   列表

无序列表

```html
<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>
```

有序列表

```html
<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>
```

自定义列表

```html
<dl>
<dt>Coffee</dt>
<dd>- black hot drink</dd>
<dt>Milk</dt>
<dd>- white cold drink</dd>
</dl>
```

####   表单

标签`<form>input元素</form>`

输入元素：文本域、密码字段、单选按钮、复选框、提交按钮

####   框架

`<iframe src='url'></iframe>`，用height、width来定义iframe标签的高度与宽度

####   脚本

JavaScript 使 HTML 页面具有更强的动态和交互性。

####   字符实体

&*entity_name*;或&#*entity_number*;替换预留字符或键盘上没有的字符

### 补充知识点

1、***.html** 文件跟 ***.jpg** 文件(f盘)在不同目录下：

```html
<img src="file:///f:/*.jpg" width="300" height="120"/>
```

2、***.html** 文件跟 ***.jpg** 图片在相同目录下：

```html
<img src="*.jpg" width="300" height="120"/>
```

3、***.html** 文件跟 ***.jpg** 图片在不同目录下：

a、图片 ***.jpg** 在 **image** 文件夹中，*.html 跟 **image** 在同一目录下：

```html
<img src="image/*.jpg/"width="300" height="120"/>
```

b、图片 ***.jpg** 在 **image** 文件夹中，***.html** 在 **connage** 文件夹中，**image** 跟 **connage** 在同一目录下：

```html
<img src="../image/*.jpg/"width="300" height="120"/>
```

4、如果图片来源于网络，那么写绝对路径：

```html
<img src="http://static.runoob.com/images/runoob-logo.png" width="300" height="120"/>
```

5、src与href的区别：src 用于替换当前元素；href 用于在当前文档和引用资源之间建立联系。前者是引入，后者是引用。

6、1到6号标题与1到6号字体逆序对应，比如1号字体对应6号标题，2号字体对应5号标题。

```html
<h1>这是1号标题</h1>
<font size="6">这是6号字体文本</font>
```

7、内容中的单词之间的空格用%20代替