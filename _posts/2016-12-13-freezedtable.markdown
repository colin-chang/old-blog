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

``` javascript
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

``` javascript
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
<iframe src="/res/freezedtable/freezedtable.html" style="border:0;width:100%;height:355px;"></iframe>
> 默认的滚动条比较难看，我们可以使用了jQuery.nicescroll等插件来美化滚动条效果

*完整代码下载：* 
<a target='_blank' href='https://gist.github.com/colin-chang/29a9d08e7ea850fda9ec012bc49cc530'>表格行列冻结示例下载</a>

##### SuperTable方式（动态创建表格）
上面手动拼接表格的方式会在Html中存在较多冗余代码，表格结构维护起来比较繁琐，我们可以通过js读取html中表格结构然后动态创建div或table来完成层叠拼接工作，这样一来我们可以简单的在html中维护一份table的干净代码即可。
>SuperTable是比较简单的一种实现方式,网上流传较多的是案例大都是Asp.Net WebForm较老的案例，且Project文件不完整，很难运行，这里提炼出了一个纯html+css+js的轻量级Demo,供大家参考。

原理与前面说的相同，这里我们不再赘述。直接上代码了。如果代码不完整，后面附有完整案例的下载链接，大家可以下载研究。

*完整Html代码：*

``` html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>锁定表格行列</title>
    <link rel="stylesheet" href="supertables.css">
</head>
<body>
<table id="tb">
    <tr>
        <th>序号</th>
        <th>备件</th>
        <th>换件日期</th>
        <th>寿命比例</th>
        <th>规格型号</th>
        <th>理论寿命</th>
    </tr>
    <tr class="light-bg">
        <td>1</td>
        <td>电机</td>
        <td>2015-08-25</td>
        <td>
            <progress value="0.6"></progress>
        </td>
        <td>XL2D</td>
        <td>6个月</td>
    </tr>
    <tr>
        <td>2</td>
        <td>蝶型弹簧</td>
        <td>2015-08-25</td>
        <td>
            <progress value="0.6"></progress>
        </td>
        <td>XL2D</td>
        <td>6个月</td>
    </tr>
    <tr class="light-bg">
        <td>3</td>
        <td>电机</td>
        <td>2015-08-25</td>
        <td>
            <progress value="0.6"></progress>
        </td>
        <td>XL2D</td>
        <td>6个月</td>
    </tr>
    <tr>
        <td>4</td>
        <td>蝶型弹簧</td>
        <td>2015-08-25</td>
        <td>
            <progress value="0.6"></progress>
        </td>
        <td>XL2D</td>
        <td>6个月</td>
    </tr>
    <tr class="light-bg">
        <td>5</td>
        <td>电机</td>
        <td>2015-08-25</td>
        <td>
            <progress value="0.6"></progress>
        </td>
        <td>XL2D</td>
        <td>6个月</td>
    </tr>
    <tr>
        <td>6</td>
        <td>蝶型弹簧</td>
        <td>2015-08-25</td>
        <td>
            <progress value="0.6"></progress>
        </td>
        <td>XL2D</td>
        <td>6个月</td>
    </tr>
    <tr class="light-bg">
        <td>7</td>
        <td>电机</td>
        <td>2015-08-25</td>
        <td>
            <progress value="0.6"></progress>
        </td>
        <td>XL2D</td>
        <td>6个月</td>
    </tr>
    <tr>
        <td>8</td>
        <td>蝶型弹簧</td>
        <td>2015-08-25</td>
        <td>
            <progress value="0.6"></progress>
        </td>
        <td>XL2D</td>
        <td>6个月</td>
    </tr>
    <tr class="light-bg">
        <td>9</td>
        <td>电机</td>
        <td>2015-08-25</td>
        <td>
            <progress value="0.6"></progress>
        </td>
        <td>XL2D</td>
        <td>6个月</td>
    </tr>
    <tr>
        <td>10</td>
        <td>蝶型弹簧</td>
        <td>2015-08-25</td>
        <td>
            <progress value="0.6"></progress>
        </td>
        <td>XL2D</td>
        <td>6个月</td>
    </tr>
    <tr class="light-bg">
        <td>11</td>
        <td>电机</td>
        <td>2015-08-25</td>
        <td>
            <progress value="0.6"></progress>
        </td>
        <td>XL2D</td>
        <td>6个月</td>
    </tr>
</table>
<script src="//cdn.bootcss.com/jquery/1.12.4/jquery.min.js"></script>
<script src="supertable.js"></script>
<script>
    (function () {
        'use strict';
        $("#tb").toSuperTable({width: "332px", height: "208px", fixedCols: 2});
    })();
</script>
</body>
</html>
```

*supertables.css：*

