---
title: knockoutjs--data-bind
categories:
  - 历史
  - JS
nav:
  - 历史
tags:
  - knockout.js
date: 2016-06-10 22:44:20
---

> 总体来说 ko 绑定分为几大类：

- 控制文本与外观(Controlling text and appearance)
- 控制流(Control flow)
- 作用于与表单字段(Working with form fields)
- 自定义(Creating custom bindings)
  <!--more-->

## 控制文本与外观

### The "visible" binding

**"visible"**绑定，可以根据传入的显隐值来确定 DOM 元素的显隐性。例如：

```bash
<div data-bind="visible: shouldShowMessage">
    当 shouldShowMessage 为true时，显示该条文本，反之，则不显示。
</div>

<script type="text/javascript">
    var viewModel = {
        shouldShowMessage: ko.observable(true) // 初始化时显示文本
    };
    viewModel.shouldShowMessage(false); // ... 此时隐藏文本
    viewModel.shouldShowMessage(true); // ... 重新显示文本
    ko.applyBindings(viewModel);
</script>
```

#### 那么具体参数是什么呢？

- 主参数

- 当参数设置为一个假值时（例如：布尔值 false， 数字值 0， 或者 null， 或者 undefined） ,该绑定将设置该元素的 style.display 值为 none，让元素隐藏。它的优先级高于你在 CSS 里定义的任何 display 样式。

- 当参数设置为一个真值时（例如：布尔值 true，或者非空 non-null 的对象或者数组） ,该绑定会删除该元素的 style.display 值，让元素可见。然后你在 CSS 里自定义的 display 样式将会自动生效。

- 如果参数是监控属性 observable 的，那元素的 visible 状态将根据参数值的变化而变化，如果不是，那元素的 visible 状态将只设置一次并且以后不在更新。

> 请注意，任何与"display"样式相关的都可以与你定义的的 CSS 样式进行组合使用（所以 CSS 样式如 x { display:table-row } 可以与 bingding 正常关联使用）。

- 其它参数
- 无

#### 你也可以使用 JavaScript 函数或者表达式作为参数。这样的话，函数或者表达式的结果将决定是否显示/隐藏这个元素。例如：

```bash
<div data-bind="visible: myValues().length > 0">
    当 'myValues' 存在值时，此条message显示
</div>

<script type="text/javascript">
    var viewModel = {
        myValues: ko.observableArray([]) // 初始值为空，所以message隐藏
    };
    viewModel.myValues.push("some value"); // 此时message可见
    ko.applyBindings(viewModel);
</script>
```

### The "text" binding

**"text"**绑定，可以根据传入的文本值来确定 DOM 元素的文本值。一般情况绑定在&lt;span&gt;或者&lt;em&gt;这种文本显示元素上，不过不限定使用在其他元素上。例如：

```bash
Today's message is: <span data-bind="text: myMessage"></span>

<script type="text/javascript">
    var viewModel = {
        myMessage: ko.observable() // 初始化为空值
    };
    viewModel.myMessage("Hello, world!"); // 文本显示
    ko.applyBindings(viewModel);
</script>
```

#### 具体参数是什么呢？

- 主参数

- KO 将参数值会设置在元素的 innerText （IE）或 textContent（Firefox 和其它相似浏览器）属性上。原来的文本将会被覆盖。

- 如果参数是监控属性 observable 的，那元素的 text 文本将根据参数值的变化而更新，如果不是，那元素的 text 文本将只设置一次并且以后不在更新。

- 如果你传的是不是数字或者字符串（例如一个对象或者数组），那显示的文本将是 yourParameter.toString()的等价内容。

- 其它参数

- 无

> Note 1: 使用函数或者表达式来控制元素的可见性

```bash
The item is <span data-bind="text: priceRating"></span> today.

<script type="text/javascript">
    var viewModel = {
        price: ko.observable(24.95)
    };
    viewModel.priceRating = ko.pureComputed(function() {
        return this.price() > 50 ? "expensive" : "affordable";
    }, viewModel);
</script>
```

