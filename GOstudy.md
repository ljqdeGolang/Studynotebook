## GO语言视频学习笔记

- **区块链介绍**

python解决了机器学习的问题，区块链解决了机器与机器之间的信任问题。

人工智能解决解放生产力问题，区块链解决生产关系问题。

- **基础**

  声明:没有初始化值，定义：进行了初始化值。

  打印：print 没有换行符，`println`有换行符，`printf`格式化输出（用占位符）：`%3d`,格式化三位输出，不足补空格（int类型），`%.3f`，保留三位小数（浮点型）`%t:bool`类型 `  %c: byte`类型  ` %s: string`类型  ` %p`: 内存地址占位符   `%T`打印对应的数据类型   ` %b:`打印二进制。

  输入：Scan 示例：`fmt.Scan(&a)  ，Scanf。`

  字符串中存贮了字符串结束标志"\0"、“\ \”表示只有一个斜杠，常用于文件操作

  并发：分布式服务器、语言层面并发。

  常量：存在内存的数据区，而变量存在栈区。

  类型转换：习惯将低类型转为高类型。

  switch不能用浮点类型。

  如果函数的参数为不定参`func add(a ...int)`，传递参数方式a[0:]...   用len获得不定参的长度。

  函数可以返回多个返回值，函数名本身就是一个指针类型数据，在内存中代码区进行储存。函数中的的信息在栈区储存，用完就销毁。

  函数类型`  type  name  func(int,int) int `  其实就是一个指针类型。

  匿名内部函数：不能调用只能内部使用。

  全局变量、常量存储在内存的数据区，局部变量储存在内存的栈区。

  匿名函数 `func (a int) int {}(30)`  括号加实参可以调用这个函数了，也可以将其赋值给其他变量，如： f:=`func(a int) int { },`    f(30)成立。

  闭包：通过匿名函数和闭包可以实现栈区的本地化。

  工程管理：`src`：源码文件夹  `pkg`: “.a”的归档文件。`bin`：`go install`安装形成的可执行文件

  数组：指定数组下标进行初始化值，var array [10]int{6:10}，第七个元素为10。

  随机数：导入头文件：math/rand time；添加随机数种子；使用随机数

  `rand.Intn() 	rand.Seed(time.Now().Unixnano()) `。

  切片存在内存的堆区，当用append扩充时，可能会导致地址发生变化。copy拷贝切片后，会形成两个独立的空间，不会相互影响。

  map在定义时，key必须是唯一的，map在函数中作为参数是引用传递(地址传递)

  go通过匿名字段（即：结构体）来表现继承的思想。

  方法：`func (s stu)add(){} `只要是满足`stu`类都可以调用此方法。 子类结构体继承父类结构体，允许使用父类结构体成员，允许使用父类的方法。父类不能继承子类信息 。

  接口定义了规则，方法实现了规则。接口是虚的，方法是实的。

  多态实现了接口的统一处理，就是将接口类型作为函数参数，如：`func UseDev(use USBer){} `其中`USBer`是一个接口。

  接口赋值需要赋地址，不能直接赋值。接口中的方法必须有具体的实现，否则不能将只实现某一个方法的对象赋值给接口变量。且接口里的方法只能是一种对象（继承父类的对象也可以）实现的方法。

  超集：继承子集接口的接口，超集可以转换为子集。

  设计模式：对于面向对象基于MVC，有26种。

  `recover `与`defer`要联合起来使用，`recover`返回值为接口类型，`recover`的使用一定要在错误之前调用，其原理是从panic手上夺得控制权，后面的程序就不会运行了。

  `os`包是文件创建的包，`\`在go语言中被认为是转义字符，所以反斜杠用`\\`,也可以用正斜杠`/`来作为文件目录。创建文件例子：

  ```go
  //创建文件，文件不存在时，建个新的，存在时，会覆盖原内容 
  fp,err :=os.Create(
      "D:/edgexiazai/a.txt")
  if err !=nil {
  	fmt.Println("文件创建失败")
  	return
  }
  defer fp.Close()
  //写入文件
  fp.Weitestring("mike")
  fp.Write(b []byte)
  fp.WriteAt(buf []byte,n)//指定位置插入内容
  //打开文件
  fp.Open()
  fp.OpenFile(文件名 string，打开模式 os.0_RDWR,打开权限 int)
  //获取文件字符个数
  fp.Seek()
  //读文件
  fp.Read()
  fp.ReadAt()
  r:=bufio.NewReader(fp)
  b,_:=r.ReadBytes('\n')// 创建缓冲区，在读取内容。\n位置为分隔符 //EOF 值为-1 文件结束标志
  //循环读取文件
  for {
      n,err :=fp.Read(b)
      if err ==io.EOF { break }
      fmt.Print(string(b[:n]))
  }
  //第二种
  r:=bufio.NewReader(fp)
  for {
      b,err:=r.ReaderBytes('\n')
      fmt.Println(string(b))
      if err ==io.EOF {
          break
      }
  }
  ```

  strings包：`contains`判断是否包含某个字符串；`Join`讲一个字符串切片拼接成一个字符串；`index`查找另一个字符是否存在，返回下标；`Repeat`将一个字符串重复N次；`Replace`替换字符；`Spilt`切分字符串；`Trim`去掉字符串前后指定内容；`Fields`去掉字符串中的空格，并返回有效数据切片；

  `strconv`包：字符串类型转换：有`FormatBool`、`FormatInt`、`Itoa`、`FormatFloat(数据，'f',保留小数位置;位数)`、`PraseBool`、`Atoi`

  将数据转换为字符添加给字符串：`AppendBool、AppendInt`  

- **`GTK`窗口程序开发：**

  第一梯队：`MFC(opengl等)、C#、QT`  第二梯队：`GTK、GUI`

