---
title: Markdown语法
date: 2023-04-06 10:03:07
categories: 文本
tags:
---
Markdown语法及实例汇总
Markdown编辑器：Typora、MarkdownPad 2、在线编辑器
目录
一、标题
1、自动生成整篇文章目录
2、使用 # 号标记
3、使用 = 和 - 标记一级和二级标题
二、段落
三、文字
1、字体
2、颜色和字号
四、分隔线
五、删除线
六、下划线
七、脚注
八、列表
1、无序列表
2、有序列表
3、列表嵌套
九、引用（区块）
1、单层引用
2、嵌套引用
3、引用里面使用列表
4、列表里面使用引用
十、代码
1、代码区块1
2、代码区块2
3、列表下嵌套代码区块
十一、链接（网址等）
1、文字替代显示
2、直接显示链接
十二、图片
1、不带标题的图片
2、带标题的图片
3、HTML方式
4、指定图片的高度与宽度（HTML方式）
十三、表格
1、简单
2、对齐
3、合并单元格
十四、其他
1、HTML标签
2、转义
3、代码块中添加代码块
4、公式
5、流程图
（1）甘特图：
（2）UML标准时序图：
（3）Mermaid流程图：
（4）Flowchart流程图：
（5）classDiagram类图：
（6）横向流程图：
（7）竖向流程图：
（8）标准流程图：
（9）标准流程图源码格式（横向）：
（10）UML时序图：
（11）UML时序图源码复杂样例：
（12）UML标准时序图样例：
（13）甘特图样例：
一、标题
1、自动生成整篇文章目录
@[TOC](目录标题)
2、使用 # 号标记
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
在这里插入图片描述

3、使用 = 和 - 标记一级和二级标题
一级标题
=================
二级标题
-----------------
在这里插入图片描述


二、段落
使用两个以上空格加上回车换行
使用空行换行
ABC  
ABCDE
ABC
ABCDE


三、文字
1、字体
* *或者_ _表示斜体，** **或者__ __表示粗体

*斜体*
_斜体_
**粗体**
__粗体__
***粗斜体***
___粗斜体___
斜体
斜体
粗体
粗体
粗斜体
粗斜体

2、颜色和字号
参考：Markdown中设置字体颜色以及背景颜色

<font color=red size=2>注意！！！</font>
<font color=orange size=4>注意！！！</font>
<font color=#0000FF size=6>注意！！！</font>
<font color=#FF00FF size=8>注意！！！</font>
注意！！！
注意！！！
注意！！！
注意！！！


四、分隔线
有以下几种格式，使用三个或以上的星号*、减号-、底线_。中间可以插入空格，但不能有其他内容。

***
* * *
---
- - -
___
_ _ _

五、删除线
在文字的两端加上两个波浪线 ~~。

~~ABCDEFG~~
ABCDEFG


六、下划线
通过 HTML 的 <u> 标签实现。

<u>下划线</u>
下划线


七、脚注
对文本的补充说明，使用[^ ]

Linux内核简介 [^Linux]。[^Linux]: 我是个脚注
在这里插入图片描述


八、列表
1、无序列表
使用星号*、加号+、减号-作为列表标记，标记后面要添加一个空格。

* 一
* 二
* 三+ 一
+ 二
+ 三- 一
- 二
- 三
一
二
三
一
二
三
一
二
三
2、有序列表
数字后面加上.，再加空格。

1. 一
2. 二
3. 三
一
二
三
3、列表嵌套
子列表前面添加四个空格或者一个制表符Tab。

1. 项目一：- 元素一- 元素二- 元素三
2. 项目二：- 元素一- 元素二- 元素三
CSDN-Markdown显示：

项目一：
元素一
元素二
元素三
项目二：
元素一
元素二
元素三
MarkdownPad 2显示：
在这里插入图片描述


九、引用（区块）
1、单层引用
段落开头使用 > 符号 ，后面添加一个空格。

