---
layout:     post
title:      "Html Table 冻结行列"
subtitle:   "类Excel表头，列头冻结效果"
date:       2016-12-13 18:30:00
author:     "Colin"
header-img: "img/2016/bg-post/freezedtable.jpg"
tags:
    - 前端开发
    - HTML
---

### 前言
当表格行列数较多时，为了用户在屏幕内容滚动时更加方便的查看数据，我们期望可以像Excel一样冻结表格的固定行和列（一般为表头和列头），遗憾的是HTML中并没有这种效果，那让我们来动手开发吧，Let's do it！

### 正文
实现表格行列冻结效果，网上较多采用以下两种方式。

+ 通过多个表格拼接层叠，Js控制多个表格间的联动。这种方式逻辑简单，容易实现，但代码冗余，难以维护，灵活性不好，适合简单场景下使用。

+ 是通过创建多个div，div之间通过css实现层叠，每个div放置当前表格的克隆。例如：需要行冻结时，创建存放冻结行表格的div，通过设置z-index属性和position属性，让冻结行表格在数据表格的上层。同理，需要列冻结时，创建存放冻结列表格的div，并放置在数据表格的上层。如果需要行列都冻结时，则除了创建冻结行、冻结列表格的div，还需要创建左上角的固定行列表格的div，并放置在所有div的最上层。 这种方式与第一种思路类似，只是实现冻结部分是动态创建，这种方式可维护性比第一种略好。

> 在一些 js 组件库中也有冻结表格的功能，若仅为使用此功能引入一个几十KB的库，有些小题大做，得不偿失。

*网上流传较多的第二种方式的案例，多存在问题，我们在此以轻量级代码简述第一种实现方式，足以应对一般需求。*

##### 表格层叠方式

###### 1. 冻结某一行（列）

1）把看起来是一个整体的表格拆分成两部分，table1负责固定部分如thead，而table2负责可以拖动的部分如tbody。
![冻结行(列)表格层叠图](/img/2016/in-post/freezedtable/structure-1.png)

*伪代码：*

``` html
<div class="table-out">
    <div class="table-top">
        <table>
            <thead>
                <tr><th>姓名</th><th>学号</th><th>年龄</th></tr>
            </thead>
        </table>
    </div>
    <div class="table-main">
        <table>
            <tbody>
                <tr><td>张三</td><td>001</td><td>17</td></tr> 
                <tr><td>李四</td><td>002</td><td>18</td></tr> 
            </tbody>     
        </table>                
    </div>
</div>
```

2）把两个table固定好了之后，监听table2的滚动，用table2的滚动带动table1的滚动（通过设置css里的left 或者 scroll，如果是绝对定位那么只能用设定css中left的方法）

*伪代码：*

``` js
$('.table-main').scroll(function() {
    var scrollLeft = $(this).scrollLeft();
    $('.table-top').scrollLeft(scrollLeft);
});
```

###### 2. 同时冻结行和列

1） 把看起来是一个整体的表格拆分成四个部分
![同时冻结行列表格层叠图](/img/2016/in-post/freezedtable/structure-2.png)
    
*伪代码：*

``` html
<div class="table-out">
    <!--固定行-->
    <div class="table-top">
        <table>
            <thead>
                <tr>
                    <th>零件名称</th>
                    <th>换件日期</th>
                    <th>寿命比例</th>
                </tr>
            </thead>
        </table>
    </div>
    <!--内容-->
    <div class="table-main">
        <table>
            <tbody>
                <tr>
                    <td>电机</td>
                    <td>2015-08-25</td>
                    <td>
                        <progress value="0.6"></progress>
                    </td>
                </tr>
                <tr>
                    <td>蝶型弹簧</td>
                    <td>2015-08-25</td>
                    <td>
                        <progress value="0.4"></progress>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <!--固定列-->
    <div class="table-left">
        <table>
            <tbody>
                <tr><td>电机</td></tr>
                <tr><td>蝶型弹簧</td></tr>
            </tbody>
        </table>
    </div>
    <!--左上角交叉区-->
    <div class="table-left-top">
        <table>
            <thead>
                <tr>
                    <th>备件名称</th>
                </tr>
            </thead>
        </table>
    </div>
</div>
```

2）把四个table固定好了之后，监听table-main的滚动，用table-main的滚动带动table-top的左右移动和table-left的上下移动。在这个示例里，我对table-left用到了绝对定位，所以给table-left设定scroll无效，但是可以使用改变table-left的css中top的属性值来使得table-left上下移动。

*伪代码：*

``` js
var tb=$('.table-out');
var tbLeft=tb.children('.table-left');
var tbTop=tb.children('.table-top');
var tbMain=tb.children('.table-main');

var leftTop = parseInt(tbLeft.css('top'));
tbMain.scroll(function() {		
    tbLeft.css('top',leftTop - tbMain.scrollTop()+'px');
    tbTop.scrollLeft(tbMain.scrollLeft());
});
``` 

*效果图：*
<iframe src="/res/freezedtable.html" style="border:0;width:100%;height:355px;"></iframe>
> 默认的滚动条比较难看，我们可以使用了jQuery.nicescroll等插件来美化滚动条效果

*完整代码下载：* 
<a target='_blank' href='https://gist.github.com/colin-chang/29a9d08e7ea850fda9ec012bc49cc530'>表格行列冻结示例下载</a>