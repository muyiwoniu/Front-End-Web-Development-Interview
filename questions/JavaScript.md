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

堆和栈的概念存在于数据结构和操作系统内存中，
在数据结构中：
- 在数据结构中，栈中数据的存取方式为先进后出。
- 堆是一个优先队列，是按优先级来进行排序的，优先级可以按照大小来规定。

在操作系统中，内存被分为栈区和堆区：
- 栈区内存由编译器自动分配释放，存放函数的参数值、局部变量的值等。其操作方式类似于数据结构中的栈。
- 堆区内存一般由开发者分配释放，若开发者不释放，程序结束时可能由垃圾回收机制回收。

### 2. 数据类型检测的方式有哪些？
1. typeof
    ```
    typeof 2;                   // number
    typeof 'str';               // string
    typeof true;                // boolean
    typeof undefined;           // undefined
    typeof null;                // object
    typeof [];                  // object
    typeof function (){};       // function
    typeof {};                  // object
    ```
    其中数组、对象、null 都会被判断为 object，其他判断都正确。

2. instanceof
    instanceof 可以正确判断对象的类型，其内部运行机制是判断在其原型链中能否找到该类型的原型。
    ```
    2 instanceof Number;                        // false
    'str' instanceof String;                    // false
    true instanceof Boolean;                    // false

    [] instanceof Array;                        // true
    function (){} instanceof Function;          // true
    {} instanceof Object;                       // true
    ```
    可以看到，instanceof 只能正确判断引用数据类型，而不能判断基本数据类型。instanceof 运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

3. constructor
    ```
    (2).constructor === Number;                     // true
    ('str').constructor === String;                 // true
    (true).constructor === Boolean;                 // true
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
    console.log(a.call(undefined));             // [object Undefined]
    console.log(a.call(null));                  // [object Null]
    console.log(a.call([]));                    // [object Array]
    console.log(a.call(function (){}));         // [object Function]
    console.log(a.call({}));                    // [object Object]
    ```
    同样是检测对象 obj 调用 toString 方法，obj.toString()的结果和Object.prototype.toString.call(obj)的结果不一样，这是为什么？
    这是因为 toString 是 Object 的原型方法，而 Array、function 等类型作为 Object 的实例，都重写了 toString 方法。不同的对象类型调用 toString 方法时，根据原型链的知识，调用的是对应的重写之后的 toString 方法（function 类型返回内容为函数体的字符串，Array类型返回元素组成的字符串…），而不会去调用 Object 上原型toString 方法（返回对象的具体类型），所以采用 obj.toString()不能得到其对象类型，只能将 obj 转换为字符串类型；因此，在想要得到对象的具体类型时，应该调用 Object 原型上的 toString 方法。

### 3. null 和 undefined 区别
首先 Undefined 和 Null 都是基本数据类型，这两个基本数据类型分别都只有一个值，就是 undefined 和 null。

undefined 代表的含义是未定义，null 代表的含义是空对象。一般变量声明了但还没有定义的时候会返回 undefined，null 主要用于赋值给一些可能会返回对象的变量，作为初始化。

undefined 在 JavaScript 中不是一个保留字，这意味着可以使用 undefined 来作为一个变量名，但是这样的做法是非常危险的，它会影响对 undefined 值的判断。我们可以通过一些方法获得安全的undefined 值，比如说 void 0。
    
当对这两种类型使用 typeof 进行判断时，Null 类型化会返回“object”，这是一个历史遗留问题。当使用双等号对两种类型的值进行比较时会返回 true，使用三个等号时会返回 false。

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

### 6. Object.is() 与比较操作符 “\=\=\=”、“\=\=” 的区别？
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

### 9. 如何判断一个对象是空对象
使用 JSON 自带的 .stringify 方法来判断：
```
if (JSON.stringify(Obj) == '{}') {
    console.log('空对象');
}
```
使用 ES6 新增的方法 Object.keys()来判断：
```
if (Object.keys(Obj).length <= 0) {
    console.log('空对象');
}
```

### 10. 为什么会有 BigInt 的提案？
JavaScript 中 Number.MAX_SAFE_INTEGER 表示最⼤安全数字，计算结果是 9007199254740991，即在这个数范围内不会出现精度丢失（⼩数除外）。但是⼀旦超过这个范围，js 就会出现计算不准确的情况，这在⼤数计算的时候不得不依靠⼀些第三⽅库进⾏解决，因此官⽅提出了 BigInt 来解决此问题。

### 11. 扩展运算符的作用及使用场景
1. 对象扩展运算符
对象的扩展运算符(...)用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中。
```
let bar = {a: 1, b: 2};
let baz = {...bar}; // {a: 1, b: 2}
```
上述方法实际上等价于：
```
let bar = {a: 1, b: 2};
let baz = Object.assign({}, bar); // {a: 1, b: 2}
```
Object.assign 方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。Object.assign 方法的第一个参数是目标对象，后面的参数都是源对象。(如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性)。
同样，如果用户自定义的属性，放在扩展运算符后面，则扩展运算符内部的同名属性会被覆盖掉.
```
let bar = {a: 1, b: 2};
let baz = {...bar, ...{a: 2, c: 2}}; // {a: 2, b: 2, c: 2}
```
利用上述特性就可以很方便的修改对象的部分属性。在 redux 中的 reducer 函数规定必须是一个纯函数，reducer 中的 state 对象要求不能直接修改，可以通过扩展运算符把修改路径的对象都复制一遍，然后产生一个新的对象返回。
需要注意：扩展运算符对对象实例的拷贝属于浅拷贝。
2. 数组扩展运算符
数组的扩展运算符可以将一个数组转为用逗号分隔的参数序列，且每次只能展开一层数组。
```
console.log(...[1, 2, 3]); // 1 2 3
console.log(...[1, [2, 3, 4], 5]); // 1 [2, 3, 4] 5
```
下面是数组的扩展运算符的应用：
将数组转换为参数序列
```
function add(x, y) {
    return x + y;
}
const numbers = [1, 2];
add(...numbers); // 3
```
复制数组
```
const arr1 = [1, 2];
const arr2 = [...arr1];
```
要记住：扩展运算符(…)用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中，这里参数对象是个数组，数组里面的所有对象都是基础数据类型，将所有基础数据类型重新拷贝到新的数组中。
合并数组
如果想在数组内合并数组，可以这样
```
const arr1 = ["two", "three"];
const arr2 = ["one", ...arr1, "four", "five"]; // ["one", "two", "three", "four", "five"]
```
扩展运算符与解构赋值结合起来，用于生成数组
```
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest // [2, 3, 4, 5]
```
需要注意：如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。
```
const [...rest, last] = [1, 2, 3, 4, 5]; // Uncaught SyntaxError: Rest element must be last element
const [first, ...rest, last] = [1, 2, 3, 4, 5]; // Uncaught SyntaxError: Rest element must be last element
```
将字符串转为真正的数组
```
[..."hello"] //  ["h", "e", "l", "l", "o"]
```
任何 Iterator 接口的对象，都可以用扩展运算符转为真正的数组
比较常见的应用是可以将某些数据结构转为数组：
```
// arguments对象
function foo() {
    const args = [...arguments];
}
```
用于替换 es5 中的 Array.prototype.slice.call(arguments)写法。
使用 Math 函数获取数组中特定的值
```
const numbers = [9, 2, 7, 1];
Math.min(...numbers); // 1
Math.max(...numbers); // 9
```
### 12. 对作用域、作用域链的理解
1. 全局作用域和函数作用域
（1）全局作用域
- 最外层函数和最外层函数外面定义的变量拥有全局作用域
- 所有未定义直接赋值的变量自动声明为全局作用域
- 所有 window 对象的属性拥有全局作用域
全局作用域有很大的弊端，过多的全局作用域变量会污染全局命名空间，容易引起命名冲突。
（2）函数作用域
声明在函数内部的变量，一般只有固定的代码片段可以访问到
作用域是分层的，内层作用域可以访问外层作用域，反之不行
2. 块级作用域
使用 ES6 中新增的 let 和 const 指令可以声明块级作用域，块级作用域可以在函数中创建也可以在一个代码块中的创建（由{ }包裹的代码片段）
let 和 const 声明的变量不会有变量提升，也不可以重复声明
在循环中比较适合绑定块级作用域，这样就可以把声明的计数器变量限制在循环内部。

