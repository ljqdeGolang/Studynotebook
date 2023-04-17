### Django/DjangoRestFramework

 1、API接口：应用程序编程接口，提供给用户操作数据的入口。

2、RPC规范：远程服务调用。

- 服务端提供唯一的访问入口地址，所有操作都理解为动作。通过请求体参数指定调用的接口名称和接口所需参数。

- 数据格式：protopuf、json、xml

3、restful:资源状态转换（表征性状态转移）。

- 把所有数据及资源看做资源，都是对资源的操作。必须把资源的名称写在URL上。

![image-20230311112053871](figure/image-20230311112053871.png)

![image-20230311113500323](figure/image-20230311113500323.png)

![image-20230311113635942](figure/image-20230311113635942.png)

4、JSON:javascript Object Notation,JS对象表示法

![image-20230311162006909](figure/image-20230311162006909.png)

![image-20230311162323308](figure/image-20230311162323308.png)

5、序列化

把数据转换格式，常见的：json/pickle/base63/struct

序列化与反序列化

将前端的数据存进数据库前等为反序列化，将数据库数据转换成前端可接受的数据格式为序列化。

![image-20230315173959386](figure/image-20230315173959386.png)

针对模型创建一个序列化类![image-20230313212218534](figure/image-20230313212218534.png)

再进行序列化

![image-20230313212109750](figure/image-20230313212109750.png)

反序列化，需要校验数据是否合规。

![image-20230313211803040](figure/image-20230313211803040.png)

为避免分发机制字段重复，于是需要新建一个视图类

![image-20230317173404877](figure/image-20230317173404877.png)

![image-20230317174345447](figure/image-20230317174345447.png)

put

![image-20230314203527376](figure/image-20230314203527376.png)



6、drf的目的 

![image-20230311165731184](figure/image-20230311165731184.png)

7、FBV（函数型视图）、CBV（类视图）

8、反射：将传入的字符串变成函数调用。

```python
func = getattr(self, func_str)
func()
setattr(self, method, func)
```

实例化：self=cls (**initkwargs)

属性方法：@property，不用括号调用

9、视图

![image-20230315170305297](figure/image-20230315170305297.png)

10、genericAPIView

![image-20230315154737854](figure/image-20230315154737854.png)

![image-20230315154504736](figure/image-20230315154504736.png)

![image-20230315154636490](figure/image-20230315154636490.png)

11、mixin混合类

![image-20230315164158253](figure/image-20230315164158253.png)

![image-20230315164232995](figure/image-20230315164232995.png)

再封装一下

from restframework import listcreateAPIView

![image-20230315164532959](figure/image-20230315164532959.png)

12、ViewSet

重新构建了分发机制

![image-20230315170011288](figure/image-20230315170011288.png)

![image-20230315165952112](figure/image-20230315165952112.png)

13、ModelViewSet

![image-20230315173515381](figure/image-20230315173515381.png)

![image-20230315173707621](figure/image-20230315173707621.png)

两张图片功能相同。

14、path组件

之前

![image-20230316154250590](figure/image-20230316154250590.png)

![image-20230315174420624](figure/image-20230315174420624.png)

![image-20230315174450408](figure/image-20230315174450408.png)