---
title: DOM-getBoundingClientRect
categories:
  - 前端
nav:
  - 历史
tags:
  - DOM
  - getBoundingClientRect
date: 2016-07-12 20:10:29
---

Element.getBoundingClientRect()方法返回元素的大小及其相对于视口的位置。

<!--more-->

# 语法

rectObject = object.getBoundingClientRect();

## 返回

返回值是一个 DOMRect 对象，这个对象是由该元素的**getClientRects()**  方法返回的一组矩形的集合,  即：是与该元素相关的 CSS  边框集合  。

返回值是一个 DOMRect 对象，它包含了一组用于描述边框的只读属性——left、top、right 和 bottom，属性单位为像素，top 和 left 的属性值是相对于视口的 top-left（上、左）位置而言的。

> 注意: Gecko 1.9.1  给 DOMRect 对象添加了 width 和 height 属性

**空边框盒**（注：没有内容的边框）会被忽略。如果所有的元素边框都是空边框，那么这个矩形给该元素返回的 width、height 值为 0，left、top 值为第一个 css 盒子（按内容顺序）的 top-left 值。

当计算边界矩形时，会考虑视口区域（或其他可滚动元素）内的滚动操作，也就是说，当滚动位置发生了改变，top 和 left 属性值就会随之立即发生变化（因此，它们的值是相对于视口的，而不是绝对的）。如果不希望属性值随视口变化，那么只要给 top、left 属性值加上当前的滚动位置（通过 window.scrollX 和 window.scrollY），这样就可以获取与当前的滚动位置无关的常量值。

为了跨浏览器兼容，请使用 window.pageXOffset 和 window.pageYOffset 代替 window.scrollX  和  window.scrollY，
当 window.pageXOffset, window.pageYOffset, window.scrollX and window.scrollY 未定义时,  使用

```bash
(((t = document.documentElement) || 
(t = document.body.parentNode)) &&typeof t.ScrollLeft == 'number' ? t : document.body).ScrollLeft
 and 
(((t = document.documentElement) || 
(t = document.body.parentNode)) 
&& typeof t.ScrollTop == 'number' ? t : document.body).ScrollTop 
```

## 举例

// rect 是一个具有四个属性 left、top、right、bottom 的 DOMRect 对象
//注：DOMRect  是  TextRectangle 或  ClientRect  的别称，他们是相同的。

```bash
var rect = obj.getBoundingClientRect();
```

## 注意

在 IE8 或者更低浏览器版本中，getBoudingClientRect()方法返回的 TextRectangle 对象没有 height 和 width 属性。同时，额外的属性（包括 height 和 width）也不能添加到 TextRectangle 对象中去。

从 Gecko 12.0 开始,当计算元素的边界矩形的时候开始考虑 CSS transforms 。
getBoundingClientRect() 在 MS IE DHTML 对象中被首次引入

## 规范

可以直接戳[CSSOM Views: The getClientRects() and getBoundingClientRect() methods](https://www.w3.org/TR/cssom-view-1/#the-getclientrects%28%29-and-getboundingclientrect%28%29-methods)阅读