**作用域链：**
在当前作用域中查找所需变量，但是该作用域没有这个变量，那这个变量就是自由变量。如果在自己作用域找不到该变量就去父级作用域查找，依次向上级作用域查找，直到访问到 window 对象后被终止，这一层层的关系就是作用域链。
作用域链的作用是保证对执行环境有权访问的所有变量和函数的有序访问，通过作用域链，可以访问到外层环境的变量和函数。
作用域链的本质上是一个指向变量对象的指针列表。变量对象是一个包含了执行环境中所有变量和函数的对象。作用域链的前端始终都是当前执行上下文的变量对象。全局执行上下文的变量对象（也就是全局对象）始终是作用域链的最后一个对象。
当查找一个变量时，如果当前执行环境中没有找到，可以沿着作用域链向后查找。

### 13. 对原型、原型链的理解
在 JavaScript 中是使用构造函数来新建一个对象的，每一个构造函数的内部都有一个 prototype 属性，它的属性值是一个对象，这个对象包含了可以由该构造函数的所有实例共享的属性和方法。当使用构造函数新建一个对象后，在这个对象的内部将包含一个指针，这个指针指向构造函数的 prototype 属性对应的值，在 ES5 中这个指针被称为对象的原型。一般来说不应该能够获取到这个值的，但是现在浏览器中都实现了__proto__ 属性来访问这个属性，但是最好不要使用这个属性，因为它不是规范中规定的。ES5 中新增了一个 Object.getPrototypeOf() 方法，可以通过这个方法来获取对象的原型。
当访问一个对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的原型对象里找这个属性，这个原型对象又会有自己的原型，于是就这样一直找下去，也就是原型链的概念。原型链的尽头一般来说都是 Object.prototype 所以这就是新建的对象为什么能够使用 toString() 等方法的原因。
特点：JavaScript 对象是通过引用来传递的，创建的每个新对象实体中并没有一份属于自己的原型副本。当修改原型时，与之相关的对象也会继承这一改变。
![JavaScript Object Layout](../assets/images/JavaScript%20Object%20Layout.jpg)

### 14. 原型链的终点是什么？如何打印出原型链的终点？
由于 Object 是构造函数，原型链终点 Object.prototype.\_\_proto__，而 Object.prototype.\_\_proto__=== null // true，所以，原型链的终点是 null。原型链上的所有原型都是对象，所有的对象最终都是由 Object 构造的，而 Object.prototype 的下一级是 Object.prototype.\_\_proto__。

### 15. 对象创建的方式有哪些？
一般使用字面量的形式直接创建对象，但是这种创建方式对于创建大量相似对象的时候，会产生大量的重复代码。但 js 和一般的面向对象的语言不同，在 ES6 之前它没有类的概念。但是可以使用函数来进行模拟，从而产生出可复用的对象创建方式，常见的有以下几种：

1. 第一种是**工厂模式**，工厂模式的主要工作原理是用函数来封装创建对象的细节，从而通过调用函数来达到复用的目的。但是它有一个很大的问题就是创建出来的对象无法和某个类型联系起来，它只是简单的封装了复用代码，而没有建立起对象和类型间的关系。
2. 第二种是**构造函数模式**。js 中每一个函数都可以作为构造函数，只要一个函数是通过 new 来调用的，那么就可以把它称为构造函数。执行构造函数首先会创建一个对象，然后将对象的原型指向构造函数的 prototype 属性，然后将执行上下文中的 this 指向这个对象，最后再执行整个函数。如果返回值不是对象，则返回新建的对象。因为 this 的值指向了新建的对象，因此可以使用 this 给对象赋值。
构造函数模式相对于工厂模式的优点是，所创建的对象和构造函数建立起了联系，因此可以通过原型来识别对象的类型。但是构造函数存在的一个缺点就是，造成了不必要的函数对象的创建，因为在 js 中函数也是一个对象，因此如果对象属性中如果包含函数的话，那么每次都会新建一个函数对象，浪费了不必要的内存空间，因为函数是所有的实例都可以通用的。
3. 第三种模式是**原型模式**，因为每一个函数都有一个 prototype属性，这个属性是一个对象，它包含了通过构造函数创建的所有实例都能共享的属性和方法。因此可以使用原型对象来添加公用属性和方法，从而实现代码的复用。这种方式相对于构造函数模式来说，解决了函数对象的复用问题。但这种模式也存在一些问题，一个是没有办法通过传入参数来初始化值；另一个是如果存在一个引用类型如 Array 这样的值，那么所有的实例将共享一个对象，一个实例对引用类型值的改变会影响所有的实例。
4. 第四种模式是**组合使用构造函数模式和原型模式**，这是创建自定义类型的最常见方式。因为构造函数模式和原型模式分开使用都存在一些问题，因此可以组合使用这两种模式，通过构造函数来初始化对象的属性，通过原型对象来实现函数方法的复用。这种方法很好的解决了两种模式单独使用时的缺点，但是有一点不足的就是，因为使用了两种不同的模式，所以对于代码的封装性不够好。
5. 第五种模式是**动态原型模式**，这一种模式将原型方法赋值的创建过程移动到了构造函数的内部，通过对属性是否存在的判断，可以实现仅在第一次调用函数时对原型对象赋值一次的效果。这一种方式很好地对上面的混合模式进行了封装。
6. 第六种模式是**寄生构造函数模式**，这一种模式和工厂模式的实现基本相同，我对这个模式的理解是，它主要是基于一个已有的类型，在实例化时对实例化的对象进行扩展。这样既不用修改原来的构造函数，也达到了扩展对象的目的。它的一个缺点和工厂模式一样，无法实现对象的识别。

### 16. 对象继承的方式有哪些？
1. 第一种是以**原型链**的方式来实现继承，但是这种实现方式存在的缺点是，在包含有引用类型的数据时，会被所有的实例对象所共享，容易造成修改的混乱。还有就是在创建子类型的时候不能向超类型传递参数。
2. 第二种方式是使用**借用构造函数**的方式，这种方式是通过在子类型的函数中调用超类型的构造函数来实现的，这一种方法解决了不能向超类型传递参数的缺点，但是它存在的一个问题就是无法实现函数方法的复用，并且超类型原型定义的方法子类型也没有办法访问到。
3. 第三种方式是**组合继承**，组合继承是将原型链和借用构造函数组合起来使用的一种方式。通过借用构造函数的方式来实现类型的属性的继承，通过将子类型的原型设置为超类型的实例来实现方法的继承。这种方式解决了上面的两种模式单独使用时的问题，但是由于我们是以超类型的实例来作为子类型的原型，所以调用了两次超类的构造函数，造成了子类型的原型中多了很多不必要的属性。
4. 第四种方式是**原型式继承**，原型式继承的主要思路就是基于已有的对象来创建新的对象，实现的原理是，向函数中传入一个对象，然后返回一个以这个对象为原型的对象。这种继承的思路主要不是为了实现创造一种新的类型，只是对某个对象实现一种简单继承，ES5 中定义的 Object.create() 方法就是原型式继承的实现。缺点与原型链方式相同。
5. 第五种方式是**寄生式继承**，寄生式继承的思路是创建一个用于封装继承过程的函数，通过传入一个对象，然后复制一个对象的副本，然后对象进行扩展，最后返回这个对象。这个扩展的过程就可以理解是一种继承。这种继承的优点就是，如果这个对象不是自定义类型和构造函数时，对一个简单对象实现继承。缺点是没有办法实现函数的复用。
6. 第六种方式是**寄生式组合继承**，组合继承的缺点就是使用超类型的实例做为子类型的原型，导致添加了不必要的原型属性。寄生式组合继承的方式是使用超类型的原型的副本来作为子类型的原型，这样就避免了创建不必要的属性。

