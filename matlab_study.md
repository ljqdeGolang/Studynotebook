### matlab学习笔记

#### 杂记

1、注释：%  或%%

2、char(65)、num2str(65)、abs(a)

3、C=A(:)，拉长A矩阵成一列。D=inv(A)，求逆。E=zeros(10,5,3),构造三个维度的10行5列的0矩阵。

rand()、randi()、randn()构造随机数，三个的具体区别后面要用到时再去查询。

4、元胞数组，数组的一种，内部元素可以属于不同的数据类型。

定义：A=cell(1,6)，感觉类似python的元组

A{2}=eye(3)，第二个位置放入三阶单位矩阵。A{5}=magic(5)，第五个位置放入一个五阶幻方。

5、结构体，类似python中的字典

books=struct('name',{{'machine learning','data learning'}},'price',[30,40])

books.name(1)取出来的是元胞数组，books.name{1}取出来的是字符串

6、矩阵的定义与构造

A=[1 2,5 8]

B=1:2:9,一到九，步长为2

C=repmat(B,3,1),行重复3遍，列重复一次

D=ones(2,4)

7、矩阵的四则运算

A=[1 2 3 4,4 5 6 7]， B=[1 1 2 2,2 2 1 1]

C=A+B, C=A-B,对应的地方相加相减

C=A*B',A乘以B的转置

C=A .*B,对应项相乘

C=A / B,相当于A乘以B的逆

C=A ./B，对应项相除

8、矩阵的下标

A=magic(5)

b=A(2,3),取二行三列的值

b=A(3,:),取第三行

[m,n]=find(A>20)%找到大于20的序号值/矩阵

9、循环结构

for 循环变量=初值：步长：终值

​		执行语句

end

步长为1可以省略。

while 条件表达式

​		执行语句

end

if 条件表达式

​		语句体

end

switch 表达式

​		case 数值或字符串1

​				语句体1；

​		case 数值或字符串2

​		otherwise

​				语句体n;

end

10、二维平面绘图

x=0:0.01:2*pi

y=sin(x)

figure %建立一个幕布

plot(x,y)

title('y=sin(x)')、xlabel('x')、ylabel("sixn(x)")，取标题和坐标值

xlim([0 2*pi]),限制x的坐标范围

[AX,H1,H2]=plotyy(x,y1,x,y2,'plot')，将两个y图像画在一张图。

set(H1,'LineStyle','--'),取不同线型

11、三维立体绘图

t=0:pi/50:pi

plot3(sin(t),cos(t),t)

hold on 暂时不显示当前图

hold off 不保存当前图

grid on，加了一定的网格线

axis square,一定优化限制

subplot(m,n,z)子图划分

12、图形的保存与导出


