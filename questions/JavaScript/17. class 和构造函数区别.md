# *class* 和构造函数区别



## 经典真题



- 根据下面 *ES6* 构造函数的书写方式，要求写出 *ES5* 的

```js
class Example { 
  constructor(name) { 
    this.name = name;
  }
  init() { 
    const fun = () => { console.log(this.name) }
    fun(); 
  } 
}
const e = new Example('Hello');
e.init();
```



## 回顾 *class* 的写法



上面的这道面试题，典型的就是考察 *ES6* 中新增的 *class* 和以前构造函数上面的区别是什么，以及如果通过 *ES5* 去模拟的话，具体如何实现。



那么在此之前，我们就先来回顾一下 *ES6* 中的 *class* 写法。



代码如下：

```js
class Computer {
    // 构造器
    constructor(name, price) {
        this.name = name;
        this.price = price;
    }
    // 原型方法
    showSth() {
        console.log(`这是一台${this.name}电脑`);
    }
    // 静态方法
    static comStruct() {
        console.log("电脑由显示器，主机，键鼠组成");
    }
}
```



上面的代码非常的简单，我们定义了一个名为 Computer 的类，该类存在 *name、price* 这两个实例属性，一个 *showSth* 的原型方法以及一个 *comStruct* 的静态方法。



我们可以简单的实例化一个对象出来，例如：

```js
var apple = new Computer("苹果", 15000);
console.log(apple.name); // 苹果
console.log(apple.price); // 15000
apple.showSth(); // 这是一台苹果电脑
Computer.comStruct(); // 电脑由显示器，主机，键鼠组成
```

在上面的代码中，我们从 *Computer* 类中实例化出来了一个 *apple* 的实例对象，然后简单访问了该对象的属性和方法。



## 回顾构造函数的写法



那么，在 *ES6* 出现之前，我们是如何实现类似于其他语言中的“类”的呢？

没错，我们是通过的构造函数，然后将方法挂在原型上面。例如：

```js
function Computer(name, price){
    this.name = name;
    this.price = price;
}
Computer.prototype.showSth = function(){
    console.log(`这是一台${this.name}电脑`);
}
Computer.comStruct = function(){
    console.log("电脑由显示器，主机，键鼠组成");
}

var apple = new Computer("苹果", 15000);
console.log(apple.name); // 苹果
console.log(apple.price); // 15000
apple.showSth(); // 这是一台苹果电脑
Computer.comStruct(); // 电脑由显示器，主机，键鼠组成
```

上面的代码就是我们经常在 *ES5* 中所书写的代码，通过构造函数来模拟类，实例方法挂在原型上面，静态方法就挂在构造函数上。

仿佛 *ES6* 的 *class* 写法就是上面构造函数写法的一种语法糖，但是事实真的如此么？



## *class* 和构造函数区别上的细则



接下来我们来详细比较一下两种写法在细节上面的一些差异。

首先我们书写两个“类”，一个用 *ES5* 的构造函数书写，一个用 *ES6* 的类的写法来书写，如下：

```js
class Computer1 {
    // 构造器
    constructor(name, price) {
        this.name = name;
        this.price = price;
    }
    // 原型方法
    showSth() {
        console.log(`这是一台${this.name}电脑`);
    }
    // 静态方法
    static comStruct() {
        console.log("电脑由显示器，主机，键鼠组成");
    }
}

function Computer2(name, price){
    this.name = name;
    this.price = price;
}
Computer2.prototype.showSth = function(){
    console.log(`这是一台${this.name}电脑`);
}
Computer2.comStruct = function(){
    console.log("电脑由显示器，主机，键鼠组成");
}
```



我们知道，构造函数也是函数，既然是函数，那么就可以通过函数调用的形式来调用该函数，例如：

```js
var i = Computer2();
console.log(i); // undefined
```

运行上面的代码，代码不会报错，因为没有使用 *new* 的方式来调用，所以不会生成一个对象，返回值就为 *undefined*。



但是如果我们这样来调用 *ES6* 书写的类，会直接报错：

```js
Computer1();
// TypeError: Class constructor Computer1 cannot be invoked without 'new'
```

可以看到，*ES6* 所书写的 *class* ，虽然我们认为背后就是构造函数实现的，但是明显是做了特殊处理的，必须通过 *new* 关键字来调用。



接下来，我们来针对两种写法，各自实例化一个对象，代码如下：

```js
var apple = new Computer2("苹果", 15000);
for(var i in apple){
    console.log(i); 
}
console.log('-------');
var huawei = new Computer1("华为", 12000);
for(var i in huawei){
    console.log(i); 
}
```

在上面的代码中， *apple* 对象是 *ES5* 构造函数的形式创建的实例，*huawei* 是 *ES6* 类的形式创建的实例。有了这两个对象后，我们遍历这两个对象的键，结果如下：

```js
name
price
showSth
-------
name
price
```

可以看到，*ES6* 中的原型方法是不可被枚举的，说明 *ES6* 对此也是做了特殊处理的。