### 17. 对 this 对象的理解
this 是执行上下文中的一个属性，它指向最后一次调用这个方法的对象。在实际开发中，this 的指向可以通过四种调用模式来判断：

第一种是函数调用模式，当一个函数不是一个对象的属性时，直接作为函数来调用时，this 指向全局对象。

第二种是方法调用模式，如果一个函数作为一个对象的方法来调用时，this 指向这个对象。

第三种是构造器调用模式，如果一个函数用 new 调用时，函数执行前会新创建一个对象，this 指向这个新创建的对象。

第四种是 apply 、 call 和 bind 调用模式，这三个方法都可以显式的指定调用函数的 this 指向。其中 apply 方法接收两个参数：一个是 this 绑定的对象，一个是参数数组。call 方法接收的参数，第一个是 this 绑定的对象，后面的其余参数是传入函数执行的参数。
也就是说，在使用 call() 方法时，传递给函数的参数必须逐个列举出来。bind 方法通过传入一个对象，返回一个 this 绑定了传入对象的新函数。这个函数的 this 指向除了使用 new 时会被改变，其他情况下都不会改变。

这四种方式，使用构造器调用模式的优先级最高，然后是 apply、call 和 bind 调用模式，然后是方法调用模式，然后是函数调用模式。

### 18. call() 和 apply() 的区别？
它们的作用一模一样，区别仅在于传入参数的形式的不同。

apply 接受两个参数，第一个参数指定了函数体内 this 对象的指向，第二个参数为一个带下标的集合，这个集合可以为数组，也可以为类数组，apply 方法把这个集合中的元素作为参数传递给被调用的函数。

call 传入的参数数量不固定，跟 apply 相同的是，第一个参数也是代表函数体内的 this 指向，从第二个参数开始往后，每个参数被依次传入函数。

### 19. 箭头函数的 this 指向哪⾥？
箭头函数不同于传统 JavaScript 中的函数，箭头函数并没有属于自己的 this，它所谓的 this 是捕获其所在上下⽂的 this 值，作为自己的 this 值，并且由于没有属于自己的 this，所以是不会被 new 调⽤的，这个所谓的 this 也不会被改变。

可以⽤Babel 理解⼀下箭头函数:
```
const obj = {
    getArrow() {
        return () => {
            console.log(this === obj);
        };
    }
}
```
转化后：
```
var obj = {
    getArrow: function getArrow() {
        var _this = this;
        return function () {
            console.log(_this === obj);
        };
    }
}
```

### 20. 如果 new 一个箭头函数的会怎么样？
箭头函数是 ES6 中的提出来的，它没有 prototype ，也没有自己的 this 指向，更不可以使用 arguments 参数，所以不能 New 一个箭头函数。

new 操作符的实现步骤如下：
1. 创建一个对象
2. 将构造函数的作用域赋给新对象（也就是将对象的__proto__属性指向构造函数的 prototype 属性）
3. 指向构造函数中的代码，构造函数中的 this 指向该对象（也就是为这个对象添加属性和方法）
4. 返回新的对象
所以，上面的第二、三步，箭头函数都是没有办法执行的。

### 21. JQuery链式调用原理
eg:　$("div").width("100px").height("100px");
实现原理：
```
let Fun = {
    fn1: function () {
        console.log("fn1");
        return this;
    },
    fn2: function () {
        console.log("fn2");
        return this;
    },
    fn3: function () {
        console.log("fn3");
        return this;
    }
}

Fun.fn1().fn2();
```
这样子便可以实现链式了，原因是在每一个方法后面都加了一个 return this。

在一个对象里面，this 指向的是对象本身，当我们调用方法的时候，这些方法都是在对象内部调用的，所以加 this 才可以访问到这些方法。

如果没有加上 return this 的话，那么执行完一个函数之后，会默认返回 undefined，undefine 是 js 内部隐式添加的。

### 22. for、for…in、for…of、forEach、map 等的区别
1. **for循环：**
根据下标遍历
常规语句遍历，可循环数字、字符串、数组。
有下标，通过下标取值，可通过return、break退出循环，或者通过continue语句跳出本次循环。
> (1)三个表达式都可以省略，但是表达式之间的分号不能省略。都省略的话就创建了一个无穷循环。
> (2)初始化变量：给循环变量、其他变量进行初始化。
> (3)条件表达式：控制循环体语句是否执行。如果只包含条件表达式for循环就变成while循环。
> (4)操作表达式：使循环趋向结束的语句。
2. **for…in语句**
根据 key 遍历
for…in 类似于增强 for 循环，我们可以简单理解为它适合遍历对象。
应用场景:遍历数组以及遍历对象的属性。
for…in 以原始插入顺序访问对象的可枚举属性，包括从原型继承而来的可枚举属性。遍历对象时会从原型上继承属性，可以用 hasOwnProperty 识别出继承属性。
for…in 用于遍历数组时，可以将数组看作对象，数组下标看作属性名，也就输出结果是数组的下标。但用 for…in 遍历数组时不一定会按照数组的索引顺序。
数组遍历下标，对象遍历属性。
3. **for…of语句**
根据值遍历
for…of 语句是一种严格的迭代语句，用于遍历可迭代对象的元素。
for…of 语句在可迭代对象（Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，为每个不同属性的值执行语句。
使用 for…in 循环时，获得的是数组的下标；使用 for…of 循环时，获得的是数组的元素值。
for…of 遍历 Map 时，可以获得整个键值对对象，也可以只获得键值，或者分别获得键与值。
用 for…of 来遍历数据，避免了所有 for…in 的弊端，与 forEach 相比可以正确响应break，continue，return语句。
for…of 虽然好用，但要注意下面的几个问题：
- 不能直接遍历对象。对象没有实现迭代器接口，直接遍历会抛出异常。如果想遍历对象的属性，可以先通过 Object.keys() 方法获取对象的属性列表，然后再遍历。
- 不能实现数组的赋值。for…of 遍历数组时并没有提供索引，无法直接修改数组。如果打算改变数组，建议使用其他遍历方法。
- 不要提前修改未迭代的项目。如果你在遍历途中修改后面项的值，在之后的迭代中获取的是新的值。
- 不要在遍历途中增删项目。如果你在遍历途中删除了未迭代的项目，会导致迭代次数的减少；如果你在遍历途中添加了新项，它们也将会被迭代。
4. **forEach语句**
foreach 语句是对数组的每个元素根据提供的函数执行一次。是for语句的特殊简化版本，不能完全取代for语句，但任何foreach语句都可以改写为for语句版本。
遍历全部数据，不能通过return结束循环，消耗性能。
用于不转换数据的全部遍历。
forEach一般只能适用于数组，功能是从头到尾把数组遍历一遍，可以有三个参数，后两个可以不写。
```
// 手写forEach
Array.prototype.myForEach = function (fn) {
    for (let i = 0; i < this.length; i++) {
        fn(this[i], i, this);
    }
}
```
5. **map**
根据index遍历
和forEach相比，使用方法一样，有三个参数，map只能对元素进行加工处理，产生一个新的数组对象。
遍历全部数据，不能通过return结束，消耗性能，不要常用。
常用于转换数据结构，比forEach快。
6. **filter**
对原数组进行过滤筛选，生成新的数组，使用和map一样有三个参数。如果对空数组进行筛选，会返回undefined。filter不会改变原数组。
遍历全部数据，返回数组，过滤成新的数组。
常用于：过滤不符合项，数组去重，过滤空字符串、undefined、null等。

