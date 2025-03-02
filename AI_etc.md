# AI —简明介绍

## 初识 Artificial Intelligence

1、AI = Computational Intelligence (CI)   三个分支

1. Evolutionary Computation: Learning from nature  (population, generation, operator, fitness, etc) 
2. Artificial Neural Network: Learning from brain as white  box (neuron, layer, weighting, summation, activation, etc. ) 
3. Fuzzy Logic system: Learning from brain as black box  (fuzzy logic, natural language, membership function,  fuzzification, inference, defuzzify, etc. 

2、Machine Learning (ML) is a subset of AI that focuses on  the ability of enabling machines to 'learn' a task from  experience (data) without programming them specifically  about that task. 

This process starts with feeding them good quality data  and training the machines by building various models  using different algorithms.  

The choice of algorithms depends on the kind of task.

Generally, 3 types: **supervised learning, unsupervised  learning and reinforcement learning**

1）Deep Learning (DL) is a subset of ML. It enables processing  of data and creating predictions using ANN.  

This web-like structure of ANN is able to process data in a  non-linear approach, which is a significant advantage over  traditional algorithms.

![image-20240808173504954](figure/image-20240808173504954.png)



![image-20240808175523098](figure/image-20240808175523098.png)

2）Reinforcement learning (RL) is a part of ML in which the  machine learns something in a way that is similar to how  humans learn.  

As an example, assume that the machine is a student.  Here the hipothetical student learns from its own mistakes  over time through trial and error. 

The algorithm decides the next action by learning  behaviours that are based on its current state and that will  maximise the reward in the future.

3、Natural Language Processing

In Natural Language Processing (NLP) where machines  analyze and understand language and speech. There are  many subparts of NLP that deal with language such as  speech recognition, natural language generation, natural  language translation, etc.

4、Computer Vision

Computer Vision uses AI to extract information from  images. This information can be object detection in the  image, identification of image content to group various  images together, etc. 

An application of computer vision is navigation for  autonomous vehicles by analysing images of surroundings.

除了以上还有许多技术概念可归类于AI技术及应用，暂不赘述。

## ANN

1、what is ANN

Artificial Neural Networks (ANN) is an information  processing paradigm that is inspired by the way the  biological nervous system such as brain process  information.  

It is composed of large number of highly interconnected  processing elements (neurons) working in unison to solve  a specific problem. 

ANN is considered “Universal Function Approximators”. It  means they can learn and compute any function at all.

2、use

ANNs can be used for:  

 supervised learning (classification, regression)   

unsupervised learning (pattern recognition, clustering) 

Model parameters are set by weighting the neural  network through “learning” on training data, typically by  optimising weights to minimize prediction error

3、types

There are many types of neural networks available or that  might be in the development stage.  

They can be classified depending on their:  

Structure、 Data flow、Neurons and their density、Layers and their depth activation filters etc

![image-20240808175104673](figure/image-20240808175104673.png)

ANN包含的种类复杂，其在机器学习、自然语言处理、图像识别等领域应用繁多，可谓是必须掌握的技能。

# NLP学习

## 入门一

入门学习途径（B站一up主推荐）

1、MLP，多层感知机

2、Transformer (Yannic视频)

3、BERT(Yannic视频)

打开huggingface网站，加载bert，就可以入门研究NLP了。 

### 一、MLP

常用四类神经网络架构来抽取原始数据的特征，MLP、CNN、RNN、Transformers

![image-20240822105748391](figure/image-20240822105748391.png)

W权重，b偏移

1个隐藏层的MLP：输入—>Dense—>Activation—>Dense—>输出

第一个Dense是隐藏层（相对输出来看），在隐藏层后引入激活函数（如RELU或sigmod），使模型变为非线性模型

超参数：隐藏层和每个隐藏层的输出

### 二、Transformer

源自谷歌论文：“Attention is All you need”，本质上是不使用数据传递信息流，使用注意力机制，来预测下个输出的概率。

第一步，词嵌入，将每个词（符号）都对应一个token。其中词嵌入矩阵为权重参数，也包含位置编码

第二步，多头注意力机制，首先设置查询矩阵，将某个“上下文问题”编成查询向量，并根据查询“问题”，设置键索引向量，最后将Wq 与Wk 点积得出值，为保证后文token不会影响前文token，将查询向量与后文索引得出的值进行负无穷取值，这一过程成为masking，再进行softmax归一化，由此构造值向量，此为单头，GPT-3有96头，因此为多头注意力机制。

第三步，将第一层多头注意力机制，根据上下文得出的新 词嵌入向量，放入多层感知机等训练模型中进行处理，输出下一层的训练数据。GPT3共有96层。

最后一步，输出概率，对最后一词的向量进行 unembedding，其中unemdedding矩阵为权重参数，得出每个词的概率，再进行softmax进行归一化，得出输出值的概率向量。

### 三、BERT

双向，在注意力机制中考虑给定token的左右上下文

需要有一个预训练数据语料库

在分类、问答方面、实体识别等方面应用较多

### 四、Google中国NLP入门课

此为学以上三部分，顺带学习的内容。

1、分词：将句子编码为数据，token

tnesorflow 中用的 `from tensorflow.keras.preprocessing.text import Tokenizer`

```
sentences ={
'I love my dog
'I love my cat
}
tokenizer =Tokenizer(num_words =100)
tokenizer.fit_on_texts(sentences)
word index= tokenizer.word index
print(word index)
```

2、序列：为句子创建数据序列，然后用工具对其进行处理，从而为训练神经网络做准备

```
sequences = tokenizer.texts_to_sequences(sentences)
```

当有不在训练集的单词，可以加入 tokenizer =Tokenizer(num_words =100, 00v_token="<OOV>"),代替不认识的单词

处理不同长度句子，paddling

from tensorflow.keras.preprocessing.sequence import pad_sequences

padded = pad sequences(sequences)

3、识别文本情感的模型

嵌入（embedding）：词向量映射到多维坐标，并具有方向，将单词向量相加（池化），即可输出情感判断。

4、循环神经网络训练用于机器学习

RNN：  数据的递归，将上层输出的y值作为前馈数值给下一层。

5、长短期记忆网络 LSTM

LSTM（Long Short-Term Memory）长短期记忆网络，是一种时间递归神经网络，适合于处理和预测时间序列中间隔和延迟相对较长的重要事件。

![image-20240820113345173](figure/image-20240820113345173.png)

在LSTM中，第一阶段是遗忘门，遗忘层决定哪些信息需要从细胞 状态中被遗忘，下一阶段是输入门，输入门确定哪些新信息能够被存放到细胞状态中，最后一个阶段是输出门，输出门确定输出什么值。

## 入门二

NLP学习路径—LF吴彦祖-up主

1.数据预处理 

2.分词工具 jieba、HanLP等 

3.机器学习相关内容 梯度下降、优化器、损失函数等 ,吴恩达、莫凡

4.深度学习框架 tensorflow、pytorch（推荐） 

5.词向量的训练和嵌入 word2vec、glove等 

6.NLP相关模型 Bert及其变种，优先复现Bert原生代码 

7.复现模型，www.paperwithcode.com

### 一、手写AI相关课程

#### 机器学习基础

1、dataset与dataloader

batch_size:一次性取数据数量

epoch：轮次

shuffle:  布尔值 是否需要打乱

类的`def __iter`__ 与 `def __next__`魔术方法，用于range遍历类时，那个自定义遍历规则。

2、梯度下降

原问题：(x-2)&sup2;=0

输入：x=100 也叫初始值（初始值很重要，凯明初始化）

预测值：98&sup2;

真值（标签）:与预测值比较的值，此为0

差距：损失loss，与真值的差距，衡量差距的就是损失函数

LOSS = (pre - label)&sup2;   —均方误差loss（MSE）

对LOSS倒数的意义：指导输入下一次取值的大小和方向

学习率：控制下一次输入的大小（步长）

3、线性回归

拿数据预测 y=kx+b 中的k和b，要求数据满足线性函数要求

————————没看完视频失效，后续无。

#### onehot文本分类

这系列课主要为词向量的嵌入等，也包含部分模型的调试运行。

### 二、数据预处理

 

tokenization、构造词表、Token to index(lookup)、数据长度处理、Batch

1、tokenization（词元化）

词粒度划分的问题：巨大的词表、复合词汇、oov问题、不同语言的分隔符不同

字粒度划分的问题：缺乏能语义表达、使序列长度过长

subword划分：BPE算法

2、构造词表

使用预训练Embedding的词表，如Glove、Word2Vec

直接使用数据集统计词表

使用Subword训练得到的词表

3、数据长度处理

1）Pad+Truncate

操作简单1.指定一个max len 2.超过的长度截断 3.不够的pad
问题:1.长尾数据的完整性被破坏 2.额外的无效计算

2）分桶

![image-20241119150325075](figure/image-20241119150325075.png)

做分桶的同时又会打batch

### 三、分词工具

1、jieba

Jieba其实并不是只有分词这一个功能，其是一个[开源](https://edu.csdn.net/cloud/pm_summit?utm_source=blogglc)框架，提供了很多在分词之上的算法，如关键词提取、词性标注等。

具体学习等需要的时候去找官方文档。

在实际应用中，我们可以使用Python等编程语言来实现这些预处理步骤。例如，使用jieba分词库进行中文分词，使用spaCy进行英文分词和词干提取，使用NLTK等库去除停用词。

### 四、机器学习入门基础

#### （一）基础

1、基础概念类

1）监督学习：给一系列输入、输出标签训练数据来训练模型，来达到只给出输入标签，预测合适输出。

分为：回归与分类

回归是预测出无数可能值中最合适的值，分类是预测一小部分可能值（离散有限的 ）中最符合的值。

2）无监督学习

输入未标记的数据，通过算法，给出输入数据的特征等、规律等

分类：聚类 

2、线性回归模型

target、prediction

f = wx+b

代价或成本函数cost function： J = (标签值y-预测值y~) 的平方误差之和除以2m(m为训练集大小) ——均方误差

![image-20241125153047746](figure/image-20241125153047746.png)

使用成本函数找到使 J 最小化的w、b值

线性回归的目标是找到参数w或w和b，使成本函数J的值最小。

1）可视化

3D碗形曲线图可视化为等高线图

3、梯度下降

梯度下降算法一般是训练神经网络模型常用的成本算法

学习率和成本函数求导，各自都有意义，学习率就是步长，控制参数取值，避免拟合过慢或者无法收敛；对成本函数求导是为了知道参数取值向最优值靠拢，控制变化方向和大小。

batch梯度下降，每次更新都会更新下数据集，因此为batch

4、多类特征

f =w_1 x1+w_2 x2+...+w_n xn + b  多元线性回归

矢量化，增加运算效率，比如numpy

多元线性回归的梯度下降

5、特征缩放（feature scaling）

当你有不同的功能，具有非常不同的值范围，其会导致梯度下降运行缓慢，重新缩放不同特征，可以加快梯度下降。

除以最大值、平均值缩放、Z-score标准化，尽量将特征值缩放到-1~1等小区间上

6、检查梯度下降是否收敛

学习曲线，横轴是迭代次数、纵轴是成本函数

一般按小选择，如果成本函数下降缓慢，则增大，比如三倍

7、特征工程

转换或组合一些特征（指 x值），形成新特征，以便形成更好的模型效果。

8、逻辑回归

用于完成分类的算法

**sigmoid函数**

![image-20241127152731811](figure/image-20241127152731811.png)

逻辑回归模型

![image-20241127153126626](figure/image-20241127153126626.png)

1）成本函数

![image-20241127160949526](figure/image-20241127160949526.png)

2）逻辑回归的梯度下降

fw_b(x_i)趋于零，则 loss函数趋于零，loss函数表示单个训练例子中的损失。

成本函数可看成均值loss

9、过拟合（高方差）

训练的模型泛化性弱，只匹配数据集，对于新数据预测不到位

决策边界，来划分是否归类于postive类还是negative类

1）解决方法

- 可以增加训练数据集
- 进行数据特征选择，去掉一些特征数据，防止过拟合
- 正则化，减小权重参数大小

10、正则化的使用

1）正则化成本函数

![image-20241129111629883](figure/image-20241129111629883.png)

成本函数加上的式子是惩罚机制，当一开始不知道哪个参数特征更重要时，就采取惩罚机制进行筛选，使w参数更小。

2）正则化线性回归、逻辑回归

#### （二）高级学习算法

1、神经网络（深度学习）基础介绍

发展历程：语音识别、图像识别、NLP、多模态（推荐、在线广告推送）

输入层、隐藏层（Dense）、输出层，每层的输出学术上称为激活值（a）

神经网络根据数据集通过训练自动学习隐藏层的功能特征，达到符合输出的模型。

多层感知机MLP就是早期神经网络模型

2、神经网络的层

每一层都有不同的神经元，每个神经元都有自己的w，来计算输出激活向量

神经网络的层数不包含输入层，但包含输出层、隐藏层

激活函数就是输出这些激活值的函数

![image-20241129175233515](figure/image-20241129175233515.png)

3、前向传播

每一层都向前推进，不断得出不同的激活值，最终输出的神经网络为前向传播算法

tenseflow实现前向传播，四层前向传播算法（这是大致实现思路）

```
x=np.array([280，17])
W= np.array([[1,-3,5],[-2,4,6]])
b=np.array([-1，1，2])
def dense(a in,w,b):
	a_out =np.zeros(units)
	for j in range(units):
		w = W[:,j]
		z = np.dot(w,x)+ b[j]
		a_out[j]= g(z)
	return a_out
def sequential(x):
	a1 =dense(x,W1，b1)
	a2 = dense(a1,W2，b2)
	a3 = dense(a2, w3, b3)
	a4 = dense(a3,W4,b4)
	f_x= a4
	return f_x
```

4、神经网络高效执行

向量化，利用GPU等硬件，使用numpy等框架，实现矩阵运算等高速运算。

```
x=np.array([[280，17]])
W= np.array([[1,-3,5],[-2,4,6]])
b=np.array([[-1，1，2]])
def dense(A_in,w,B):
	Z=np.matmul(A_in,W) + B
	A_out = g(Z)
	return A_out
```

5、TensorFlow实现

二元交叉熵函数（也是逻辑回归的loss函数）：
$$
L(f(x),y)=-ylog(f(x))-(1-y)log(1-f(x))
$$
模型训练过程：

![image-20241203114017744](figure/image-20241203114017744.png)

神经网络的采用反向传播的方法实现成本函数的最小化

6、激活函数

1）ReLU激活函数
$$
g(z)=max(0,z)
$$
2）激活函数选择

输出层选择激活函数，取决于输出y的大致结果，如果是二分类0或1，则使用sigmod，如果有正有负，则使用线性激活函数g(z)=z，如果只能为正值，则用RELU。

隐藏层基本都选择ReLU作为激活函数，因为sigmod函数计算较慢，且存在两个方向的平缓，会导致梯度下降计算更慢，总得来说就是效率更低

3）为什么要使用激活函数

如果不会用激活函数，或只使用线性激活函数，则神经网络的隐藏层基本都是线性表达，无法拟合更复杂的非线性等函数，因此需要使用ReLU或sigmod激活函数

7、多类multiclass classification problem

即输出y不只有0或1两类，需要对应多种类别的分类问题

softmax回归，用来解决多类问题

![image-20241203165857005](figure/image-20241203165857005.png)

loss函数：
$$
loss(a_1,...,a_2,y)= -loga_1 、if y = 1, or, -loga_2 、if y=2 ,...., or ，-loga_n 、 if y=n
$$

由于softmax回归输出时，会有计算误差，因此，在实际代码中，会将输出层定为线性激活函数，最后计算成本函数时，再将softmax引入，以此来减少误差。

8、多标签分类

对不同标签的分类问题统一输出，如机器视觉识别出是否有行人、汽车和公交车。而多类分类是输出同标签不同概率，最后来判断属于哪一类。

9、Adam优化算法（ Adaptive Moment estimation）

一种自动调整学习率，以保证成本函数最小化的算法

10、其他类型的神经网络层

上述描述的神经网络都是Dense密集层

卷积层

每个神经元只关注部分上一层的输入，参数包含输入窗口、每层神经元个数等

11、机器学习模型评估

1）划分训练集和测试集

模型训练完后，使用成本函数，来评判训练集和测试集的分数，或者看预测正确的分数（分类问题上）

2）模型选择——划分训练集、交叉验证集（验证集、开发集）、测试集

采用交叉验证集进行模型选择(如特征阶数、正则化参数等)，测试集专注于评估模型泛化误差

12、诊断偏差和方差

![image-20241204153942183](figure/image-20241204153942183.png)

1)  High bias(underfit)

 J_train will be high
$$
J_{train} \approx J_{cv}
$$
2)  High variance(overfit)
$$
J_{cv} >> J_{trian}
$$
 (J_train may be low)