> Note 2: 使用函数或者表达式来控制元素的可见性

因为该绑定是设置元素的**innerText**或**textContent** （而不是 innerHTML），所以它是安全的，没有 HTML 或者脚本注入的风险。例如：如果你编写如下代码：

```bash
viewModel.myMessage("<i>Hello, world!</i>");
```

… 它不会显示斜体字，而是原样输出标签。如果你需要显示 HTML 内容，请参考 html 绑定.

### The "html" binding

html 绑定到 DOM 元素上，使得该元素显示的 HTML 值为你绑定的参数。如果在你的 view model 里声明 HTML 标记并且 render 的话，那非常有用。

```bash
<div data-bind="html: details"></div>
 
<script type="text/javascript">
    var viewModel = {
        details: ko.observable() // Initially blank
    };
    viewModel.details("<em>For further details, view the report <a href='report.html'>here</a>.</em>"); // HTML content appears
</script>
```

#### 参数有哪些呢

- 主参数
- KO 设置该参数值到元素的 innerHTML 属性上，元素之前的内容将被覆盖。

- 如果参数是监控属性 observable 的，那元素的内容将根据参数值的变化而更新，如果不是，那元素的内容将只设置一次并且以后不在更新。

- 如果你传的是不是数字或者字符串（例如一个对象或者数组），那显示的文本将是 yourParameter.toString()的等价内容。

- 其它参数

- 无

> 注：关于 HTML encoding
> 因为该绑定设置元素的 innerHTML，你应该注意不要使用不安全的 HTML 代码，因为有可能引起脚本注入攻击。如果你不确信是否安全（比如显示用户输入的内容），那你应该使用 text 绑定，因为这个绑定只是设置元素的 text 值 innerText 和 textContent。

- 依赖性

除 KO 核心类库外，无依赖。

### The "css" binding

css 绑定是添加或删除一个或多个 CSS class 到 DOM 元素上。 非常有用，比如当数字变成负数时高亮显示。（注：如果你不想应用 CSS class 而是想引用 style 属性的话，请参考 style 绑定。）

```bash
<div data-bind="css: { profitWarning: currentProfit() < 0 }">
   Profit Information
</div>
 
<script type="text/javascript">
    var viewModel = {
        currentProfit: ko.observable(150000) // Positive value, so initially we don't apply the "profitWarning" class
    };
    viewModel.currentProfit(-50); // Causes the "profitWarning" class to be applied
</script>
```

- 主参数

- 该参数是一个 JavaScript 对象，属性是你的 CSS class 名称，值是比较用的 true 或 false，用来决定是否应该使用这个 CSS class。

- 你可以一次设置多个 CSS class。例如，如果你的 view model 有一个叫 isServre 的属性，

```bash
<div data-bind="css: { profitWarning: currentProfit() < 0, majorHighlight: isSevere }">
```

- 非布尔值会被解析成布尔值。例如， 0 和 null 被解析成 false，21 和非 null 对象被解析成 true。

- 如果参数是监控属性 observable 的，那随着值的变化将会自动添加或者删除该元素上的 CSS class。如果不是，那 CSS class 将会只添加或者删除一次并且以后不在更新。

- 你可以使用任何 JavaScript 表达式或函数作为参数。KO 将用它的执行结果来决定是否应用或删除 CSS class。

- 其它参数

- 无

> 注：应用的 CSS class 的名字不是合法的 JavaScript 变量命名
> 如果你想使用 my-class class，你不能写成这样：

```bash
<div data-bind="css: { my-class: someValue }">...</div>
```

> 因为 my-class 不是一个合法的命名。解决方案是：在 my-class 两边加引号作为一个字符串使用。这是一个合法的 JavaScript 对象 文字（从 JSON 技术规格说明来说，你任何时候都应该这样使用，虽然不是必须的）。例如,