**比较**
优缺点
for:
优点：程序简洁，结构清晰，循环初始化，循环变量化和循环条件位置突出。
缺点：结构比while循环复杂，容易出编码错误。

for…in:
优点：类似于增强for循环，适合遍历对象。
缺点：对象遍历属性，包括从原型继承而来的可枚举属性，性能较差，如不需要，则要额外处理。数组遍历下标，但不一定会按照数组的索引顺序。

for…of:
优点：避免了for…in的所有缺点，支持break,continue,return。支持遍历Array（数组）, String（字符串）, Map（映射）, Set（集合）,TypedArray（类型化数组）、arguments、NodeList对象、Generator等可迭代的数据结构。
缺点：不适用于遍历普通对象。

forEach:
优点：遍历的时候更加简洁，效率和for循环相同，不用关心集合下标的问题，减少了出错的效率。
缺点：不能同时遍历多个集合，在遍历的时候无法修改和删除集合数据，不能使用break，continue语句跳出循环，或者使用return从函数体返回，对于空数组不会执行回调函数。

**区别**
四个算法语句区别主要体现在响应break, continue, return上和使用的对象上。

for 语句性能最好；能响应break, continue, return控制循环。

forEach 无法响应break, continue, return控制循环。

for…in 无法响应break, continue, return控制循环；for…in 主要针对对象，它不仅会遍历对象本身的属性，还会查找遍历原型上的属性；遍历的顺序不确定。

for…of 能响应break, continue, return控制循环，还能遍历map、set 等类数组，但是不能遍历普通的对象

for…of 是 ES6 新增的遍历方式，允许遍历一个含有 iterator 接口的数据结构（数组、对象等）并且返回各项的值，和 ES3 中的 for…in 的区别如下
for…of 遍历获取的是对象的键值，for…in 获取的是对象的键名；
for…in 会遍历对象的整个原型链，性能非常差不推荐使用，而for…of 只遍历当前对象不会遍历原型链；
对于数组的遍历，for…in 会返回数组中所有可枚举的属性(包括原型链上可枚举的属性)，for…of 只返回数组的下标对应的属性值；
总结：for…in 循环主要是为了遍历对象而生，不适用于遍历数组；for…of 循环可以用来遍历数组、类数组对象，字符串、Set、Map 以及 Generator 对象。

### 23. 可以迭代大部分数据类型的 for…of 为什么不能遍历普通对象？怎么解决 for…of 遍历对象问题？
我们知道，ES6 中引入 for…of 循环，很多时候用以替代 for…in 和 forEach() ，并支持新的迭代协议。for…of 允许你遍历 Array（数组）, String（字符串）, Map（映射）, Set（集合）,TypedArray(类型化数组)、arguments、NodeList对象、Generator等可迭代的数据结构等。for…of 语句在可迭代对象上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句。
```
// 普通对象
const obj = {
    foo: 'value1',
    bar: 'value2'
}
for (const item of obj) {
    console.log(item)
}
// Uncaught TypeError: obj is not iterable
```
能够被 for…of 正常遍历的，都需要实现一个遍历器 Iterator。而数组、字符串、Set、Map结构，早就内置好了 Iterator（迭代器），它们的原型中都有一个 Symbol.iterator 方法，而Object对象并没有实现这个接口，使得它无法被 for…of 遍历。
```
Array.prototype[Symbol.iterator];
// ƒ values() { [native code] }
 
String.prototype[Symbol.iterator];
// ƒ [Symbol.iterator]() { [native code] }
 
Set.prototype[Symbol.iterator];
// ƒ values() { [native code] }
 
Map.prototype[Symbol.iterator];
// ƒ entries() { [native code] }
 
Object.prototype[Symbol.iterator];
// undefined
```
如何让对象可以被 for…of 遍历，当然是给它添加遍历器，代码如下：
```
Object.prototype[Symbol.iterator] = function () {
    // 这里Object.keys不会获取到Symbol.iterator属性，原因见下文
    const keys = Object.keys(this);
    let index = 0;
    return {
        next: () => {
            if (index < keys.length) {
                // 迭代结果 未结束
                return {
                    value: this[keys[index++]],
                    done: false
                };
            } else {
                // 迭代结果 结束
                return {
                    value: undefined,
                    done: true
                };
            }
        }
    };
};
```
在此，有一点需要说明的是，不用担心 [Symbol.iterator] 属性会被 Object.keys() 获取到导致遍历结果出错，因为 Symbol.iterator 这样的 Symbol 属性，需要通过 Object.getOwnPropertySymbols(obj) 才能获取，Object.getOwnPropertySymbols() 方法返回一个给定对象自身的所有 Symbol 属性的数组。
有一些场合会默认调用 Iterator 接口（即 Symbol.iterator 方法）：
- 扩展运算符...：这提供了一种简便机制，可以将任何部署了 Iterator 接口的数据结构，转为数组。也就是说，只要某个数据结构部署了 Iterator 接口，就可以对它使用扩展运算符，将其转为数组（毫不意外的，代码 [...{}] 会报错，而 [...'123'] 会输出数组 ['1', '2', '3']）。
- 数组和可迭代对象的解构赋值（解构是ES6提供的语法糖，其实内在是针对可迭代对象的Iterator接口，通过遍历器按顺序获取对应的值进行赋值。而普通对象解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。）；
- yield*：_yield* 后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口；
- 由于数组的遍历会调用遍历器接口，所以任何接受数组作为参数的场合，其实都调用；
- 字符串是一个类似数组的对象，也原生具有Iterator接口，所以也可被 for…of 迭代。

如何使用生成器实现遍历，代码如下：
```
Object.prototype[Symbol.iterator] = function* () {
    const keys = Object.keys(this);
    for (let i = 0; i < keys.length; i++) {
        yield keys[i];
    }
};

// 使用
const obj = {
    a: 1,
    b: 2,
    c: 3
}
for (const key of obj) {
    console.log(key, obj[key]);
} // a 1 // b 2 // c 3
```

### 24. for…of 遍历数组时并没有提供索引，无法直接修改数组
可以通过实现 Number 原型对象上的迭代接口解决。
代码如下：
```
Number.prototype[Symbol.iterator] = function* () {
    const num = this.valueOf();
    for (let i = 0; i < num; i++) {
        yield i;
    }
}

// 使用
const arr = [0, 1, 2, 3, 4];
for (const index of arr.length) {
    arr[index] *= 2;
}
console.log(arr); // [0, 2, 4, 6, 8]
```
补充：如果是在 ts 中扩展 for…of 后，使用时会提示错误，加入下列接口声明就好了：
```
declare interface Object {
    [Symbol.iterator]: any
}
declare interface Number {
    [Symbol.iterator]: any
}
```

