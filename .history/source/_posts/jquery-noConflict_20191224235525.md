---
title: jquery-noConflict
nav:
- 前端
categories:
  - 前端
tags:
- jquery
- jquery-noConflict
date: 2016-08-17 23:14:47
---
jquery博大精深，你真的明白吗？你是否只会使用个简单的选择器呢？
<!-- more -->
无冲突处理也叫多库共存。直接来代码，你是否真的明白。本人看了半小时，恕我太笨，想了好多东西。
```bash
var window = this,
	undefined,
	_jQuery = window.jQuery,
	_$ = window.$,
	jQuery = window.jQuery = window.$ = function(selector,context){
	return new jQuery.fn.init(selector,context)
};

jQuery.extend({
	noConflict:function(deep){
		window.$ = _$;
		if(deep)
			window.jQuery = _jQuery;
		return jQuery;
	}
});
```
`注意一点：`使用jquery的noconflict方法，需要先引入别的库。不知道你看到上面的处理方式，是否有疑问呢？为啥这样就能解决冲突呢？其实重点只有一个jQuery和window.jQuery 到底有啥区别呢？
栗子一颗：
```bash
<head>
    <meta charset="UTF-8">
    <title>DIY A JQ</title>
    <script>
        $ = 'old $';
        jQuery = 'old JQ'
    </script>
    <script src="jQuery.js"></script>
</head>
<body>

<div>hello world</div>

<script>
    var $$$ = $.noConflict(true);
    console.log($);        //old $
    console.log(jQuery); 	//old JQ
    console.log($$$); 		//$(selector, [startNode]) { [Command Line API] }
</script>
```