另外，*ES6* 的 *class* 中的所有代码均处于严格模式之下，这里我们也可以进行一个简单的验证。例如，对两种方式的 *showSth* 原型方法稍作修改，如下：

```js
class Computer1 {
    ...
    // 原型方法
    showSth(i,i) {
        console.log(`这是一台${this.name}电脑`);
    }
   	...
}
function Computer2(name, price){
   ...
}
Computer2.prototype.showSth = function(j,j){
    i = 10;
    console.log(`这是一台${this.name}电脑`);
}
...
```

在上面的代码中，我们为各自的 *showSth* 方法添加了重复的形式参数。我们知道，在严格模式中方法书写重复形参是不被允许的。

所以在运行代码时，*ES6* 的 *class* 声明方式会报错，错误信息如下：

```js
// SyntaxError: Duplicate parameter name not allowed in this context
```



还有就是，如果是 *ES6* 形式所声明的类，原型上的方法是不允许通过 *new* 来调用的。

这里我们也可以做一个简单的测试，如下：

```js
function Computer2(name, price){
    this.name = name;
    this.price = price;
}
Computer2.prototype.showSth = function(){
    i = 10;
    console.log(`这是一台${this.name}电脑`);
}
Computer2.comStruct = function(){
    console.log("电脑由显示器，主机，键鼠组成");
}

var apple = new Computer2("苹果", 15000);
var i = new apple.showSth(); // 这是一台undefined电脑
console.log(i); // {}
```

在上面的代码中，我们首先实例化了一个 *apple* 对象，在该对象的原型上面拥有一个 *showSth* 的实例方法，然后我们对其进行了 *new* 操作，可以看到返回了一个对象。



但是如果是 *ES6* 形式所声明的类，上面的做法将不被允许。示例如下：

```js
class Computer1 {
    // 构造器
    constructor(name, price) {
        this.name = name;
        this.price = price;
    }
    // 原型方法
    showSth() {
        console.log(`这是一台${this.name}电脑`);
    }
    // 静态方法
    static comStruct() {
        console.log("电脑由显示器，主机，键鼠组成");
    }
}
var huawei = new Computer1("华为", 12000);
var i = new huawei.showSth(); // TypeError: huawei.showSth is not a constructor
console.log(i);
```

在上面的代码中，我们企图对 *Computer1* 实例对象 *huawei* 的原型方法 *showSth* 进行 *new* 操作，可以看到，这里报出了 *TypeError*。



## *Babel* 中具体的实现



通过上面的各种例子，我们可以知道 *ES6* 中的 *class* 实现并不是我们单纯所想象的就是之前 *ES5* 写构造函数的写法，虽然本质上是构造函数，但是内部是做了各种处理的。



这里，我们就来使用 *Babel* 对下面的代码进行转义，转义之前的代码如下：

```js
class Computer {
    // 构造器
    constructor(name, price) {
        this.name = name;
        this.price = price;
    }
    // 原型方法
    showSth() {
        console.log(`这是一台${this.name}电脑`);
    }
    // 静态方法
    static comStruct() {
        console.log("电脑由显示器，主机，键鼠组成");
    }
}
```

转义后的代码如下：

```js
"use strict";
function _classCallCheck(instance, Constructor) {
    if (!(instance instanceof Constructor)) {
        throw new TypeError("Cannot call a class as a function");
    }
}

function _defineProperties(target, props) {
    for (var i = 0; i < props.length; i++) {
        var descriptor = props[i];
        descriptor.enumerable = descriptor.enumerable || false;
        descriptor.configurable = true;
        if ("value" in descriptor)
            descriptor.writable = true;
        Object.defineProperty(target, descriptor.key, descriptor);
    }
}

function _createClass(Constructor, protoProps, staticProps) {
    if (protoProps)
        _defineProperties(Constructor.prototype, protoProps);
    if (staticProps)
        _defineProperties(Constructor, staticProps);
    return Constructor;
}

var Computer = /*#__PURE__*/function () {
    // 构造器
    function Computer(name, price) {
        _classCallCheck(this, Computer);

        this.name = name;
        this.price = price;
    } // 原型方法


    _createClass(Computer, [{
        key: "showSth",
        value: function showSth() {
            console.log("\u8FD9\u662F\u4E00\u53F0".concat(this.name, "\u7535\u8111"));
        } // 静态方法

    }], [{
        key: "comStruct",
        value: function comStruct() {
            console.log("电脑由显示器，主机，键鼠组成");
        }
    }]);

    return Computer;
}();
var apple = new Computer("苹果", 15000);
console.log(apple.name); // 苹果
console.log(apple.price); // 15000
apple.showSth(); // 这是一台苹果电脑
Computer.comStruct(); // 电脑由显示器，主机，键鼠组成
```

可以看到，果然没有我们想象的那么简单，接下来我们就来一点一点剖析转义的结果。

首先整体来讲分为下面几块：

```js
"use strict";
function _classCallCheck(instance, Constructor) { ... }

function _defineProperties(target, props) { ... }

function _createClass(Constructor, protoProps, staticProps) { ... }

var Computer = /*#__PURE__*/function () { ... }();
```

