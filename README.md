# 大厂前端面试题精选

## JavaScript 部分

### 1. JavaScript 有哪些数据类型，它们的区别？
JavaScript 共有八种数据类型，分别是 Number、String、Boolean、Undefined、Null、Object、Symbol、BigInt。

其中 Symbol 和 BigInt 是 ES6中新增的数据类型：
- Symbol 代表创建后独一无二且不可变的数据类型，它主要是为了解决可能出现的全局变量冲突的问题。
- BigInt 是一种数字类型的数据，它可以表示任意精度格式的整数，使用 BigInt 可以安全地存储和操作大整数，即使这个数已经超出了Number 能够表示的安全整数范围。

这些数据可以分为原始数据类型和引用数据类型：
- 栈：原始数据类型（Number、String、Boolean、Undefined、Null）
- 堆：引用数据类型（对象、数组和函数）

两种类型的区别在于存储位置的不同：
- 原始数据类型直接存储在栈（stack）中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；
- 引用数据类型存储在堆（heap）中的对象，占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。

堆和栈的概念存在于数据结构和操作系统内存中，在数据结构中：
- 在数据结构中，栈中数据的存取方式为先进后出。
- 堆是一个优先队列，是按优先级来进行排序的，优先级可以按照大小来规定。

在操作系统中，内存被分为栈区和堆区：
- 栈区内存由编译器自动分配释放，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。
- 堆区内存一般由开发者分配释放，若开发者不释放，程序结束时可能由垃圾回收机制回收。

### 2. 数据类型检测的方式有哪些？
1. typeof
    ```
    typeof 2;                   // number
    typeof true;                // boolean
    typeof 'str';               // string
    typeof [];                  // object
    typeof function (){};       // function
    typeof undefined;           // undefined
    typeof null;                // object
    ```
    其中数组、对象、null 都会被判断为 object，其他判断都正确。

2. instanceof
    instanceof 可以正确判断对象的类型，其内部运行机制是判断在其原型链中能否找到该类型的原型。
    ```
    2 instanceof Number;                        // false
    true instanceof Boolean;                    // false
    'str' instanceof String;                    // false

    [] instanceof Array;                        // true
    function (){} instanceof Function;          // true
    {} instanceof Object;                       // true
    ```
    可以看到，instanceof 只能正确判断引用数据类型，而不能判断基本数据类型。instanceof 运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

3. constructor
    ```
    (2).constructor === Number;                     // true
    (true).constructor === Boolean;                 // true
    ('str').constructor === String;                 // true
    ([]).constructor === Array;                     // true
    (function (){}).constructor === Function;       // true
    ({}).constructor === Object;                    // true
    ```
    constructor 有两个作用，一是判断数据的类型，二是对象实例通过constrcutor 对象访问它的构造函数。
    需要注意，如果创建一个对象来改变它的原型，constructor 就不能用来判断数据类型了：
    ```
    function Fn(){}
    Fn.prototype = new Array();
    var f = new Fn();
    console.log(f.constructor === Fn);          // false
    console.log(f.constructor === Array);       // true
    ```
    
4. Object.prototype.toString.call()
    Object.prototype.toString.call() 使用 Object 对象的原型方法toString 来判断数据类型：
    ```
    var a = Object.prototype.toString;
    console.log(a.call(2));                     // [object Number]
    console.log(a.call('str'));                 // [object String]
    console.log(a.call(true));                  // [object Boolean]
    console.log(a.call([]));                    // [object Array]
    console.log(a.call(function (){}));         // [object Function]
    console.log(a.call({}));                    // [object Object]
    console.log(a.call(undefined));             // [object Undefined]
    console.log(a.call(null));                  // [object Null]
    ```
    同样是检测对象 obj 调用 toString 方法，obj.toString()的结果和Object.prototype.toString.call(obj)的结果不一样，这是为什么？
    这是因为 toString 是 Object 的原型方法，而 Array、function 等类型作为 Object 的实例，都重写了 toString 方法。不同的对象类型调用 toString 方法时，根据原型链的知识，调用的是对应的重写之后的 toString 方法（function 类型返回内容为函数体的字符串，Array类型返回元素组成的字符串…），而不会去调用 Object 上原型toString 方法（返回对象的具体类型），所以采用 obj.toString()不能得到其对象类型，只能将 obj 转换为字符串类型；因此，在想要得到对象的具体类型时，应该调用 Object 原型上的 toString 方法。