```bash
<div data-bind="css: { 'my-class': someValue }">...</div>
```

### The "style" binding

style 绑定是添加或删除一个或多个 DOM 元素上的 style 值。比如当数字变成负数时高亮显示，或者根据数字显示对应宽度的 Bar。（注：如果你不是应用 style 值而是应用 CSS class 的话，请参考 CSS 绑定。）

```bash
<div data-bind="style: { color: currentProfit() < 0 ? 'red' : 'black' }">
   Profit Information
</div>
 
<script type="text/javascript">
    var viewModel = {
        currentProfit: ko.observable(150000) // Positive value, so initially black
    };
    viewModel.currentProfit(-50); // Causes the DIV's contents to go red
</script>
```

当 currentProfit 小于 0 的时候 div 的 style.color 是红色，大于的话是黑色。

- 主参数

- 该参数是一个 JavaScript 对象，属性是你的 style 的名称，值是该 style 需要应用的值。

- 你可以一次设置多个 style 值。例如，如果你的 view model 有一个叫 isServre 的属性，

```bash
<div data-bind="style: { color: currentProfit() < 0 ? 'red' : 'black', fontWeight: isSevere() ? 'bold' : '' }">...</div>
```

- 如果参数是监控属性 observable 的，那随着值的变化将会自动添加或者删除该元素上的 style 值。如果不是，那 style 值将会只应用一次并且以后不在更新。

- 你可以使用任何 JavaScript 表达式或函数作为参数。KO 将用它的执行结果来决定是否应用或删除 style 值。

- 其它参数

- 无

> 注：应用的 style 的名字不是合法的 JavaScript 变量命名
> 如果你需要应用 font-weight 或者 text-decoration，你不能直接使用，而是要使用 style 对应的 JavaScript 名称。
> 错误： { font-weight: someValue }; 正确： { fontWeight: someValue }
> 错误： { text-decoration: someValue }; 正确： { textDecoration: someValue }