3) High bias and High variance

 J_train will be high and
$$
J_{cv} >> J_{train}
$$
13、正则化和方差与偏差

对已形成的模型，设定正则化，但参数λ不确定，使用交叉验证集选择合适的正则化参数λ，避免高偏差和高方差

14、学习曲线 learning curves

横坐标是数据集大小，纵坐标是误差

学习曲线在 表现基准线 的基础上，通过曲线走势表明当前训练模型的偏差和方差是否合理·。

对于模型高方差和高偏差的问题解决思路可参考如下：

![image-20241205145912324](figure/image-20241205145912324.png)

15、神经网络中的偏差与方差

神经网络中一般都是低偏差的，如果出现偏差较高，增大网络结构，都能降低，一般都是高方差问题。

![image-20241205151510384](figure/image-20241205151510384.png)

16、机器学习的迭代发展

![image-20241205154649848](figure/image-20241205154649848.png)

其中误差分析是对出现问题（分类出错，预测错误）的数据进行分析，分为不同主题，挑选重点方向，进行优化，决定下一步方向   

17、数据的添加

1）专注于对模型有用的数据添加，比如错误分析中得出某类数据误差较大，则可添加该类数据

2）数据增强，模仿训练集创造新的训练集。图像识别和语音识别可运用数据增强，将原有数据进行微微调整（如字母倒转、扭曲、模糊，语音加上背景噪音），扩展训练数据集