> 引用
> 区块
> 学无止境
引用
区块
学无止境

2、嵌套引用
一个 > 符号是最外层，两个 > 符号是第一层嵌套，以此类推。

> 引用
>> 区块
>>> 学无止境
引用

区块

学无止境

3、引用里面使用列表
> 区块
> 1.列表 一
> 2. 列表二
> + 列表一
> + 列表二
> + 列表三
区块
1.列表 一
2. 列表二

列表一
列表二
列表 三
4、列表里面使用引用
只支持无序列表，不支持有序列表。
> 前添加四个空格就会放进区块中
* 列表 一> 引用
* 列表二
列表 一
引用

列表二

十、代码
1、代码区块1
方法一：

	```cprintf("Hello World");```
显示效果：

printf("Hello World");
方法二： 前面添加四个空格或者一个制表符Tab。

	printf("Hello World");	
显示效果：

printf("Hello World");	

2、代码区块2
用两个反引号`包裹内容。

`printf("Hello World");`
printf("Hello World");


3、列表下嵌套代码区块
1. 命令行模式下启动图形模式：```bashsudo startx```
2. 按键切换到图形界面<kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>F7</kbd>3. 开启用户图形界面```bashsudo  systemctl  set-default graphical.targetreboot```
命令行模式下启动图形模式：

sudo startx
按键切换到图形界面

Ctrl + Alt + F7

开启用户图形界面

sudo  systemctl  set-default graphical.target
reboot

十一、链接（网址等）
1、文字替代显示
[【新】CSDN文章一键打印、输出PDF（自动阅读全文、全清爽模式）](https://blog.csdn.net/p1279030826/article/details/106602341)
【新】CSDN文章一键打印、输出PDF（自动阅读全文、全清爽模式）

2、直接显示链接
<https://blog.csdn.net/p1279030826/article/details/116043264>
https://blog.csdn.net/p1279030826/article/details/116043264


十二、图片
1、不带标题的图片
![属性文本](图片地址)

![头像](https://img-blog.csdnimg.cn/img_convert/6548a86aa869cc8d0ea114382e9e197b.png)
头像

2、带标题的图片
![alt 属性文本](图片地址 "可选标题")

![头像](https://img-blog.csdnimg.cn/img_convert/6548a86aa869cc8d0ea114382e9e197b.png "滑稽")
头像

3、HTML方式
<img src="https://profile.csdnimg.cn/5/D/9/3_p1279030826">

4、指定图片的高度与宽度（HTML方式）
<img src="https://profile.csdnimg.cn/5/D/9/3_p1279030826" width="20%">

十三、表格
1、简单
|  项目   | 描述  |
|  ----  | ----  |
| 单元格1  | 单元格2 |
| 单元格3  | 单元格4 |
项目	描述
单元格1	单元格2
单元格3	单元格4
2、对齐
| 左对齐 | 右对齐 | 居中对齐 |
| :-----| ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |
左对齐	右对齐	居中对齐
单元格	单元格	单元格
单元格	单元格	单元格
3、合并单元格
Markdown目前还不支持合并单元格，使用HTML合并单元格。
参考：Markdown的表格合并单元格方法

<table><tr><th>班级</th><th>课程</th><th>平均分</th></tr><tr><td rowspan="3">1班</td><td>语文</td><td>100</td></tr><tr><td>数学</td><td>100</td></tr><tr><td>英语</td><td>100</td></tr>
</table>
班级	课程	平均分
1班	语文	100
数学	100
英语	100

十四、其他
1、HTML标签
不在 Markdown 涵盖范围之内的标签，都可以直接在文档里面用 HTML 撰写。
目前支持的 HTML 元素有：<kbd> <b> <i> <em> <sup> <sub> <br>等 。

<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd>
Ctrl+Alt+Del

文本换行：

<br> <br/>

2、转义
使用反斜杠转义特殊字符。

**粗体** 
\*\* 粗体 \*\*
粗体
** 粗体 **

Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：

\   反斜线
`   反引号
*   星号
_   下划线
{}  花括号
[]  方括号
()  小括号
#   井字号
+   加号
-   减号
.   英文句点
!   感叹号
3、代码块中添加代码块
即第十一条的代码，我在代码块中演示了要怎么添加代码块：

	```cprintf("Hello World");```