### 25. 对 AJAX 的理解，实现一个 AJAX 请求
AJAX 是 Asynchronous JavaScript and XML 的缩写，指的是通过 JavaScript 的异步通信，从服务器获取 XML 文档从中提取数据，再更新当前网页的对应部分，而不用刷新整个网页。
创建 AJAX 请求的步骤：
创建一个 XMLHttpRequest 对象。
在这个对象上使用 open 方法创建一个 HTTP 请求，open 方法所需要的参数是请求的方法、请求的地址、是否异步和用户的认证信息。
在发起请求前，可以为这个对象添加一些信息和监听函数。比如说可以通过 setRequestHeader 方法来为请求添加头信息。还可以为这个对象添加一个状态监听函数。一个 XMLHttpRequest 对象一共有 5 个状态，当它的状态变化时会触发 onreadystatechange 事件，可通过设置监听函数，来处理请求成功后的结果。当对象的 readyState 变为 4 的时候，代表服务器返回的数据接收完成，这个时候可以通过判断请求的状态，如果状态是 2xx 或者 304 的话则代表返回正常。
这个时候就可以通过 response 中的数据来对页面进行更新了。
当对象的属性和监听函数设置完成后，最后调用 sent 方法来向服务器发起请求，可以传入参数作为发送的数据体。
```
const SERVER_URL = "/server";
let xhr = new XMLHttpRequest();
// 创建 Http 请求
xhr.open("GET", url, true);
// 设置状态监听函数
xhr.onreadystatechange = function () {
    if (this.readyState !== 4) return;
    // 当请求成功时
    if (this.status === 200) {
        handle(this.response);
    } else {
        console.error(this.statusText);
    }
};
// 设置请求失败时的监听函数
xhr.onerror = function () {
    console.error(this.statusText);
};
// 设置请求头信息
xhr.responseType = "json";
xhr.setRequestHeader("Accept", "application/json");
// 发送 Http 请求
xhr.send(null);
```
使用 Promise 封装 AJAX：
```
// promise 封装实现：
function getJSON(url) {
    // 创建一个 promise 对象
    let promise = new Promise(function (resolve, reject) {
        let xhr = new XMLHttpRequest();
        // 新建一个 http 请求
        xhr.open("GET", url, true);
        // 设置状态的监听函数
        xhr.onreadystatechange = function () {
            if (this.readyState !== 4) return;
            // 当请求成功或失败时，改变 promise 的状态
            if (this.status === 200) {
                resolve(this.response);
            } else {
                reject(new Error(this.statusText));
            }
        };
        // 设置错误监听函数
        xhr.onerror = function () {
            reject(new Error(this.statusText));
        };
        // 设置响应的数据类型
        xhr.responseType = "json";
        // 设置请求头信息
        xhr.setRequestHeader("Accept", "application/json");
        // 发送 http 请求
        xhr.send(null);
    });
    return promise;
}
```

### 26. ajax、axios、fetch 的区别
1. AJAX
Ajax 即“AsynchronousJavascriptAndXML”（异步 JavaScript 和 XML），是指一种创建交互式网页应用的网页开发技术。它是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。通过在后台与服务器进行少量数据交换，Ajax 可以使网页实现异步更新。
这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。传统的网页（不使用 Ajax）如果需要更新内容，必须重载整个网页页面。其缺点如下：
本身是针对 MVC 编程，不符合前端 MVVM 的浪潮。
基于原生 XHR 开发，XHR 本身的架构不清晰。
不符合关注分离（Separation of Concerns）的原则。
配置和调用方式非常混乱，而且基于事件的异步模型不友好。
2. Fetch
fetch 号称是 AJAX 的替代品，是在 ES6 出现的，使用了 ES6 中的 promise 对象。Fetch 是基于 promise 设计的。Fetch 的代码结构比起 ajax 简单多。fetch 不是 ajax 的进一步封装，而是原生 js，没有使用 XMLHttpRequest 对象。
fetch 的优点：
语法简洁，更加语义化。
基于标准 Promise 实现，支持 async/await。
更加底层，提供的 API 丰富（request, response）。
脱离了 XHR，是 ES 规范里新的实现方式。
fetch 的缺点：
fetch 只对网络请求报错，对 400，500 都当做成功的请求，服务器返回 400，500 错误码时并不会 reject，只有网络错误这些导致请
求不能完成时，fetch 才会被 reject。
fetch 默认不会带 cookie ， 需要添加配置项： fetch(url, {credentials: 'include'})。
fetch 不支持 abort ，不支持超时控制 ， 使用 setTimeout 及 Promise.reject 的实现的超时控制并不能阻止请求过程继续在后台运行，造成了流量的浪费。
fetch 没有办法原生监测请求的进度，而 XHR 可以。
3. Axios
Axios 是一种基于 Promise 封装的 HTTP 客户端，其特点如下：
浏览器端发起 XMLHttpRequests 请求。
node 端发起 http 请求。
支持 Promise API。
监听请求和返回。
对请求和返回进行转化。
取消请求。
自动转换 json 数据。
客户端支持抵御 XSRF 攻击。

### 27. 跨域的产生原因及解决方案
1. 跨域的原因
跨域是是因为浏览器的同源策略限制，是浏览器的一种安全机制，服务端之间是不存在跨域的。
所谓同源指的是两个页面具有相同的协议、主机和端口，三者有任一不相同即会产生跨域。

2. 跨域举例

| 当前页面url | 被请求页面url | 是否跨域 | 原因 |
|---|---|---|---|
| http://www.test.com/ | http://www.test.com/index.html | 否 | 同源（协议、域名、端口号相同） |
| http://www.test.com/ | https://www.test.com/index.html | 跨域 | 协议不同（http / https） |
| http://www.test.com/ | http://www.baidu.com/ | 跨域 | 主域名不同（test / baidu） |
| http://www.test.com/ | https://blog.test.com/ | 跨域 | 子域名不同（www / blog） |
| http://www.test.com:8080/ | https://www.test.com:7001/ | 跨域 | 端口号不同（8080 / 7001） |

3. 同源策略目的
保证用户信息安全，防止恶意网站窃取数据。同源策略是必须的，否则 cookie 可以共享。

4. 同源策略的限制范围
- cookie、localstorage、indexdb 无法读取。
- DOM 无法获取。
- ajax 请求不能发送

