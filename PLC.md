## PLC(可编程序控制器)

### PLC的基本结构

微处理器(CPU)：是PLC的运算、控制中心。

存储器：用来存放PLC的系统程序、用户程序以及工作数据。

现场信号的输入输出接口：人们通过技术手段把各种各样的生产信息转变成模拟信号、开关量信号、数字量信号的形式，而PLC具备处理这三种形式信号的能力。

- 开关量输入接口：PLC与现场以开关量为输出形式的检测元件如操作按钮、行程开关等的连接通道
- 开关量输出接口：PLC与现场执行机构的连接通道。主要是继电器输出、晶闸管输出和晶体管输出三种形式。

I/O扩展接口：用于扩展PLC的功能和规模。

通信接口：实现与这些外部设备通信：编程器、通用计算机、PLC间通信、打印机和磁带机等。

电源：交流供电一般采用单相交流220V、直流供电一般24V。

### PLC的工作方式

扫描工作方式：依次对各种规定的操作项目进行访问和处理。

工作过程

- 以故障诊断和处理为主的公共操作
- 处理工业现场数据的I/O操作：输入采样、输出刷新
- 执行用户程序
- 外设命令操作

### PLC的编程元件及编程语言

图形化语言：梯形图、顺序功能图、功能块图。文本化语言：指令表、结构文本。

#### 梯形图简介

图形符号：接点类、线圈类、功能和功能块。

#### 指令表简介

其是一种低级语言，与汇编语言类似

结构：操作符与紧随其后的操作数组成，是所谓的面向累加器(Accu)的语言。

操作符：

- 一般操作符：装入指令：LD N和逻辑指令：AND N(与指令)等
- 运算及比较操作符：算术指令：ADD 加指令、SUB减指令；比较指令：GT 大于
- 跳转及调用操作符：跳转指令： JMP C，N；调用指令：CALL C,N

### 欧姆龙C系列PLC及其指令系统简介

各编程功能元件：

- 输入继电器
- 输出继电器
- 内部辅助继电器
- 特殊辅助继电器
- 暂时记忆继电器
- 保持继电器
- 辅助记忆继电器
- 链接继电器
- 定时器/计数器
- 数据存储器

CPM1A的编程指令

### 三菱FX<sub>2N</sub>系列PLC及其基本指令简介

基本组成：

- 输入继电器由X表示，输出继电器由Y表示
- 辅助继电器：特殊辅助继电器(M8000~M8255)

- 状态器(S)
- 定时器(T)
- 计数器(C0~C255)
- 数据寄存器(D)
- 指针：分支用指针(P)、中断用指针(I)

基本指令：LD、LDI、OUT指令、AND、ANI指令等

步进梯形指令

### 西门子S7-200PLC及其指令系统简介

PLC硬件的特点与功能：

- 集成的24V负载电源
- 不同的设备类型
- 数字量I/O点
- 模拟量I/O点
- 中断输入
- 高速计数器
- 扩展
- 模拟电位器
- 脉冲输出
- 实时时钟
- EEPROM存储器模块
- 电池模块
- S7-200 CN系列PLC的CPU内部集成的PPI接口

#### 数据类型及存储器范围

数字量的输入与输出：数字量输入(I)、数字量输出(Q)

模拟量的输入与输出：模拟量输入-A表示模拟量，I表示输入。模拟量输出-Q表示输出。

变量：V表示变量存储区

标志位(辅助继电器)：存储中间变量或控制信息的存储区-M表示。

顺序控制存储区：配合顺序控制指令-S表示

特殊标志存储区：为用户提供一些特殊的控制功能和系统信息-SM表示

累加器存储区：向子程序传递参数，存放计算的中间值-AC表示

定时器：T表示

计数器：对输入脉冲进行累积，实现技术操作-C表示

高速计数器：需要高频计数时

局部变量存储器

#### 程序结构

线性化编程、分部式编程、结构化编程

S7-200采用的是线性化编程，有三部分构成：用户程序、数据块(DB1)、参数块。

#### 编程及组态软件

1、STEP-Micro/WIN 编程软件，专为S7-200系列开发的编程软件。

2、西门子工业控制组太软件WinCC，解决可视化任务提供的人机界面系统。

### PLC系统的分析、设计与应用

PLC系统分析的主要方法：文字叙述法、图形分析法、逻辑函数法

分析实例过程：

- 分析工艺过程、明确控制要求
- 分析PLC的I/O端子分配及I/O分配表
- PLC控制程序分析

PLC系统的设计：允许硬件电路和软件分开进行设计。

- 设计内容：控制系统的总体结构论证、PLC机型的选择、硬件电路设计、软件设计和组装调试。

#### PLC系统硬件电路设计

PLC型号的选择：

- 功能及结构

- 输入输出模块选择
- I/O点数的估计
- 所需内存容量的估算
- 响应时间

输入输出编号及地址配置

输入电路设计

输出电路设计：继电器输出、晶闸管输出、晶体管输出

#### PLC系统的软件编程

PLC的程序设计往往与硬件设计同时进行。

程序设计步骤：准备工作、程序框图设计、应用程序的编写、程序调试、编写程序说明书。
