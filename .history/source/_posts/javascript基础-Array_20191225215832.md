---
title: javascript基础-Array
date: 2017-07-10 16:04:40
categories:
  - 历史
  - 前端
tags:
  - JavaScript
  - Array
---

简单介绍一下

<!--more-->

## 创建数组

```bash
1. var arr = new Array(1,2,3,4)
2. var arr = Array(1,2,3,4)
3. var arr = [1,2,3,4]
```

> ps: 注意 var arr = new Array(3) 与 var arr = new Array(1,2,3)代表的意义是不同的，前者代表新建长度为 3 的一个空数组，后者是建立有 1，2，3 元素的数组

## 数组方法

### `concat`

//作用，是将几个数组连接成一个新数组

```bash
var arr = new Array(1,2,3,4,5);
var arr1 = arr.concat(6,7,8);
console.log(arr1)     // [1, 2, 3, 4, 5, 6, 7, 8]

```

### `join`

//作用，将数组中的元素连接起来，默认是以`，`连接

```bash
var arr = [1,2,3,4];
var arr1 = arr.join('-');
console.log(arr1);  // '1-2-3-4'
```

### `push`

//作用，向数组尾部添加新值，并返回该数组长度...可用于模拟入栈

```bash
var arr = [1,2];
var arr1 = arr.push(3);
console.log(arr1);  // 3
```

### `pop`

//作用，从数组移出最后一个元素，并返回该元素。...可用于模拟出栈

```bash
var arr = [1,2,3,4];
var arr1 = arr.pop();
console.log(arr1);   // 4

```

### `unshift`

//作用，从数组开始位置添加新元素，并返回长度。。。模拟入队列

```bash
var arr = [2,3,4];
var arr1 = arr.unshift(1);
```

### `shift`

//作用，从数组开始为值取数据，并返回该元素。。。。模拟出队列

```bash
var arr = [1,2,3,4]
var arr1 = arr.shift();
```

### `slice`

//作用，从数组提取一个片段，并作为一个新数组返回。不会破坏原数组
//用法： slice(start,end)

```bash
var arr = [1,2,3,4];
var arr1 = arr.slice(0,1);
console.log(arr1); // [1]
console.log(arr); // [1,2,3,4]
```

### `splice`

//作用，从数组截取特定长度的数据，并对原数据进行更新（删除，新增等）操作。会破坏原数组

```bash
//删除索引1-3
var arr = [1,2,3,4,5];
var arr1 = arr.splice(1,3);
//更新索引1-3
var arr = [1,2,3,4,5];
var arr1 = arr.splice(1,3,'update1','update2','update3');
//新增索引1-3
var arr = [1,2,3,4,5];
var arr1 = arr.splice(1,3,'update1','update2','update3','update4');
```

### `reverse`

//作用，颠倒数组顺序

```bash
var arr = [1,2,3,4,5];
var arr1 = arr.reverve(); //[5, 4, 3, 2, 1]
```

### `sort`

//作用，给数组排序。
//语法，arr.sort(compareFunction(a,b){})

- 如果 compareFunction(a, b) 小于 0 ，那么 a 会被排列到 b 之前；
- 如果 compareFunction(a, b) 等于 0 ， a 和 b 的相对位置不变。备注： ECMAScript 标准并不保证这一行为，而且也不是所有浏览器都会遵守（例如 Mozilla 在 2003 年之前的版本）；
- 如果 compareFunction(a, b) 大于 0 ， b 会被排列到 a 之前。
  compareFunction(a, b) 必须总是对相同的输入返回相同的比较结果，否则排序的结果将是不确定的。

比较函数格式如下：

```bash
function compare(a, b) {
  if (a is less than b by some ordering criterion) {
    return -1;
  }
  if (a is greater than b by the ordering criterion) {
    return 1;
  }
  // a must be equal to b
  return 0;
}
```

希望比较数字而非字符串，比较函数可以简单的以 a 减 b，如下的函数将会将数组升序排列

```bash
function compareNumbers(a, b) {
  return a - b;
}
```

### `indexOf`

作用，在数组中搜索`条件`并返回第一个匹配的索引。

```bash
var arr = [1,'a','b','c'];
console.log(arr.indexOf('b')); //2
```

### `lastIndexOf`

//作用，与`indexOf`类似，只不过从尾部索引
var arr = [1,'a','b','c'];
console.log(arr.lastIndexOf('b')); //1

````
### `forEach`
//作用，循环数组
```bash
var arr = [1,2,3,4];
arr.forEach(function(v,k){
    console.log(v);
});
// 1\n,2\n,3\n,4
````

### `map`

遍历数组，并通过 callback 对数组元素进行操作，并将所有操作结果放入数组中并返回该数组

```bash
var a1 = ['a', 'b', 'c'];
var a2 = a1.map(function(currentValue, index, array) { return currentValue.toUpperCase(); });
console.log(a2); //  A,B,C
```

> callback
> 生成新数组元素的函数，使用三个参数：

- currentValue
  callback 的第一个参数，数组中正在处理的当前元素。
- index
  callback 的第二个参数，数组中正在处理的当前元素的索引。
- array
  callback 的第三个参数，map 方法被调用的数组。
- thisArg
  可选的。执行 callback 函数时 使用的 this 值。

### `filter(callback[, thisObject])`

callback 在这里担任的是过滤器的角色，当元素符合条件，过滤器就返回 true，而 filter 则会返回所有符合过滤条件的元素

```bash
var a1 = ['a', 10, 'b', 20, 'c', 30];
var a2 = a1.filter(function(item) { return typeof item == 'number'; });
console.log(a2); // 10,20,30
```

### `every(callback[, thisObject])`

every 其实类似 filter，只不过它的功能是判断是不是数组中的所有元素都符合条件，并且返回的是布尔值

```bash
function isNumber(value){
  return typeof value == 'number';
}
var a1 = [1, 2, 3];
console.log(a1.every(isNumber)); //  true
var a2 = [1, '2', 3];
console.log(a2.every(isNumber)); // false
```

### `some(callback[, thisObject])`

类似 every，不过前者要求都符合筛选条件才返回 true，后者只要有符合条件的就返回 true

```bash
function isNumber(value){
  return typeof value == 'number';
}
var a1 = [1, 2, 3];
console.log(a1.some(isNumber)); //  true
var a2 = [1, '2', 3];
console.log(a2.some(isNumber)); //  true
var a3 = ['1', '2', '3'];
console.log(a3.some(isNumber)); //  false
```

### `reduce`

他数组元素两两递归处理的方式把数组计算成一个值

```bash
var a = [10, 20, 30];
var total = a.reduce(function(first, second) { return first + second; }, 0);
console.log(total)
```

[详见这里](http://www.supernever.com/2017/01/24/reduce/)

### `reduceRight(callback[, initalvalue])`

同 reduce，只不过从尾部开始