5. 规避策略
    1. cookie：设置document.domain共享cookie。
    2. DOM获取：（父子页面通信）H5引入了一个API，这个API为windows对象新增了一个window.postMessage方法，允许跨窗口通信，无论这两个窗口是否同源。
    > eg: window.opener.postMessage(content,origin)
    content是消息的具体内容，origin是协议 + 域名 + 端口
    3. JSONP：JSONP 是服务器无客户端跨源通信的常用方法。基本思想是网页通过添加一个 \<script> 标签，向服务器请求 json 数据，这种做法不受同源政策的限制，服务器收到请求后，将数据放在一个指定名字的回调函数里面传回来。（只能发GET请求）
    4. WebSocket：WebSocket 是一种通信协议。使用 ws://（非加密）和 wss://（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。
    5. CORS
    跨域资源共享（corss-origin resource sharing）：CORS 需要浏览器和服务器同时支持。目前所有浏览器都支持该功能，IE浏览器不能低于IE10。整个 CORS 通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS 通信与同源的 AJAX 通信没有差别，代码完全一样。浏览器一旦发现 AJAX 请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。因此，实现 CORS 通信的关键是服务器。只要服务器实现了 CORS 接口，就可以跨源通信。
    两种请求：浏览器将 CORS 请求分成两类：简单请求（simple request）和非简单请求（not-so-simple request）。
    ![CORS请求](../assets/images/CORS%E8%AF%B7%E6%B1%82.jpg)
    - 简单请求
        - 对于简单请求，浏览器直接发出 CORS 请求。具体来说，就是在 Header 中增加一个 Origin 字段。如果浏览器发现跨源 AJAX 请求是简单请求，就自动在头信息之中，添加一个 Origin 字段。
        ```
        GET /cors HTTP/1.1 Origin: http://easywork.xin //浏览器添加字段，说明本次请求来自哪个源（协议+域名+端口）。 Host: 119.23.214.114 Accept-Language: en-US Connection: keep-alive User-Agent: Mozilla/5.0...
        ```
        - 服务器根据 Origin 的值决定是否同意这次请求。
        如果 Origin 指定的源在不在后端的许可白名单范围内，服务器会返回一个正常的 http 回应。浏览器接收后发现，这个 response 的 Header 没有包含 Access-Control-Allow-Origin 字段，就知道出错了，从而抛出一个错误，被 XMLHttpRequest 的 onerror 回调函数捕获。这种错误无法通过状态码识别，因此HTTP response的状态码有可能是200。
        如果Origin指定的域名在许可的范围内，则服务器返回的相应中，会多出几个头信息字段：
        ```
        Access-Control-Allow-Origin: http://easywork.xin Access-Control-Allow-Credentials: true Access-Control-Expose-Headers: FooBar Content-Type: text/html; charset=utf-8
        ```
        Access-Control-Allow-Origin：（必须字段）它的值要么是请求时 Origin 的值，要么是*，表示接受任意域名的请求。
        Access-Control-Allow-Credentials：（可选字段）它是一个 bool 值，表示是否允许发送 Cookie。默认情况下，Cookie 不包括在 CORS 请求之中。设为 true，表示服务器明确许可，Cookie 可以包含在请求中，一起发给服务器。CORS 请求默认不发送 Cookie 和 HTTP 认证信息。如果要把 Cookie 发送到服务器，一方面要服务器同意，指定 Access-Control-Allow-Credentials: true；另一方面，前端必须在 AJAX 请求中打开 withCredentials 属性：
        ```
        var xhr = new XMLHttpRequest(); 
        xhr.withCredentials = true;
        ```
        注意：如果要发送 Cookie，Access-Control-Allow-Origin 不能设置为* ，必须指定明确的，与请求网页一致的域名。同时，Cookie 依然遵守同源政策，只有用服务器域名设置的Cookie 才会上传，其他域名的 Cookie 并不会上传。
        Access-Control-Expose-Headers：（可选字段）CORS 请求时，XMLHttpRequest 对象的 getResponseHeader(args) 方法只能拿到6个基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果想拿到其他字段，就必须在 Access-Control-Expose-Headers 里面指定。

    - 非简单请求
    非简单请求是那种对服务器有特殊要求的请求，比如请求方法是 PUT 或 DELETE，或者 Content-Type 字段的类型是 application/json。
    非简单请求的 CORS 请求，会在正式通信之前，增加一次 HTTP 查询请求，称为“预检”请求（preflight）。浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些 HTTP 动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的 XMLHttpRequest 请求，否则就报错。
    一旦服务器通过了“预检”请求，以后每次浏览器正常请求 CORS 请求，就跟简单请求一样。会有 Origin 字段，响应头里也会有对应的 Access-Control-Allow-Origin 字段。

    **与JSONP比较**
    CORS 与 JSONP 的使用目的相同，但是比 JSONP 更强大。JSONP 只支持 GET 请求，CORS 支持所有类型的 HTTP 请求。JSONP 的优势在于支持老式的浏览器，以及可以向比支持 CORS 的网站请求数据。

### 28. jsonp的跨域原理
jsonp 之所以能跨域，是因为他并不是发送 ajax 请求，并不是利用 XMLHTTPRequest 对象和服务端进行通信，它其实是利用动态创建的 script 标签，而 script 标签是没有同源策略限制的，可以跨域的。
创建 script 标签，然后将其 src 指向我们真实的服务端的地址，在这个地址的后面有一个参数比如 calback=a，然后服务端可以解析到这个url 中的 callback=a。服务端在返回的数据时，就会调用 a 方法，去包裹一段数据，然后返回这段 js 代码，相当于在前端去执行这个 a 方法。那么在前端发送请求之前，就要在 window 上去注册这个 a 方法，那么在服务端返回这个 a 方法执行的时候，就可以去之前在 window 上定义的 a 方法中获得数据了。
1. 利用的就是 script 的 src 标签属性没有跨域限制来实现的。
    - script的特性：会将引用的外部文件的文本内容当做 js 代码来进行解析
    - 将远程服务器的资源添加到当前路径（位置）进行解析
    - json 的格式：属性名必须有冒号，例如{“b”：145}
    - jsonp == json + padding

2. 执行的过程
    - 前端定义一个解析函数，例如 jsonpCallback = function（res）{}
    - 通过 params 的形式包装 script 的请求参数，并且声明执行函数（如 cb = jsonpCallback）
    - 后端获取到前端声明的执行函数（jsonpCallback），并以携带参数并且调用执行函数的方式传递给前端
    - 前端在 script 标签返回资源的时候就会执行 jsonpCallback，并以回调函数的方式拿到返回的数据了

3. 优缺点
缺点：只能进行get请求
优点：兼容性好，在一些老的浏览器中也可以运行

4. 完整的jsonp代码：
```
// 定义
function JSONP({
    url,
    params = {},
    callbackKey = 'cb',
    callback
}) {
    // 定义本地的唯一callbackId，若是没有的话则初始化为1
    JSONP.callbackId = JSONP.callbackId || 1;
    let callbackId = JSONP.callbackId;
    // 把要执行的回调加入到JSON对象中，避免污染window
    JSONP.callbacks = JSONP.callbacks || [];
    JSONP.callbacks[callbackId] = callback;
    // 把这个名称加入到参数中: 'cb=JSONP.callbacks[1]'
    params[callbackKey] = `JSONP.callbacks[${callbackId}]`;
    // 得到'id=1&cb=JSONP.callbacks[1]'
    const paramString = Object.keys(params).map(key => {
        return `${key}=${encodeURIComponent(params[key])}`
    }).join('&')
    // 创建 script 标签
    const script = document.createElement('script');
    script.setAttribute('src', `${url}?${paramString}`);
    document.body.appendChild(script);
    // id自增，保证唯一
    JSONP.callbackId++;
}

// 使用
JSONP({
    url: 'http://localhost:8080/api/jsonps',
    params: {
        a: '2&b=3',
        b: '4'
    },
    callbackKey: 'cb',
    callback(res) {
        console.log(res)
    }
})
JSONP({
    url: 'http://localhost:8080/api/jsonp',
    params: {
        id: 1
    },
    callbackKey: 'cb',
    callback(res) {
        console.log(res)
    }
})
```
**注意**
encodeURI 和 encodeURIComponent 的区别：
- encodeURI() 不会对本身属于 URI 的字符进行编码，例如：“/”,“：”，“#”，“？”
- encodeURIComponent() 则会对他发现的任何的非标准字符进行编码，同时使用 decodeURIcomponent() 来解码。

