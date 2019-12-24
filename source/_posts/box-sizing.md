---
title: box-sizing
date: 2017-11-10 15:01:25
tags:
- css3
- box-sizing
---
简单介绍一下 
<!--more-->

`box-sizing` 属性用于更改用于计算元素宽度和高度的默认的` CSS 盒子模型`。可以使用此属性来模拟不正确支持CSS盒子模型规范的浏览器的行为。

`box-sizing `属性:

- `content-box`  是默认值。如果你设置一个元素的宽为100px，那么这个元素的内容区会有100px宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。

```
尺寸计算公式：width = 内容的宽度，height = 内容的高度。宽度和高度都不包含内容的边框（border）和内边距（padding）。

```

- `border-box` 告诉浏览器去理解你设置的边框和内边距的值是包含在width内的。也就是说，如果你将一个元素的width设为100px,那么这100px会包含其它的border和padding，内容区的实际宽度会是width减去border + padding的计算值。大多数情况下这使得我们更容易的去设定一个元素的宽高

```
width = border + padding + 内容的  width，
height = border + padding + 内容的 height。
```


<mark style="background-color:red">一些专家甚至建议所有的Web开发者们将所有的元素的box-sizing都设为border-box。</mark>