``` css
.sBase {
	position: relative;
    width: 100%;
    height: 100%;
    overflow: hidden;
}

/* HEADERS */
.sHeader {
	position: absolute;
	z-index: 3;
	background-color: #ffffff;
}
.sHeaderInner {
	position: relative;
}
.sHeaderInner table {
	border-spacing: 0px 0px !important;
	border-collapse: collapse !important;
	width: 1px !important;
	table-layout: fixed !important;
	background-color: #ffffff; /* Here b/c of Opera 9.25 :( */
}

/* HEADERS - FIXED */
.sFHeader {
	position: absolute;
	z-index: 4;
	overflow: hidden;
}
.sFHeader table {
	border-spacing: 0px 0px !important;
	border-collapse: collapse !important;
	width: 1px !important;
	table-layout: fixed !important;
	background-color: #ffffff; /* Here b/c of Opera 9.25 :( */
}

/* BODY */
.sData {
	position: absolute;
	z-index: 2;
	overflow: auto;
	background-color: #ffffff;
}
.sData table {
	border-spacing: 0px 0px !important;
	border-collapse: collapse !important;
	width: 1px !important;
	table-layout: fixed !important;
}

/* BODY - FIXED */
.sFData {
	position: absolute;
	z-index: 1;
	background-color: #ffffff;
}
.sFDataInner {
	position: relative;
}
.sFData table {
	border-spacing: 0px 0px !important;
	border-collapse: collapse !important;
	width: 1px !important;	
	table-layout: fixed !important;
}


/*
///////////////////////////////////////////////////////////////////////////////////////// 
// Super Tables - Skin Classes
// Remove if not needed
///////////////////////////////////////////////////////////////////////////////////////// 
*/

/* sDefault */
.sDefault {
	margin: 0px;
	padding: 0px;
	border: none;
	font-family: Verdana, Arial, sans serif;
	font-size: 0.8em;
}
.sDefault th, .sDefault td {
	border: 1px solid #cccccc;
	padding: 3px 6px 3px 4px;
	white-space: nowrap;
}
.sDefault th {
	background-color: #e5e5e5;
	border-color: #c5c5c5;
}
.sDefault-Fixed {
	background-color: #eeeeee;
	border-color: #c5c5c5;
}

/* sSky */
.sSky {
	margin: 0px;
	padding: 0px;
	border: none;
	font-family: Verdana, Arial, sans serif;
	font-size: 0.8em;
}
.sSky th, .sSky td {
	border: 1px solid #9eb6ce;
	padding: 3px 6px 3px 4px;
	white-space: nowrap;
}
.sSky th {
	background-color: #CFDCEE;
}
.sSky-Fixed {
	background-color: #e4ecf7;
}

/* sOrange */
.sOrange {
	margin: 0px;
	padding: 0px;
	border: none;
	font-family: Verdana, Arial, sans serif;
	font-size: 0.8em;
}
.sOrange th, .sOrange td {
	border: 1px solid #cebb9e;
	padding: 3px 6px 3px 4px;
	white-space: nowrap;
}
.sOrange th {
	background-color: #ECD8C7;
}
.sOrange-Fixed {
	background-color: #f7ede4;
}

/* sDark */
.sDark {
	margin: 0px;
	padding: 0px;
	border: none;
	font-family: Verdana, Arial, sans serif;
	font-size: 0.8em;
	color: #ffffff;
}
.sDark th, .sDark td {
	border: 1px solid #555555;
	padding: 3px 6px 3px 4px;
	white-space: nowrap;
}
.sDark th {
	background-color: #000000;
}
.sDark-Fixed {
	background-color: #222222;
}
.sDark-Main {
	background-color: #333333;
}
```

*supertable.js：*

