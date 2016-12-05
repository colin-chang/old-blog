---
layout:     post
title:      "Welcome to the world of MarkDown"
subtitle:   " \"Hello World, Hello Blog, Hello MarkDown\""
date:       2016-12-06 03:03:00
author:     "Colin"
header-img: "img/post-bg-2015.jpg"
tags:
    - MarkDown,笔记,语言
---

## MarkDown 常用语法 基础语法

> 1.标题
# h1
## h2
### h3
#### h4
##### h5
###### h6

> 2.引用

***
> 一级引用
>> 二级引用
>>> 三级引用  

---

> 3.列表
#### 1.有序列表
1. 第一点
2. 第二点
4. 第三点

#### 2.无序列表
+ world
    + US
    + China
        + 北京
        + 山东
    
使用Tab键逐级缩进，表示不同层级

> 4.图片

`![alt](url 'title')`
![百度](//www.baidu.com/img/bd_logo1.png '百度搜索')

> 5.超链接/锚点

#### 超链接
`[text](url 'title')`
[蝉翼科技](http://chanyikeji.com '北京蝉翼科技有限公司')
#### 锚点
[表格](#table)

> 6.代码块

`行内代码示例`
```
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

> 7.粗体/斜体/分割线/删除线

**粗体**
*斜体*
~~删除线~~
水平线

---

> 8.表格

<span id='table'>声明锚点</span>

|序号|交易名|交易说明|备注|
|-:|:-:|:-|-|
|1|prfcfg|菜单配置|通过此交易查询到所有交易码和菜单|
|2|gentmo|编译所有交易||
|100000|sysdba|数据库表模型汇总||

> 9.流程图/公式

#### 流程图
```flow
st=>start: Start
e=>end
op=>operation: My Operation
cond=>condition: Yes or No?

st->op->cond
cond(yes)->e
cond(no)->op
```

#### 时序图
```sequence
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```
#### 公式
 可以创建行内公式 $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$或者块级公式
 $$	x = \dfrac{-b \pm \sqrt{b^2 - 4ac}}{2a} $$

流程图、LaTeX公式等复杂语法不常使用，具体使用方式参加以下资料
*参考资料*
[MarkDown中使用数学公式](http://www.ituring.com.cn/article/32403)
[MarkDown中使用流程图](http://blog.csdn.net/aizhaoyu/article/details/44350821)

> 10.HTML兼容性

MarkDown完全兼容HTML，我们可以直接在MarkDown中使用HTML标签。MarkDown对部分HTML标签的内容作了的优化和封装。