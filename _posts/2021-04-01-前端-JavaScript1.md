# JavaScript

## 简介

在1995 年 Netscape 一位名为 Brendan Eich 的工程师创造了 JavaScript，随后在 1996 年初，JavaScript 首先被应用于 Netscape 2 浏览器上。最初的 JavaScript 名为 LiveScript，但是因为一个糟糕的营销策略而被重新命名，该策略企图利用Sun Microsystem的Java语言的流行性，将它的名字从最初的 LiveScript 更改为 JavaScript——尽管两者之间并没有什么共同点。这便是之后混淆产生的根源。

与大多数编程语言不同，JavaScript 没有输入或输出的概念。它是一个在宿主环境（host environment）下运行的脚本语言，任何与外界沟通的机制都是由宿主环境提供的。浏览器是最常见的宿主环境，但在非常多的其他程序中也包含 JavaScript 解释器，如 Adobe Acrobat、Adobe Photoshop、SVG 图像、Yahoo! 的 Widget 引擎，[Node.js](http://nodejs.org/) 之类的服务器端环境，NoSQL 数据库（如开源的 [Apache CouchDB](http://couchdb.apache.org/)）、嵌入式计算机，以及包括 [GNOME](http://www.gnome.org/) （注：GNU/Linux 上最流行的 GUI 之一）在内的桌面环境等等。



## 概览

JavaScript 是一种多范式的动态语言，它包含类型、运算符、标准内置（ built-in）对象和方法。它的语法来源于 Java 和 C，所以这两种语言的许多语法特性同样适用于 JavaScript。JavaScript 通过原型链而不是类来支持面向对象编程（有关 ES6 类的内容参考这里[`Classes`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)，有关对象原型参考见此[继承与原型链](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)）。JavaScript同样支持函数式编程——因为它们也是对象，函数也可以被保存在变量中，并且像其他对象一样被传递。

#### JavaScript的类型

* Number（数字）
* String（字符串）
* Boolean（布尔）
* Symbol（符号）（ES2015新增）
* Object（对象）
  * Function（函数）
  * Array（数组）
  * Date（日期）
  * RegExp（正则表达式）
* null（空）
* undefined（未定义）

##### 整数

内置函数<u>parseInt()</u>将字符串转换为整型。函数的第二个参数表示字符串表示数字的基（进制）：

```javascript
parseInt("123", 10);	// 123
parseInt("010", 10);	// 10
parseInt("010");  //  8
parseInt("0x10"); // 16
parseInt("11", 2); // 3
parseInt("hello", 10); // NaN

```

一元运算符 + 也可以把数字字符串转换成数值：

```javascript
+ "42";   // 42
+ "010";  // 10
+ "0x10"; // 16
NaN + 5; //NaN
isNaN(NaN); // true
```

##### 字符串

JavaScript 中的字符串是一串[Unicode 字符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Values,_variables,_and_literals#Unicode.E7.BC.96.E7.A0.81)序列。这对于那些需要和多语种网页打交道的开发者来说是个好消息。更准确地说，它们是一串UTF-16编码单元的序列，每一个编码单元由一个 16 位二进制数表示。每一个Unicode字符由一个或两个编码单元来表示。

```javascript
"hello".length; // 5
"hello".charAt(0); // "h"
"hello, world".replace("world", "mars"); // "hello, mars"
"hello".toUpperCase(); // "HELLO"
```

##### 其他类型

JavaScript中的**null**表示一个空值（non-value），必须使用null关键字才能访问，**undefined**是一个<u>”undefined（未定义）“</u>类型的对象，表示一个未初始化的值，也就是没有被分配的值。

> undefined实际上是一个不允许修改的常量。

JavaScript的布尔类型，这个类型有两个值**true**和**false**（两个关键字）。

JavaScript转换为布尔类型的规则：

* false、0、空字符串（”“）、NaN、null和undefined被转换为**false**
* 其他所有值被转换为**true**

##### 变量

**let** 语句声明一个块级作用域的本地变量，并且可选的将其初始化为一个值。

```javascript
let a;
let name = 'Simon';

// myLetVariable 在这里 不能 被引用
for (let myLetVariable = 0; myLetVariable < 5; myLetVariable++) {
  // myLetVariable 只能在这里引用
}
// myLetVariable 在这里 *不能* 被引用

```

**const** 允许声明一个不可变的常量。这个常量在定义域内总是可见的。

```javascript
const Pi = 3.14; // 设置 Pi 的值
Pi = 1; // 将会抛出一个错误因为你改变了一个常量的值。
```

**var** 是最常见的声明变量的关键字。它没有其他两个关键字的种种限制。这是因为它是传统上在 JavaScript 声明变量的唯一方法。使用 **`var`** 声明的变量在它所声明的整个函数都是可见的。

```javascript
var a;
var name = "simon";
// myVarVariable在这里 *能* 被引用
for (var myVarVariable = 0; myVarVariable < 5; myVarVariable++) {
  // myVarVariable 整个函数中都能被引用
}
// myVarVariable 在这里 *能* 被引用
```

如果声明了一个变量却没有对其赋值，那么这个变量的类型就是 `undefined`。

JavaScript 与其他语言的（如 Java）的重要区别是在 JavaScript 中语句块（blocks）是没有作用域的，只有函数有作用域。因此如果在一个复合语句中（如 if 控制结构中）使用 var 声明一个变量，那么它的作用域是整个函数（复合语句在函数中）。 但是从 ECMAScript Edition 6 开始将有所不同的， `let` 和 `const` 关键字允许你创建块作用域的变量。

##### 运算符

如果你用一个字符串加上一个数字（或其他值），那么操作数都会被首先转换为字符串。如下所示：

```javascript
"3" + 4 + 5; // 345
3 + 4 + "5"; // 75
```

这里不难看出一个实用的技巧——通过与空字符串相加，可以将某个变量快速转换成字符串类型。

JavaScript 中的**比较操作**使用 `<`、`>`、`<=` 和 `>=`，这些运算符对于数字和字符串都通用。相等的比较稍微复杂一些。由两个“`=`（等号）”组成的相等运算符有类型自适应的功能，具体例子如下：

```javascript
123 == "123" // true
1 == true; // true
```

如果在比较前不需要自动类型转换，应该使用由三个“`=`（等号）”组成的相等运算符：

```javascript
1 === true; //false
123 === "123"; // false
```

JavaScript 还支持 `!=` 和 `!==` 两种不等运算符，具体区别与两种相等运算符的区别类似。

##### 对象 [参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/A_re-introduction_to_JavaScript)

...



##### 数组 [参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/A_re-introduction_to_JavaScript)

JavaScript 中的数组是一种特殊的对象。它的工作原理与普通对象类似（以数字为属性名，但只能通过`[]` 来访问），但数组还有一个特殊的属性——`length`（长度）属性。这个属性的值通常比数组最大索引大 1。

> 记住：数组的长度是比数组最大索引值多一的数。



##### 函数

`return` 语句在返回一个值并结束函数。如果没有使用 `return` 语句，或者一个没有值的 `return` 语句，JavaScript 会返回 `undefined`。

如果调用函数时没有提供足够的参数，缺少的参数会被 `undefined` 替代。

> 函数体中一个名为 [`arguments`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions_and_function_scope/arguments) 的内部对象，这个对象就如同一个类似于数组的对象一样，包括了所有被传入的参数。

```javascript
function avg() {
    var sum = 0;
    for (var i = 0, j = arguments.length; i < j; i++) {
        sum += arguments[i];
    }
    return sum / arguments.length;
}
avg(2, 3, 4, 5); // 3.5
```

可以使用[剩余参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Rest_parameters)来替换arguments的使用。这个**剩余参数操作符**在函数中以：**...variable** 的形式被使用，它将包含在调用函数时使用的未捕获整个参数列表到这个变量中。

```javascript
function avg(...args) {
  var sum = 0;
  for (let value of args) {
    sum += value;
  }
  return sum / args.length;
}

avg(2, 3, 4, 5); // 3.5
```



需要注意的是 JavaScript 函数是它们本身的对象——就和 JavaScript 其他一切一样——你可以给它们添加属性或者更改它们的属性，这与前面的对象部分一样。



##### 自定义对象

```javascript
function makePerson(first, last) {
    return {
        first: first,
        last: last,
        fullName: function() {
            return this.first + ' ' + this.last;
        },
        fullNameReversed: function() {
            return this.last + ', ' + this.first;
        }
    }
}
s = makePerson("Simon", "Willison");
s.fullName(); // "Simon Willison"
s.fullNameReversed(); // Willison, Simon
```

上面的代码里有一些我们之前没有见过的东西：关键字 [`this`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)。当使用在函数中时，`this` 指代当前的对象，也就是调用了函数的对象。如果在一个对象上使用[点或者方括号](https://developer.mozilla.org/en/JavaScript/Reference/Operators/Member_Operators)来访问属性或方法，这个对象就成了 `this`。如果并没有使用“点”运算符调用某个对象，那么 `this` 将指向全局对象（global object）。这是一个经常出错的地方。

**改进1**

```javascript
function Person(first, last) {
    this.first = first;
    this.last = last;
    this.fullName = function() {
        return this.first + ' ' + this.last;
    }
    this.fullNameReversed = function() {
        return this.last + ', ' + this.first;
    }
}
var s = new Person("Simon", "Willison");
```

引入了另外一个关键字：[`new`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)，它和 `this` 密切相关。它的作用是创建一个崭新的空对象，然后使用指向那个对象的 `this` 调用特定的函数。注意，含有 `this` 的特定函数不会返回任何值，只会修改 `this` 对象本身。`new` 关键字将生成的 `this` 对象返回给调用方，而被 `new` 调用的函数称为构造函数。习惯的做法是将这些函数的首字母大写，这样用 `new` 调用他们的时候就容易识别了。

**改进2**

```javascript
function personFullName() {
    return this.first + ' ' + this.last;
}
function personFullNameReversed() {
    return this.last + ', ' + this.first;
}
function Person(first, last) {
    this.first = first;
    this.last = last;
    this.fullName = personFullName;
    this.fullNameReversed = personFullNameReversed;
}
```

**改进3**

```javascript
function Person(first, last) {
    this.first = first;
    this.last = last;
}
Person.prototype.fullName = function() {
    return this.first + ' ' + this.last;
}
Person.prototype.fullNameReversed = function() {
    return this.last + ', ' + this.first;
}
```

`Person.prototype` 是一个可以被`Person`的所有实例共享的对象。它是一个名叫原型链（prototype chain）的查询链的一部分：当你试图访问 `Person `某个实例（例如上个例子中的s）一个没有定义的属性时，解释器会首先检查这个 `Person.prototype` 来判断是否存在这样一个属性。所以，任何分配给 `Person.prototype` 的东西对通过 `this` 对象构造的实例都是可用的。



这个特性功能十分强大，JavaScript 允许你在程序中的任何时候修改原型（prototype）中的一些东西，也就是说你可以在运行时(runtime)给已存在的对象添加额外的方法（类似于ruby的动态添加方法）：

```javascript
s = new Person("Simon", "Willison");
s.firstNameCaps();  // TypeError on line 1: s.firstNameCaps is not a function

Person.prototype.firstNameCaps = function() {
    return this.first.toUpperCase()
}
s.firstNameCaps(); // SIMON
```



##### 对象的apply和call

##### 内部函数

JavaScript 允许在一个函数内部定义函数，也就是 JavaScript 中的嵌套函数，一个很重要的细节是，它们可以访问父函数作用域中的变量：

```javascript
function parentFunc() {
  var a = 1;

  function nestedFunc() {
    var b = 4; // parentFunc 无法访问 b
    return a + b;
  }
  return nestedFunc(); // 5
}
```

##### 闭包

闭包是 JavaScript 中最强大的抽象概念之一。

```javascript
function makeAdder(a) {
  return function(b) {
    return a + b;
  }
}
var add5 = makeAdder(5);
var add20 = makeAdder(20);
add5(6); // 11
add20(7); // 27
```

每当 JavaScript 执行一个函数时，都会创建一个作用域对象（scope object），用来保存在这个函数中创建的局部变量。所以，当调用`makeAdder(5)`的时候，解释器创建了一个作用域对象记为`adderScope`，它带有一个属性`a`，这个属性被当做参数传入`makeAdder`函数。然后`makeAdder`返回一个新创建的函数记为`newAdder`。通常JavaScript的垃圾回收器会在这时回收`adderScope`，但是`makeAdder`的返回值`newAdder`拥有一个指向作用域`adderScope`的引用。最终，作用域`adderScope`不会被垃圾回收器回收，直到没有任何引用指向新函数`newAdder`。

作用域对象组成了一个名为作用域链（scope chain）的（调用）链。它和 JavaScript 的对象系统使用的原型（prototype）链相类似。

一个**闭包**，就是 一个函数 与其 被创建时所带有的作用域对象 的组合。闭包允许你保存状态——所以，它们可以用来代替对象。[这个 StackOverflow 帖子里](http://stackoverflow.com/questions/111102/how-do-javascript-closures-work)有一些关于闭包的详细介绍。

























## 在Rails中使用JavaScript



### Web浏览器常规工作原理

在浏览器输入地址 http://localhost:3000 后，浏览器（客户端）会向服务器发起一个请求。然后浏览器处理响应，获取相关的静态资源文件，比如JavaScript、样式表和图像，然后显示页面内容。点击链接后发生的事情也是如此：获取页面，获取静态资源，把全部内容放在一起，显示最终网页。这个过程叫做**”请求响应循环“**。

### 非侵入式JavaScript

Rails使用一种叫做**”非侵入式JavaScript“（Unobtrusive JavaScript）**的技术把JavaScript依附到DOM上。

**”行间JavaScript“**实例：

```html
<a href="#" onclick="this.style.backgroundColor='#990000'">Paint it red</a>
```

**”非侵入式JavaScript“**实例：

```html
<a href="#" data-background-color="#990000">Paint it red</a>
<a href="#" data-background-color="#009900" data-text-color="#FFFFFF">Paint it green</a>
<a href="#" data-background-color="#000099" data-text-color="#FFFFFF">Paint it blue</a>
```

```coffeescript
@paintIt = (element, backgroundColor, textColor) ->
  element.style.backgroundColor = backgroundColor
  if textColor?
    element.style.color = textColor
 
$ ->
  $("a[data-background-color]").click (e) ->
    e.preventDefault()
 
    backgroundColor = $(this).data("background-color")
    textColor = $(this).data("text-color")
    paintIt(this, backgroundColor, textColor)
```

这种方法称为**”非侵入式JavaScript“**，因为JavaScript代码不再和HTML混合在一起。这样做正确分离了关注点，易于修改功能。我们可以轻易把这种效果添加到其他链接上，只要添加相应的data属性即可。