- **进阶：**

  **指针**：

  ```go
  var *p int，*p=100,这里的*p又称为解引用。
  p=new(int)在堆上开辟新的空间.
  空指针：未被初始化的指针。	var p *int		*p --> err
  野指针：被一片无效的地址空间初始化。
  ```

  **内存布局**

  ![image-20210304111315529](C:\Users\LJQ\AppData\Roaming\Typora\typora-user-images\image-20210304111315529.png)

  栈帧：用来给函数运行提供内存空间，取内存于stack（栈 ）上，当函数调用时，产生栈帧，函数调用结束，释放栈帧。栈帧存储：1、局部变量 2、形参 3、内存字段描述值（储存栈顶、栈底指针值）

  变量存储：等号 左边的变量，代表 变量所指向的内存空间。	（写）

  ​            等号 右边的变量，代表 变量内存空间存储的数据值。	（读）

  传地址（引用）：将形参的地址值作为函数参数传递。

  传值（数据据）：将实参的值拷贝一份给形参。

  传引用：	在A栈帧内部，修改B栈帧中的变量值。

  **切片**：不是一个数组的指针，是一种数据结构体，用来操作数组内部元素。

  ```go
  切片名称 [ low : high : max ]
  low: 起始下标位置
  high：结束下标位置	len = high - low
  容量：cap = max - low
  截取数组，初始化切片时，没有指定切片容量时,切片容量跟随原数组（切片）。
  	s[:high:max] :从0开始，到high结束（不包含）.
  	s[low:] :从low开始，到末尾
  	s[: high]:从0开始，到high结束。容量跟随原先容量。【常用】
  切片做函数参数 —— 传引用。（传地址）
  append：在切片末尾追加元素
  	append(切片对象， 待追加元素）
  	向切片增加元素时，切片的容量会自动增长。1024 以下时，一两倍方式增长。
  copy：
  	copy（目标位置切片， 源切片） 拷贝过程中，直接对应位置拷贝。
  ```
  **map**： 

  ```go
  key： 唯一、无序。 不能是引用类型数据。
  map 不能使用 cap（）
  创建方式：
  	1.  var m1 map[int]string		--- 不能存储数据（只声明map，没有空间，不能直接存储key--value。）
  
  	2. m2 := map[int]string{}		---能存储数据
  
  	3. m3 := make(map[int]string)		---默认len = 0
  
  	4. m4 := make(map[int]string, 10)
  
  初始化：
  
  	1. var m map[int]string = map[int]string{ 1: "aaa", 2:"bbb"}	保证key彼此不重复。
  
  	2. m := map[int]string{ 1: "aaa", 2:"bbb"}
  
  赋值:
  
  	赋值过程中，如果新map元素的key与原map元素key 相同 	——> 覆盖（替换）
  
  	赋值过程中，如果新map元素的key与原map元素key 不同	——> 添加
  
  map的使用：
  		  
  	遍历map：
  
  		for  key值， value值 := range map {
  		} 
  
  		for  key值 := range map {
  		}	
  
  	判断map中key是否存在。
  
  		 map[下标] 运算：返回两个值， 第一个表 value 的值，如果value不存在。 nil
  		第二个表 key是否存在的bool类型。存在 true， 不存在false
  
  	删除map：
  		delete()函数： 参1： 待删除元素的map	参2： key值
  		delete（map， key）删除一个不存在的key ， 不会报错。
  		map 做函数参数和返回值，传引用。
  ```

  **线程进程**：

  *并行*：借助多核`cpu`实现    【真 并行】

  *并发*：宏观：用户体验上，程序在并行执行					【假 并行】

  ​			微观：多个计划任务，顺序执行。在飞速的切换，轮换使用`CPU`时间轮片。 

  *进程并发*：

  ​			程序：编译成功得到的二进制文件。    占用磁盘空间    死的  1

  ​			进程：运行起来的程序。     占用系统资源（内存）     活的  N

  进程的状态：初始态、就绪态、运行态、挂起（阻塞）态、终止态。

  *线程并发*：

  ​		线程：`LWP`：轻量级的进程。	最小的执行单位。  —`cpu`分配时间轮片的对象

  ​		进程：最小的系统资源分配的单位。

  *同步*：为了数据混乱，解决与时间有关的错误

  ​		协同步调：规划先后顺序 

  ​		携程同步机制：

  ​				互斥锁（互斥量）：建议锁。拿到锁以后，才能访问数据，没有拿到锁的线程，阻塞等待。拿到锁的线程放锁。

  ​				读写锁：一把锁（读属性、写属性）。 写独占，读共享。写锁优先级高。 

  *协程并发*：`Python、Lua、Ruset`    提高程序执行效率

  ​		协程： 轻量级的线程

  总结：进程并发：稳定性强。线程并发：节省资源。协程并发：效率高

  

## 《Go程序设计语言》学习笔记



## 零散知识点总结

1、通道和并发

```go
go func chan int) { //读写均可的channel c } (a)
go func(c <- chan int) { //只读的Channel } (a)
go func(c chan <- int) {  //只写的Channel } (a)
```

2、接口对象执行的类型动态转换/查询——类型断言

```go
//1、具体值的类型断言
a,ok :=v.([]byte)//判断接口v的底层是否保存了字节切片类型的值，如果有，就返回ok=1 

//2、接口类型的类型断言，还未搞懂
type arg interface {}
a := arg.(int) //将arg保存的底层int值传给a，如果arg接口没有Int值，会panic

```

3、当用range遍历切片时，改变其值，也能改变切片的值。

4、下面的条件语句中的中断命令跳出循环的情况。

```go
for i:=range A {
	switch {
	case n==1:
		break //这里的break没有作用，因为会默认为跳出switch
	case n!=1:
		continue //这里的continue是有作用的，可以跳出本次for循环
	}
    
    //下面的都能跳出for循环
	if n>1 {
		break
	}
	if n<1 {
		continue
	}
}
```

 