### 29. 对 Promise 的理解
Promise 是异步编程的一种解决方案，它是一个对象，可以获取异步操作的消息，他的出现大大改善了异步编程的困境，避免了地狱回调，它比传统的解决方案回调函数和事件更合理和更强大。
所谓 Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。
1. Promise 的实例有三个状态:
Pending（进行中）
Resolved（已完成）
Rejected（已拒绝）
当把一件事情交给 promise 时，它的状态就是 Pending，任务完成了状态就变成了 Resolved、没有完成失败了就变成了 Rejected。
2. Promise 的实例有两个过程：
pending -> fulfilled : Resolved（已完成）
pending -> rejected：Rejected（已拒绝）
注意：一旦从进行状态变成为其他状态就永远不能更改状态了。
**Promise 的特点：**
对象的状态不受外界影响。promise 对象代表一个异步操作，有三种状态，pending（进行中）、fulfilled（已成功）、rejected（已失
败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态，这也是 promise 这个名字的由来——“承诺”；
一旦状态改变就不会再变，任何时候都可以得到这个结果。promise 对象的状态改变，只有两种可能：从 pending 变为 fulfilled，从 pending 变为 rejected。这时就称为 resolved（已定型）。如果改变已经发生了，你再对 promise 对象添加回调函数，也会立即得到这个结果。这与事件（event）完全不同，事件的特点是：如果你错过了它，再去监听是得不到结果的。
**Promise 的缺点：**
无法取消 Promise，一旦新建它就会立即执行，无法中途取消。
如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。
当处于 pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）
**总结：**
Promise 对象是异步编程的一种解决方案，最早由社区提出。Promise 是一个构造函数，接收一个函数作为参数，返回一个 Promise 实例。
一个 Promise 实例有三种状态，分别是 pending、resolved 和 rejected，分别代表了进行中、已成功和已失败。实例的状态只能由 pending 转变 resolved 或者 rejected 状态，并且状态一经改变，就凝固了，无法再被改变了。
状态的改变是通过 resolve() 和 reject() 函数来实现的，可以在异步操作结束后调用这两个函数改变 Promise 实例的状态，它的原型上定义了一个 then 方法，使用这个 then 方法可以为两个状态的改变注册回调函数。这个回调函数属于微任务，会在本轮事件循环的末尾执行。
注意：在构造 Promise 的时候，构造函数内部的代码是立即执行的。

### 30. Promise 解决了什么问题？
在工作中经常会碰到这样一个需求，比如我使用 ajax 发一个 A 请求后，成功后拿到数据，需要把数据传给 B 请求；那么需要如下编写代码：
```
let fs = require('fs')
fs.readFile('./a.txt', 'utf8', function (err, data) {
    fs.readFile(data, 'utf8', function (err, data) {
        fs.readFile(data, 'utf8', function (err, data) {
            console.log(data);
        })
    })
})
```
上面的代码有如下缺点：
后一个请求需要依赖于前一个请求成功后，将数据往下传递，会导致多个 ajax 请求嵌套的情况，代码不够直观。
如果前后两个请求不需要传递参数的情况下，那么后一个请求也需要前一个请求成功后再执行下一步操作，这种情况下，那么也需要如上编写代码，导致代码不够直观。
Promise 出现之后，代码变成这样：
```
function read(url) {
    return new Promise((resolve, reject) => {
        fs.readFile(url, 'utf8', function (error, data) {
            error && reject(error);
            resolve(data);
        })
    })
}

read('./a.txt').then(data => {
    return read(data);
}).then(data => {
    return read(data);
}).then(data => {
    console.log(data);
});
```
这样代码看起了就简洁了很多，解决了地狱回调的问题。

### 31. 对 async/await 的理解
async/await 其实是 Generator 的语法糖，它能实现的效果都能用 then 链来实现，它是为优化 then 链而开发出来的。从字面上来看，async 是“异步”的简写，await 则为等待，所以很好理解 async 用于申明一个 function 是异步的，而 await 用于等待一个异步方法执行完成。当然语法上强制规定 await 只能出现在 asnyc 函数中，先来看看 async 函数返回了什么：
```
async function testAsy() {
    return 'hello world';
}
let result = testAsy();
console.log(result);
```
返回：
```
Promise {<fulfilled>: "hello world"}
__proto__: Promise
[[PromiseState]]: "fulfilled"
[[PromiseResult]]: "hello world"
```
所以，async 函数返回的是一个 Promise 对象。async 函数（包含函数语句、函数表达式、Lambda 表达式）会返回一个 Promise 对象。
如果在函数中 return 一个直接量，async 会把这个直接量通过 Promise.resolve() 封装成 Promise 对象。
async 函数返回的是一个 Promise 对象，所以在最外层不能用 await 获取其返回值的情况下，当然应该用原来的方式：then() 链来处理这个 Promise 对象，就像这样：
```
async function testAsy() {
    return 'hello world';
}
let result = testAsy();
console.log(result);
result.then(v => {
    console.log(v); // hello world
})
```
那如果 async 函数没有返回值，又该如何？很容易想到，它会返回 Promise.resolve(undefined)。
联想一下 Promise 的特点——无等待，所以在没有 await 的情况下执行 async 函数，它会立即执行，返回一个 Promise 对象，并且，绝不会阻塞后面的语句。这和普通返回 Promise 对象的函数并无二致。
注意：Promise.resolve(x) 可以看作是 new Promise(resolve => resolve(x)) 的简写，可以用于快速封装字面量对象或其他对象，将其封装成 Promise 实例。

### 32. async/await 的优势
单一的 Promise 链并不能发现 async/await 的优势，但是，如果需要处理由多个 Promise 组成的 then 链的时候，优势就能体现出来了（很有意思，Promise 通过 then 链来解决多层回调的问题，现在又用 async/await 来进一步优化它）。
假设一个业务，分多个步骤完成，每个步骤都是异步的，而且依赖于上一个步骤的结果。仍然用 setTimeout 来模拟异步操作：
```
/**
 * 传入参数 n，表示这个函数执行的时间（毫秒）
 * 执行的结果是 n + 200，这个值将用于下一步骤
 */

function takeLongTime(n) {
    return new Promise(resolve => {
        setTimeout(() => resolve(n + 200), n);
    });
}

function step1(n) {
    console.log(`step1 with ${n}`);
    return takeLongTime(n);
}

function step2(n) {
    console.log(`step2 with ${n}`);
    return takeLongTime(n);
}

function step3(n) {
    console.log(`step3 with ${n}`);
    return takeLongTime(n);
}
```
现在用 Promise 方式来实现这三个步骤的处理：
```
function doIt() {
    console.time("doIt");
    const time1 = 300;
    step1(time1)
        .then(time2 => step2(time2))
        .then(time3 => step2(time3))
        .then(result => console.log(`result is $(result)`));
    console.timeEnd("doIt");
}
doIt();

// step1 with 300
// step2 with 500
// step3 with 700
// result is 900
// doIt: 1507.251ms
```
输出结果 result 是 step3() 的参数 700 + 200 = 900。doIt() 顺序执行了三个步骤，一共用了 300 + 500 + 700 = 1500 毫秒，和 console.time()/console.timeEnd() 计算的结果一致。
如果用 async/await 来实现呢，会是这样：
```
async function doIt() {
    console.time("doIt");
    const time1 = 300;
    const time2 = await step1(time1);
    const time3 = await step2(time2);
    const result = await step3(time3);
    console.takeLongTime(`result is $(result)`);
    console.timeEnd("doIt");
}
doIt();
```
结果和之前的 Promise 实现是一样的，但是这个代码看起来是不是清晰得多，几乎跟同步代码一样。

