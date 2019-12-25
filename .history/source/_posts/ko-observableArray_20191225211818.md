---
title: knockout.js--observableArray
categories:
  - 历史
tags: 
- javascript
- knockout.js
date: 2016-06-06 21:19:08
---
先来一段英文装装13:
If you want to detect and respond to changes on one object, you’d use observables. If you want to detect and respond to changes of a collection of things, use an observableArray. This is useful in many scenarios where you’re displaying or editing multiple values and need repeated sections of UI to appear and disappear as items are added and removed.
我不会告诉你，这是官网的原文。。。。。
<!--more-->
### 什么意思呢
哈哈哈，我相信历史小基佬们都知道什么意思，也就是ko给我们不单单提供了一个ko.observable(),用于单一绑定；而且也给我们提供了一个直接绑定数组的监听，对，就是这货ko.observableArray();
### 那到底怎么用呢
## 直接来栗子
```bash
var myObservableArray = ko.observableArray();    // 初始化一个空数组
myObservableArray.push('Some value');			 //塞值
```
## 当然你也可以直接这样赋值
```bash
var anotherObservableArray = ko.observableArray([
    { name: "Bungle", type: "Bear" },
    { name: "George", type: "Hippo" },
    { name: "Zippy", type: "Unknown" }
]);
```
## 那么到底在项目中怎么用呢
还是一样，这次来一个表单演示(view)：
```bash
<form data-bind="submit: addItem">
    New item:
    <input data-bind='value: itemToAdd, valueUpdate: "afterkeydown"' />
    <button type="submit" data-bind="enable: itemToAdd().length > 0">Add</button>
    <p>Your items:</p>
    <select multiple="multiple" width="50" data-bind="options: items"> </select>
</form>
```
js代码（view model）怎么写呢：
```bash
var SimpleListModel = function(items) {
    this.items = ko.observableArray(items);
    this.itemToAdd = ko.observable("");
    this.addItem = function() {
        if (this.itemToAdd() != "") {
            this.items.push(this.itemToAdd()); //增加item。改变item通过修改observablearray促使相关的UI更新。
            this.itemToAdd(""); // 清空text框，因为他还要被itemToAdd监听
        }
    }.bind(this);  // 确保this总是绑定在view model上
};
 
ko.applyBindings(new SimpleListModel(["Alpha", "Beta", "Gamma"]));
```
还用分析一下嘛？好吧，不给分析，请看详细演示栗子。[戳我嘛](http://jsbin.com/fiyivo/edit?html,output)
不知道小屌们对<font color="red">**data-bind**</font>，是否感到好奇呢？戳回首页看看有没有你的答案呢？
















