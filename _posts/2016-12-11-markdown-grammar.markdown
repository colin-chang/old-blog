---
layout:     post
title:      "MarkDown基础语法"
subtitle:   "快速构架丰富多彩的多媒体文档，从此文档熠熠生辉"
date:       2016-12-11 18:40:00
author:     "Colin"
header-img: "img/2016/bg-post/markdown-grammar.jpg"
tags:
    - 高效工作
---

> "Markdown 是一种轻量级的「标记语言」"

## 导语

Markdown 是一种轻量级的「标记语言」，目标是实现易读易写，目前被越来越多的写作爱好者，撰稿者广泛使用。Markdown 的语法十分简单。常用的标记符号也不超过十个，这种相对于更为复杂的 HTML 标记语言来说，Markdown 可谓是十分轻量的，学习成本也不需要太多，且一旦熟悉这种语法规则，会有一劳永逸的效果。

## 正文

### 1.标题
标题是每篇文章都需要也是最常用的格式，在 Markdown 中，如果一段文字被定义为标题，只要在这段文字前加 # 号即可。
`# 一级标题`
`## 二级标题`
`### 三级标题`
以此类推，总共六级标题，建议在井号后加一个空格，这是最标准的 Markdown 语法。

*示例：*

# h1
## h2
### h3
#### h4
##### h5
###### h6

### 2.引用
如果你需要引用一小段别处的句子，那么就要用引用的格式。只需要在文本前加入 &gt; 这种尖括号（大于号）即可

*示例：*
> 一级引用
>> 二级引用
>>> 三级引用

### 3.列表
在 Markdown 中，列表的显示只需要在文字前加上 - 或 * 即可变为无序列表，有序列表则直接在文字前加1. 2. 3. 符号要和文字之间加上一个字符的空格。无序列表使用Tab键逐级缩进，表示不同层级。

*示例：*

1. 第一点
2. 第二点
4. 第三点

+ world
    + US
    + China
        + Beijing
        + ShanDong
    
### 4.图片
使用以下语法即可插入图片。
`![alt](url 'title')`

*示例：*
![百度](//www.baidu.com/img/bd_logo1.png '百度搜索')

### 5.超链接/锚点
超链接和锚点的语法与插入图片类似，图片语法在最前面多一个`!`。

`[text](url 'title')`

*超链接示例：*[蝉翼科技](http://chanyikeji.com '北京蝉翼科技有限公司')

*锚点示例：*[跳转到表格章节](#table)

### 6.代码块
如果你是个程序猿，需要在文章里优雅的引用代码框，在 Markdown下实现也非常简单。
行内代码只需要用两个" ` "把中间的代码包裹起来，多行代码使用" ``` "包裹。

*行内代码示例：*

`Console.WriteLine('Hello world')`

*多行代码示例：*

``` js
/**
 * Created by zhangcheng on 2016/11/29.
 */
// 全局配置
(function (angular) {
  angular.module("global", []).constant('appConfig', {
    'host': '127.0.0.1',
    'port': '63342'
  });
})(angular);
```

### 7.粗体/斜体/分割线/删除线
分割线的语法只需要三个 - 号或三个 * 号

*水平线示例：*

---

两个 * 包含一段文本就是粗体的语法，用一个  * 包含一段文本就是斜体的语法。两个 ~ 包裹文本为删除线效果。

*示例：*

**粗体**
*斜体*
~~删除线~~

### 8.表格

<span id='table'>表格锚点</span>

markdown中表格语法简单但书写略繁琐，一行内列之间通过 \| 分割。
表格通过以下方式声明每列的对齐方式。声明对齐方式的前一行为表头默认有加粗效果。

*表格对齐方式：*

|左对齐|右对齐|两端对齐|默认对齐|
|:-|-:|:-:|-|
|\:-|\-:|\:-:|\-|

*表格示例：*


|序号|交易名|交易说明|备注|
|-:|:-:|:-|-|
|1|prfcfg|菜单配置|通过此交易查询到所有交易码和菜单|
|2|gentmo|编译所有交易||
|100000|sysdba|数据库表模型汇总||

### 9.结构图/公式

*流程图：*
```flow
st=>start: Start
e=>end
op=>operation: My Operation
cond=>condition: Yes or No?

st->op->cond
cond(yes)->e
cond(no)->op
```

*时序图：*
```sequence
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```
*公式：*

 可以创建行内公式 
 `$\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$`
 或者块级公式
 `$$	x = \dfrac{-b \pm \sqrt{b^2 - 4ac}}{2a} $$`



*参考资料:*

流程图、LaTeX公式等复杂语法不常使用，具体使用方式参加以下资料<br>
[MarkDown中使用数学公式](http://www.ituring.com.cn/article/32403) <br>
[MarkDown中使用流程图](http://blog.csdn.net/aizhaoyu/article/details/44350821)

### 10.HTML兼容性

MarkDown完全兼容HTML，我们可以直接在MarkDown中使用HTML标签。MarkDown对部分HTML标签的内容作了的优化和封装。

> 参考资料:
+ [中文文档](http://www.appinn.com/markdown/basic.html)
+ [语法文档](http://daringfireball.net/projects/markdown/syntax)
<br/>