### 3. null 和 undefined 区别
首先 Undefined 和 Null 都是基本数据类型，这两个基本数据类型分别都只有一个值，就是 undefined 和 null。

undefined 代表的含义是未定义，null 代表的含义是空对象。一般变量声明了但还没有定义的时候会返回 undefined，null 主要用于赋值给一些可能会返回对象的变量，作为初始化。

undefined 在 JavaScript 中不是一个保留字，这意味着可以使用undefined 来作为一个变量名，但是这样的做法是非常危险的，它会影响对 undefined 值的判断。我们可以通过一些方法获得安全的undefined 值，比如说 void 0。
    
当对这两种类型使用 typeof 进行判断时，Null 类型化会返回“object”，这是一个历史遗留的问题。当使用双等号对两种类型的值进行比较时会返回 true，使用三个等号时会返回 false。

### 4. 如何获取安全的 undefined 值？
因为 undefined 是一个标识符，所以可以被当作变量来使用和赋值， 但是这样会影响 undefined 的正常判断。表达式 void ___ 没有返回值，因此返回结果是 undefined。void 并不改变表达式的结果， 只是让表达式不返回值。因此可以用 void 0 来获得 undefined。

### 5. intanceof 操作符的实现原理及实现
instanceof 运算符用于判断构造函数的 prototype 属性是否出现在对象的原型链中的任何位置。
```
function myInstanceof(left, right) {
    // 获取对象的原型
    let proto = Object.getPrototypeOf(left);
    // 获取构造函数的 prototype 对象
    let prototype = right.prototype;

    // 判断构造函数的 prototype 对象是否在对象的原型链上
    while (true) {
        if (!proto) return false;
        if (proto === prototype) return true;
        // 如果没有找到，就继续从其原型上找，Object.getPrototypeOf 方法用来获取指定对象的原型
        proto = Object.getPrototypeOf(proto);
    }
}
```

### 6. Object.is() 与比较操作符 “===”、“==” 的区别？
使用双等号（==）进行相等判断时，如果两边的类型不一致，则会进行强制类型转化后再进行比较。

使用三等号（===）进行相等判断时，如果两边的类型不一致时，不会做强制类型准换，直接返回 false。

使用 Object.is 来进行相等判断时，一般情况下和三等号的判断相同，它处理了一些特殊的情况，比如 -0 和 +0 不再相等，两个 NaN 是相等的。

### 7. 什么是 JavaScript 中的包装类型？
在 JavaScript 中，基本类型是没有属性和方法的，但是为了便于操作基本类型的值，在调用基本类型的属性或方法时 JavaScript 会在后台隐式地将基本类型的值转换为对象，如：
```
const a = 'abc';
a.length;           // 3
a.toUpperCase();    // 'ABC'
```
在 访 问 'abc'.length 时 ， JavaScript 将 'abc' 在后台转换成String('abc')，然后再访问其 length 属性。

JavaScript 也可以使用 Object 函数显式地将基本类型转换为包装类型：
```
var a = 'abc';
Object(a);  // String {"abc"}
```
也可以使用 valueOf 方法将包装类型倒转成基本类型：
```
var a = 'abc';
var b = Object(a);
var c = b.valueOf();    // 'abc'
```
看看如下代码会打印出什么：
```
var a = new Boolean(false);
if (!a) {
    console.log("Oops");    // never runs
}
```
答案是什么都不会打印，因为虽然包裹的基本类型是 false，但是 false 被包裹成包装类型后就成了对象，所以其非值为 false，所以循环体中的内容不会运行。

### 8. const 对象的属性可以修改吗?
const 保证的并不是变量的值不能改动，而是变量指向的那个内存地址不能改动。对于基本类型的数据（数值、字符串、布尔值），其值就保存在变量指向的那个内存地址，因此等同于常量。 

但对于引用类型的数据（主要是对象和数组）来说，变量指向数据的内存地址，保存的只是一个指针，const 只能保证这个指针是固定不变的，至于它指向的数据结构是不是可变的，就完全不能控制了。