### 33. async/await 对比 Promise 的优势
代码读起来更加同步，Promise 虽然摆脱了回调地狱，但是 then 的链式调⽤也会带来额外的阅读负担
Promise 传递中间值⾮常麻烦，⽽async/await⼏乎是同步的写法，⾮常优雅
错误处理友好，async/await 可以⽤成熟的 try/catch，而 Promise 的错误捕获⾮常冗余
调试友好，Promise 的调试很差，由于没有代码块，你不能在⼀个返回表达式的箭头函数中设置断点，如果你在⼀个.then 代码块中使⽤调试器的步进(step-over)功能，调试器并不会进⼊后续的.then 代码块，因为调试器只能跟踪同步代码的每⼀步。

### 34. Proxy 可以实现什么功能？
在 Vue3.0 中通过 Proxy 来替换原本的 Object.defineProperty 来实现数据响应式。
Proxy 是 ES6 中新增的功能，它可以用来自定义对象中的操作。
```
let p = new Proxy(target, handler)
```
代表需要添加代理的对象，handler 用来自定义对象中的操作，比如可以用来自定义 set 或者 get 函数。
下面来通过 Proxy 来实现一个数据响应式：
```
let onWatch = (obj, setBind, getLogger) => {
    let handler = {
        get(target, property, receiver) {
            getLogger(target, property);
            return Reflect.get(target, property, receiver);
        },
        set(target, property, value, receiver) {
            setBind(value, property);
            return Reflect.set(target, property, value);
        }
    }
    return new Proxy(obj, handler);
}
let obj = { a: 1 };
let p = onWatch(
    obj, 
    (v, property) => {
        console.log(`监听到属性${property}改变为${v}`);
    },
    (target, property) => {
        console.log(`'${property}' = ${target[property]}`);
    }
)
p.a = 2;
p.a // 'a' = 2
```
在上述代码中，通过自定义 set 和 get 函数的方式，在原本的逻辑中插入了我们的函数逻辑，实现了在对对象任何属性进行读写时发出通知。
当然这是简单版的响应式实现，如果需要实现一个 Vue 中的响应式，需要在 get 中收集依赖，在 set 派发更新，之所以 Vue3.0 要使用 Proxy 替换原本的 API 原因在于 Proxy 无需一层层递归为每个属性添加代理，一次即可完成以上操作，性能上更好，并且原本的实现有一些数据更新不能监听到，但是 Proxy 可以完美监听到任何方式的数据改变，唯一缺陷就是浏览器的兼容性不好。

### 35. 什么是尾调用，使用尾调用有什么好处？
尾调用指的是函数的最后一步调用另一个函数。代码执行是基于执行栈的，所以当在一个函数里调用另一个函数时，会保留当前的执行上下文，然后再新建另外一个执行上下文加入栈中。使用尾调用的话，因为已经是函数的最后一步，所以这时可以不必再保留当前的执行上下文，从而节省了内存，这就是尾调用优化。但是 ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。

### 36. 异步编程的实现方式
JavaScript 中的异步机制可以分为以下几种：
**回调函数** 的方式，使用回调函数的方式有一个缺点是，多个回调函数嵌套的时候会造成回调函数地狱，上下两层的回调函数间的代码耦合度太高，不利于代码的可维护。
**Promise** 的方式，使用 Promise 的方式可以将嵌套的回调函数作为链式调用。但是使用这种方法，有时会造成多个 then 的链式调用，可能会造成代码的语义不够明确。
**generator** 的方式，它可以在函数的执行过程中，将函数的执行权转移出去，在函数外部还可以将执行权转移回来。当遇到异步函数执行的时候，将函数执行权转移出去，当异步函数执行完毕时再将执行权给转移回来。因此在 generator 内部对于异步操作的方式，可以以同步的顺序来书写。使用这种方式需要考虑的问题是何时将函数的控制权转移回来，因此需要有一个自动执行 generator 的机制，比如说 co 模块等方式来实现 generator 的自动执行。
**async 函数** 的方式，async 函数是 generator 和 promise 实现的一个自动执行的语法糖，它内部自带执行器，当函数内部执行到一个 await 语句的时候，如果语句返回一个 promise 对象，那么函数将会等待 promise 对象的状态变为 resolve 后再继续向下执行。因此可以将异步逻辑，转化为同步的顺序来书写，并且这个函数可以自动执行。

### 37. 对 JSON 的理解
JSON 是一种基于文本的轻量级的数据交换格式。它可以被任何的编程语言读取和作为数据格式来传递。
在项目开发中，使用 JSON 作为前后端数据交换的方式。在前端通过将一个符合 JSON 格式的数据结构序列化为 JSON 字符串，然后将它传递到后端，后端通过 JSON 格式的字符串
解析后生成对应的数据结构，以此来实现前后端数据的一个传递。
因为 JSON 的语法是基于 js 的，因此很容易将 JSON 和 js 中的对象弄混，但是应该注意的是 JSON 和 js 中的对象不是一回事，JSON 中对象格式更加严格，比如说在 JSON 中属性值不能为函数，不能出现 NaN 这样的属性值等，因此大多数的 js 对象是不符合 JSON 对象的格式的。
在 js 中提供了两个函数来实现 js 数据结构和 JSON 格式的转换处理，JSON.stringify 函数，通过传入一个符合 JSON 格式的数据结构，将其转换为一个 JSON 字符串。如果传入的数据结构不符合 JSON 格式，那么在序列化的时候会对这些值进行对应的特殊处理，使其符合规范。在前端向后端发送数据时，可以调用这个函数将数据对象转化为 JSON 格式的字符串。
JSON.parse() 函数，这个函数用来将 JSON 格式的字符串转换为一个 js 数据结构，如果传入的字符串不是标准的 JSON 格式的字符串的话，将会抛出错误。当从后端接收到 JSON 格式的字符串时，可以通过这个方法来将其解析为一个 js 数据结构，以此来进行数据的访问。

### 38. 常用的正则表达式有哪些？
```
// （1）匹配 16 进制颜色值
var regex = /#([0-9a-fA-F]{6}|[0-9a-fA-F]{3})/g;
// （2）匹配日期，如 yyyy-mm-dd 格式
var regex = /^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01])$/;
// （3）匹配 QQ 号
var regex = /^[1-9][0-9]{4-10}$/g;
// （4）匹配 手机号码
var regex = /^1[34578]\d{9}$/g;
// （5）匹配 4-16 位账号
var regex = /^[a-zA-Z\$][a-zA-Z0-9_\$]{4,16}$/;
```

### 39. 什么是 DOM 和 BOM？
DOM 指的是文档对象模型，它指的是把文档当做一个对象，这个对象主要定义了处理网页内容的方法和接口。
BOM 指的是浏览器对象模型，它指的是把浏览器当做一个对象来对待，这个对象主要定义了与浏览器进行交互的法和接口。BOM 的核心是 window，而 window 对象具有双重角色，它既是通过 js 访问浏览器窗口的一个接口，又是一个 Global（全局）对象。这意味着在网页中定义的任何对象，变量和函数，都作为全局对象的一个属性或者方法存在。window 对象含有 location 对象、navigator 对象、screen 对象等子对象，并且 DOM 的最根本的对象 document 对象也是 BOM 的 window 对象的子对象。

### 40. escape、encodeURI、encodeURIComponent 的区别
encodeURI 是对整个 URI 进行转义，将 URI 中的非法字符转换为合法字符，所以对于一些在 URI 中有特殊意义的字符不会进行转义。
encodeURIComponent 是对 URI 的组成部分进行转义，所以一些特殊字符也会得到转义。
escape 和 encodeURI 的作用相同，不过它们对于 unicode 编码为 0xff 之外字符的时候会有区别，escape 是直接在字符的 unicode 编码前加上 %u，而 encodeURI 首先会将字符转换为 UTF-8 的格式，再在每个字节前加上 %。