3）数据合成，人工生成符合的数据集，一般用于计算机视觉

18、迁移学习

用大量自身任务无关的数据训练好初步模型，再将模型进行调整，如改变输出层，再在预训练好的模型上用自己的小数据集进行训练，实现微调，得出符合自己要求的模型。

在用自身数据训练时，可选择只调整训练输出层参数，也可重新训练所有参数。

也可以采用别人训练好的模型作为预训练模型，但输入要相同，因为能实现迁移的底层原因是模型有共通之处。

19、机器学习的项目周期

![image-20241205173808661](figure/image-20241205173808661.png)

部署涉及到的新领域：MLOps(machine learning operations)

20、处理倾斜数据集

当某个数据集中，y为1的数据很少，形成数据倾斜。

采取精确率和召回率来表征学习算法的性能

pression = True postive / #predicted postive

recall = True postive / #actual postive

F1值
$$
F1 = 1/(1/2(1/pression+1/recall))
$$

$$
= 2PR/(P+R)
$$

21、决策树

1）概览

根节点、决策节点、叶节点

在这需要考虑的问题主要是：1)如何根据特性划分根节点？如最大纯度 2）如何停止分裂？如：当节点全是同种类、到达最大深度、往下划分低于阈值、样例太少等

2）测量纯度

利用熵来表示数据纯度，具体方程：
$$
H(P_1)=-p_1log_2(p_1)-(1-p_1)log_2(1-p_1)
$$
p1为样例中表示为符合正值的比例。熵越高数据杂度越高

