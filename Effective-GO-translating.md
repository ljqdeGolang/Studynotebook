## 介绍

Go是一种新语言。尽管它借鉴了现有语言的思想，但它具有非同寻常的特性，这使得更有效率的Go程序拥有与同样是相似语言编写的程序不同的特性。直接将C ++或Java程序转换为Go程序，不太可能得到一个令人满意的结果—因为Java程序是用Java而不是Go编写的。另一方面，从Go角度考虑问题可能会写出一个成功但完全不同的程序。换句话说，要编写Go语言，理解其属性和习惯用法很重要。了解Go编程中已建立的约定习俗（例如命名，格式设置，程序构造等）也很重要，这样您编写的程序将易于其他Go程序员理解。

本文档提供了编写清晰，惯用的Go代码的技巧。它增加了[语言规范](https://golang.org/ref/spec)，“ [Go](https://tour.golang.org/)之[旅”](https://tour.golang.org/)和“[如何编写Go代码”](https://golang.org/doc/code.html)这些内容，而所有这些内容是你首先应该阅读的。

### 例子

[Go的官方包源 ](https://golang.org/src/) 的目的不仅在于作为核心库，而且也作为如何使用该语言的例子。此外，许多包都包含可运行的，自包含的可执行示例，您可以直接在[golang.org](https://golang.org/)网站上运行该示例 ，例如 [示例](https://golang.org/pkg/strings/#example_Map)（如有必要，请单击“示例”一词以将其打开运行）。如果您对如何解决问题或可能如何实施解决方案有疑问，则库中的文档，代码和示例可以提供答案，方法和环境背景。

## 格式化

格式化问题是最有争议但结果最不重要的问题。人们可以适应不同的格式样式，但是如果每个人都遵循相同的样式，那这会更有利于他们，他们将会花更少的时间甚至没必要花时间在这个课题上。问题是如何在没有冗长的规范性样式指南的情况下接近这种理想情况。

我们使用的Go，可以采用一种不寻常的方法，让机器处理大多数格式化问题。即`gofmt`程序（也可称为`go fmt`，以包级别而不是源文件级别运行），它读取Go程序，并以缩进和垂直对齐的标准样式发出源代码，同时保留注释，并在必要时重新格式化注释。如果您想知道如何处理一些新的布局情况，请运行`gofmt`；如果答案似乎不正确，请重新排列程序（或提交关于`gofmt`的bug文件），<u>请不要绕开这个问题。</u>

例如，无需花时间对结构字段上的注释进行排列。 `Gofmt`将为您做到这一点。给出声明

```go
类型T struct {
    名称字符串//对象的名称
    值int //其值
}
```

`gofmt` 将列对齐：

```go
类型T struct {
    名称字符串//对象的名称
    值int //其值
}
```

标准库中的所有Go代码都已使用`gofmt`格式化。

遗留下的一些非常简短的格式详细信息：

- 缩进

  我们使用制表符进行缩进，`gofmt`在默认情况下都发制表符。仅在必要时使用空格。

- 线长

  Go没有行长限制，不用担心穿孔卡溢出。如果感觉行长太长，则将其括起来并用额外的制表符缩进。

- 括弧

  Go比C和Java需要的括号少：控制结构（`if`， `for`，`switch`）在它们的语法中没有圆括号。同样，运算符优先级层次更短更清晰，因此`x << 8 + y << 16 `表示的间距意味的是什么，这与其他语言不同。

## 注释

Go提供了C样式的`/* */`块注释和C ++样式的`//`行注释。行注释是常态；块注释主要显示为包的注释，但它在表达式中很有用，或者用于禁用大量代码。

`godoc`程序（也为Web服务器）处理Go源文件以提取相关包内容的文档。顶层声明之前出现的注释（中间没有换行符）与声明一起被提取，以用作该项目的解释性文本。这些注释的性质和样式决定了`godoc`文档生成的质量。

每个包都应有一个*package注释*，即在package子句前的一个块注释。对于多文件包，包注释仅需要出现在一个文件中，<u>任何一个文件都可以</u>。包注释应该引入包，并提供与包整体相关的信息。它会首先出现在`godoc`页面上，并应设置随后的详细文档。

```go
/ *
软件包regexp为正则表达式实现了一个简单的库。

接受的正则表达式的语法为：

    正则表达式：
        串联{'|' 串联}
    级联：
        {封闭}
    关闭：
        术语['*'| '+'| '？' ]
    术语：
        '^'
        '$'
        '.'
        字符
        '['['^']字符范围']'
        '（'regexp'）'
* /
软件包正则表达式
```

如果软件包很简单，则软件包注释可以简短。

```go
//包路径用通用的规则来
//处理斜杠分隔的文件名路径。
```

注释不需要额外的格式，例如星号标语。生成的输出甚至可能不会以固定宽度的字体显示，因此像`godoc`不必依赖对齐的间距，而像`gofmt`就依赖。注释是未解释的纯文本，因此HTML和其他注释如`_this_`都将一字不差地重现，而这些不应该使用。`godoc`所做的一项调整是以固定宽度的字体显示缩进的文本，这适用于程序块。对于 [`fmt`包](https://golang.org/pkg/fmt/)的注释使用此效果良好。

根据上下文的不同，`godoc`甚至可能不会重新格式化注释，因此请确保它们看起来简洁明了：使用正确的拼写，标点和句子结构，折叠长行等。

在包中，顶层声明之前的任何注释都将用作该声明的*doc注释*。程序中的每个导出（大写）名称都应带有文档注释。

Doc注释最好作为完整的句子使用，从而可以进行各种各样的自动演示。第一句应该是单句摘要，以声明的名称开头。

```go
//编译会解析一个正则表达式，如果成功，则返回
//可用于文本匹配的Regexp。
func Compile(str string) (*Regexp，error){
```

如果每个文档注释都以其描述的项目名称开头，则可以使用[go](https://golang.org/cmd/go/)工具的[doc](https://golang.org/cmd/go/#hdr-Show_documentation_for_package_or_symbol) 子命令`grep`，并通过运行输出。想象一下，您忘记了名称“ Compile”，但在寻找正则表达式的解析函数，因此您可以运行该命令

```go
$ go doc -all regexp | grep -i parse
```

如果包中的所有文档注释都以“此函数...”开头，则`grep` 不会帮助您记住该名称。但是，由于包每个文档注释都以名称开头，因此您会看到类似这样的内容，该名称会重现起您要查找的单词。

```go
$ go doc -all regexp | grep -i解析
    编译将解析正则表达式，如果成功，则返回Regexp
    MustCompile，其类似于Compile，但如果无法解析该表达式，则会发生错误。
    解析，它简化了全局变量保存的安全初始化
$
```

Go的声明语法允许对声明进行分组。单个文档注释可以引入一组相关的常量或变量。由于整个声明都已提出，因此这样的注释常常是敷衍的。

```go
//未能解析表达式返回的错误代码。
var（
    ErrInternal = errors.New("regexp：internal error")
ErrUnmatchedLpar = errors.New ("regexp：unmatched '('")
ErrUnmatchedRpar = errors.New (" regexp：unmatched ')'")
    ...
）
```

分组还可以表明项目之间的关系，例如一组变量受互斥锁保护的事实。

```go
var（
    countLock sync.Mutex
    inputCount uint32
    outputCount uint32
    errorCount uint32
）
```

## 名称

名称在Go语言中与其他语言一样重要。它们甚至具有语义效果：包外部名称的可见性取决于其首字符是否为大写。因此，值得花一些时间讨论Go程序中的命名约定。

### 包名称

导入软件包时，软件包名称将成为内容的访问器。

```go
import "bytes"
```

导入包可以访问`bytes.Buffer`。如果每个使用该包的人都可以使用相同的名称来引用其内容，这将很有帮助，这意味着该软件包的名称应该很好：简短，简洁，令人回味。按照惯例，包使用小写的单字名称，不需要下划线或字母大小写。为了简便起见，因此每个使用您的包Err的人都会输入该名称，而且不用担心先验冲突。包名称仅是导入的默认名称。它不必在所有源代码中都是唯一的，并且在发生冲突的极少数情况下，导入包可以选择其他名称以在本地使用。在任何情况下，混淆都是很少的，因为导入中的文件名决定了所使用的包。

另一个惯例是，包名称是其源目录的基本名称。`src/encoding/base64` 导入时包名称为`"encoding/base64"`，但名称也为`base64`而不是`encoding_base64`和 `encodingBase64`。

程序包的导入者将使用该名称来引用其内容，因此，程序包中导出的名称可以使用该方法来避免卡顿。（不要使用这种`import .`表示法，它可以简化必须在所测试的包之外运行的测试，但应避免这样做。）例如，`bufio`程序包中的缓冲读取器类型称为`Reader`，而不是`BufReader`，因为用户将其视为`bufio.Reader`，这是一个简洁明了的名称。此外，由于导入的实体始终使用其包名称来寻址，因此`bufio.Reader` 不会与冲突`io.Reader`。同样，通常会调用来创建新实例的函数（`ring.Ring`这是Go中*构造函数*的定义）`NewRing`，但是由于 `Ring`是程序包导出的唯一类型，由于调用了程序包`ring`，因此将其称为just `New`，程序包的客户端将其视为`ring.New`。使用包结构可以帮助您选择好名字。

另一个简短的例子是`once.Do`； `once.Do(setup)`方便易读，写成`once.DoOrWaitUntilDone(setup)`也不会改善。长名不会自动使其更具可读性。有用的文档注释通常比加长名称更有价值。

### <u>接受者</u>

Go不会自动为getter和setter提供支持。自己提供getter和setter并没有问题，这样做通常也是适当的，但是在getter的名字中使用`Get`既不惯用，且也没有必要。如果您有一个名为`owner`（小写，未导出）的字段 ，则应调用getter方法`Owner`（大写，已导出），而不是`GetOwner`。使用大写名称进行导出提供了钩子函数，以将字段与方法区分开。如果需要，可以为setter函数使用`SetOwner`名称。这两种名称在实践中都很易读：

```
owner：= obj.Owner（）
if owner !={
    obj.SetOwner(user)
}
```

### 接口名称

按照惯例，一个方法接口由该方法name加上后缀-er或类似的修改命名来构成代理名：`Reader`， `Writer`，`Formatter`， `CloseNotifier`等。

有许多类似这样的名称，遵循它们和它们关联的函数名称很有效的。 `Read`，`Write`，`Close`，`Flush`， `String`等都有规范名称和意义。为避免混淆，除非您的方法具有相同的名称和含义，否则请不要给它们使用以上这些名称。相反，如果您的类型实现的方法的含义与熟知类型上的方法的含义相同，则可为其赋予相同的名称和签名；您可用`String`not `ToString`调用string-converter方法。

### 混合字母

最后，Go中的约定是使用`MixedCaps` 或`mixedCaps`而不使用下划线来编写多字名称。

## 分号

与C一样，Go的形式语法可用分号来终止语句，但是与C中不同，这些分号不会出现在源代码中。相反，词法分析器在扫描时使用一种简单规则自动插入分号，因此输入文本几乎没有分号。

规则是这样的。如果换行符之前的最后一个标记是标识符（包括诸如`int`和的词`float64`），基本文字（例如数字或字符串常量）或者标记之一，词法分析器总是在标记后插入分号。

```go
break continue fallthrough return ++ -- ) }
```

这可以概括为：“如果换行符位于可以结束语句的标记之后，请插入分号”。

也可以在紧接大括号前省略分号，因此可以使用如下语句：

```go
 go func() { for { dst <- <-src } }()
```

这就不需要分号。惯用的Go程序仅在诸如`for`循环子句之类的地方使用分号 ，以分隔初始化程序，条件和延续元素。如果您以这种方式编写代码，则在一行上分隔多个语句也是必需的。

分号插入规则的造成的后果是，你不能把一个控制结构`if`，`for`，`switch`，`select`中的左括号放在下一行。如果这样做，将在分号之前插入一个分号，这可能会导致不想要的效果。这样写

```go
if i < f() {
    g()
}
```

不像这样

```go
if i < f()  // wrong!
{           // wrong!
    g()
}
```

## 控制结构

Go的控制结构与C的控制结构相关，但在许多重要方面有所不同。Go没有`do`和`while`循环，只有 `for`的概念； 更灵活的`switch`； `if`和`switch`接受可选的初始化语句，如`for`； `break`和`continue`声明接受可选的标签，以确定哪些中断或继续; 并且有新的控制结构，包括类型开关和多路通信多路复用器`select`。语法也略有不同：go中没有括号，并且主体必须始终用大括号分隔。

### If

在Go中，一个简单的`if`语句样子是这样的：

```go
if x > 0 {
    return y
}
```

强制括号鼓励在多行编写简单的`if`语句。无论如何这样做都是一种好的风格，尤其是当函数体包含诸如`return`或`break`的控制语句时。

由于`if`和`switch`接受初始化语句，通常会看到用来设置局部变量的语句。

```go
if err := file.Chmod(0664); err != nil {
    log.Print(err)
    return err
}
```

在Go的库中，你会发现，当一个`if`申明语句后没有下一条申明语句，不必要的`else`可以被省略，这种情况就是函数体有`break`，`continue`， `goto`，或`return`。

```go
f, err := os.Open(name)
if err != nil {
    return err
}
codeUsing(f)
```

这是一种常见情况的示例，在这种情况下，代码必须防范一系列错误。如果成功的控制流贯穿代码页，代码将很易读，也消除了可能出现的错误。由于错误情况倾向于以`return` 语句结尾，因此生成的代码不需要`else`语句。

```go
f, err := os.Open(name)
if err != nil {
    return err
}
d, err := f.Stat()
if err != nil {
    f.Close()
    return err
}
codeUsing(f, d)
```

### 重新声明和重新分配

旁白：上一节中的最后一个示例演示了短声明`:=`如何工作的详细信息 。调用的声明`os.Open`为：

```go
f, err := os.Open(name)
```

该语句声明了两个变量`f`和`err`。几行后，对`f.Stat` 的调用

```go
d, err := f.Stat()
```

看起来好像在声明`d`和`err`。但是请注意，`err`这两个语句中都会出现。这种重复是合法的：`err`由第一个语句声明，但仅在第二个语句中*重新分配*。这意味着对`f.Stat`的调用使用的是上面声明的现有变量`err`，并为其赋予一个新值。

在`:=`声明中，`v`即使是已经声明了的变量，也可能会出现一个变量，条件是：

- 此声明与的现有声明在同一范围内`v` （如果`v`已经在外部范围中声明，则该声明将创建一个新变量），
- 初始化中的对应值可分配给`v`
- 声明创建了至少一个其他变量。

这种不寻常的特性是纯粹的实用主义，例如很容易在长`if-else`链中使用单个`err`值。您会看到它经常使用。

在这里值得注意的是，在Go中，函数参数和返回值的范围与函数主体相同，即使它们按词法出现在包围主体的括号之外。

### For

Go`for`循环类似于C，但不相同。它统一了`for` 和`while`且没有`do-while`。go中共有三种形式，其中只有一种具有分号。

```go
// Like a C for
for init; condition; post { }

// Like a C while
for condition { }

// Like a C for(;;)
for { }
```

简短的声明使在循环中正确声明索引变量变得容易。

```go
sum := 0
for i := 0; i < 10; i++ {
    sum += i
}
```

如果要遍历数组，切片，字符串或映射，或者从通道读取，则`range`子句可以处理该循环。

```go
for key, value := range oldMap {
    newMap[key] = value
}
```

如果只需要范围内的第一项（键或索引），请舍去第二项：

```go
for key := range m {
    if key.expired() {
        delete(m, key)
    }
}
```

如果只需要遍历（值）中的第二项，请使用*空白标识符*（下划线）来丢弃第一项：

```go
sum := 0
for _, value := range array {
    sum += value
}
```

如[后面的部分](https://golang.org/doc/effective_go#blank)所述，空白标识符有许多用途。

对于字符串，`range`它可以为您做更多的工作，通过解析UTF-8来分解单个Unicode代码点。错误的编码会占用一个字节并产生替换符文U + FFFD。`rune`（名称（具有关联的内置类型）是单个Unicode代码点的Go术语。有关详细信息，请参见[语言规范](https://golang.org/ref/spec#Rune_literals)。）

```go
for pos, char := range "日本\x80語" { // \x80 is an illegal UTF-8 encoding
    fmt.Printf("character %#U starts at byte position %d\n", char, pos)
}
```

打印

```go
character U+65E5 '日' starts at byte position 0
character U+672C '本' starts at byte position 3
character U+FFFD '�' starts at byte position 6
character U+8A9E '語' starts at byte position 7
```

最后，Go没有逗号运算符，`++`和`--` 语句不是表达式。因此，如果您想要在一个`for`循环中运行多个变量， 则应使用并行赋值（尽管这排除了`++`和`--`）。

```go
// Reverse a
for i, j := 0, len(a)-1; i < j; i, j = i+1, j-1 {
    a[i], a[j] = a[j], a[i]
}
```

### Switch

Go的`switch`比C更通用。表达式不必是常量，甚至不必是整数，case从上到下进行评估，直到找到匹配项为止；如果`switch`没有表达式，则默认为 `true`。因此习惯性的将 `if`- `else`- `if`-`else` 链写为`switch`。

```go
func unhex(c byte) byte {
    switch {
    case '0' <= c && c <= '9':
        return c - '0'
    case 'a' <= c && c <= 'f':
        return c - 'a' + 10
    case 'A' <= c && c <= 'F':
        return c - 'A' + 10
    }
    return 0
}
```

不会自动掉线，但case可以用逗号分隔的列表显示。

```go
func shouldEscape(c byte) bool {
    switch c {
    case ' ', '?', '&', '=', '#', '+', '%':
        return true
    }
    return false
}
```

尽管它们在Go中不像其他一些类似C的语言那样普遍，但可以使用`break`语句来使`switch`尽早终止。但是，有时需要跳出周围的循环而不是switch，而在Go中，可以通过在循环上放置标签和“breaking”标签来实现跳出循环。此示例显示了两种用法。

```go
Loop:
	for n := 0; n < len(src); n += size {
		switch {
		case src[n] < sizeOne:
			if validateOnly {
				break
			}
			size = 1
			update(src[n])

		case src[n] < sizeTwo:
			if n+1 >= len(src) {
				err = errShortInput
				break Loop
			}
			if validateOnly {
				break
			}
			size = 2
			update(src[n] + src[n+1]<<shift)
		}
	}
```

当然，该`continue`语句还接受可选标签，但仅适用于循环。

要结束本段循环，这里有一个字节切片的比较惯例，它使用了`switch`声明

```go
//比较返回比较两个字节片的整数，
//按照字典顺序。
//如果a == b，结果将为0；如果a <b，结果将为-1；如果a> b，结果将为+1
func Compare(a, b []byte) int {
    for i := 0; i < len(a) && i < len(b); i++ {
        switch {
        case a[i] > b[i]:
            return 1
        case a[i] < b[i]:
            return -1
        }
    }
    switch {
    case len(a) > len(b):
        return 1
    case len(a) < len(b):
        return -1
    }
    return 0
}
```

### 类型转换

类型转换也可以用来发现接口变量的动态类型。这种*类型转换*使用括号内带有类型关键字的类型声明语法。如果类型转换在表达式中声明了变量，则该变量在每个子句中将具有对应的类型。在这种情况下重用名称也是惯用的，实际上是在每种情况下声明一个具有相同名称但类型不同的新变量。

```go
var t接口{}
t = functionOfSomeType（）
开关t：= t。（type）{
默认：
    fmt.Printf（“意外类型％T \ n”，t）//％T打印t具有的任何类型
案例布尔：
    fmt.Printf（“ boolean％t \ n”，t）// t的类型为bool
case int：
    fmt.Printf（“ integer％d \ n”，t）// t的类型为int
案例*布尔：
    fmt.Printf（“指向布尔值％t \ n的指针，* t）// t的类型为* bool
案例* int：
    fmt.Printf（“整数％d \ n的指针”，* t）// t的类型为* int
}
```

## 函数

### 多个返回值

Go的不寻常功能之一是函数和方法可以返回多个值。这种形式可以用来改进C程序中的一些笨拙的习惯用法：内联错误返回，例如`-1`for`EOF` 和修改由地址传递的参数。

在C语言中，写入错误由负数表示，错误代码被隐藏在易失性位置中。在Go中，`Write` 可以返回一个计数和一个错误：“是的，您写入了一些字节，但不是全部，因为您的设备内存已满”。`Write`软件包中文件的方法标签`os`为：

```go
func (file *File) Write(b []byte) (n int, err error)
```

而如文件所说，它返回写入的字节数和一个非空`error`当`n` `!=` `len(b)`的时候。这是一种常见的样式。有关更多示例，请参见错误处理部分。

类似的方法消除了将指针传递给返回值以模拟参考参数的需要。这是一个简单的函数，可从字节片中的某个位置获取一个数字，然后返回该数字和下一个位置。

```go
func nextInt(b []byte, i int) (int, int) {
    for ; i < len(b) && !isDigit(b[i]); i++ {
    }
    x := 0
    for ; i < len(b) && isDigit(b[i]); i++ {
        x = x*10 + int(b[i]) - '0'
    }
    return x, i
}
```

您可以使用它来扫描输入切片b中的数字，如下所示：

```go
    for i := 0; i < len(b); {
        x, i = nextInt(b, i)
        fmt.Println(x)
    }
```

### 命名结果参数

可以给Go函数的返回或结果“参数”命名，并将其用作常规变量，就像传入的参数一样。命名后，函数开始时会将它们初始化为零值。如果函数执行`return`不带参数的语句，则将结果参数的当前值用作返回值。

命名不是强制性的，但它们可以使代码更短，更清晰：它们是文档。如果我们命名`nextInt`则返回的结果显而易见是`int` 。

```go
func nextInt(b []byte, pos int) (value, nextPos int) {
```

由于命名结果已初始化并绑定到未经修饰的返回，因此它们既简化又清晰。`io.ReadFull`这个版本就很好地使用了它们：

```go
func ReadFull(r Reader, buf []byte) (n int, err error) {
    for len(buf) > 0 && err == nil {
        var nr int
        nr, err = r.Read(buf)
        n += nr
        buf = buf[nr:]
    }
    return
}
```

### Defer

<u>在执行`defer`返回的函数之前，Go的`defer`语句计划立即调用延迟函数</u>。这是一种不寻常但有效的处理情况的方法，例如无论函数返回哪个路径都必须释放资源。典型的例子是解锁互斥锁和关闭文件。

```go
// Contents以字符串形式返回文件的内容。
func Contents(filename string) (string, error) {
    f, err := os.Open(filename)
    if err != nil {
        return "", err
    }
    defer f.Close()  // f.Close will run when we're finished.

    var result []byte
    buf := make([]byte, 100)
    for {
        n, err := f.Read(buf[0:])
        result = append(result, buf[0:n]...) // append is discussed later.
        if err != nil {
            if err == io.EOF {
                break
            }
            return "", err  // f will be closed if we return here.
        }
    }
    return string(result), nil // f will be closed if we return here.
}
```

推迟调用诸如`Close`之类的函数有两个优点。首先，它确保您永远不会忘记关闭文件，如果以后编辑函数以添加新的返回路径，则很容易犯此类错误。其次，这意味着关闭代码和启动代码在同一区域，这比将其放置在函数的末尾要清晰得多。

延迟函数的参数（如果函数是方法，则包括接收方）在*延迟* 执行时（而不是在*调用*执行时）进行评估。除了避免担心变量在函数执行时会更改值外，这还意味着单个延迟的调用站点可以推迟多个函数的执行。这是一个简单的例子。

```go
for i := 0; i < 5; i++ {
    defer fmt.Printf("%d ", i)
}
```

延迟的功能按LIFO顺序执行，因此该代码将 `4 3 2 1 0`在函数返回时被打印。一个更合理的示例是通过程序跟踪函数执行的简单方法。我们可以编写一些简单的跟踪例程，如下所示：

```go
func trace(s string)   { fmt.Println("entering:", s) }
func untrace(s string) { fmt.Println("leaving:", s) }

// Use them like this:
func a() {
    trace("a")
    defer untrace("a")
    // do something....
}
```

利用这个事实，我们可以在`defer`执行时很好地评估延迟函数的参数。跟踪例程可以将参数设置为取消跟踪例程。如下例：

```go
func trace(s string) string {
    fmt.Println("entering:", s)
    return s
}

func un(s string) {
    fmt.Println("leaving:", s)
}

func a() {
    defer un(trace("a"))
    fmt.Println("in a")
}

func b() {
    defer un(trace("b"))
    fmt.Println("in b")
    a()
}

func main() {
    b()
}
```

打印

```
输入：b
 b
输入：a
a
离开：a
离开：b
```

对于习惯于使用其他语言的块级资源管理的程序员来说，这`defer`似乎很奇怪，但是它最有趣，功能最强大的应用恰恰是因为它不是基于块的而是基于函数的。在 关于`panic`，`recover`的小节中，我们将看到其可能性的另一个示例。

## 数据

### 用 `new`分配

Go有两个分配初始化词，内置函数 `new`和`make`。虽然它们执行不同的操作，并应用于不同的类型，这可能会造成混淆，但是它们的规则很简单。让我们先谈谈`new`。它是一个分配内存的内置函数，但与其他语言中的同名函数不同，它不会*初始化*内存，只会将其设为零值。也就是说， `new(T)`为类型为`T`的项目分配零值并返回其地址，值为`*T`。在Go术语中，它返回一个指向新分配的零值`T`类型的指针。

由于`new`返回的内存值制为零，因此在设计数据结构时分配使用每种类型的零值而无需进一步初始化将很有帮助。这意味着数据结构的用户可以用`new`创建新的数据结构并开始使用。例如，`bytes.Buffer`的文档指出“`Buffer`的零值是准备使用的空缓冲区”。同样，`sync.Mutex`没有显式的构造函数或`Init`方法。而`sync.Mutex` 的零值定义为未锁定的互斥锁。

零值即有用属性会暂时起作用。可见如下类型声明。

```go
type SyncedBuffer struct {
    lock    sync.Mutex
    buffer  bytes.Buffer
}
```

类型`SyncedBuffer`的值也可以在分配或声明后立即使用。在下一个代码段中，两者`p`和`v`都可以正常工作，而无需进一步分配值。

```go
p := new(SyncedBuffer)  // type *SyncedBuffer
var v SyncedBuffer      // type  SyncedBuffer
```

### 构造函数和复合文字

有时零值不够好，因此需要初始化构造函数，如本例中从`os`包派生的那样。

```go
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    f := new(File)
    f.fd = fd
    f.name = name
    f.dirinfo = nil
    f.nepipe = 0
    return f
}
```

那里有很多样板。我们可以使用*复合文字*来简化它，就是一种表达式，每次对其求值时都会创建一个新实例。

```go
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    f := File{fd, name, nil, 0}
    return &f
}
```

注意，与C语言不同，完全可以返回局部变量的地址。函数返回后，与变量关联的存储将保留。实际上，采用复合文字的地址会在每次对其求值时分配一个新实例，因此我们可以将后两行结合在一起。

```go
    return &File{fd, name, nil, 0}
```

复合文字的字段按顺序排列，并且必须全部存在。但是，通过将元素显式标记为键值对，初始化器可以按任何顺序出现，而缺失的则保留为各自的零值。因此我们可以写到

```go
return &File{fd: fd, name: name}
```

在有限的情况下，如果一个复合文字完全不包含任何字段，它将为该类型创建一个零值。表达式`new(File)`和`&File{}`是等效的。

我们也可以为数组，切片和映射创建复合文字，其中字段标签为索引或映射键。在这些例子中，初始化工作无论是`Enone`， `Eio`和`Einval`的值，只要它们是不同的。

```go
a := [...]string   {Enone: "no error", Eio: "Eio", Einval: "invalid argument"}
s := []string      {Enone: "no error", Eio: "Eio", Einval: "invalid argument"}
m := map[int]string{Enone: "no error", Eio: "Eio", Einval: "invalid argument"}
```

### 用 `make`分配

讲回到分配。内置函数`make(T, `*args*`)`的用途不同于`new(T)`。它仅能创建切片，字典和通道，并返回类型T（不是T指针）的初始化值（非零）。这三种类型描述有区别的原因是，在使用之前必须初始化的数据结构的引用。例如，切片是一个三项描述符，其中有包含指向数据（在数组内部），长度和容量的指针，在初始化这些项目之前，切片为零值。对于切片，字典和通道， 初始化内部数据结构并准备要使用的值。例如，

```go
make([]int, 10, 100)
```

分配一个100个整数的数组，然后创建一个长度为10且容量为100的切片结构，指向该数组的前10个元素。（制作切片时，可以省略容量；有关更多信息，请参见切片部分。）相反，`new([]int)`返回指向新分配的，归零切片结构的指针，即指向`nil`切片值的指针。

这些示例说明了`new`和 之间的区别`make`。

```go
var p *[]int = new([]int)       // 分配切片结构；*p==nil; 很少使用
var v  []int = make([]int, 100) // 切片v现在引用一个包含100个整数的新数组

// 不必要的复杂度
var p *[]int = new([]int)
*p = make([]int, 100, 100)

// 惯用语
v := make([]int, 100)
```

请记住，这`make`仅适用于字典，切片和通道，不返回指针。要获得显式指针`new`，请为变量分配地址或使用其地址。

### 数组

数组在规划内存的详细布局时很有用，有时可以帮助避免分配，但是数组主要是切片的构建块，切片是下一节的主题。为奠定该主题的基础，以下是有关数组的几句提示。

在Go和C中，数组的工作方式之间存在主要差异。在Go中，

- 数组是值。将一个数组分配给另一个数组将复制所有元素。
- 特别地，如果将数组传递给函数，它将接收该数组的*副本*，而不是指向它的指针。
- 数组的大小是其类型的一部分。类型`[10]int` 和`[20]int`是不同的。

值特性是既有用又花费巨大的。如果您想要类C的行为和效率，可以将指针传递给数组。

```go
func Sum(a *[3]float64) (sum float64) {
    for _, v := range *a {
        sum += v
    }
    return
}

array := [...]float64{7.0, 8.5, 9.1}
x := Sum(&array)  // Note the explicit address-of operator
```

但是，即使这种样式也不是惯用的Go，请改用切片。

### 切片

切片连接数组可为数据序列化提供更通用，更强大和更方便的接口。除了具有明确维度的项（例如转换矩阵）外，Go中的大多数数组的 编程都是使用切片而不是简单数组完成的。

切片包含对基础数组的引用，如果将一个切片分配给另一个切片，则两个切片均引用同一数组。如果函数采用slice参数，则对slice的元素所做的更改将对调用者可见，这类似于将指针传递给基础数组。因此`Read` 函数可以接受一个切片参数，而不是一个指针和一个计数; 切片内的长度设置了要读取多少数据的上限。这是包`os`中文件类型 `Read`方法的签名 ： 

```go
func (f *File) Read(buf []byte) (n int, err error)
```

该方法返回读取的字节数和错误值（如果有）。读入`buf`大缓冲区的前32字节 ，切分了缓冲区。

```go
  n, err := f.Read(buf[0:32])
```

这种切片是普通且有效的。实际上，暂时不考虑效率，以下代码段还将读取缓冲区的前32个字节。

```go
     var n int
    var err error
    for i := 0; i < 32; i++ {
        nbytes, e := f.Read(buf[i:i+1])  // Read one byte.
        n += nbytes
        if nbytes == 0 || e != nil {
            err = e
            break
        }
    }
```

切片的长度可以更改，只要它仍然适合基础数组的限制即可；只需将其分配自身的一部分给切片即可。切片的*容量*（可通过内置函数访问`cap`）表示了切片可能采用的最大长度。这是将数据追加到切片的函数。如果数据超出容量，则会重新分配片。返回结果切片。该函数使用`nil`切片时，`len`和`cap`是合法的 ，并返回0。

```go
func Append(slice, data []byte) []byte {
    l := len(slice)
    if l + len(data) > cap(slice) {  // reallocate
        // Allocate double what's needed, for future growth.
        newSlice := make([]byte, (l+len(data))*2)
        // The copy function is predeclared and works for any slice type.
        copy(newSlice, slice)
        slice = newSlice
    }
    slice = slice[0:l+len(data)]
    copy(slice[l:], data)
    return slice
}
```

之后必须返回切片，因为尽管`Append` 可以修改的`slice`的元素，但切片本身（保存指针，长度和容量的运行时数据结构）是按值传递的。

使用append`内置函数，添加到切片的方法非常有用 。但是，要了解该功能的设计，我们需要更多信息，因此我们将在稍后返回。

### 二维切片

Go的数组和切片是一维的。要创建等效于2维数组或切片的数组，必须定义一个数组的数组或切片的切片，如下所示：

```go
type Transform [3][3]float64  // A 3x3 array, really an array of arrays.
type LinesOfText [][]byte     // A slice of byte slices.
```

由于切片的长度是可变的，因此有可能使每个内部切片的长度不同。这可能是常见的情况，例如在我们的`LinesOfText` 示例中：每行都有独立的长度。

```go
text := LinesOfText{
	[]byte("Now is the time"),
	[]byte("for all good gophers"),
	[]byte("to bring some fun to the party."),
}
```

有时有必要分配2维切片，例如在处理像素的扫描线时可能会出现这种情况。有两种方法可以实现此目的。一种是独立分配每个切片；另一种是分配单个数组，并将单个切片指向该数组。使用哪种取决于您的应用程序。如果切片可能增大或缩小，则应独立分配它们，以免覆盖下一行；如果不是，则使用单一分配构造对象可能会更有效。作为参考，以下是这两种方法的示意图。首先，一次一行：

```go
// Allocate the top-level slice.
picture := make([][]uint8, YSize) // One row per unit of y.
// Loop over the rows, allocating the slice for each row.
for i := range picture {
	picture[i] = make([]uint8, XSize)
}
```

现在作为一种分配，分成几行：

```go
// Allocate the top-level slice, the same as before.
picture := make([][]uint8, YSize) // One row per unit of y.
// 分配一个大切片以容纳所有像素
pixels := make([]uint8, XSize*YSize) // Has type []uint8 even though picture is [][]uint8.
// 循环遍历各行，从其余像素切片的前面切成每行。
for i := range picture {
	picture[i], pixels = pixels[:XSize], pixels[XSize:]
}
```

### 字典（映射）

映射是一种方便且功能强大的内置数据结构，该结构将一种类型的值（*键*）与另一种类型的值（*元素*或*值*）相关联。键可以是定义了相等运算符的任何类型，例如整数，浮点数和复数，字符串，指针，接口（只要动态类型支持相等），结构和数组。切片不能用作映射键，因为未在其上定义相等性。像切片一样，映射保留对基础数据结构的引用。如果将字典传递给更改字典内容的函数，则更改将在调用方中可见。

可以使用带有冒号分隔的键/值对的常规复合文字语法来构建字典，因此在初始化过程中轻松构建它们。

```go
var timeZone = map[string]int{
    "UTC":  0*60*60,
    "EST": -5*60*60,
    "CST": -6*60*60,
    "MST": -7*60*60,
    "PST": -8*60*60,
}
```

语法上分配和获取映射值的方式类似于对数组和切片执行相同的操作，不同之处在于索引不必为整数。

```go
offset := timeZone["EST"]
```

尝试使用映射中不存在的键来获取映射值时，将为映射中的条目类型返回零值。例如，如果映射包含整数，则查找不存在的键将返回`0`。集合可以实现为具有`bool`值类型的映射。将映射项设置为`true`将值放入集合中，然后通过简单的索引对其进行测试。

```go
attended := map[string]bool{
    "Ann": true,
    "Joe": true,
    ...
}

if attended[person] { // will be false if person is not in the map
    fmt.Println(person, "was at the meeting")
}
```

有时您需要从零值中区分出缺失的条目。是否有条目为`"UTC"` 或为0，因为它根本不在字典中？您可以采用多种分配形式进行区分。

```go
var seconds int
var ok bool
seconds, ok = timeZone[tz]
```

由于显而易见的原因，这被称为“可用逗号”的语法。在此示例中，如果`tz`存在，`seconds` 将被适当传值，`ok`为true。如果不是， `seconds`则将其设置为零，并且`ok`为false。这是一个将其与良好的错误报告结合在一起的函数：

```go
func offset(tz string) int {
    if seconds, ok := timeZone[tz]; ok {
        return seconds
    }
    log.Println("unknown time zone:", tz)
    return 0
}
```

要在字典中测试是否存在而不必担心实际值是否被使用，可以使用[空白标识符](https://golang.org/doc/effective_go#blank)（`_`）代替该值的常规变量。

```go
_, present := timeZone[tz]
```

要删除字典键值对，请使用`delete` 内置函数，其内置参数是字典和要删除的键。即使字典上已经没有键值，也可以这样做。

```go
delete(timeZone, "PDT")  // Now on Standard Time
```

### 打印

Go中的格式化打印使用类似于C的`printf` 族的样式，但功能更丰富，更通用。该函数存在于`fmt` 包和有大写的名称：`fmt.Printf`，`fmt.Fprintf`， `fmt.Sprintf`等。字符串函数（`Sprintf`等）返回字符串，而不是填充提供的缓冲区。

您不需要提供格式字符串。对于每一个`Printf`， `Fprintf`和`Sprintf`有另一种双功能，如`Print`和`Println`。这些函数不采用格式字符串，而是为每个参数生成默认格式。这些`Println`版本还会在参数之间插入一个空格，并在输出中添加一个换行符，而`Print`仅当双方的操作数都不是字符串时，这些版本才会添加空格。在此示例中，每行产生相同的输出。

```go
fmt.Printf("Hello %d\n", 23)
fmt.Fprint(os.Stdout, "Hello ", 23, "\n")
fmt.Println("Hello", 23)
fmt.Println(fmt.Sprint("Hello ", 23))
```

格式化的打印功能`fmt.Fprint` 和同类功能将实现该`io.Writer`接口的任何对象作为第一个参数。变量`os.Stdout` 和`os.Stderr`都是熟悉的实例。

从这里开始与C背道而驰。首先，诸如`%d`这样的数字格式不带有标志性或大小标志。相反，打印例程使用参数的类型来决定这些属性。

```go
var x uint64 = 1<<64 - 1
fmt.Printf("%d %x; %d %x\n", x, x, int64(x), int64(x))
```

打印值

```go
18446744073709551615 ffffffffffffffff; -1 -1
```

如果只需要默认转换（例如，十进制表示整数），则可以使用包罗万象的格式`%v`（表示“值”）；结果如`Print`和`Println`打印的一样。此外，该格式可以打印*任何*值，甚至可以打印数组，切片，结构和映射。这是上一节中定义的时区图的打印语句。

```go
fmt.Printf("%v\n", timeZone)  // or just fmt.Println(timeZone)
```

给出输出：

```go
map[CST:-21600 EST:-18000 MST:-25200 PST:-28800 UTC:0]
```

对于字典，`Printf`和同属类按字母顺序对输出进行键值排序。

打印结构体时，修改格式`%+v`会用其名称注释结构的字段，对于任何值，备用格式`%#v`都会以完整的Go语法打印该值。

```go
type T struct {
    a int
    b float64
    c string
}
t := &T{ 7, -2.35, "abc\tdef" }
fmt.Printf("%v\n", t)
fmt.Printf("%+v\n", t)
fmt.Printf("%#v\n", t)
fmt.Printf("%#v\n", timeZone)
```

打印

```go
&{7 -2.35 abc   def}
&{a:7 b:-2.35 c:abc     def}
&main.T{a:7, b:-2.35, c:"abc\tdef"}
map[string]int{"CST":-21600, "EST":-18000, "MST":-25200, "PST":-28800, "UTC":0}
```

（请注意“＆”号。）通过`%q`当使用类型为`string`或`[]byte`时，引号字符串格式也是可用的。如果可能，替代格式`%#q`将使用反引号代替。（该`%q`格式也适用于整数和符文，生成单引号符文常量。）此外，`%x`适用于字符串，字节数组和字节片以及整数，生成长十六进制字符串，且格式为空格（`% x`），在字节之间放置空格。

另一种方便的格式是`%T`，它打印值的*类型*。

```go
fmt.Printf("%T\n", timeZone)
```

打印

```go
map [string] int
```

如果要控制自定义类型的默认格式，只需定义一种在类型上带有`String() string`签名的方法即可。对于我们的简单类型`T`，可能看起来像这样。

```go
func (t *T) String() string {
    return fmt.Sprintf("%d/%g/%q", t.a, t.b, t.c)
}
fmt.Printf("%v\n", t)
```

以下格式打印

```
7 / -2.35 /“ abc \ tdef”
```

（如果您需要打印类型*值*`T`以及指向的指针`T`，则`String`的接收者必须为值类型；此示例使用了指针，因为这对于结构类型更有效且更惯用。有关[指针与值接收器的关系](https://golang.org/doc/effective_go#pointers_vs_values)，请参见以下部分更多信息。）

我们的`String`方法之所以能够调用，是因为`Sprintf`打印例程是完全可重入的，并且可以通过这种方式包装。但是，对这种方法有一个重要的细节需要了解：不要通过`Sprintf`无限期地调用 `String`方法的方式来构造`string`方法。如果`Sprintf` 调用尝试将接收方直接打印为字符串，则可能会发生这种情况，而字符串又会再次调用该方法。如本例所示，这是一个常见且容易犯的错误。

```go
type MyString string

func (m MyString) String() string {
    return fmt.Sprintf("MyString=%s", m) // Error: will recur forever.
}
```

它也很容易修复：将参数转换为基本字符串类型，该类型没有方法。

```go
type MyString string

func (m MyString) String() string {
    return fmt.Sprintf("MyString=%s", m) // Error: will recur forever.
}
```

在[初始化部分，](https://golang.org/doc/effective_go#initialization)我们将看到另一种避免这种递归的技术。

另一种打印技术是将打印例程的参数直接传递给另一个此类例程。标签`Printf`使用`...interface{}`类型 作为其最终参数，以指定可以在格式之后显示任意数量的参数（任意类型）。

```go
func Printf(format string, v ...interface{}) (n int, err error) {
```

在`Printf`函数中，v其作为`[]interface{}`类型的变量， 但如果将其传递给另一个可变参数函数，则其作用类似于常规参数列表。这是我们上面使用的`log.Println`功能的实现。它直接以实际格式将其参数传递给 `fmt.Sprintln`。

```go
// Println以fmt.Println的方式打印到标准记录器。
func Println(v ...interface{}) {
    std.Output(2, fmt.Sprintln(v...))  // Output takes parameters (int, string)
}
```

我们在嵌套调用中将`···`写在`v`之后，`Sprintln`告诉编译器将其`v`视为参数列表。否则，它将`v`作为单个切片参数传递 。

打印比我们这里讨论的还要多。有关详细信息，请参见`godoc`软件包的`fmt`文档。

顺便说一句，`...`参数可以是特定类型，例如`...int` 对于选择最小整数列表的min函数而言：

```go
func Min(a ...int) int {
    min := int(^uint(0) >> 1)  // largest int
    for _, i := range a {
        if i < min {
            min = i
        }
    }
    return min
}
```

### 添加

现在，我们缺少了解释设计内置功能`append`所需的内容。标签`append` 与上面自定义的`Append`函数不同。示意如下：

```go
func append（slice [] T，elements ... T）[] T
```

其中*T*是任何给定类型的占位符。实际上，您无法在Go中编写`T` 这种由调用者确定类型的函数。这就是`append`内置的原因：它需要编译器的支持。

`append`就是将元素附加到切片的末尾并返回结果。需要返回结果，因为与我们手动操作`Append`一样，底层数组可能会更改。这个简单的例子

```go
x := []int{1,2,3}
x = append(x, 4, 5, 6)
fmt.Println(x)
```

打印`[1 2 3 4 5 6]`。所以`append`工作有点像`Printf`，收集任意数量的参数。

但是，如果我们想要`Append`将一个切片附加到一个切片上，那怎么办？简单：`...`就像在上述引用中一样，就像引用`Output`一样。此代码段产生与上面相同的输出。

```
x := []int{1,2,3}
y := []int{4,5,6}
x = append(x, y...)
fmt.Println(x)
```

没有`...`，它就不会编译，因为类型是错误的。`y`不是type `int`。

## 初始化

尽管从表面上看，它与C或C ++中的初始化没有太大区别，但是Go中的初始化功能更强大。可以在初始化期间构建复杂的结构，并且正确处理了初始化对象之间（甚至不同包之间）的排序问题。

### 常量

Go中的常量就是常量。它们是在编译时创建的，即使在函数中定义为局部变量时也只能是数字，字符（符文），字符串或布尔值。由于编译时的限制，定义它们的表达式必须是可由编译器评估的常量表达式。例如， `1<<3`是一个常量表达式，而 `math.Sin(math.Pi/4)`不是，因为函数调用`math.Sin`需要在运行时发生。

在Go中，使用枚举器创建枚举常量`iota` 。由于`iota`可以作为表达式的一部分，并且表达式可以隐式重复，因此可以轻松地构建复杂的值集。

```go
type ByteSize float64

const (
    _           = iota // ignore first value by assigning to blank identifier
    KB ByteSize = 1 << (10 * iota)
    MB
    GB
    TB
    PB
    EB
    ZB
    YB
)
```

附加如`String`方法到任何用户定义的类型的能力使得任意值都可以自动格式化自身以进行打印。尽管您会看到它最常用于结构，但该技术对于标量类型（例如浮点类型`ByteSize`）也很有用。

```go
func (b ByteSize) String() string {
    switch {
    case b >= YB:
        return fmt.Sprintf("%.2fYB", b/YB)
    case b >= ZB:
        return fmt.Sprintf("%.2fZB", b/ZB)
    case b >= EB:
        return fmt.Sprintf("%.2fEB", b/EB)
    case b >= PB:
        return fmt.Sprintf("%.2fPB", b/PB)
    case b >= TB:
        return fmt.Sprintf("%.2fTB", b/TB)
    case b >= GB:
        return fmt.Sprintf("%.2fGB", b/GB)
    case b >= MB:
        return fmt.Sprintf("%.2fMB", b/MB)
    case b >= KB:
        return fmt.Sprintf("%.2fKB", b/KB)
    }
    return fmt.Sprintf("%.2fB", b)
}
```

表达式`YB`打印为`1.00YB`，而`ByteSize(1e13)`打印为`9.09TB`。

这里的使用`Sprintf` 来实现`ByteSize`的`String`方法是不是因为转换的安全（避免重复下去），而是因为它要求`Sprintf`有`%f`，这不是一个字符串格式：`Sprintf`只调用`String`时，它要一个字符串的方法，以及`%f` 想要一个浮点点值。

### 变量

变量可以像常量一样被初始化，但是初始值是在运行时计算的通用表达式。

```go
var (
    home   = os.Getenv("HOME")
    user   = os.Getenv("USER")
    gopath = os.Getenv("GOPATH")
)
```

### 初始化功能

最后，每个源文件都可以定义自己的幂零根基`init`函数来设置所需的任何状态。（实际上，每个文件可以具有多个 `init`函数。）最后意味：在包中的所有变量声明评估了它们的初始值设定项之后才调用int：只有在所有导入的包都已初始化之后才对它们进行赋值。

除了不能表示为声明的初始化外，`init`函数的常见用法是在实际执行开始之前验证或修复程序状态的正确性。

```go
func init() {
    if user == "" {
        log.Fatal("$USER not set")
    }
    if home == "" {
        home = "/home/" + user
    }
    if gopath == "" {
        gopath = home + "/go"
    }
    // gopath may be overridden by --gopath flag on command line.
    flag.StringVar(&gopath, "gopath", gopath, "override default GOPATH")
}
```

## 方法

### 指针与值

如在`ByteSize`中我们所见，可以为任何命名类型（指针或接口除外）定义方法；接收者不必是结构。

在上面的切片讨论中，我们编写了一个`Append` 函数。我们可以将其定义为切片方法。为此，我们首先声明一个可以绑定该方法的命名类型，然后使该方法的接收者成为该类型的值。

```go
type ByteSlice []byte

func (slice ByteSlice) Append(data []byte) []byte {
    // Body exactly the same as the Append function defined above.
}
```

这仍然需要方法返回更新的切片。我们可以通过重新定义一个采取`ByteSlice`指针的方法作为它的接收器来消除笨拙性，如此该方法可以覆盖调用者的切片。

```go
func (p *ByteSlice) Append(data []byte) {
    slice := *p
    // Body as above, without the return.
    *p = slice
}
```

实际上，我们可以做得更好。如果我们修改函数，使其看起来像是标准`Write`方法，就像这样，

```go
func (p *ByteSlice) Write(data []byte) (n int, err error) {
    slice := *p
    // Again as above.
    *p = slice
    return len(data), nil
}
```

然后`*ByteSlice`类型满足标准 `io.Writer`接口，这很方便。例如，我们可以打印成一个。

```go
 var b ByteSlice
    fmt.Fprintf(&b, "This hour has %d days\n", 7)
```

我们传递的是`ByteSlice`的地址，因为仅`*ByteSlice`满足`io.Writer`。有关接收者的指针与值的规则是，可以在指针和值上调用值方法，但是只能在指针上调用指针方法。

之所以出现此规则，是因为指针方法可以修改接收者。在值上调用它们将导致该方法是接收该值的副本，因此任何修改都将被丢弃。而该语言不允许出现此错误。但是，有一个简单的例外。当值是可寻址的时，该语言将通过自动插入地址运算符来处理在值上调用指针方法的常见情况。在我们的示例中，变量`b`是可寻址的，因此我们可以使用b.write调用Write方法。编译器会将为我们其重写为`(&b).Write`。

顺便一提，在字节片上使用write的想法对于实现bytes.Buffer至关重要。

## 接口及其他类型

### 接口

Go中的接口提供了一种指定对象行为的方法：如果可以满足*这一点*，则可以在*此处*使用它 。我们已经看过几个简单的例子。自定义打印可以用一种`String`方法来实现，而`Fprintf`可以用`Write`方法输出任何类型。只有一个或两个方法的接口在Go代码中很常见，并且通常会使用从该方法派生的名称（例如`io.Writer` 实现的名称为`Write`。

一个类型可以实现多个接口。例如，如果一个集合实现了 `sort.Interface`，其中包含`Len()`， `Less(i, j int) bool`以及`Swap(i, j int)`，也可以有是一个自定义的格式，它就可以通过在sort包中的例程进行排序，在这个设计的例子中，`Sequence`两者都满足。

```go
type Sequence []int

// Methods required by sort.Interface.
func (s Sequence) Len() int {
    return len(s)
}
func (s Sequence) Less(i, j int) bool {
    return s[i] < s[j]
}
func (s Sequence) Swap(i, j int) {
    s[i], s[j] = s[j], s[i]
}

// Copy returns a copy of the Sequence.
func (s Sequence) Copy() Sequence {
    copy := make(Sequence, 0, len(s))
    return append(copy, s...)
}

// Method for printing - sorts the elements before printing.
func (s Sequence) String() string {
    s = s.Copy() // Make a copy; don't overwrite argument.
    sort.Sort(s)
    str := "["
    for i, elem := range s { 
        if i > 0 {
            str += " "
        }
        str += fmt.Sprint(elem)
    }
    return str + "]"
}
```

### 转换次数

Sequence的`String`方法是重新创建`Sprint`已经能对切片进行的工作。（它的复杂度为O（N²），这很差。）如果在调用Sprint之前将`Sequence`转换为普通的`[]int`格式，这就可以分担压力（并加快速度）。

```go
func (s Sequence) String() string {
    s = s.Copy()
    sort.Sort(s)
    return fmt.Sprint([]int(s))
}
```

此方法从`String`方法安全调用Sprintf的转换技术的另一个示例 。因为如果忽略类型名称，这两个类型（`Sequence`和`[]int`）是相同的，因此在它们之间进行转换是合法的。转换不会创建新值，它只是暂时充当现有值具有新类型的行为。（还有其他合法的转换，例如从整数到浮点的转换，它们创建了一个新值。）

在Go程序中，通过访问不同的方法集来转换表达式类型是一种习惯用法。例如，我们可以使用现有类型`sort.IntSlice`将整个示例简化为：

```go
type Sequence []int

// Method for printing - sorts the elements before printing
func (s Sequence) String() string {
    s = s.Copy()
    sort.IntSlice(s).Sort()
    return fmt.Sprint([]int(s))
}
```

现在，不是由`Sequence`实现多个接口（如排序和打印），我们有能力使一个数据项转换为多种类型（`Sequence`，`sort.IntSlice` 和`[]int`），每个都能做这项工作的某些部分。在实践中，这种情况较不常见，但有效。

### 接口转换和类型断言

[类型转换](https://golang.org/doc/effective_go#type_switch)是转换的一种形式：它们采用一个接口，并且对于转换中的每种情况，在某种意义上都将其转换为该情况的类型。这是下面的代码如何在`fmt.Printf`中使用类型转换将值转换为字符串的简化版本。如果已经是字符串，则我们希望接口保留实际的字符串值，而如果它具有 `String`方法，则需要调用该方法的结果。

```go
type Stringer interface {
    String() string
}

var value interface{} // Value provided by caller.
switch str := value.(type) {
case string:
    return str
case Stringer:
    return str.String()
}
```

第一种情况找到了具体值。第二个将接口转换为另一个接口。这样对混合类型就很好。

如果我们只关心一种类型该怎么办？如果我们知道该值包含`string` 而我们只想提取它？单例类型开关可以，类型断言也可以。类型断言采用接口值并从中提取指定的显式类型的值。该语法借鉴于开启类型转换的子句，但必须具有显式类型而不是`type`关键字：

```go
value.(typeName)
```

结果是具有`typeName`静态类型的新值。该类型必须是接口保留的具体类型，也可以是可以将值转换为的第二种接口类型。为了提取我们知道的值中的字符串，我们可以这样写：

```go
str := value.(string)
```

但是，如果事实证明该值不包含字符串，则程序将因运行时错误而崩溃。为了防止这种情况，请使用"comma, ok" 惯用法来安全地测试该值是否为字符串：

```go
str, ok := value.(string)
if ok {
    fmt.Printf("string value is: %q\n", str)
} else {
    fmt.Printf("value is not a string\n")
}
```

如果类型断言失败，`str`则该类型断言将仍然存在并且属于字符串类型，但是它将具有零值（一个空字符串）。

为了说明该功能，这里有一个`if`-`else` 语句，该语句等效于打开此部分的类型转换。

```go
if str, ok := value.(string); ok {
    return str
} else if str, ok := value.(Stringer); ok {
    return str.String()
}
```

### 概论

如果类型仅存在于实现接口，并且永远不会有超出该接口的导出方法，则无需导出类型本身。仅导出接口即可清楚地知道该值除了接口中描述的内容外没有其他有趣的方式。它还避免了需要在通用方法的每个实例上重复文档。

在这种情况下，构造函数应返回接口值而不是实现类型。作为一个例子，在散列库既`crc32.NewIEEE`和`adler32.New` 返回接口类型`hash.Hash32`。在Go程序中将CRC-32算法替换为Adler-32，仅需要更改构造函数调用即可；其余代码不受算法更改的影响。

一种相似的方法允许将各个`crypto`包中的流密码算法与它们链接在一起的分组密码分开。包中的`Block`接口中的`crypto/cipher`包指定了分组密码的行为，该行为提供了单个数据块的加密。然后，类似于`bufio`包，实现该接口的密码包可用于构造`Stream`接口表示的流式密码，而无需了解块加密的详细信息。

该 `crypto/cipher`接口是这样的：

```go
type Block interface {
    BlockSize() int
    Encrypt(dst, src []byte)
    Decrypt(dst, src []byte)
}

type Stream interface {
    XORKeyStream(dst, src []byte)
}
```

这是计数器模式（CTR）流的定义，它将块密码转换为流密码。注意，分组密码的详细信息已被删除：

```go
// NewCTR返回一个Stream，该Stream使用中的给定Block进行加密/解密
//计数器模式。iv的长度必须与块的块大小相同。
func NewCTR(block Block, iv []byte) Stream
```

`NewCTR`不仅适用于一种特定的加密算法和数据源，而且适用于`Block`接口的任何以及任何 `Stream`的实现。因为它们返回接口值，所以用其他加密模式替换CTR加密是本地化的更改。构造函数调用必须进行编辑，但是由于周围的代码必须仅将结果视为a `Stream`，因此不会注意到差异。

### 接口和方法

由于几乎所有内容都可以附加方法，因此几乎所有内容都可以满足接口。包中有一个说明性示例`http` ，它定义了`Handler`接口。任何实现`Handler`方法的对象都可以处理HTTP请求。

```go
type Handler interface {
    ServeHTTP(ResponseWriter, *Request)
}
```

`ResponseWriter`本身是一个接口，对将响应返回给客户端所需的方法提供了访问。这些方法包括标准`Write`方法，因此 `http.ResponseWriter`可以在可以使用`io.Writer`的任何地方使用 。 `Request`是一个包含解析来自客户端的请求的结构。

为简便起见，让我们忽略POST，并假设HTTP请求始终是GET；简化不会影响处理程序的设置方式。这是处理程序的简单实现，用于计算访问页面的次数。

```go
//简单的计数器服务器。
type Counter struct {
    n int
}

func (ctr *Counter) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    ctr.n++
    fmt.Fprintf(w, "counter = %d\n", ctr.n)
}
```

（注意我们的主题，注意`Fprintf`如何打印到 `http.ResponseWriter`。）在真实服务器中，访问`ctr.n`需要防止并发访问。请参阅`sync`和`atomic`软件包以获取建议。

下例供参考，这里是如何将这样的服务器附加到URL树上的节点。

```go
import "net/http"
...
ctr := new(Counter)
http.Handle("/counter", ctr)
```

但是为什么要构造一个`Counter`结构？整数就足够了。（接收方必须是一个指针，这样增量才能对调用方可见。）

```go
//更简单的计数器服务器。
type Counter int

func (ctr *Counter) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    *ctr++
    fmt.Fprintf(w, "counter = %d\n", *ctr)
}
```

如果您的程序有一些内部状态需要通知已访问页面怎么办？那就将通道绑定到网页。

```go
//每次访问都会发送通知的渠道。
//（可能希望通道被缓冲。）
type Chan chan *http.Request

func (ch Chan) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    ch <- req
    fmt.Fprint(w, "notification sent")
}
```

最后，假设我们要介绍`/args`调用服务器二进制文件时使用的参数。编写函数以打印参数就很容易了。

```
func ArgServer（）{
    fmt.Println（os.Args）
}
```

我们如何将其变成HTTP服务器？我们可以使用 某种类型的`ArgServer`方法来忽略其值，但是有一种更简洁的方法。由于我们可以为除指针和接口之外的任何类型定义方法，因此我们可以为函数编写方法。该`http`包包含以下代码：

```go
// HandlerFunc类型是一个适配器，允许使用
//作为HTTP处理程序的普通函数。如果f是一个函数
//具有适当的签名，HandlerFunc（f）是一个
//调用f的处理程序对象。
type HandlerFunc func(ResponseWriter, *Request)

// ServeHTTP calls f(w, req).
func (f HandlerFunc) ServeHTTP(w ResponseWriter, req *Request) {
    f(w, req)
}
```

`HandlerFunc`是具有方法的类型`ServeHTTP`，因此该类型的值可以处理HTTP请求。看一下该方法的实现：接收者是一个函数`f`，并且该方法调用`f`。这看起来可能很奇怪，但是接收方是通道和在该通道上发送方法没有什么不同。

为了`ArgServer`成为HTTP服务器，我们首先将其修改为具有正确的签名。

```go
// Argument server.
func ArgServer(w http.ResponseWriter, req *http.Request) {
    fmt.Fprintln(w, os.Args)
}
```

`ArgServer`现在有相同的签名`HandlerFunc`，因此它可以被转换成该类型来访问它的方法，就像我们转换`Sequence`到`IntSlice` 访问`IntSlice.Sort`。设置它的代码很简洁：

```go
http.Handle("/args", http.HandlerFunc(ArgServer))
```

当有人访问该页面时`/args`，安装在该页面上的处理程序具有值`ArgServer` 和类型`HandlerFunc`。HTTP服务器将以接收者身份调用`ServeHTTP` 类型的方法，将依次调用 `ArgServer`方法（通过`f(w, req)` 内部调用`HandlerFunc.ServeHTTP`）。然后将显示参数。

在本节中，我们由结构，整数，通道和函数组成了HTTP服务器，这都是因为接口只是方法集，几乎可以定义任何类型。

## 空白标识符

在[`for` `range`循环](https://golang.org/doc/effective_go#for) 和[map](https://golang.org/doc/effective_go#maps)的文章中，我们已经多次提到了空白标识符 。可以使用任何类型的任何值来分配或声明空白标识符，并且可以无害地丢弃该值。这有点像写入Unix`/dev/null`文件：它表示只写值，用作需要变量但实际值无关的占位符。它的用途超出了我们已经看到的范围。

### 多重分配中的空白标识符

在`for` `range`循环中使用空白标识符是一般情况的一种特殊情况：多重分配。

如果赋值在左侧需要多个值，但是程序不会使用其中一个值，则赋值左侧的空白标识符可以避免创建虚拟变量的需要，并明确说明：该值将被丢弃。例如，当调用返回一个值和一个错误但仅错误重要的函数时，请使用空白标识符丢弃不相关的值。

```go
if _, err := os.Stat(path); os.IsNotExist(err) {
	fmt.Printf("%s does not exist\n", path)
}
```

有时，您会看到丢弃该错误值以忽略该错误的代码。这是可怕的做法。始终检查错误返回；提供这些错误是有原因的。

```go
// Bad! This code will crash if path does not exist.
fi, _ := os.Stat(path)
if fi.IsDir() {
    fmt.Printf("%s is a directory\n", path)
}
```

### 未使用的导入和变量

导入包或声明变量而不使用它是错误的。未使用的导入会使程序臃肿，并且编译缓慢，而已初始化但未使用的变量至少会浪费计算量，并且可能表明有一个较大的错误。但是，当程序正在积极开发中时，经常会出现未使用的导入和变量，并且为了继续进行编译而删除它们，而稍后又需要它们，可能会很烦人。空白标识符提供了一种解决方法。

这个写得一半的程序有两个未使用的导入（`fmt`和`io`）和一个未使用的变量（`fd`），因此不能编译，但是看到到目前为止的代码是否正确会很放心。

```go
package main

import (
    "fmt"
    "io"
    "log"
    "os"
)

func main() {
    fd, err := os.Open("test.go")
    if err != nil {
        log.Fatal(err)
    }
    // TODO: use fd.
}
```

要使对未使用输入包的报错保持沉默，请使用空白标识符来引用引入包中的符号。同样，将未使用的变量`fd`分配给空白标识符也将使未使用的变量错误消失。该版本的程序久可以编译。

```go
package main

import (
    "fmt"
    "io"
    "log"
    "os"
)

var _ = fmt.Printf // For debugging; delete when done.
var _ io.Reader    // For debugging; delete when done.

func main() {
    fd, err := os.Open("test.go")
    if err != nil {
        log.Fatal(err)
    }
    // TODO: use fd.
    _ = fd
}
```

按照惯例，使导入错误沉默的全局声明应在导入之后立即写出并进行注释，以使其易于查找，并提醒以后进行清理。

### 导入副作用

像在前面的例子，未使用的导入包`fmt`或`io`应该最终被删除或被分配给空白识别码使用。但是有时仅用于副作用导入包是可以的，而无需任何显式使用。例如， `net/http/pprof`包在其`init`功能期间注册提供调试信息的HTTP处理程序。它具有导出的API，但是大多数客户端只需要注册处理程序，即可通过网页访问数据。仅用其边缘作用而导入软件包，请将软件包重命名为空白标识符：

```
import _ "net/http/pprof"
```

这种导入形式清楚地表明该软件包是出于其副作用而被导入的，因为该软件包没有其他可能的用途：在此文件中，它没有名称。（如果这样做，并且我们没有使用该名称，则编译器将拒绝该程序。）

### 接口检查

正如我们在上面关于[接口](https://golang.org/doc/effective_go#interfaces_and_types)的讨论中所看到的，类型不必显式声明它实现了接口。相反，类型仅通过实现接口的方法来实现接口。实际上，大多数接口转换都是静态的，因此在编译时进行检查。例如，除非实现接口，否则将`*os.File`传递给期望`io.Reader`的函数将不会编译 。

但是，某些接口检查确实在运行时发生。`encoding/json` 包中有一个实例，它定义了一个`Marshaler` 接口。当JSON编码器收到实现该接口的值时，编码器将调用该值的编组方法将其转换为JSON，而不是执行标准转换。编码器在运行时使用以下[类型断言](https://golang.org/doc/effective_go#interface_conversions)检查此属性：

```go
m, ok := val.(json.Marshaler)
```

如果只需要m, ok := val.(json.Marshaler)询问某个类型是否实现了一个接口，而不实际使用接口本身（也许作为错误检查的一部分），则可以使用空白标识符忽略类型声明的值：

```go
if _, ok := val.(json.Marshaler); ok {
    fmt.Printf("value %v of type %T implements json.Marshaler\n", val, val)
}
```

这种情况出现有必要在实现该接口的类型的包确保该类型可以实施时。如果某个类型（例如`json.RawMessage`）需要自定义JSON表示形式，则应实现 `json.Marshaler`，但是没有静态转换会导致编译器自动对此进行验证。如果类型不满足该接口，则JSON编码器仍将起作用，但将不使用自定义实现。为了确保实现正确，可以在包中使用使用空白标识符的全局声明：

```go
var _ json.Marshaler = (*RawMessage)(nil)
```

在这个声明中，涉及的转换分配 `*RawMessage`到`Marshaler` 需要`*RawMessage`实现`Marshaler`方法，并且该属性将在编译时进行检查。如果`json.Marshaler`接口发生更改，则此软件包将不再编译，我们将注意到需要对其进行更新。

在此构造中出现空白标识符表示该声明仅存在于类型检查中，而不用于创建变量。但是，请不要对满足接口的每种类型执行此操作。按照惯例，只有在代码中不存在静态转换的情况下才使用此类声明，而这种情况很少见。

## 嵌入

Go没有提供典型的类型驱动的子类化概念，但是它确实具有通过*将*类型*嵌入*结构或接口中来“借用”实现的各个部分的能力。

接口嵌入非常简单。我们之前提到过`io.Reader`and`io.Writer`接口；这是他们的定义。

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}
```

该`io`包导出了几个其他接口，他们指定可以实现几个这样的方法的对象。例如，`io.ReadWriter`是同时包含`Read`和`Write`的接口。我们可以`io.ReadWriter`通过显式列出这两种方法来指定，但是嵌入这两种接口以形成新的接口更容易，也更具启发性，如下所示：

```go
// ReadWriter is the interface that combines the Reader and Writer interfaces.
type ReadWriter interface {
    Reader
    Writer
}
```

这恰好表示正如它名称所示：一个`ReadWriter`可以做`Reader`*和*`Writer` 能做的; 它是嵌入式接口的并集。只有接口可以嵌入接口中。

相同的基本思想适用于结构，但意义更深远。`bufio`包具有两个结构类型， `bufio.Reader`和`bufio.Writer`，其中每一个都能实现来自 `io`包的类似接口。并且`bufio`还实现了缓冲的读取器/写入器，该操作通过使用嵌入将读取器和写入器组合为一个结构来完成：它列出了结构中的类型，但未提供字段名称。

```go
// ReadWriter存储指向Reader和Writer的指针。
//实现io.ReadWriter。
type ReadWriter struct {
    *Reader  // *bufio.Reader
    *Writer  // *bufio.Writer
}
```

嵌入的元素是指向结构的指针，当然，必须先将其初始化为指向有效结构，然后才能使用它们。该`ReadWriter`结构可以写成

```go
type ReadWriter struct {
    reader *Reader
    writer *Writer
}
```

但是为了提升字段的方法并满足`io`接口要求，我们还需要提供转发方法，如下所示：

```go
func (rw *ReadWriter) Read(p []byte) (n int, err error) {
    return rw.reader.Read(p)
}
```

通过直接嵌入结构，我们避免了统计。嵌入式类型的方法使用起来很方便，这意味着`bufio.ReadWriter` 不仅具有方法`bufio.Reader`和`bufio.Writer`，同时也满足了所有三个接口： `io.Reader`， `io.Writer`，和 `io.ReadWriter`。

嵌入和子类的不同是重要的方法。当我们嵌入一个类型时，该类型的方法成为外部类型的方法，但是当它们被调用时，该方法的接收者是内部类型，而不是外部类型。在我们的示例中，调用`bufio.ReadWriter`的方法read时，其效果与上面写出的转发方法完全相同；接收者是`ReadWriter`的`reader`字段，而不是 `ReadWriter`本身。

嵌入也可以很方便。此示例显示了一个嵌入的字段以及一个常规的命名字段。

```go
type Job struct {
    Command string
    *log.Logger
}
```

该`Job`类型现在有`Print`，`Printf`，`Println` 和`*log.Logger`的方法。当然，我们可以给出一个`Logger` 字段名，但是没有必要这样做。现在，一旦初始化，我们就可以登录到`Job`：

```
job.Println("starting now...")
```

该`Logger`是有Job结构的常规字段，所以我们可以用通常的方法进行初始化`Job`构造函数，像这样，

```
func NewJob（命令字符串，logger * log.Logger）* Job {
    返回＆Job {command，logger}
}
```

或使用复合文字，

```
job：=＆Job {command，log.New（os.Stderr，“ Job：”，log.Ldate）}
```

如果我们需要直接引用一个嵌入式字段，则该字段的类型名称（忽略包限定符）将用作字段名称，就像`Read`方法中的`ReadWriter`结构体。在这，如果我们需要访问 `*log.Logger`中的`Job`变量，我们会写`job.Logger`，这对我们想要改进`Logger`方法是很有益的。

```go
func (job *Job) Printf(format string, args ...interface{}) {
    job.Logger.Printf("%q: %s", job.Command, fmt.Sprintf(format, args...))
}
```

嵌入类型引入了名称冲突的问题，但是解决它们的规则很简单。首先，一个字段或方法`X`将`x`的其他项目隐藏在该类型的更深层嵌套的部分中。如果`log.Logger`包含名为`Command`的字段或方法，则`Job`的`Command`字段将占据主导地位。

其次，如果相同的名称出现在相同的嵌套级别，那通常是错误的。如果`Job`结构体包含另一个称为`log.Logger`的字段或方法，则嵌入`Logger`将是错误的。但是，如果是在类型定义之外的程序中从未提及和重复的名称，则是可以的。这种合理性为从外部嵌入的类型所做的更改提供了一些保护；如果添加的字段与另一个子类型中的另一个字段发生冲突（如果这两个字段都不曾使用过），那这样则没有问题。

## 并发

### 通过交互共享

并发编程是热度很大的主题，这里有一些Go特有的亮点。

实现对共享变量的正确访问的精妙之处使得在许多环境中进行并行编程变得很困难。Go鼓励采用一种不同的方法，即共享值在通道上传递，并且决不由单独的执行线程主动共享。在任何给定时间，只有一个goroutine可以访问该值。根据设计，不会发生数据争用。为了鼓励这种思维方式，我们将其简化为一个标语：

> 不要通过共享内存进行通信；而是通过通信共享内存。

这种方法能够做到很多事。例如，最好通过在整数变量周围放置一个互斥锁来最好地完成引用计数。但是作为一种高级方法，使用通道来控制访问权限使编写清晰且正确的程序变得更加容易。

思考这种模型的方法就是考虑一个CPU上运行的典型单线程程序。它不需要同步原语。现在运行另一个这样的实例；它也不需要同步。现在让这两个人交流；如果通信是同步器，则仍然不需要其他同步。例如，Unix管道非常适合此模型。尽管Go的并发方法源自Hoare的通信顺序过程（CSP），但它也可以被视为Unix管道的类型安全的泛化。

### Goroutines

之所以称为*goroutine，*是因为现有的术语（线程，协程，进程等）传达了不准确的含义。goroutine有一个简单的模型：在同一地址空间中与其他goroutine同时执行的函数。它是轻量级的，仅比分配堆栈空间花费更多。而且堆栈从小开始，因此花费空间小，代价小，并且可以通过根据需要分配（和释放）堆存储来增大堆栈。

Goroutine被多路复用到多个OS线程上，因此，如果一个应阻塞，例如在等待I / O时，其他将继续运行。他们的设计隐藏了线程创建和管理的许多复杂性。

给函数或方法调用加上`go`关键字前缀，在新的goroutine中运行该调用。调用完成后，goroutine会静默退出。（效果类似于在后台运行命令的Unix Shell 。）

```go
go list.Sort()  // run list.Sort concurrently; don't wait for it.
```

函数文字在goroutine调用中可以派上用场。

```go
func Announce(message string, delay time.Duration) {
    go func() {
        time.Sleep(delay)
        fmt.Println(message)
    }()  // Note the parentheses - must call the function.
}
```

在Go中，函数文字是闭包：实现可确保函数所引用的变量只要处于活动状态就可以保留。

这些示例不太实用，因为这些函数无法发出完成信号的方式。为此，我们需要渠道。

### 通道

与映射一样，通道也用`make`分配，且结果值引用基础数据结构。如果提供了可选的整数参数，则它将设置通道的缓冲区大小。对于无缓冲或同步通道，默认值为零。

```go
ci := make(chan int)            // unbuffered channel of integers
cj := make(chan int, 0)         // unbuffered channel of integers
cs := make(chan *os.File, 100)  // buffered channel of pointers to Files
```

无缓冲通道将通信（值的交换）与同步相结合，从而确保两个计算（goroutines）处于可知状态。

有很多不错使用通道的惯用法。这是一个我们最初使用的例子。在上一节中，我们在后台启动了排序。通道允许启动goroutine等待排序完成。

```go
c：= make（chan int）//分配频道
//在goroutine中开始排序；完成后，在通道上发出信号。
go func（）{
    list.Sort（）
    c <-1 //发送信号；值无所谓。
}（）
doSomethingForAWhile（）
<-c //等待排序完成；丢弃发送的值。
```

接收器始终阻塞，直到有数据要接收为止。如果通道未缓冲，则发送方将阻塞，直到接收方收到该值为止。如果通道具有缓冲区，则直到将值复制到缓冲区为止，发送方的该值阻塞；否则，发送方才阻塞。如果缓冲区已满，则意味着等待直到某些接收器检索到一个值。

可以像信号灯一样使用缓冲的通道，例如以限制吞吐量。在此示例中，传入的请求被传递到`handle`，后者将值发送到通道，处理该请求，然后从通道接收一个值，以为下一个使用者准备“信号量”。通道缓冲区的容量将限制同时呼叫`process`的数量。

```go
var sem = make(chan int, MaxOutstanding)

func handle(r *Request) {
    sem <- 1    // Wait for active queue to drain.
    process(r)  // May take a long time.
    <-sem       // Done; enable next request to run.
}

func Serve(queue chan *Request) {
    for {
        req := <-queue
        go handle(req)  // Don't wait for handle to finish.
    }
}
```

一旦`MaxOutstanding`处理程序执行`process`完毕，任何其他处理程序都将阻止尝试发送到已填充满的通道缓冲区，直到现有处理程序之一处理完成并从缓冲区接收消息为止。

但是，这种设计有一个问题：即使每个请求的`MaxOutstanding` 随时都在运行，它也会为每个传入请求创建一个新的goroutine 。如此一来，如果请求太快，程序可能会消耗无限的资源。我们可以通过更改`Serve`以控制goroutine的创建来解决该问题。这是一个显而易见的解决方案，但是请注意，它有一个错误，我们将在之后修复它：

```go
func Serve(queue chan *Request) {
    for req := range queue {
        sem <- 1
        go func() {
            process(req) // Buggy; see explanation below.
            <-sem
        }()
    }
}
```

那个错误是在Go的`for`循环中，循环变量在每次迭代中都会重复使用，因此`req` 变量在所有goroutine中共享，那不是我们想要的。我们需要确保`req`在每个goroutine都是唯一的。这里有一种实现方法，将`req`值作为参数传递给goroutine中的闭包：

```go
func Serve(queue chan *Request) {
    for req := range queue {
        sem <- 1
        go func(req *Request) {
            process(req)
            <-sem
        }(req)
    }
}
```

将此版本与先前版本进行比较，以了解在声明和运行闭包方式上的差异。另 一个解决方案是仅创建一个具有相同名称的新变量，如下例所示：

```go
func Serve(queue chan *Request) {
    for req := range queue {
        sem <- 1
        go func(req *Request) {
            process(req)
            <-sem
        }(req)
    }
}
```

这样 写起来似乎很奇怪

```
req := req
```

但这在Go中是合法且惯用的。您将获得具有相同名称的变量的新版本，故意在本地隐藏循环变量，但每个goroutine均具有唯一性。

回到编写服务器的普遍问题，另一种管理资源的较好方法是启动所有从请求通道读取的固定数量的`handle`goroutine。goroutine的数量将同时限制调用`process`的数量。`Serve`函数还接受一个通知其退出的通道；启动goroutines后，它将阻止从该通道接收值。

```go
func handle(queue chan *Request) {
    for r := range queue {
        process(r)
    }
}

func Serve(clientRequests chan *Request, quit chan bool) {
    // Start handlers
    for i := 0; i < MaxOutstanding; i++ {
        go handle(clientRequests)
    }
    <-quit  // Wait to be told to exit.
}
```

### 通道的通道

Go的最重要属性之一是通道也是基础阶层的值，可以像其他通道一样进行分配和传递。此属性的常见用法是实现安全的并行多路分解。

在上一节的示例中，`handle`是请求的理想处理程序，但我们没有定义其处理的类型。如果该类型包括用于回应的通道，则每个客户端可以提供自己的响应路径。这是`Request`类型的图解定义。

```go
type Request struct {
    args        []int
    f           func([]int) int
    resultChan  chan int
}
```

客户端提供一个函数及其参数，以及在请求体内部接收答案的通道。

```go
func sum(a []int) (s int) {
    for _, v := range a {
        s += v
    }
    return
}

request := &Request{[]int{3, 4, 5}, sum, make(chan int)}
// Send request
clientRequests <- request
// Wait for response.
fmt.Printf("answer: %d\n", <-request.resultChan)
```

在服务器端，处理程序功能是唯一可更改的内容。

```go
func handle(queue chan *Request) {
    for req := range queue {
        req.resultChan <- req.f(req.args)
    }
}
```

要使它变为现实，显然还有很多工作要做，但是此代码是一个用于速率受限，并行，无阻塞RPC系统的框架，并且看不到互斥量。

### 并行化

这类方法的另一个应用是使多个CPU内核之间的计算并行化。如果可以将计算分解为可以独立执行的独立部分，则可以并行化计算，并存在每个部分完成时发出信号的通道。

假设我们要对一个项目的载体执行复杂的操作，并且对每个项目的操作的值都是独立的，如这个假想示例中所示。

```go
type Vector []float64

// Apply the operation to v[i], v[i+1] ... up to v[n-1].
func (v Vector) DoSome(i, n int, u Vector, c chan int) {
    for ; i < n; i++ {
        v[i] += u.Op(v[i])
    }
    c <- 1    // signal that this piece is done
}
```

我们以循环方式独立启动各个部分，每个CPU一个。他们可以按任何顺序完成，但这无关紧要。在启动所有goroutine之后，我们通过排空通道来对完成信号计数。

```go
const numCPU = 4 // number of CPU cores

func (v Vector) DoAll(u Vector) {
    c := make(chan int, numCPU)  // Buffering optional but sensible.
    for i := 0; i < numCPU; i++ {
        go v.DoSome(i*len(v)/numCPU, (i+1)*len(v)/numCPU, u, c)
    }
    // Drain the channel.
    for i := 0; i < numCPU; i++ {
        <-c    // wait for one task to complete
    }
    // All done.
}
```

可以为运行时探知哪个值合适，而不是为numCPU创建一个常量值。`runtime.NumCPU`函数 返回计算机中硬件CPU内核的数量，因此我们可以编写

```
var numCPU = runtime.NumCPU（）
```

还有一个 `runtime.GOMAXPROCS`函数，可以报告（或设置）Go程序可以同时运行的用户指定的内核数。它有`runtime.NumCPU`的默认值但可以通过设置类似名称的shell环境变量或使用正数调用该函数来覆盖它。用零调用它只是查询值。因此，如果我们想满足用户的资源请求，我们应该写

```go
var numCPU = runtime.GOMAXPROCS(0)
```

确保不要混淆并发的思想（将程序构造为独立执行的组件）和并行性，并发执行并行计算以提高多个CPU的效率。尽管Go的并发特性可以使一些问题易于并行计算，但Go是一种并发语言，而不是并行语言，并且并非所有并行化问题都适合Go的模型。有关区别的讨论，请参见此[博客文章中](https://blog.golang.org/2013/01/concurrency-is-not-parallelism.html)引用的示例 。

### 缓冲区泄漏

并发编程工具甚至可以使非并发方法更易表示。这是从RPC包中抽象出来的示例。客户端goroutine循环从某个来源（可能是网络）接收数据。为了避免分配和释放缓冲区，它会保留一个空闲列表，并使用一个缓冲的通道来表示它。如果通道为空，则会分配一个新的缓冲区。消息缓冲区准备就绪后，它将通过 `serverChan`发送到服务器。

```go
var freeList = make(chan *Buffer, 100)
var serverChan = make(chan *Buffer)

func client() {
    for {
        var b *Buffer
        // Grab a buffer if available; allocate if not.
        select {
        case b = <-freeList:
            // Got one; nothing more to do.
        default:
            // None free, so allocate a new one.
            b = new(Buffer)
        }
        load(b)              // Read next message from the net.
        serverChan <- b      // Send to server.
    }
}
```

服务器循环从客户端接收每个消息，对其进行处理，然后将缓冲区返回到空闲列表。

```go
func server() {
    for {
        b := <-serverChan    // Wait for work.
        process(b)
        // Reuse buffer if there's room.
        select {
        case freeList <- b:
            // Buffer on free list; nothing more to do.
        default:
            // Free list full, just carry on.
        }
    }
}
```

客户端尝试从`freeList`中检索缓冲区；如果没有可用的，它将分配一个新的。除非列表已满，否则服务器发送至`freeList`的发送值将放`b`回到空闲列表中，在这种情况下，缓冲区会放在底层上，以供垃圾收集器回收。（`default`语句中的`select`子句在没有额外情况下的准备就绪时执行，这意味着`selects`永不阻塞。）此实现仅依靠几行内容就建立了一个无泄漏存储桶列表，并依赖于缓冲通道和垃圾收集器进行记录。

## 错误

库的惯例必须经常向调用者返回某种错误指示。如前所述，Go的多值返回可以轻松地在正常返回值旁边返回详细的错误描述。使用此功能提供详细的错误信息是一种很好的方式。例如，正如我们将看到的那样，`os.Open`不仅会在失败时返回一个指针`nil`，还会返回一个描述错误原因的错误值。

按照惯例，错误的类型为`error`，是一个简单的内置接口。

```go
type error interface {
    Error() string
}
```

库作者可以自由地用一个更丰富的模型来实现此接口，从而不仅可以看到错误，而且可以提供一些上下文。如前所述，除了用通常的`*os.File` 返回值外，`os.Open`还返回错误值。如果文件成功打开，则错误将为`nil`，但是如果出现问题，它将包含 `os.PathError`：

```go
// PathError records an error and the operation and
// file path that caused it.
type PathError struct {
    Op string    // "open", "unlink", etc.
    Path string  // The associated file.
    Err error    // Returned by the system call.
}

func (e *PathError) Error() string {
    return e.Op + " " + e.Path + ": " + e.Err.Error()
}
```

`PathError`的会`Error`产生一个这样的字符串：

```go
open /etc/passwx: no such file or directory
```

错误包括有问题的文件名，操作以及所触发的操作系统错误，即使在导致错误的调用远未打印的情况下也有用。它比普通的“没有这样的文件或目录”提供更多信息。

在可行的情况下，错误字符串应标识其来源，例如通过使用前缀来命名产生错误的操作或程序包。例如，在 `image`包中，由于格式未知而导致解码错误的字符串表示形式是“ image：unknown format”。

在意拥有精确错误详细信息的调用者可以使用类型切换或类型断言来查找特定错误并提取详细信息。为此，`PathErrors` 可能包括检查内部`Err` 字段是否存在可恢复的故障。

```go
for try := 0; try < 2; try++ {
    file, err = os.Create(filename)
    if err == nil {
        return
    }
    if e, ok := err.(*os.PathError); ok && e.Err == syscall.ENOSPC {
        deleteTempFiles()  // Recover some space.
        continue
    }
    return
}
```

这里的第二条if语句是另一种[类型断言](https://golang.org/doc/effective_go#interface_conversions)。如果失败，`ok`则为false，`e` 为`nil`。如果成功， 则`ok`为true，表示错误的类型为`*os.PathError`，然后为`e`，我们可以检查该错误的更多信息。

### 混乱

向调用者报告错误的通常方法是返回一个 作为额外返回值的`error`。规范的 `Read`方法是一个众所周知的实例。它返回一个字节数和一个`error`。但是，如果错误无法恢复怎么办？有时程序根本无法继续。

为此，内置函数`panic` 实际上会创建一个运行时错误，该错误将使程序停止运行（但请参阅下一节）。该函数采用一个任意类型的参数（通常是字符串），以便在程序死亡时打印出来。这也是一种指示发生了不可能的事情的方法，例如退出无限循环。

```go
// A toy implementation of cube root using Newton's method.
func CubeRoot(x float64) float64 {
    z := x/3   // Arbitrary initial value
    for i := 0; i < 1e6; i++ {
        prevz := z
        z -= (z*z*z-x) / (3*z*z)
        if veryClose(z, prevz) {
            return z
        }
    }
    // A million iterations has not converged; something is wrong.
    panic(fmt.Sprintf("CubeRoot(%g) did not converge", x))
}
```

这只是一个示例，但实际的库函数应避免使用`panic`。如果问题可以掩盖或解决，最好还是让事情继续运行而不是取消整个程序。反例可能存在在初始化期间：如果该库确实无法进行设置，那么混乱是可以理解的。

```go
var user = os.Getenv("USER")

func init() {
    if user == "" {
        panic("no value for $USER")
    }
}
```

### 恢复

当`panic`被调用时（包括对运行时错误的隐式调用，例如，对切片进行索引编制索引或失败类型声明），它将立即停止当前函数的执行并开始展开goroutine的堆栈，并在此过程中运行所有延迟函数。如果解散指令到达goroutine栈的顶部，程序将终止。但是可以使用内置函数`recover`来重新获得对goroutine的控制并恢复正常执行。

调用将`recover`停止展开并返回传递给`panic`的参数。因为解散时运行的唯一代码是在延迟函数内部，所以`recover` 仅在延迟函数内部有用。

`recover`的应用情况之一是关闭服务器内部失败的goroutine，而不会杀死其他正在执行的goroutine。

```go
func server(workChan <-chan *Work) {
    for work := range workChan {
        go safelyDo(work)
    }
}

func safelyDo(work *Work) {
    defer func() {
        if err := recover(); err != nil {
            log.Println("work failed:", err)
        }
    }()
    do(work)
}
```

在此示例中，如果`do(work)`出现紧急情况，它将记录结果，并且goroutine将纯净退出而不会打扰其他程序。延迟的关闭过程中无需执行任何其他操作；调用`recover`就可以完全处理条件。

因为`recover`总是返回`nil`，除非直接从延迟函数调用，因此延迟代码可以调用本身使用的库例程，也就是他们本身使用`panic`和`recover`不会失败。例如，`safelyDo`中的deferred函数可能在调用之前先调用日志记录函数`recover`，并且该日志记录代码将不受混乱状态的影响。

有了我们的恢复模式，`do` 函数（及其调用的任何函数）都可以通过调用干净地摆脱任何不良`panic`情况。我们可以使用该方法来简化复杂软件中的错误处理。让我们看一下`regexp`软件包的理想版本，该版本通过`panic`调用本地错误类型来报告解析错误。这是`Error`，`error`方法和`Compile`函数的定义。

```go
// Error is the type of a parse error; it satisfies the error interface.
type Error string
func (e Error) Error() string {
    return string(e)
}

// error is a method of *Regexp that reports parsing errors by
// panicking with an Error.
func (regexp *Regexp) error(err string) {
    panic(Error(err))
}

// Compile returns a parsed representation of the regular expression.
func Compile(str string) (regexp *Regexp, err error) {
    regexp = new(Regexp)
    // doParse will panic if there is a parse error.
    defer func() {
        if e := recover(); e != nil {
            regexp = nil    // Clear return value.
            err = e.(Error) // Will re-panic if not a parse error.
        }
    }()
    return regexp.doParse(str), nil
}
```

如果`doParse`出现紧急情况，恢复块会将返回值设置为`nil`，因为延迟函数可以修改命名的返回值。然后分配`err`，它将通过断言其具有本地类型来检查问题是否为解析错误`Error`。如果不是这样，则类型声明将失败，从而导致运行时错误，该错误将继续展开堆栈，就像没有任何中断一样。此检查意味着，如果发生意外情况（例如索引超出范围），即使我们正在使用`panic`或`recover`处理解析错误，代码也将失败。

有了错误处理，`error`方法（因为它是绑定到类型的方法，所以它很适合和很自然，因为它具有与内置`error`类型相同的名称），可以很容易地报告解析错误，而不必担心展开解析堆栈用手：

```go
if pos == 0 {
    re.error("'*' illegal at start of expression")
}
```

尽管此模式很有用，但应仅在包内使用。 `Parse`将内部`panic`调用转化为 `error`值；它不会向其客户公开`panics` 。这是必须遵循的良好规则。

顺便一提，如果发生实际错误，re-panic会更改panic值。但是，原始故障和新故障都将显示在崩溃报告中，因此问题的根本原因仍然可见。因此，这种简单的re-panic方法通常足够用了-毕竟是崩溃。但是，如果您只想显示原始值，则可以编写更多代码来过滤意外的问题并使用原始错误re-panic。这留给读者练习。

## Web服务器

让我们完成一个完整的Go程序，一个Web服务器。这实际上是一种Web重复服务器。Google提供了一项服务`chart.apis.google.com`，它 可以将数据自动格式化为图表和图形。但是，很难以交互方式使用它，因为您需要将数据作为查询放入URL。这里的程序为一种数据形式提供了一个更好的接口：给定一小段文本，它会在图表服务器上调用以产生QR码，即编码文本的盒子矩阵。该图像可以用手机的摄像头捕获，并解释为如URL，从而省去了在手机的小键盘上键入URL的麻烦。

这是完整的程序。解释如下。

```go
package main

import (
    "flag"
    "html/template"
    "log"
    "net/http"
)

var addr = flag.String("addr", ":1718", "http service address") // Q=17, R=18

var templ = template.Must(template.New("qr").Parse(templateStr))

func main() {
    flag.Parse()
    http.Handle("/", http.HandlerFunc(QR))
    err := http.ListenAndServe(*addr, nil)
    if err != nil {
        log.Fatal("ListenAndServe:", err)
    }
}

func QR(w http.ResponseWriter, req *http.Request) {
    templ.Execute(w, req.FormValue("s"))
}

const templateStr = `
<html>
<head>
<title>QR Link Generator</title>
</head>
<body>
{{if .}}
<img src="http://chart.apis.google.com/chart?chs=300x300&cht=qr&choe=UTF-8&chl={{.}}" />
<br>
{{.}}
<br>
<br>
{{end}}
<form action="/" name=f method="GET">
    <input maxLength=1024 size=70 name=s value="" title="Text to QR Encode">
    <input type=submit value="Show QR" name=qr>
</form>
</body>
</html>
`
```

最多的`main`部分应该易于遵循。flag为我们的服务器设置默认的HTTP端口。模板变量`templ`是有趣的地方。它构建了一个HTML模板，该模板将由服务器执行以显示页面。稍后再详细说明。

`main`函数解析flag，并使用我们上面讨论的机制将QR函数绑定到服务器的根路径。然后`http.ListenAndServe`被引用启动服务器；服务器运行时会阻塞。

`QR`只会接收包含表单数据的请求，并以名为s的表单值对数据执行模板。

模板包`html/template`功能强大；该程序仅涉及其部分功能。本质上，它通过替换从传递给`templ.Execute`的数据项派生的元素（在本例中为表单值）来即时重写HTML文本。在模板文本（`templateStr`）中，用双括号分隔的段表示模板动作。仅当当前数据项的值（点）为非空时，从`{{if .}}`到才 `{{end}}`执行`.`。即，当字符串为空时，该模板部分被抑制。

这两个`{{.}}`片段表示要在网页上显示提供给模板的数据（查询字符串）。HTML模板包会自动提供适当的转义符，因此可以安全地显示文本。

模板字符串的其余部分只是页面加载时显示的HTML。如果解释太快，请参阅 模板包的[文档](https://golang.org/pkg/html/template/)以进行更全面的讨论。

在那里，您可以找到：一个有用的Web服务器，其中包含几行代码以及一些数据驱动的HTML文本。Go的功能强大到足以在几行中完成很多事情。