方法是里面的代码块使用制表符Tab缩进就可以了。如图所示：
在这里插入图片描述

4、公式
Gamma公式：$\Gamma(n) = (n-1)!\quad\forall
n\in\mathbb N$ Euler integral公式：
$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$
Gamma公式：
Γ(n)=(n−1)!∀n∈N\Gamma(n) = (n-1)!\quad\forall n\in\mathbb NΓ(n)=(n−1)!∀n∈N

Euler integral公式：
Γ(z)=∫0∞tz−1e−tdt.\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,. Γ(z)=∫0∞​tz−1e−tdt.

5、流程图
以下示例来自CSDN教程：

（1）甘特图：
	```mermaidganttdateFormat  YYYY-MM-DDtitle Adding GANTT diagram functionality to mermaidsection 现有任务已完成               :done,    des1, 2014-01-06,2014-01-08进行中               :active,  des2, 2014-01-09, 3d计划中               :         des3, after des2, 5d```
Mon 06
Mon 13
已完成
进行中
计划中
现有任务
Adding GANTT diagram functionality to mermaid
（2）UML标准时序图：
	```mermaidsequenceDiagram张三 ->> 李四: 你好！李四, 最近怎么样?李四-->>王五: 你最近怎么样，王五？李四--x 张三: 我很好，谢谢!李四-x 王五: 我很好，谢谢!Note right of 王五: 李四想了很长时间, 文字太长了<br/>不适合放在一行.李四-->>张三: 打量着王五...张三->>王五: 很好... 王五, 你怎么样?```
张三
李四
王五
你好！李四, 最近怎么样?
你最近怎么样，王五？
我很好，谢谢!
我很好，谢谢!
李四想了很长时间, 文字太长了
不适合放在一行.
打量着王五...
很好... 王五, 你怎么样?
张三
李四
王五
标题：复杂使用
（3）Mermaid流程图：
	```mermaidgraph LRA[长方形] -- 链接 --> B((圆))A --> C(圆角长方形)B --> D{菱形}C --> D```
链接
长方形
圆
圆角长方形
菱形
（4）Flowchart流程图：
	```mermaidflowchatst=>start: 开始e=>end: 结束op=>operation: 我的操作cond=>condition: 确认？st->op->condcond(yes)->econd(no)->op```
开始
我的操作
确认？
结束
yes
no
（5）classDiagram类图：
	```mermaidclassDiagramClass01 <|-- AveryLongClass : Cool<<interface>> Class01Class09 --> C2 : Where am i?Class09 --* C3Class09 --|> Class07Class07 : equals()Class07 : Object[] elementDataClass01 : size()Class01 : int chimpClass01 : int gorillaclass Class10 {>>service>>int idsize()}```
«interface»Class01
int chimpint gorilla
size()
AveryLongClass
Class09
C2
C3
Class07
Object[] elementData
equals()
Class10
>>service>>int id
size()
Cool
Where am i?
以下示例来自菜鸟教程：

（6）横向流程图：
	```mermaidgraph LRA[方形] -->B(圆角)B --> C{条件a}C -->|a=1| D[结果1]C -->|a=2| E[结果2]F[横向流程图]```
a=1
a=2
方形
圆角
条件a
结果1
结果2
横向流程图
（7）竖向流程图：
	```mermaidgraph TDA[方形] --> B(圆角)B --> C{条件a}C --> |a=1| D[结果1]C --> |a=2| E[结果2]F[竖向流程图]```