3）信息增益

分裂后熵减少最多的信息增益，则为更好的决策节点

![image-20241206153524346](figure/image-20241206153524346.png)

4）决策树训练过程

- 根据训练集，从根节点开始训练
- 计算所有特征分类的信息增益，选择最高者作为下一节点
- 再将样例数据集分配到左右节点，继续选择特征，重复信息增益计算过程，直到达到标准
- 标准有：当节点只有一类时，可停止；超过最大深度；信息增益增加低于阈值；节点样例集低于阈值

5）one-hot热编码

当某特征出现K（>2）个的分类值，则可创造K个二元特征值（取值为0或1）

6）输入有连续值

这时，通过取特定值，转化为离散输入，计算信息增益，哪个最大则选某特定值作为其阈值分割

7）决策树的集合

单个决策树对数据变动非常敏感，因此采用较多的决策树来投票预测结果，使算法更加健壮

22、回归树算法

输出是预测数字，则在选择特征进行拆分时，计算预测值的方差，使用加权平均方差作为标准，当方差的减少最多则为最好的特征

23、随机森林

1）放回抽样

根据原有样例，每次取不同数据，组成新的数据集，这样就能构造不同的训练数据集

2）原理

- 利用放回抽样将数据集进行微小变化，将变化的数据用来训练单个决策树，训练几十次或上百次。
- 其中，在每次单个决策树训练时，并不给所有特征（n个）进行训练，而是选取 k（k<n，一般取根号n）个特征，来进行训练，来消除关联性、减少计算量、更好地方过拟合

24、XGBoost 

eXtreme Gradient Boosting

这个算法的改进是：在进行放回抽样的过程发生变化，下一次训练集生成，选择每个样例的概率不是相等的，在每一次迭代训练决策树后，对没有很好训练的例样例给予更高的权重，权重各不相同，以此不断迭代。

25、什么时候选择决策树？

决策树

- 适合结构化数据，不适合非结构化数据
- 训练更快
- 小决策树的可解释性高

神经网络

- 适合所有类型数据
- 训练更慢
- 能和迁移学习一起使用，用于预训练
- 多个神经网络模型结合可组成跟强大的机器学习系统

#### （三）无监督学习

1、聚类

步骤1：随机选取几点作为簇群的质心（centriods），计算所有点到各个质心的距离，离某质心最近的归于其簇群

步骤2：计算刚分出的同一簇群的平均距离，将刚才质心移动到此点，每个簇群都做这样的处理

重复以上两个步骤，直到收敛。

### 五、pytorch框架学习

（一）基础

pytorch