我们一块一块的来看。

```js
function _classCallCheck(instance, Constructor) {
    if (!(instance instanceof Constructor)) {
        throw new TypeError("Cannot call a class as a function");
    }
}
```

第一个方法叫做 *classCallCheck*，从名字上面我们也可以看出，这个方法是核对构造方法的调用形式的，接收两个参数，一个是实例对象，另一个是构造函数，通过 *instanceof* 来看参数 *instance* 是否是 *Constructor* 的实例，如果不是就抛出错误。



接下来是 *_defineProperties* 方法，我们对此方法稍作了修改，打印 *target* 和 *props* 的值

```js
function _defineProperties(target, props) {
    console.log("target:::",target);
    console.log("props:::",props);
    for (var i = 0; i < props.length; i++) {
        var descriptor = props[i];
        descriptor.enumerable = descriptor.enumerable || false;
        descriptor.configurable = true;
        if ("value" in descriptor)
            descriptor.writable = true;
        Object.defineProperty(target, descriptor.key, descriptor);
    }
}
```

结果如下：

```js
target::: {}
props::: [ { key: 'showSth', value: [Function: showSth] } ]
target::: [Function: Computer]
props::: [ { key: 'comStruct', value: [Function: comStruct] } ]
```

可以看出，该方法就是设置对象方法的属性描述符，包含是否可遍历呀，是否可写呀等信息，设置完成后将方法挂在 *target* 对象上面。



下一个是 *_createClass* 函数，我们仍然将三个参数打印出来

```js
function _createClass(Constructor, protoProps, staticProps) {
    console.log("Constructor::",Constructor);
    console.log("protoProps::",protoProps);
    console.log("staticProps::",staticProps);
    if (protoProps)
        _defineProperties(Constructor.prototype, protoProps);
    if (staticProps)
        _defineProperties(Constructor, staticProps);
    return Constructor;
}
```

结果如下：

```js
Constructor:: [Function: Computer]
protoProps:: [ { key: 'showSth', value: [Function: showSth] } ]
staticProps:: [ { key: 'comStruct', value: [Function: comStruct] } ]
```

可以看出，接收的三个参数一次为构造函数、原型上的方法，静态方法。接下来在该方法里面所做的事情也就非常清晰了。



最后就是我们的构造函数了：

```js
var Computer = /*#__PURE__*/function () {
    // 构造器
    function Computer(name, price) {
        // 进行调用确认
        _classCallCheck(this, Computer);
				// 添加实例属性
        this.name = name;
        this.price = price;
    } // 原型方法

		// 将实例方法和静态方法添加到构造函数上面
    _createClass(Computer, [{
        key: "showSth",
        value: function showSth() {
            console.log("\u8FD9\u662F\u4E00\u53F0".concat(this.name, "\u7535\u8111"));
        } // 静态方法

    }], [{
        key: "comStruct",
        value: function comStruct() {
            console.log("电脑由显示器，主机，键鼠组成");
        }
    }]);

    return Computer;
}();
```

明白了 *_createClass* 方法的作用后，该方法的代码也就非常的清晰了。



## 真题解答



- 根据下面 *ES6* 构造函数的书写方式，要求写出 *ES5* 的

```js
class Example { 
  constructor(name) { 
    this.name = name;
  }
  init() { 
    const fun = () => { console.log(this.name) }
    fun(); 
  } 
}
const e = new Example('Hello');
e.init();
```

> 参考答案：
>
> ```js
> "use strict";
> 
> function _classCallCheck(instance, Constructor) { 
>     if (!(instance instanceof Constructor)) { 
>       throw new TypeError("Cannot call a class as a function"); 
>     } 
> }
> 
> function _defineProperties(target, props) { 
>     for (var i = 0; i < props.length; i++) { 
>       var descriptor = props[i]; 
>       descriptor.enumerable = descriptor.enumerable || false; 
>       descriptor.configurable = true; 
>       if ("value" in descriptor) 
>         descriptor.writable = true; 
>       Object.defineProperty(target, descriptor.key, descriptor); 
>     } 
> }
> 
> function _createClass(Constructor, protoProps, staticProps) { 
>     if (protoProps) 
>       _defineProperties(Constructor.prototype, protoProps); 
>     if (staticProps) 
>       _defineProperties(Constructor, staticProps); 
>     return Constructor; 
> }
> 
> var Example = /*#__PURE__*/function () {
>   function Example(name) {
>     _classCallCheck(this, Example);
> 
>     this.name = name;
>   }
> 
>   _createClass(Example, [{
>     key: "init",
>     value: function init() {
>       var _this = this;
> 
>       var fun = function fun() {
>         console.log(_this.name);
>       };
> 
>       fun();
>     }
>   }]);
> 
>   return Example;
> }();
> 
> var e = new Example('Hello');
> e.init();
> ```
>
> 这里可以解释出 *_classCallCheck、_defineProperties、_createClass* 这几个方法各自的作用是什么。



-*EOF*-