a=1
a=2
方形
圆角
条件a
结果1
结果2
竖向流程图
（8）标准流程图：
	```mermaidflowchatst=>start: 开始框op=>operation: 处理框cond=>condition: 判断框(是或否?)sub1=>subroutine: 子流程io=>inputoutput: 输入输出框e=>end: 结束框st->op->condcond(yes)->io->econd(no)->sub1(right)->op```
开始框
处理框
判断框(是或否?)
输入输出框
结束框
子流程
yes
no
（9）标准流程图源码格式（横向）：
	```mermaidflowchatst=>start: 开始框op=>operation: 处理框cond=>condition: 判断框(是或否?)sub1=>subroutine: 子流程io=>inputoutput: 输入输出框e=>end: 结束框st(right)->op(right)->condcond(yes)->io(bottom)->econd(no)->sub1(right)->op```
开始框
处理框
判断框(是或否?)
输入输出框
结束框
子流程
yes
no
（10）UML时序图：
	```mermaidsequenceDiagram对象A->>对象B: 对象B你好吗?（请求）Note right of 对象B: 对象B的描述Note left of 对象A: 对象A的描述(提示)对象B-->>对象A: 我很好(响应)对象A->>对象B: 你真的好吗？```
对象A
对象B
对象B你好吗?（请求）
对象B的描述
对象A的描述(提示)
我很好(响应)
你真的好吗？
对象A
对象B
标题：复杂使用
（11）UML时序图源码复杂样例：
	```mermaidsequenceDiagramTitle: 标题：复杂使用对象A->>对象B: 对象B你好吗?（请求）Note right of 对象B: 对象B的描述Note left of 对象A: 对象A的描述(提示)对象B-->>对象A: 我很好(响应)对象B->>小三: 你好吗小三-->>对象A: 对象B找我了对象A->>对象B: 你真的好吗？Note over 小三,对象B: 我们是朋友participant CNote right of C: 没人陪我玩```
对象A
对象B
小三
C
对象B你好吗?（请求）
对象B的描述
对象A的描述(提示)
我很好(响应)
你好吗
对象B找我了
你真的好吗？
我们是朋友
没人陪我玩
对象A
对象B
小三
C
标题：复杂使用
（12）UML标准时序图样例：
	```mermaid%% 时序图例子,-> 直线，-->虚线，->>实线箭头sequenceDiagramparticipant 张三participant 李四张三->王五: 王五你好吗？loop 健康检查王五->王五: 与疾病战斗endNote right of 王五: 合理 食物 <br/>看医生...李四-->>张三: 很好!王五->李四: 你怎么样?李四-->王五: 很好!```
张三
李四
王五
王五你好吗？
与疾病战斗
loop
[健康检查]
合理 食物
看医生...
很好!
你怎么样?
很好!
张三
李四
王五
标题：复杂使用
（13）甘特图样例：
	```mermaidganttdateFormat  YYYY-MM-DDtitle 软件开发甘特图section 设计需求                      :done,    des1, 2014-01-06,2014-01-08原型                      :active,  des2, 2014-01-09, 3dUI设计                     :         des3, after des2, 5d未来任务                     :         des4, after des3, 5dsection 开发学习准备理解需求                      :crit, done, 2014-01-06,24h设计框架                             :crit, done, after des2, 2d开发                                 :crit, active, 3d未来任务                              :crit, 5d耍                                   :2dsection 测试功能测试                              :active, a1, after des3, 3d压力测试                               :after a1  , 20h测试报告                               : 48h```
Mon 06
Mon 13
Mon 20
需求
原型
UI设计
未来任务
学习准备理解需求
设计框架
开发
未来任务
耍
功能测试
压力测试
测试报告
设计
开发
测试
软件开发甘特图

参考：菜鸟教程——Markdown 教程


本文链接:https://www.ngui.cc/el/1256340.html