参考：[style 名称和对应的 JavaScript 名称列表](http://www.comptechdoc.org/independent/web/cgi/javamanual/javastyle.html)

- 依赖性

- 除 KO 核心类库外，无依赖。

### The "attr" binding

attr 绑定提供了一种方式可以设置 DOM 元素的任何属性值。你可以设置 img 的 src 属性，连接的 href 属性。使用绑定，当模型属性改变的时候，它会自动更新。

```bash
<a data-bind="attr: { href: url, title: details }">
    Report
</a>
 
<script type="text/javascript">
    var viewModel = {
        url: ko.observable("year-end.html"),
        details: ko.observable("Report including final year-end statistics")
    };
</script>
```

- 主参数

- 该参数是一个 JavaScript 对象，属性是你的 attribute 名称，值是该 attribute 需要应用的值。

- 如果参数是监控属性 observable 的，那随着值的变化将会自动添加或者删除该元素上的 attribute 值。如果不是，那 attribute 值将会只应用一次并且以后不在更新。

- 其它参数

- 无

> 注：应用的属性名字不是合法的 JavaScript 变量命名

如果你要用的属性名称是 data-something 的话，你不能这样写：

```bash
<div data-bind="attr: { data-something: someValue }">...</div>
```

> … 因为 data-something 不是一个合法的命名。解决方案是：在 data-something 两边加引号作为一个字符串使用。这是一个合法的 JavaScript 对象 文字（从 JSON 技术规格说明来说，你任何时候都应该这样使用，虽然不是必须的）。例如,

```bash
<div data-bind="attr: { ‘data-something’: someValue }">...</div>
```

- 依赖性
- 除 KO 核心类库外，无依赖。

## 控制流

### foreach 绑定

foreach 绑定其实说的简单点，它就是将数组（observableArray）进行遍历，按照给定的规则将所有数据层层展示出来。

#### 数组迭代

```bash
<table>
    <thead>
        <tr><th>First name</th><th>Last name</th></tr>
    </thead>
    <tbody data-bind="foreach: people">
        <tr>
            <td data-bind="text: firstName"></td>
            <td data-bind="text: lastName"></td>
        </tr>
    </tbody>
</table>
 
<script type="text/javascript">
    ko.applyBindings({
        people: [
            { firstName: 'Bert', lastName: 'Bertington' },
            { firstName: 'Charles', lastName: 'Charlesforth' },
            { firstName: 'Denise', lastName: 'Dentiste' }
        ]
    });
</script>
```

#### 动态添加／删除数组值

- View 代码

```bash
<h4>People</h4>
<ul data-bind="foreach: people">
    <li>
        Name at position <span data-bind="text: $index"> </span>:
        <span data-bind="text: name"> </span>
        <a href="#" data-bind="click: $parent.removePerson">Remove</a>
    </li>
</ul>
<button data-bind="click: addPerson">Add</button>
```

- View model 代码

```bash
function AppViewModel() {
    var self = this;
 
    self.people = ko.observableArray([
        { name: 'Bert' },
        { name: 'Charles' },
        { name: 'Denise' }
    ]);
 
    self.addPerson = function() {
        self.people.push({ name: "New at " + new Date() });
    };
 
    self.removePerson = function() {
        self.people.remove(this);
    }
}
 
ko.applyBindings(new AppViewModel());
```

完整代码请[查看](http://jsbin.com/jitoxihose/edit?html,output)

> note1:可以使用\$data 作为每一层数组的索引

```bash
<ul data-bind="foreach: months">
    <li>
        The current item is: <b data-bind="text: $data"></b>
    </li>
</ul>
 
<script type="text/javascript">
    ko.applyBindings({
        months: [ 'Jan', 'Feb', 'Mar', 'etc' ]
    });
</script>
```

---

> note2:也可以使用其他上下文索引$index,$parent

```bash
<h1 data-bind="text: blogPostTitle"></h1>
<ul data-bind="foreach: likes">
    <li>
        <b data-bind="text: name"></b> likes the blog post <b data-bind="text: $parent.blogPostTitle"></b>
    </li>
</ul>
```

这里可以注释说一下，$index就是指循环的第几层，从0开始。而$parent 指的是跟循环体同级的索引，也就是类似上面栗子中循环体 likes 与 blogPostTitle 所属一级，所以可以索引到，当然这样解释有点牵强，你可以自己理解。

> note3:可以给 foreach 循环添加索引

```bash
<ul data-bind="foreach: { data: categories, as: 'category' }">
    <li>
        <ul data-bind="foreach: { data: items, as: 'item' }">
            <li>
                <span data-bind="text: category.name"></span>:
                <span data-bind="text: item"></span>
            </li>
        </ul>
    </li>
</ul>
 
<script>
    var viewModel = {
        categories: ko.observableArray([
            { name: 'Fruit', items: [ 'Apple', 'Orange', 'Banana' ] },
            { name: 'Vegetables', items: [ 'Celery', 'Corn', 'Spinach' ] }
        ])
    };
    ko.applyBindings(viewModel);
//固定写法就是 data:XX,as:'XXX'，<font color="red">特别注意</font>as后面的一定得是字符串
</script>
```

---

> note4:对于没有容器时，如何使用 foreach 进行循环
> 如果你想要达到这种效果

```bash
<ul>
    <li class="header">Header item</li>
    <!--以下内容由一个数组动态创建 -->
    <li>Item A</li>
    <li>Item B</li>
    <li>Item C</li>
</ul>
```

你想要循环的数据既不能挂在 ul 上面（如果挂在 ul 上第一个 li 也会被重复加载），又不能使用其他 DOM 元素因为 ul 里面只允许 li 标签
这种情况我怎么是不是就没有办法实现了呢？很明显不是的。可以采用如下方式实现：

```bash
<ul>
    <li class="header">Header item</li>
    <!-- ko foreach: myItems -->
        <li>Item <span data-bind="text: $data"></span></li>
    <!-- /ko -->
</ul>
 
<script type="text/javascript">
    ko.applyBindings({
        myItems: [ 'A', 'B', 'C' ]
    });
</script>
```

&lt;!-- ko --&gt;和&lt;!-- /ko--&gt;knockout 理解这个虚拟元素的语法，如果你有一个真正的容器元素时会进行绑定。

> note5:后处理或者给生成的 DOM 节点添加动画效果
> 如果你需要对生成的 DOM 元素运行一些进一步的自定义逻辑，你可以使用任何的 afterRender/afterAdd/beforeRemove/beforeMove/afterMove 回调处理。
> 以 afterAdd 举例如下：

```bash
<ul data-bind="foreach: { data: myItems, afterAdd: yellowFadeIn }">
    <li data-bind="text: $data"></li>
</ul>
 
<button data-bind="click: addItem">Add</button>
 
<script type="text/javascript">
    ko.applyBindings({
        myItems: ko.observableArray([ 'A', 'B', 'C' ]),
        yellowFadeIn: function(element, index, data) {
            $(element).filter("li")
                      .animate({ backgroundColor: 'yellow' }, 200)
                      .animate({ backgroundColor: 'white' }, 800);
        },
        addItem: function() { this.myItems.push('New item'); }
    });
</script>
```

其他暂不举例了，通过字面意思其实就能知道什么意思。

### if 绑定

if 与 visible 用法类似，但也不尽相同。
这个例子表明，如果绑定可以动态地添加和删除标记为可观察值变化的部分。
view 层：

```bash
<label><input type="checkbox" data-bind="checked: displayMessage" /> Display message</label>
 
<div data-bind="if: displayMessage">Here is a message. Astonishing.</div>
```

view modal 层：

```bash
ko.applyBindings({
    displayMessage: ko.observable(false)
});
```

再看一个栗子：

```bash
<ul data-bind="foreach: planets">
    <li>
        Planet: <b data-bind="text: name"> </b>
        <div data-bind="if: capital">
            Capital: <b data-bind="text: capital.cityName"> </b>
        </div>
    </li>
</ul>
 
 
<script>
    ko.applyBindings({
        planets: [
            { name: 'Mercury', capital: null },
            { name: 'Earth', capital: { cityName: 'Barnsley' } }       
        ]
    });
</script>
```

当然 if 也可以这样写：

```bash
<ul>
    <li>This item always appears</li>
    <!-- ko if: someExpressionGoesHere -->
        <li>I want to make this item present/absent dynamically</li>
    <!-- /ko -->
</ul>
```

### ifnot 绑定

ifnot 不打算详细讲解，其实跟 if 一样使用方法没啥好解释的。

### with 绑定

with 绑定说的浅显一点就是，重新绑定作用域。
来个栗子吧。

```bash
<h1 data-bind="text: city"> </h1>
<p data-bind="with: coords">
    Latitude: <span data-bind="text: latitude"> </span>,
    Longitude: <span data-bind="text: longitude"> </span>
</p>
 
<script type="text/javascript">
    ko.applyBindings({
        city: "London",
        coords: {
            latitude:  51.5001524,
            longitude: -0.1262362
        }
    });
</script>
```

使用 with 直接将 p 标签作用域绑定到了 coords 上面，也就是不是最外层的{city:'',coords:''}这一层，而是 coords:{'':''}这一内层。
查看一个复杂一点点的栗子，
view 层，依旧采用 form 表单提交：

```bash
<form data-bind="submit: getTweets">
    Twitter account:
    <input data-bind="value: twitterName" />
    <button type="submit">Get tweets</button>
</form>
 
<div data-bind="with: resultData">
    <h3>Recent tweets fetched at <span data-bind="text: retrievalDate"> </span></h3>
    <ol data-bind="foreach: topTweets">
        <li data-bind="text: text"></li>
    </ol>
 
    <button data-bind="click: $parent.clearResults">Clear tweets</button>
</div>
```

view-model 层：

```bash
function AppViewModel() {
    var self = this;
    self.twitterName = ko.observable('@example');
    self.resultData = ko.observable(); // No initial value
 
    self.getTweets = function() {
        var name = self.twitterName(),
            simulatedResults = [
                { text: name + ' What a nice day.' },
                { text: name + ' Building some cool apps.' },
                { text: name + ' Just saw a famous celebrity eating lard. Yum.' }
            ];
 
        self.resultData({ retrievalDate: new Date(), topTweets: simulatedResults });
    }
 
    self.clearResults = function() {
        self.resultData(undefined);
    }
}
 
ko.applyBindings(new AppViewModel());
```

> note: with 绑定依旧可以使用注释的写法。

### component 绑定

对于 component 绑定，先来个栗子，见识一下，看看初步印象咋样。
view 层代码：

```bash
<h4>First instance, without parameters</h4>
<div data-bind='component: "message-editor"'></div>
 
<h4>Second instance, passing parameters</h4>
<div data-bind='component: {
    name: "message-editor",
    params: { initialText: "Hello, world!" }
}'></div>
```

view model 层代码：

```bash
ko.components.register('message-editor', {
    viewModel: function(params) {
        this.text = ko.observable(params && params.initialText || '');
    },
    template: 'Message: <input data-bind="value: text" /> '
            + '(length: <span data-bind="text: text().length"></span>)'
});
 
ko.applyBindings();
暂时只提一点，name和params这是固定的key值。后续会单独写一篇，详细介绍 component。
```

## 作用于与表单字段

### click 绑定

### event 绑定

### submit 绑定

### enable 绑定

### disable 绑定

### value 绑定

### textInput 绑定

### hasFocus 绑定

### checked 绑定

### options 绑定

嗯，自我感觉这个还有点意思哈，首先呢，这个目的看一眼：options 绑定控制什么样的 options 在 drop-down 列表里（例如：&lt;select&gt;）或者 multi-select 列表里 （例如：&lt;select size='6'&gt;）显示。此绑定不能用于&lt;select&gt;之外的元素。关联的数据应是数组（或者是 observable 数组），&lt;select&gt;会遍历显示数组里的所有的项

> 注：对于 multi-select 列表，设置或者获取选择的多项需要使用 selectedOptions 绑定。对于 single-select 列表，你也可以使用 value 绑定读取或者设置元素的 selected 项。

#### 栗子 1(单纯下拉)：

```bash
<p>Destination country: <select data-bind="options: availableCountries"></select></p>


<script type="text/javascript">
    var viewModel = {
        availableCountries: ko.observableArray(['France', 'Germany', 'Spain']) // These are the initial options
    };

    // ... then later ...
    viewModel.availableCountries.push('China'); // Adds another option
</script>
```

#### 栗子 2（多选）:

```bash
<p>Choose some countries you'd like to visit: <select data-bind="options: availableCountries" size="5" multiple="true"></select></p>

<script type="text/javascript">
    var viewModel = {
        availableCountries: ko.observableArray(['France', 'Germany', 'Spain'])
    };
</script>
```

#### Drop-down list 展示的任意 JavaScript 对象，不仅仅是字符串

```bash
<p>
    Your country:
    <select data-bind="options: availableCountries,
　　　　　　　　　　　　　　optionsText: 'countryName', value: selectedCountry, optionsCaption: 'Choose...'"></select>
</p>

<div data-bind="visible: selectedCountry"> <!-- Appears when you select something -->
    You have chosen a country with population
    <span data-bind="text: selectedCountry() ? selectedCountry().countryPopulation : 'unknown'"></span>.
</div>

<script type="text/javascript">
    // Constructor for an object with two properties
var country =function (name, population) {
        this.countryName = name;
        this.countryPopulation = population;
    };

     var viewModel = {
        availableCountries: ko.observableArray([
            new country("UK", 65000000),
            new country("USA", 320000000),
            new country("Sweden", 29000000)
        ]),
        selectedCountry: ko.observable() // Nothing selected by default
    };
</script>
```

#### Drop-down list 展示的任意 JavaScript 对象，显示 text 是 function 的返回值

```bash
<select data-bind="options: availableCountries,
                   optionsText: function(item) {
                       return item.countryName + ' (pop: ' + item.countryPopulation + ')'
                   },
                   value: selectedCountry,
                   optionsCaption: 'Choose...'"></select>
```

那么看了这么多栗子，那到底配置的参数有什么呢？

- 主参数

- 该参数是一个数组（或者 observable 数组）。对每个 item，KO 都会将它作为一个&lt;option&gt; 添加到&lt;select&gt;里，之前的 options 都将被删除。

- 如果参数是一个 string 数组，那你不需要再声明任何其它参数。&lt;select&gt;元素会将每个 string 显示为一个 option。不过，如果你让用户选择的是一个 JavaScript 对象数组（不仅仅是 string），那就需要设置 optionsText 和 optionsValue 这两个参数了。

- 如果参数是监控属性 observable 的，那元素的 options 项将根据参数值的变化而更新，如果不是，那元素的 value 值将只设置一次并且以后不在更新。

* 其它参数

* optionsCaption

       有时候，默认情况下不想选择任何option项。但是single-select drop-down列表由于每次都要默认选择以项目，怎么避免这个问题呢？常用的方案是加一个“请选择的”或者“Select an item”的提示语，或者其它类似的，然后让这个项作为默认选项。

       我们使用optionsCaption参数就能很容易实现，它的值是字符串型，作为默认项显示。例如：

       &lt;select data-bind='options: myOptions, optionsCaption: "Select an item...", value: myChosenValue'&gt;&lt;/select&gt;

       KO会在所有选项上加上这一个项，并且设置value值为undefined。所以，如果myChosenValue被设置为undefined（默认是observable的），那么上述的第一个项就会被选中。

- optionsText

       上面的例3展示的绑定JavaScript对象到option上 – 不仅仅是字符串。这时候你需要设置这个对象的那个属性作为drop-down列表或multi-select列表的text来显示。例如，例3中使用的是设置额外的参数optionsText将对象的属性名countryName作为显示的文本。

       如果不想仅仅显示对象的属性值作为每个item项的text值，那你可以设置optionsText 为JavaScript 函数，然后再函数里通过自己的逻辑返回相应的值（该函数参数为item项本身）。例4展示的就是返回item的2个属性值合并的结果。

* optionsValue

       和optionsText类似, 你也可以通过额外参数optionsValue来声明对象的那个属性值作为该&lt;option&gt;的value值。

       经典场景：如在更新options的时候想保留原来的已经选择的项。例如，当你重复多次调用Ajax获取car列表的时候，你要确保已经选择的某个car一直都是被选择上，那你就需要设置optionsValue为“carId”或者其它的unique标示符，否则的话KO找不知道之前选择的car是新options里的哪一项。

- selectedOptions

       对于multi-select列表，你可以用selectedOptions读取和设置多个选择项。技术上看它是一个单独的绑定，有自己的文档，请参考： selectedOptions绑定。

> 注：已经被选择的项会再 options 改变的时候保留

当使用 options 绑定&lt;select&gt;元素的时候，如果 options 改变，KO 将尽可能第保留之前已经被选择的项不变（除非是你事先手工删除一个或多个已经选择的项）。这是因为 options 绑定尝试依赖 value 值的绑定（single-select 列表）和 selectedOptions 绑定（multi-select 列表）。

- 依赖性

- 除 KO 核心类库外，无依赖。

### selectedOptions 绑定

### uniqueName 绑定

## 自定义
