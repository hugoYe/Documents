## MarkDown 语法学习总结

### 目录索引
+ [概述](#0)

+ [1. 常用标记](#1)
  * [1.1 标题](#1.1)
  * [1.2 目录](#1.2)
  * [1.3 引用](#1.3)
  * [1.4 代码块](#1.4)
  * [1.5 行内代码](#1.5)
  * [1.6 导入图片](#1.6)
  * [1.7 列表](#1.7)
  * [1.8 粗体和斜体](#1.8)
  * [1.9 表格](#1.9)
  * [1.10 分割线](#1.10)
  * [1.11 链接](#1.11)
  * [1.12 反斜杠](#1.12)
  * [1.13 空格](#1.13)

+ [2. 次常用标记](#2)
  * [2.1 标签分类](#2.1)
  * [2.2 删除线 ](#2.2)
  * [2.3 注脚](#2.3)

+ [3. 不常用标记](#3)
  * [3.1 实现页内跳转](#3.1)

+ [4. 专项使用标记](#4)

<h3 id="0"></h3> 
**二八定律说**：

百分之二十的知识解决百分之八十的问题。

其实你只需要掌握基本语法标记就可以愉快的玩耍了。经过几个月使用Markdown写文档，发现掌握下面这些标记语法，就可以完成日常文档书写了。什么？要画流程图？这些需求对于大部分时间来说，你是用不到的，你只需要建立一个知识储备就好。遇到了想不起来？打开看一下就是了。想记住？对不起，这种事倍功半的事情，还是少做为妙， 毕竟时间是硫酸，管你是什么都能够腐化，只是快慢而已。

那么问题来了，为什么这几个常用的要记住呢？因为这几个是经常使用的，虽然熟能生巧，日久便记住了，但是在熟能生巧的路上总不能天天翻看知识储备吧。太影响效率。何不花一点点时间强行记住，那么在日久记住的道路上，岂不是一路顺风？闲话不多说，来看看你要掌握的语法标记吧。如果你想学习和使用Markdown，我建议：
* **常用标记** 要先花一些时间熟记，后面经常使用的话就会形成习惯了，不过脑的正常书写，跟打字一样；
* **次常用标记** 要有基本的印象，能记住也是可以的；
* **不常用标记** 和 **专用标记** just看看就好，等到使用的时候 百度一下，你就知道 。


<h3 id="1"></h3> 
### 1. 常用标记

<h4 id="1.1"></h4> 
#### 1.1 标题

##### 1.1.1 说明

* 使用 # 表示标题，一级标题使用一个 # ，二级标题使用两个 ## ，以此类推，共有六级标题。
* 使用 ===== 表示高阶标题，使用 --------- 表示次阶标题。

##### 1.1.2 示例

**标题语法如下：**
``` 
# 这是一级标题
## 这是二级标题
### 这是三级标题
###### 这是六级标题

这是一级标题
========

这是二级标题
--------------
```

**效果显示如下：**
# 这是一级标题

## 这是二级标题

### 这是三级标题

###### 这是六级标题

这是一级标题
=======

这是二级标题
----------


##### 1.1.3 注意

* # 和标题之间最好加一个空格。不要问我为什么，貌似有时候不会被识别为标题？已经忘记自己为什么要加空格了，也许是任性。
* ==== 和 ---- 表示标题时，大于等于2个都可以表示。
* 我通常在标题分级时使用标题标记，这个的用处很明了了。


<h4 id="1.2"></h4> 
#### 1.2 目录

##### 1.2.1 说明

使用 [TOC] 生成目录，如一开始的目录所示。**（然而此方法并不靠谱，囧。。。）**。
另外可以采用结合html语言的方法来生成目录树。

##### 1.2.2 示例

**示例1**

```
[TOC]
```

**示例2**

**语法规则如下：**

```
+ [1. 标题](#num1)
  * [1.1 子标题](#num1.1)
  * [1.2 子标题](#num1.2)
  * [1.3 子标题](#num1.3)
  
+ [2. 标题](#num2)
  * [2.1 子标题](#num2.1)
  * [2.2 子标题 ](#num2.2)
  * [2.3 子标题](#num2.3)

+ [3. 标题](#num3)
  * [3.1 子标题](#num3.1)

+ [4. 标题](#num4)


<h4 id="num1">1. 标题</h4> 
 <h5 id="num1.1">1.1 子标题</h5> 
 <h5 id="num1.2">1.2 子标题</h5> 
 <h5 id="num1.3">1.3 子标题</h5> 
<h4 id="num2">2. 标题</h4> 
 <h5 id="num2.1">2.1 子标题</h5> 
 <h5 id="num2.2">2.2 子标题</h5> 
 <h5 id="num2.3">2.3 子标题</h5> 
<h4 id="num3">3. 标题</h4> 
 <h5 id="num3.1">3.1 子标题</h5> 
<h4 id="num4">4. 标题</h4> 
```

**效果显示如下：**

+ [1. 标题](#num1)
  * [1.1 子标题](#num1.1)
  * [1.2 子标题](#num1.2)
  * [1.3 子标题](#num1.3)
  
+ [2. 标题](#num2)
  * [2.1 子标题](#num2.1)
  * [2.2 子标题 ](#num2.2)
  * [2.3 子标题](#num2.3)

+ [3. 标题](#num3)
  * [3.1 子标题](#num3.1)

+ [4. 标题](#num4)


<h4 id="num1">1. 标题</h4> 
 <h5 id="num1.1">1.1 子标题</h5> 
 <h5 id="num1.2">1.2 子标题</h5> 
 <h5 id="num1.3">1.3 子标题</h5> 
<h4 id="num2">2. 标题</h4> 
 <h5 id="num2.1">2.1 子标题</h5> 
 <h5 id="num2.2">2.2 子标题</h5> 
 <h5 id="num2.3">2.3 子标题</h5> 
<h4 id="num3">3. 标题</h4> 
 <h5 id="num3.1">3.1 子标题</h5> 
<h4 id="num4">4. 标题</h4> 

##### 1.2.3 注意

* 1.如果你的标题都是按照Markdown语法书写的话，可以自动生成层级目录。
* 2.我常用 为知笔记 记笔记，可惜 为知笔记 不支持[TOC]标记，一个悲伤的故事。
* 3.[TOC] 标记可能只能放在一级标题的前面，视不同的编译器而定。







<h4 id="1.3"></h4> 
#### 1.3 引用

##### 1.3.1 说明

使用 > 表示引用， >> 表示引用里面再套一层引用，依次类推。

##### 1.3.2 示例

例1：

**引用语法如下：**

```
> 这是一级引用
>>这是二级引用
>>> 这是三级引用

>这是一级引用
```

**效果显示如下：**

> 这是一级引用
>>这是二级引用
>>> 这是三级引用

>这是一级引用

例2：

**引用语法如下：**

```
> 这是一级引用
>>这是二级引用
>>> 这是三级引用
>这是一级引用
```
**效果显示如下：**

> 这是一级引用
>>这是二级引用
>>> 这是三级引用
>这是一级引用

##### 1.3.3 注意

1. 如果 > 和 >> 嵌套使用的话，从 >> 退到 > 时，必须之间要加一个空格或者 > 作为过渡，否则默认为下一行和上一行是同一级别的引用。如示例所示。
2. 引用标记里可以使用其他标记，如：有序列表或无序列表标记，代码标记等。
3. 我通常在 *引用别人的话或者某些时候做说明*  时使用引用标记，其实我一直拿不准到底什么情况下使用引用标记才是正确的。 **如果你知道，我只想说：请务必告诉我**。


<h4 id="1.4"></h4> 
#### 1.4 代码块

##### 1.4.1 说明

使用```表示代码块。

##### 1.4.2 示例

**语法规则如下：**

\`\`\`javascript 

var canvas = document.getElementById("canvas");

var context = canvas.getContext("2d");

\`\`\`

**显示效果如下：**

```javascript
var canvas = document.getElementById("canvas");

var context = canvas.getContext("2d");
```

##### 1.4.3 注意

1. `这个符号是在 Esc 键下面，切换到英文下即可。
2. ```后面的 javascript 表示此段代码为javascript代码，Markdown会自行使用javascript代码颜色渲染。这里也可以不写。PS：谁能够提供一个完整的Markdown可以渲染的语言列表啊，比如：linux命令这里写什么？
3. 本文档所有使用讲解Markdown语法标记示例的地方都是使用代码块标记的。

<h4 id="1.5"></h4> 
#### 1.5 行内代码

##### 1.5.1 说明

使用` `表示行内代码。
##### 1.5.2 示例
**语法规则如下：**

```
这是`javascript`代码
```

**显示效果如下：**

这是`javascript`代码

##### 1.5.3 注意

1. 本页部分文字中间的英文字母就是使用行内代码标记标记的。
2. 这个的使用场景我也有些模糊。我常在文字间有英文的时候使用，但有时又不知道该不该使用，困扰。 **如果你知道，请告诉我。**


<h4 id="1.6"></h4> 
#### 1.6 导入图片

##### 1.6.1 说明

使用 `![Alt text](/path/to/img.jpg "Optional title")` 导入图片。其中：
* `Alt text` 为如果图片无法显示时显示的文字；
* `/path/to/img.jpg` 为图片所在路径；
* `Optional title` 为显示标题。显示效果为在你将鼠标放到图片上后，会显示一个小框提示，提示的内容就是 `Optional title` 里的内容。

##### 1.6.2 示例

**语法规则如下：**

```
![Markdown](http://images.cnitblog.com/blog/404392/201501/122257231047591.jpg)
```
**显示效果如下：**

![Markdown](http://images.cnitblog.com/blog/404392/201501/122257231047591.jpg)

##### 1.6.3 注意

1. 导入的图片路径可以使用绝对路径也可以使用相对路径，建议使用相对路径。
2. 我通常的做法是Markdown文档的同级目录下建立一个pictures文件夹，里面放置所有所需的图片，如果图片多的话，你也可以在pictures文件夹里建立子文件夹归类。


<h4 id="1.7"></h4> 
#### 1.7 列表

##### 1.7.1 说明

使用 `1. 2. 3.` 表示有序列表，使用 `*` 或 `-` 或 `+` 表示无序列表。

##### 1.7.2 示例

**例1：有序列表**

**语法规则如下：**

```
1. 第一点
2. 第二点
4. 第三点
```
**显示效果如下：**

1. 第一点
2. 第二点
4. 第三点

**例2：无序列表**

**语法规则如下：**

```
+ 呵呵
  * 嘉嘉
  - 嘻嘻
  - 吼吼
    - 嘎嘎
    + 桀桀
* 哈哈
```

**显示效果如下：**

+ 呵呵
  * 嘉嘉
  - 嘻嘻
  - 吼吼
    - 嘎嘎
    + 桀桀
* 哈哈

##### 1.7.3 注意

1. 无序列表或有序列表标记和后面的文字之间要有一个空格隔开。
2. 有序列表标记不是按照你写的数字进行显示的，而是根据当前有序列表标记所在位置显示的，如`示例1`所示。
3. 无序列表的项目符号是按照实心圆、空心圆、实心方格的层级关系递进的，如`例2`所示。通常情况下，同一层级使用同一种标记表示，便于自己查看和管理。
4. 无序列表和有序列表标记的使用场景也很明了，故不多说。


<h4 id="1.8"></h4> 
#### 1.8 粗体和斜体

##### 1.8.1 说明

使用 `**` 或者 `__` 表示粗体。

使用 `*` 或者 `_` 表示斜体。

##### 1.8.2 示例

**语法规则如下：**

```
 **粗体1**    __粗体2__
 *斜体1*    _斜体2_
```

**显示效果如下：**

**粗体1**    __粗体2__

*斜体1*    _斜体2_
 

##### 1.8.3 注意

1. 前后的 `*` 或 `_` 与要 **加粗或倾斜** 的字体之间不能有空格。
2. 我通常在强调时使用加粗标记，在和一行中的加粗区分且也表示强调时使用倾斜标记，这里的倾斜标记的使用场景不明确。 **如果你知道：请务必告诉我。**


<h4 id="1.9"></h4> 
#### 1.9 表格

##### 1.9.1 说明

具体使用方式请看示例。
* `------:` 为右对齐。
* `:------` 为左对齐。
* `:------:` 为居中对齐。
* `-------` 为使用默认居中对齐。

##### 1.9.2 示例

**语法规则如下：**

```
|         序号    |    交易名    |    交易说明    |    备注    |
|    ------: |    :-------:    |    :---------   |    ------    |
|    1    |    prfcfg    |    菜单配置    |    可以通过此交易查询到所有交易码和菜单的对应关系    |
|    2    |    gentmo    |    编译所有交易    |    |
|    100000    |    sysdba    |    数据库表模型汇总    |    |
```

**显示效果如下：**

|         序号    |    交易名    |    交易说明    |    备注    |
|    ------: |    :-------:    |    :---------   |    ------    |
|    1    |    prfcfg    |    菜单配置    |    可以通过此交易查询到所有交易码和菜单的对应关系    |
|    2    |    gentmo    |    编译所有交易    |    |
|    100000    |    sysdba    |    数据库表模型汇总    |    |

##### 1.9.3 注意

* 每个Markdown解析器都不一样，可能左右居中对齐方式的表示方式不一样。


<h4 id="1.10"></h4> 
#### 1.10 分割线

##### 1.10.1 说明

使用 `---` 或者 `***` 或者 `* * *` 表示水平分割线。

##### 1.10.2 示例

**语法规则如下：**

```
---

***

* * *
```

**显示效果如下：**
---

***

* * *

##### 1.10.3 注意
1. 只要 `*` 或者 `-` 大于等于三个就可组成一条平行线。
2. 使用 `---` 作为水平分割线时，要在它的前后都空一行，防止 `---` 被当成标题标记的表示方式(如示例中的`显示效果如下`字样)。


<h4 id="1.11"></h4> 
#### 1.11 链接

##### 1.11.1 说明

使用 `[](link "Optional title")` 表示行内链接。其中：

* `[]` 内的内容为要添加链接的文字。
* `link` 为链接地址。
* `Optional title` 为显示标题。显示效果为在你将鼠标放到链接上后，会显示一个小框提示，提示的内容就是 Optional title 里的内容。

参考式链接如例所示。

##### 1.11.2 示例

**例1：行内链接**

**语法规则如下：**

```
这就是我们常用的地址：[Baidu](www.baidu.com "百度一下，你就知道" )
```

**显示效果如下：**

这就是我们常用的地址：[Baidu](www.baidu.com "百度一下，你就知道" )

**例2：参考式链接**

**语法规则如下：**

```
这就是我们常用的地址：[Baidu][1]

[1]:www.baidu.com "百度一下，你就知道" 
```

**显示效果如下：**

这就是我们常用的地址：[Baidu][1]

[1]:www.baidu.com "百度一下，你就知道" 

##### 1.11.3 注意

1. 参考式链接和行内链接的效果是一样的，各有利弊。行内连接清晰易懂，可以清楚的知道链接的地址，但是不便于多次利用。参考式链接可以重复使用，但不能即刻知道链接的地址。
2. 使用场景很明了，不多说。


<h4 id="1.12"></h4> 
#### 1.12 反斜杠

##### 1.12.1 说明

使用 `\` 表示反斜杠。在你不想显示Markdown标记时可以使用反斜杠。

##### 1.12.2 示例

**语法规则如下：**

```
\*这里不会显示斜体\*
```

**显示效果如下：**

\*这里不会显示斜体\*


<h4 id="1.13"></h4> 
#### 1.13 空格

##### 1.13.1 说明

Markdown语法会忽略首行开头的空格，如果要体现出首行开头空两个的效果，可以使用 **全角符号下的空格** ，windows下使用 shift+空格 切换。


<h3 id="2"></h3> 
### 2. 次常用标记

<h4 id="2.1"></h4> 
#### 2.1 标签分类

##### 2.1.1 说明

使用 `标签:` 或者 `Tags:` 表示标签标记。

##### 2.1.2 示例

**语法规则如下：**

```
标签: 数学 英语
Tags: 数学 英语
```

**显示效果如下：**

标签： 数学 英语

Tags： 数学 英语

##### 2.1.3 注意

1. `标签:` 或者 `Tags:` 的冒号要使用半角冒号。
2. 基本没使用过这个标记，不过应用场景应该是归类。便于快速了解文章分类。难道可以通过某种方式来遍历到标签标记？不甚了解。 **如你知道：请告诉我。**


<h4 id="2.2"></h4> 
#### 2.2 删除线 

##### 2.2.1 说明

使用 `~~` 表示删除线。

##### 2.2.2 示例

**语法规则如下：**

```
~~这是一条删除线~~
```

**显示效果如下：**

~~这是一条删除线~~

##### 2.2.3 注意

1. 注意 `~~` 和 要添加删除线的文字之间不能有空格。
2. 我常使用在显示的告诉自己这行文字是要删除的。


<h4 id="2.3"></h4> 
#### 2.3 注脚

##### 2.3.1 说明

使用 `[^footer]` 表示注脚。

##### 2.3.2 示例

**语法规则如下：**

```
这是一个注脚测试[^footer1]。

[^footer1]: 这是一个测试，用来阐释注脚。
```

**显示效果如下：**

这是一个注脚测试[^footer1]。

[^footer1]: 这是一个测试，用来阐释注脚。

##### 2.3.3 注意

* 我常在需要解释一个名词，或者一本书，或者一个人时使用脚注标记。


<h3 id="3"></h3> 
### 3. 不常用标记

<h4 id="3.1"></h4> 
#### 3.1 实现页内跳转

##### 3.1.1 说明

使用html代码实现页内跳转。在要跳转到的位置定义个锚 `<span id = "jump">hehe</span>` ，然后使用 `[你好](#jump)` 将 `你好` 设置为一单击即跳转到 `hehe` 所在位置的效果。

##### 3.1.2 示例

**语法规则如下：**

```
[你好](#jump)
<span id = "jump">hehe</span>
```

**显示效果如下：**

[你好](#jump)
<span id = "jump">hehe</span>


<h3 id="4"></h3> 
### 4. 专项使用标记

#### 4.1 流程图

以后在总结吧，现在的我完全没有使用上，没有需求就先不总结了。

#### 4.2 LaTeX公式

以后在总结吧，现在的我完全没有使用上，没有需求就先不总结了。
