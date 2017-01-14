---
layout:     post
title:      "Knowledge.Tree使用说明"
subtitle:   "复杂树形结构动态展现"
date:       2017-01-12 17:00:00
author:     "Colin"
header-img: "img/2016/bg-post/markdown-grammar.jpg"
tags:
    - 前端开发
    - Canvas
---

> "源自专家经验库的灵感设计"

## Reference

`konva.min.js`<br>
`knowledge.tree.js`

## Parameter

### option

<table>
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>含义</th>
        <th>必需</th>
    </tr>
    <tr>
        <td>container</td>
        <td>string</td>
        <td>绘图容器Id</td>
        <td>true</td>
    </tr>
    <tr>
        <td>x</td>
        <td>number</td>
        <td>内容区起始x坐标</td>
        <td>false</td>
    </tr>
    <tr>
        <td>width</td>
        <td>number</td>
        <td>舞台宽度</td>
        <td>false</td>
    </tr>
    <tr>
        <td>height</td>
        <td>number</td>
        <td>舞台高度</td>
        <td>false</td>
    </tr>
    <tr>
        <td>rootText</td>
        <td>string</td>
        <td>根节点title</td>
        <td>true</td>
    </tr>
    <tr>
        <td>service</td>
        <td>Object</td>
        <td>数据服务</td>
        <td>true</td>
    </tr>
    <tr>
        <td>showDialog</td>
        <td>Function</td>
        <td>点击或触摸节点处理函数</td>
        <td>true</td>
    </tr>
</table>

### service

``` javascript
{
    bomResource: Promise;// /model/bom/list
    failureResource: Promise;// /knowledge/listfaultfailure
    causeResource: Promise;// /knowledge/listfaultcause
    treatmentResource: Promise;// /knowledge/listfaulttrentment
    searchBoms: Promise;// /model/bom/treekl
    searchFailure: Promise;// /knowledge/listfaultfailure?moduleId=01&partId=10&kw=
}
```

### showDialog
*示例：*

``` javascript
function showDialog(title,content){
  //do something here
}
```

## Code
``` javascript
let tree = new KnowledgeTree({
    container: 'container',
    height: document.querySelector('ion-content').clientHeight,
    x: 10,
    rootText: 'ZJ-109',
    service: service,
    showDialog: function (title, content) {
        alert(content);
    }
});

//搜索现象或faultCode
tree.search('keyword');
```