``` javascript
(function ($) {
    //声明superTable
    var superTable = function (tableId, options) {
        /////* Initialize */
        options = options || {};
        this.cssSkin = options.cssSkin || "";
        this.headerRows = parseInt(options.headerRows || "1");
        this.fixedCols = parseInt(options.fixedCols || "0");
        this.colWidths = options.colWidths || [];
        this.initFunc = options.onStart || null;
        this.callbackFunc = options.onFinish || null;
        this.initFunc && this.initFunc();

        /////* Create the framework dom */
        this.sBase = document.createElement("div");
        this.sFHeader = this.sBase.cloneNode(false);
        this.sHeader = this.sBase.cloneNode(false);
        this.sHeaderInner = this.sBase.cloneNode(false);
        this.sFData = this.sBase.cloneNode(false);
        this.sFDataInner = this.sBase.cloneNode(false);
        this.sData = this.sBase.cloneNode(false);
        this.sColGroup = document.createElement("COLGROUP");

        this.sDataTable = document.getElementById(tableId);
        this.sDataTable.style.margin = "0px";
        /* Otherwise looks bad */
        if (this.cssSkin !== "") {
            this.sDataTable.className += " " + this.cssSkin;
        }
        if (this.sDataTable.getElementsByTagName("COLGROUP").length > 0) {
            this.sDataTable.removeChild(this.sDataTable.getElementsByTagName("COLGROUP")[0]);
            /* Making our own */
        }
        this.sParent = this.sDataTable.parentNode;
        this.sParentHeight = this.sParent.offsetHeight;
        this.sParentWidth = this.sParent.offsetWidth;

        /////* Attach the required classNames */
        this.sBase.className = "sBase";
        this.sFHeader.className = "sFHeader";
        this.sHeader.className = "sHeader";
        this.sHeaderInner.className = "sHeaderInner";
        this.sFData.className = "sFData";
        this.sFDataInner.className = "sFDataInner";
        this.sData.className = "sData";

        /////* Clone parts of the data table for the new header table */
        var alpha, beta, touched, clean, cleanRow, i, j, k, m, n, p;
        this.sHeaderTable = this.sDataTable.cloneNode(false);
        if (this.sDataTable.tHead) {
            alpha = this.sDataTable.tHead;
            this.sHeaderTable.appendChild(alpha.cloneNode(false));
            beta = this.sHeaderTable.tHead;
        } else {
            alpha = this.sDataTable.tBodies[0];
            this.sHeaderTable.appendChild(alpha.cloneNode(false));
            beta = this.sHeaderTable.tBodies[0];
        }
        alpha = alpha.rows;
        for (i = 0; i < this.headerRows; i++) {
            beta.appendChild(alpha[i].cloneNode(true));
        }
        this.sHeaderInner.appendChild(this.sHeaderTable);

        if (this.fixedCols > 0) {
            this.sFHeaderTable = this.sHeaderTable.cloneNode(true);
            this.sFHeader.appendChild(this.sFHeaderTable);
            this.sFDataTable = this.sDataTable.cloneNode(true);
            this.sFDataInner.appendChild(this.sFDataTable);
        }

        /////* Set up the colGroup */
        alpha = this.sDataTable.tBodies[0].rows;
        for (i = 0, j = alpha.length; i < j; i++) {
            clean = true;
            for (k = 0, m = alpha[i].cells.length; k < m; k++) {
                if (alpha[i].cells[k].colSpan !== 1 || alpha[i].cells[k].rowSpan !== 1) {
                    i += alpha[i].cells[k].rowSpan - 1;
                    clean = false;
                    break;
                }
            }
            if (clean === true) break;
            /* A row with no cells of colSpan > 1 || rowSpan > 1 has been found */
        }
        cleanRow = (clean === true) ? i : 0;
        /* Use this row index to calculate the column widths */
        for (i = 0, j = alpha[cleanRow].cells.length; i < j; i++) {
            if (i === this.colWidths.length || this.colWidths[i] === -1) {
                this.colWidths[i] = alpha[cleanRow].cells[i].offsetWidth;
            }
        }
        for (i = 0, j = this.colWidths.length; i < j; i++) {
            this.sColGroup.appendChild(document.createElement("COL"));
            this.sColGroup.lastChild.setAttribute("width", this.colWidths[i]);
        }
        this.sDataTable.insertBefore(this.sColGroup.cloneNode(true), this.sDataTable.firstChild);
        this.sHeaderTable.insertBefore(this.sColGroup.cloneNode(true), this.sHeaderTable.firstChild);
        if (this.fixedCols > 0) {
            this.sFDataTable.insertBefore(this.sColGroup.cloneNode(true), this.sFDataTable.firstChild);
            this.sFHeaderTable.insertBefore(this.sColGroup.cloneNode(true), this.sFHeaderTable.firstChild);
        }

        /////* Style the tables individually if applicable */
        if (this.cssSkin !== "") {
            this.sDataTable.className += " " + this.cssSkin + "-Main";
            this.sHeaderTable.className += " " + this.cssSkin + "-Headers";
            if (this.fixedCols > 0) {
                this.sFDataTable.className += " " + this.cssSkin + "-Fixed";
                this.sFHeaderTable.className += " " + this.cssSkin + "-FixedHeaders";
            }
        }

        /////* Throw everything into sBase */
        if (this.fixedCols > 0) {
            this.sBase.appendChild(this.sFHeader);
        }
        this.sHeader.appendChild(this.sHeaderInner);
        this.sBase.appendChild(this.sHeader);
        if (this.fixedCols > 0) {
            this.sFData.appendChild(this.sFDataInner);
            this.sBase.appendChild(this.sFData);
        }
        this.sBase.appendChild(this.sData);
        this.sParent.insertBefore(this.sBase, this.sDataTable);
        this.sData.appendChild(this.sDataTable);

        /////* Align the tables */
        var sDataStyles, sDataTableStyles;
        this.sHeaderHeight = this.sDataTable.tBodies[0].rows[(this.sDataTable.tHead) ? 0 : this.headerRows].offsetTop;
        sDataTableStyles = "margin-top: " + (this.sHeaderHeight * -1) + "px;";
        sDataStyles = "margin-top: " + this.sHeaderHeight + "px;";
        sDataStyles += "height: " + (this.sParentHeight - this.sHeaderHeight) + "px;";
        if (this.fixedCols > 0) {
            /* A collapsed table's cell's offsetLeft is calculated differently (w/ or w/out border included) across broswers - adjust: */
            this.sFHeaderWidth = this.sDataTable.tBodies[0].rows[cleanRow].cells[this.fixedCols].offsetLeft;
            if (window.getComputedStyle) {
                alpha = document.defaultView;
                beta = this.sDataTable.tBodies[0].rows[0].cells[0];
                if (navigator.taintEnabled) { /* If not Safari */
                    this.sFHeaderWidth += Math.ceil(parseInt(alpha.getComputedStyle(beta, null).getPropertyValue("border-right-width")) / 2);
                } else {
                    this.sFHeaderWidth += parseInt(alpha.getComputedStyle(beta, null).getPropertyValue("border-right-width"));
                }
            } else if (/*@cc_on!@*/0) { /* Internet Explorer */
                alpha = this.sDataTable.tBodies[0].rows[0].cells[0];
                beta = [alpha.currentStyle["borderRightWidth"], alpha.currentStyle["borderLeftWidth"]];
                if (/px/i.test(beta[0]) && /px/i.test(beta[1])) {
                    beta = [parseInt(beta[0]), parseInt(beta[1])].sort();
                    this.sFHeaderWidth += Math.ceil(parseInt(beta[1]) / 2);
                }
            }

            /* Opera 9.5 issue - a sizeable data table may cause the document scrollbars to appear without this: */
            if (window.opera) {
                this.sFData.style.height = this.sParentHeight + "px";
            }

            this.sFHeader.style.width = this.sFHeaderWidth + "px";
            sDataTableStyles += "margin-left: " + (this.sFHeaderWidth * -1) + "px;";
            sDataStyles += "margin-left: " + this.sFHeaderWidth + "px;";
            sDataStyles += "width: " + (this.sParentWidth - this.sFHeaderWidth) + "px;";
        } else {
            sDataStyles += "width: " + this.sParentWidth + "px;";
        }
        this.sData.style.cssText = sDataStyles;
        this.sDataTable.style.cssText = sDataTableStyles;

        /////* Set up table scrolling and IE's onunload event for garbage collection */
        (function (st) {
            if (st.fixedCols > 0) {
                st.sData.onscroll = function () {
                    st.sHeaderInner.style.right = st.sData.scrollLeft + "px";
                    st.sFDataInner.style.top = (st.sData.scrollTop * -1) + "px";
                };
            } else {
                st.sData.onscroll = function () {
                    st.sHeaderInner.style.right = st.sData.scrollLeft + "px";
                };
            }
            if (/*@cc_on!@*/0) { /* Internet Explorer */
                window.attachEvent("onunload", function () {
                    st.sData.onscroll = null;
                    st = null;
                });
            }
        })(this);

        this.callbackFunc && this.callbackFunc();
    };

    //附加到JQuery中
    $.fn.extend(
        {
            toSuperTable: function (options) {

                //SuperTable默认配置
                var setting = $.extend(
                    {
                        headerRows: 1,//锁定行
                        fixedCols: 1,//锁定列
                        width: "100%", height: "100%", padding: "0px",
                        overflow: "hidden", colWidths: undefined,
                        onStart: function () {
                        },
                        onFinish: function () {
                        },
                        cssSkin: "sOrange"//皮肤选择，在supertables.css底部定义
                    }, options);
                return this.each(function () {
                    var q = $(this);
                    var id = q.attr("id");
                    q.removeAttr("style").wrap("<div id='" + id + "_box'></div>");
                    var nonCssProps = ["fixedCols", "headerRows", "onStart", "onFinish", "cssSkin", "colWidths"];
                    var container = $("#" + id + "_box");
                    for (var p in setting) {
                        if ($.inArray(p, nonCssProps) == -1) {
                            container.css(p, setting[p]);
                            delete setting[p];
                        }
                    }
                    var mySt = new superTable(id, setting);
                });
            }
        });
})(jQuery);
```

*效果图：*
<iframe src="/res/freezedtable/supertable.html" style="border:0;width:100%;height:225px;"></iframe>

*完整代码下载：*
[SuperTable轻量级完整案例下载](/res/freezedtable/SuperTable.zip)