# *JavaScript* 面试题汇总

### 1. 根据下面 *ES6* 构造函数的书写方式，要求写出 *ES5* 的

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
> function Example(name) {
>       'use strict';
>       if (!new.target) {
>            throw new TypeError('Class constructor cannot be invoked without new');
>       }
>       this.name = name;
> }
> 
> Object.defineProperty(Example.prototype, 'init', {
>       enumerable: false,
>       value: function () {
>            'use strict';
>            if (new.target) {
>                throw new TypeError('init is not a constructor');
>            }
>            var fun = function () {
>                console.log(this.name);
>            }
>            fun.call(this);
>       }
> })
> ```
>

> 解析：
>
> 此题的关键在于是否清楚 *ES6* 的 *class* 和普通构造函数的区别，记住它们有以下区别，就不会有遗漏：
> 
>   1. *ES6* 中的 *class* 必须通过 *new* 来调用，不能当做普通函数调用，否则报错
>   
>       因此，在答案中，加入了 *new.target* 来判断调用方式
>   
>   2. *ES6* 的 *class* 中的所有代码均处于严格模式之下
> 
>    因此，在答案中，无论是构造函数本身，还是原型方法，都使用了严格模式
> 
>   3. *ES6* 中的原型方法是不可被枚举的
>   
>       因此，在答案中，定义原型方法使用了属性描述符，让其不可枚举
>    
>    4. 原型上的方法不允许通过 *new* 来调用
>    
>       因此，在答案中，原型方法中加入了 *new.target* 来判断调用方式



### 2. 数组去重有哪些方法？（美团 *19* 年）

> 参考答案：
>
> ```js
> // 数字或字符串数组去重，效率高
> function unique(arr) {
>       var result = {}; // 利用对象属性名的唯一性来保证不重复
>       for (var i = 0; i < arr.length; i++) {
>            if (!result[arr[i]]) {
>                result[arr[i]] = true;
>            }
>       }
>       return Object.keys(result); // 获取对象所有属性名的数组
> }
> 
> // 任意数组去重，适配范围广，效率低
> function unique(arr) {
>       var result = []; // 结果数组
>       for (var i = 0; i < arr.length; i++) {
>            if (!result.includes(arr[i])) {
>                result.push(arr[i]);
>            }
>       }
>       return result;
> }
> 
> // 利用ES6的Set去重，适配范围广，效率一般，书写简单
> function unique(arr) {
>       return [...new Set(arr)]
> }
> ```



### 3. 描述下列代码的执行结果

```js
foo(typeof a);
function foo(p) {
    console.log(this);
    console.log(p);
    console.log(typeof b);
    let b = 0;
}
```

> 参考答案：
>
> 报错，报错的位置在 `console.log(typeof b);`
>
> 报错原因：*ReferenceError: Cannot access 'b' before initialization*

> 解析：
>
> 这道题考查的是 *ES6* 新增的声明变量关键字 *let* 以及暂时性死区的知识。*let* 和以前的 *var* 关键字不一样，无法在 *let* 声明变量之前访问到该变量，所以在 *typeof b* 的地方就会报错。



### 4. 描述下列代码的执行结果

```js
class Foo {
    constructor(arr) { 
        this.arr = arr; 
    }
    bar(n) {
        return this.arr.slice(0, n);
    }
}
var f = new Foo([0, 1, 2, 3]);
console.log(f.bar(1));
console.log(f.bar(2).splice(1, 1));
console.log(f.arr);
```

> 参考答案：
>
> [ 0 ]
> [ 1 ]
> [ 0, 1, 2, 3 ]

> 解析：
>
> 主要考察的是数组相关的知识。 *f* 对象上面有一个属性 *arr*，*arr* 的值在初始化的时候会被初始化为 *[0, 1, 2, 3]*，之后就完全是考察数组以及数组方法的使用了。



### 5. 描述下列代码的执行结果

```js
01 function f(count) {
02    console.log(`foo${count}`);
03    setTimeout(() => { console.log(`bar${count}`); });
04 }
05 f(1);
06 f(2);
07 setTimeout(() => { f(3); });
```

> 参考答案：
>
> foo1
> foo2
> bar1
> bar2
> foo3
> bar3

> 解析：
>
> 这个完全是考察的异步的知识。调用 *f(1)* 的时候，会执行同步代码，打印出 *foo1*，然后 *03* 行的 *setTimeout* 被放入到异步执行队列，接下来调用 *f(2)* 的时候，打印出 *foo2*，后面 *03* 行的 *setTimeout* 又被放入到异步执行队列。然后执行 *07* 行的语句，被放入到异步执行队列。至此，所有同步代码就都执行完毕了。
>
> 接下来开始执行异步代码，那么大家时间没写，就都是相同的，所以谁先被放入到异步队列，谁就先执行，所以先打印出 *bar1*、然后是 *bar2*，接下来执行之前 *07* 行放入到异步队列里面的 *setTimeout*，先执行 *f* 函数里面的同步代码，打印出 *foo3*，然后是最后一个异步，打印出 *bar3*



### 6. 描述下列代码的执行结果

```js
var a = 2;
var b = 5;
console.log(a === 2 || 1 && b === 3 || 4);
```

> 参考答案：
>
> *true*
>
> 考察的是逻辑运算符。在 || 里面，只要有一个为真，后面的直接短路，都不用去计算。所以 *a === 2* 得到 *true* 之后直接短路了，返回 *true*。



### 7. 描述下列代码的执行结果

```js
export class ButtonWrapper {
    constructor(domBtnEl, hash) {
        this.domBtnEl = domBtnEl;
        this.hash = hash;
        this.bindEvent();
    }
    bindEvent() {
        this.domBtnEl.addEventListener('click', this.clickEvent, false);
    }
    detachEvent() {
        this.domBtnEl.removeEventListener('click', this.clickEvent);
    }
    clickEvent() {
        console.log(`The hash of the button is: ${this.hash}`);
    }
}
```

> 参考答案：
>
> 上面的代码导出了一个 *ButtonWrapper* 类，该类在被实例化的时候，实例化对象上面有两个属性，分别是 *domBtnEl* 和 *hash*，*domBtnEl* 是一个 *DOM* 节点，之后为这个 *domBtnEl* 绑定了点击事件，点击后打印出 *The hash of the button is: hash* 那句话。*detachEvent* 是移除点击事件，当调用实例化对象的 *detachEvent* 方法时，点击事件就会被移除。



### 8. 箭头函数有哪些特点

> 参考答案：
>
> 1. 更简洁的语法，例如
>    - 只有一个形参就不需要用括号括起来
>    - 如果函数体只有一行，就不需要放到一个块中
>    - 如果 *return* 语句是函数体内唯一的语句，就不需要 *return* 关键字
> 2. 箭头函数没有自己的 *this*，*arguments*，*super*
> 3. 箭头函数 *this* 只会从自己的作用域链的上一层继承 *this*。



### 9. 说一说类的继承

> 参考答案：
>
> 继承是面向对象编程中的三大特性之一。
>
> *JavaScript* 中的继承经过不断的发展，从最初的对象冒充慢慢发展到了今天的圣杯模式继承。
>
> 其中最需要掌握的就是**伪经典继承**和**圣杯模式**的继承。
>
> 很长一段时间，JS 继承使用的都是**组合继承**。这种继承也被称之为伪经典继承，该继承方式综合了原型链和盗用构造函数的方式，将两者的优点集中了起来。
>
> 组合继承弥补了之前原型链和盗用构造函数这两种方式各自的不足，是 *JavaScript* 中使用最多的继承方式。
>
> 组合继承最大的问题就是效率问题。最主要就是父类的构造函数始终会被调用两次：一次是在创建子类原型时调用，另一次是在子类构造函数中调用。
>
> 本质上，子类原型最终是要包含超类对象的所有实例属性，子类构造函数只要在执行时重写自己的原型就行了。
>
> 圣杯模式的继承解决了这一问题，其基本思路就是不通过调用父类构造函数来给子类原型赋值，而是取得父类原型的一个副本，然后将返回的新对象赋值给子类原型。

> 解析：该题主要考察就是对 *js* 中的继承是否了解，以及常见的继承的形式有哪些。最常用的继承就是**组合继承**（伪经典继承）和圣杯模式继承。下面附上 *js* 中这两种继承模式的详细解析。
>
> 下面是一个组合继承的例子：
>
> ```js
> // 基类
> var Person = function (name, age) {
>     this.name = name;
>     this.age = age;
> }
> Person.prototype.test = "this is a test";
> Person.prototype.testFunc = function () {
>     console.log('this is a testFunc');
> }
> 
> // 子类
> var Student = function (name, age, gender, score) {
>     Person.apply(this, [name, age]); // 盗用构造函数
>     this.gender = gender;
>     this.score = score;
> }
> Student.prototype = new Person(); // 改变 Student 构造函数的原型对象
> Student.prototype.testStuFunc = function () {
>     console.log('this is a testStuFunc');
> }
> 
> // 测试
> var zhangsan = new Student("张三", 18, "男", 100);
> console.log(zhangsan.name); // 张三
> console.log(zhangsan.age); // 18
> console.log(zhangsan.gender); // 男
> console.log(zhangsan.score); // 100
> console.log(zhangsan.test); // this is a test
> zhangsan.testFunc(); // this is a testFunc
> zhangsan.testStuFunc(); // this is a testStuFunc
> ```
>
>  
>
> 在上面的例子中，我们使用了组合继承的方式来实现继承，可以看到无论是基类上面的属性和方法，还是子类自己的属性和方法，都得到了很好的实现。
>
> 
>
> 但是在组合继承中存在效率问题，比如在上面的代码中，我们其实调用了两次 *Person*，产生了两组 *name* 和 *age* 属性，一组在原型上，一组在实例上。
>
> 
>
> 也就是说，我们在执行 *Student.prototype = new Person( )* 的时候，我们是想要 *Person* 原型上面的方法，属性是不需要的，因为属性之后可以通过 *Person.apply(this, [name, age])* 拿到，但是当你 *new Person( )* 的时候，会实例化一个 *Person* 对象出来，这个对象上面，属性和方法都有。
>
> 
>
> 圣杯模式的继承解决了这一问题，其基本思路就是不通过调用父类构造函数来给子类原型赋值，而是取得父类原型的一个副本，然后将返回的新对象赋值给子类原型。
>
> 
>
> 下面是一个圣杯模式的示例：
>
> 
>
> ```js
> // target 是子类，origin 是基类
> // target ---> Student, origin ---> Person
> function inherit(target, origin) {
>     function F() { }; // 没有任何多余的属性
> 
>     // origin.prototype === Person.prototype, origin.prototype.constructor === Person 构造函数
>     F.prototype = origin.prototype;
> 
>     // 假设 new F() 出来的对象叫小 f
>     // 那么这个 f 的原型对象 === F.prototype === Person.prototype
>     // 那么 f.constructor === Person.prototype.constructor === Person 的构造函数
>     target.prototype = new F();
> 
>     // 而 f 这个对象又是 target 对象的原型对象
>     // 这意味着 target.prototype.constructor === f.constructor
>     // 所以 target 的 constructor 会指向 Person 构造函数
> 
>     // 我们要让子类的 constructor 重新指向自己
>     // 若不修改则会发现 constructor 指向的是父类的构造函数
>     target.prototype.constructor = target;
> }
> 
> 
> // 基类
> var Person = function (name, age) {
>     this.name = name;
>     this.age = age;
> }
> Person.prototype.test = "this is a test";
> Person.prototype.testFunc = function () {
>     console.log('this is a testFunc');
> }
> 
> 
> // 子类
> var Student = function (name, age, gender, score) {
>     Person.apply(this, [name, age]);
>     this.gender = gender;
>     this.score = score;
> }
> inherit(Student, Person); // 使用圣杯模式实现继承
> // 在子类上面添加方法
> Student.prototype.testStuFunc = function () {
>     console.log('this is a testStuFunc');
> }
> 
> // 测试
> var zhangsan = new Student("张三", 18, "男", 100);
> 
> console.log(zhangsan.name); // 张三
> console.log(zhangsan.age); // 18
> console.log(zhangsan.gender); // 男
> console.log(zhangsan.score); // 100
> console.log(zhangsan.test); // this is a test
> zhangsan.testFunc(); // this is a testFunc
> zhangsan.testStuFunc(); // this is a testStuFunc
> ```
>
> 在上面的代码中，我们在 *inherit* 方法中创建了一个中间层，之后让 *F* 的原型和父类的原型指向同一地址，再让子类的原型指向这个 *F* 的实例化对象来实现了继承。
>
> 
>
> 这样我们的继承，属性就不会像之前那样实例对象上一份，原型对象上一份，拥有两份。圣杯模式继承是目前 *js* 继承的最优解。
>
> 
>
> 最后我再画个图帮助大家理解，如下图：
>
> 
>
> 组合模式（伪经典模式）下的继承示意图：
>
> <img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-08-08-022111.png" alt="image-20210808102111003" style="zoom:50%;" />
>
> 圣杯模式下的继承示意图：
>
> 
>
> <img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-08-08-021304.png" alt="image-20210808101303180" style="zoom:50%;" />
>
> 



### 10. *new* 操作符都做了哪些事？

> 参考答案：
>
> *new* 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。
>
> *new* 关键字会进行如下的操作：
> 步骤 *1*：创建一个空的简单 *JavaScript* 对象，即 { } ;
> 步骤 *2*：链接该对象到另一个对象（即设置该对象的原型对象）；
> 步骤 *3*：将步骤 *1* 新创建的对象作为 *this* 的上下文；
> 步骤 *4*：如果该函数没有返回对象，则返回 *this*。



### 11. *call、apply、bind* 的区别 ？

> 参考答案：
>
> *call* 和 *apply* 的功能相同，区别在于传参的方式不一样:
>
> - *fn.call(obj, arg1, arg2, ...)* 调用一个函数, 具有一个指定的 *this* 值和分别地提供的参数(参数的列表)。
> - *fn.apply(obj, [argsArray])* 调用一个函数，具有一个指定的 *this* 值，以及作为一个数组（或类数组对象）提供的参数。
>
> *bind* 和 *call/apply* 有一个很重要的区别，一个函数被 *call/apply* 的时候，会直接调用，但是 *bind* 会创建一个新函数。当这个新函数被调用时，*bind( )* 的第一个参数将作为它运行时的 *this*，之后的一序列参数将会在传递的实参前传入作为它的参数。



### 12. 事件循环机制（宏任务、微任务）

> 参考答案：
>
> 在 *js* 中任务会分为同步任务和异步任务。
>
> 如果是同步任务，则会在主线程（也就是 *js* 引擎线程）上进行执行，形成一个执行栈。但是一旦遇到异步任务，则会将这些异步任务交给异步模块去处理，然后主线程继续执行后面的同步代码。
>
> 当异步任务有了运行结果以后，就会在任务队列里面放置一个事件，这个任务队列由事件触发线程来进行管理。
>
> 一旦执行栈中所有的同步任务执行完毕，就代表着当前的主线程（*js* 引擎线程）空闲了，系统就会读取任务队列，将可以运行的异步任务添加到执行栈中，开始执行。
>
> 在 *js* 中，任务队列中的任务又可以被分为 *2* 种类型：宏任务（*macrotask*）与微任务（*microtask*）
>
> 宏任务可以理解为每次执行栈所执行的代码就是一个宏任务，包括每次从事件队列中获取一个事件回调并放到执行栈中所执行的任务。
>
> 微任务可以理解为当前宏任务执行结束后立即执行的任务。



### 13. 你了解 *node* 中的事件循环机制吗？*node11* 版本以后有什么改变

> 参考答案：
>
> *Node.js* 在主线程里维护了一个**事件队列，**当接到请求后，就将该请求作为一个事件放入这个队列中，然后继续接收其他请求。当主线程空闲时（没有请求接入时），就开始循环事件队列，检查队列中是否有要处理的事件，这时要分两种情况：如果是非 *I/O* 任务，就亲自处理，并通过回调函数返回到上层调用；如果是 *I/O* 任务，就从**线程池**中拿出一个线程来处理这个事件，并指定回调函数，然后继续循环队列中的其他事件。
>
> 当线程中的 *I/O* 任务完成以后，就执行指定的回调函数，并把这个完成的事件放到事件队列的尾部，等待事件循环，当主线程再次循环到该事件时，就直接处理并返回给上层调用。 这个过程就叫 **事件循环** (*Event Loop*)。
>
> 无论是 *Linux* 平台还是 *Windows* 平台，*Node.js* 内部都是通过**线程池**来完成异步 *I/O* 操作的，而 *LIBUV* 针对不同平台的差异性实现了统一调用。因此，***Node.js* 的单线程仅仅是指 *JavaScript* 运行在单线程中，而并非 *Node.js* 是单线程。**
>
> *Node.JS* 的事件循环分为 *6* 个阶段：
>
> - *timers* 阶段：这个阶段执行 *timer*（ *setTimeout、setInterval* ）的回调
> - *I/O callbacks* 阶段：处理一些上一轮循环中的少数未执行的 *I/O* 回调
> - *idle、prepare* 阶段：仅 *Node.js* 内部使用
> - *poll* 阶段：获取新的 *I/O* 事件, 适当的条件下 *Node.js* 将阻塞在这里
> - *check* 阶段：执行 *setImmediate( )* 的回调
> - *close callbacks* 阶段：执行 *socket* 的 *close* 事件回调
>
> 事件循环的执行顺序为：
>
> 外部输入数据 –-> 轮询阶段（ *poll* ）-–> 检查阶段（ *check* ）-–> 关闭事件回调阶段（ *close callback* ）–-> 定时器检测阶段（ *timer* ）–-> *I/O* 事件回调阶段（ *I/O callbacks* ）-–>闲置阶段（ *idle、prepare* ）–->轮询阶段（按照该顺序反复运行）...
>
> 浏览器和 *Node.js* 环境下，微任务任务队列的执行时机不同
>
> - *Node.js* 端，微任务在事件循环的各个阶段之间执行
> - 浏览器端，微任务在事件循环的宏任务执行完之后执行
>
> *Node.js v11.0.0* 版本于 *2018* 年 *10* 月，主要有以下变化：
>
> 1. *V8* 引擎更新至版本 *7.0*
> 2. *http、https* 和 *tls* 模块默认使用 *WHESWG URL* 解析器。
> 3. 隐藏子进程的控制台窗口默认改为了 *true*。
> 4. *FreeBSD 10*不再支持。
> 5. 增加了多线程 *Worker Threads*



### 14. 什么是函数柯里化？

> 参考答案：
>
> 柯里化（*currying*）又称部分求值。一个柯里化的函数首先会接受一些参数，接受了这些参数之后，该函数并不会立即求值，而是继续返回另外一个函数，刚才传入的参数在函数形成的闭包中被保存起来。待到函数被真正需要求值的时候，之前传入的所有参数都会被一次性用于求值。
>
> 举个例子，就是把原本：
>
> *function(arg1,arg2)* 变成 *function(arg1)(arg2)*
> *function(arg1,arg2,arg3)* 变成 *function(arg1)(arg2)(arg3)*
> *function(arg1,arg2,arg3,arg4)* 变成 *function(arg1)(arg2)(arg3)(arg4)*
>
> 总而言之，就是将：
>
> *function(arg1,arg2,…,argn)* 变成 *function(arg1)(arg2)…(argn)*



### 15. *promise.all* 方法的使用场景？数组中必须每一项都是 *promise* 对象吗？不是 *promise* 对象会如何处理 ？

> 参考答案：
>
> ***promise.all(promiseArray)*** 方法是 *promise* 对象上的静态方法，该方法的作用是将多个 *promise* 对象实例包装，生成并返回一个新的 *promise* 实例。
>
> 此方法在集合多个 *promise* 的返回结果时很有用。
>
> 返回值将会按照参数内的 *promise* 顺序排列，而不是由调用 *promise* 的完成顺序决定。
>
> ***promise.all* 的特点**
>
> 接收一个*Promise*实例的数组或具有*Iterator*接口的对象
>
> 如果元素不是*Promise*对象，则使用*Promise.resolve*转成*Promise*对象
>
> 如果全部成功，状态变为*resolved*，返回值将组成一个数组传给回调
>
> 只有有一个失败，状态就变为 *rejected*，返回值将直接传递给回调  *all( )*的返回值，也是新的 *promise* 对象



### 16. *this* 的指向哪几种 ？

> 参考答案：
>
> 总结起来，*this* 的指向规律有如下几条：
>
> - 在函数体中，非显式或隐式地简单调用函数时，在严格模式下，函数内的 *this* 会被绑定到 *undefined* 上，在非严格模式下则会被绑定到全局对象 *window/global* 上。
> - 一般使用 *new* 方法调用构造函数时，构造函数内的 *this* 会被绑定到新创建的对象上。
> - 一般通过 *call/apply/bind* 方法显式调用函数时，函数体内的 *this* 会被绑定到指定参数的对象上。
> - 一般通过上下文对象调用函数时，函数体内的 *this* 会被绑定到该对象上。
> - 在箭头函数中，*this* 的指向是由外层（函数或全局）作用域来决定的。



### 17. *JS* 中继承实现的几种方式

> 参考答案：
>
> *JS* 的继承随着语言的发展，从最早的对象冒充到现在的圣杯模式，涌现出了很多不同的继承方式。每一种新的继承方式都是对前一种继承方式不足的一种补充。
>
> 1. 原型链继承
>
> - 重点：让新实例的原型等于父类的实例。
> - 特点：实例可继承的属性有：实例的构造函数的属性，父类构造函数属性，父类原型的属性。（新实例不会继承父类实例的属性！）
> - 缺点：
>   - 1、新实例无法向父类构造函数传参。
>   - 2、继承单一。
>   - 3、所有新实例都会共享父类实例的属性。（原型上的属性是共享的，一个实例修改了原型属性，另一个实例的原型属性也会被修改！）
>
> 2. 借用构造函数继承
>
> - 重点：用 *call( )* 和 *apply( )* 将父类构造函数引入子类函数（在子类函数中做了父类函数的自执行（复制））
> - 特点：
>   　　　- 1、只继承了父类构造函数的属性，没有继承父类原型的属性。
>      - 2、解决了原型链继承缺点1、2、3。
>      - 3、可以继承多个构造函数属性（call多个）。
>      - 4、在子实例中可向父实例传参。
> - 缺点：
>   　　　- 1、只能继承父类构造函数的属性。
>      - 2、无法实现构造函数的复用。（每次用每次都要重新调用）
>      - 3、每个新实例都有父类构造函数的副本，臃肿。
>
> 3. 组合模式（又被称之为伪经典模式）
>
> - 重点：结合了两种模式的优点，传参和复用
> - 特点：
>   　　　- 1、可以继承父类原型上的属性，可以传参，可复用。
>         　　　- 2、每个新实例引入的构造函数属性是私有的。
> - 缺点：调用了两次父类构造函数（耗内存），子类的构造函数会代替原型上的那个父类构造函数。
>
> 4. 寄生组合式继承（圣杯模式）
>
> - 重点：修复了组合继承的问题



### 18. 什么是事件监听

> 参考答案：
>
> 首先需要区别清楚事件监听和事件监听器。
>
> 在绑定事件的时候，我们需要对应的书写一个事件处理程序，来应对事件发生时的具体行为。
>
> 这个事件处理程序我们也称之为事件监听器。
>
> 当事件绑定好后，程序就会对事件进行监听，当用户触发事件时，就会执行对应的事件处理程序。
>
> 关于事件监听，*W3C* 规范中定义了 *3* 个事件阶段，依次是捕获阶段、目标阶段、冒泡阶段。
>
> - **捕获**阶段：在事件对象到达事件目标之前，事件对象必须从 *window* 经过目标的祖先节点传播到事件目标。 这个阶段被我们称之为捕获阶段。在这个阶段注册的事件监听器在事件到达其目标前必须先处理事件。
>
> - **目标** 阶段：事件对象到达其事件目标。 这个阶段被我们称为目标阶段。一旦事件对象到达事件目标，该阶段的事件监听器就要对它进行处理。如果一个事件对象类型被标志为不能冒泡。那么对应的事件对象在到达此阶段时就会终止传播。
> - **冒泡** 阶段：事件对象以一个与捕获阶段相反的方向从事件目标传播经过其祖先节点传播到 *window*。这个阶段被称之为冒泡阶段。在此阶段注册的事件监听器会对相应的冒泡事件进行处理。



### 19. 什么是 *js* 的闭包？有什么作用？

> 参考答案：
>
> 一个函数和对其周围状态（*lexical environment*，词法环境）的引用捆绑在一起（或者说函数被引用包围），这样的组合就是**闭包**（*closure*）。也就是说，闭包让你可以在一个内层函数中访问到其外层函数的作用域。在 *JavaScript* 中，每当创建一个函数，闭包就会在函数创建的同时被创建出来。
>
> 闭包的用处：
>
> 1. 匿名自执行函数
> 2. 结果缓存
> 3. 封装
> 4. 实现类和继承



### 20. 事件委托以及冒泡原理

> 参考答案：
>
> 事件委托，又被称之为事件代理。在 *JavaScript* 中，添加到页面上的事件处理程序数量将直接关系到页面整体的运行性能。导致这一问题的原因是多方面的。
>
> 首先，每个函数都是对象，都会占用内存。内存中的对象越多，性能就越差。其次，必须事先指定所有事件处理程序而导致的 *DOM* 访问次数，会延迟整个页面的交互就绪时间。
>
> 对事件处理程序过多问题的解决方案就是事件委托。
>
> 事件委托利用了事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。例如，*click* 事件会一直冒泡到 *document* 层次。也就是说，我们可以为整个页面指定一个 *onclick* 事件处理程序，而不必给每个可单击的元素分别添加事件处理程序。
>
> 事件冒泡（*event bubbling*），是指事件开始时由最具体的元素（文档中嵌套层次最深的那个节点）接收，然后逐级向上传播到较为不具体的节点（文档）。



### 21. *let const var* 的区别？什么是块级作用域？如何用？ 

> 参考答案：
>
> 1. *var* 定义的变量，没有块的概念，可以跨块访问, 不能跨函数访问，有变量提升。
> 2. *let* 定义的变量，只能在块作用域里访问，不能跨块访问，也不能跨函数访问，无变量提升，不可以重复声明。
> 3. *const* 用来定义常量，使用时必须初始化(即必须赋值)，只能在块作用域里访问，而且不能修改，无变量提升，不可以重复声明。
>
> 最初在 *JS* 中作用域有：全局作用域、函数作用域。没有块作用域的概念。
>
> *ES6* 中新增了块级作用域。块作用域由 { } 包括，*if* 语句和 *for* 语句里面的 { } 也属于块作用域。
>
> 在以前没有块作用域的时候，在 if 或者 for 循环中声明的变量会泄露成全局变量，其次就是 { } 中的内层变量可能会覆盖外层变量。块级作用域的出现解决了这些问题。



### 22. *ES5* 的方法实现块级作用域（立即执行函数） *ES6* 呢？ 

> 参考答案：
>
> *ES6* 原生支持块级作用域。块作用域由 { } 包括，*if* 语句和 *for* 语句里面的 { } 也属于块作用域。
>
> 使用 *let* 声明的变量或者使用 *const* 声明的常量，只能在块作用域里访问，不能跨块访问。



### 23. *ES6* 箭头函数的特性 

> 参考答案：
>
> 1. 更简洁的语法，例如
>    - 只有一个形参就不需要用括号括起来
>    - 如果函数体只有一行，就不需要放到一个块中
>    - 如果 *return* 语句是函数体内唯一的语句，就不需要 *return* 关键字
> 2. 箭头函数没有自己的 *this*，*arguments*，*super*
> 3. 箭头函数 *this* 只会从自己的作用域链的上一层继承 *this*。



### 24. 箭头函数与普通函数的区别 ？

> 参考答案：
>
> 1. 外形不同。箭头函数使用箭头定义，普通函数中没有
>
> 2. 普通函数可以有匿名函数，也可以有具体名函数，但是箭头函数都是匿名函数。
>
> 3. **箭头函数不能用于构造函数，不能使用 *new*，**普通函数可以用于构造函数，以此创建对象实例。
>
> 4. **箭头函数中 *this* 的指向不同，**在普通函数中，*this* 总是指向调用它的对象，如果用作构造函数，*this* 指向创建的对象实例。
>    箭头函数本身不创建 *this*，也可以说箭头函数本身没有 *this*，但是它在声明时可以捕获其所在上下文的 *this* 供自己使用。
>
> 5. 每一个普通函数调用后都具有一个 *arguments* 对象，用来存储实际传递的参数。
>
>    但是箭头函数并没有此对象。**取而代之用rest参数来解决**。
>
> 6. 箭头函数不能用于 *Generator* 函数，不能使用 *yeild* 关键字。
>
> 7. 箭头函数不具有 *prototype* 原型对象。而普通函数具有 *prototype* 原型对象。
>
> 8. 箭头函数不具有 *super*。
>
> 9. 箭头函数不具有 *new.target*。



### 25. *JS* 的基本数据类型有哪些？基本数据类型和引用数据类型的区别

> 参考答案：
>
> 在 *JavaScript* 中，数据类型整体上来讲可以分为两大类：**基本类型**和**引用数据类型**
>
> 基本数据类型，一共有 *8* 种：
>
> ```text
> string，symbol，number，boolean，undefined，null，bigInt
> ```
>
> 其中 *symbol*、*bigInt* 类型是在 *ES6* 及后续版本里面新添加的基本数据类型。
>
> 引用数据类型，就只有 *1* 种：
>
> ```js
> object
> ```
>
> 基本数据类型的值又被称之为原始值或简单值，而引用数据类型的值又被称之为复杂值或引用值。
>
> 两者的区别在于：
>
> **原始值是表示 *JavaScript* 中可用的数据或信息的最底层形式或最简单形式。**简单类型的值被称为原始值，是因为它们是**不可细化**的。
>
> 也就是说，数字是数字，字符是字符，布尔值是 *true* 或 *false*，*null* 和 *undefined* 就是 *null* 和 *undefined*。这些值本身很简单，不能够再进行拆分。由于原始值的数据大小是固定的，所以**原始值的数据是存储于内存中的栈区里面的。**
>
> 在 *JavaScript* 中，对象就是一个引用值。因为对象可以向下拆分，拆分成多个简单值或者复杂值。**引用值在内存中的大小是未知的，因为引用值可以包含任何值，而不是一个特定的已知值，所以引用值的数据都是存储于堆区里面。**
>
> 最后总结一下两者的区别：
>
> 1. 访问方式
>    - 原始值：访问到的是值
>    - 引用值：访问到的是引用地址
> 2. 比较方式
>    - 原始值：比较的是值
>    - 引用值：比较的是地址
>
> 3. 动态属性
>    - 原始值：无法添加动态属性
>    - 引用值：可以添加动态属性
> 4. 变量赋值
>    - 原始值：赋值的是值
>    - 引用值：赋值的是地址



### 26. *NaN* 是什么的缩写

> 参考答案：
>
> *NaN* 的全称为 *Not a Number*，表示非数，或者说不是一个数。虽然 NaN 表示非数，但是它却属于 *number* 类型。
>
> *NaN* 有两个特点：
>
> 1. 任何涉及 *NaN* 的操作都会返回 *NaN*
> 2. *NaN* 和任何值都不相等，包括它自己本身



### 27. *JS* 的作用域类型

> 参考答案：
>
> 在 *JavaScript* 里面，作用域一共有 4 种：全局作用域，局部作用域、函数作用域以及 *eval* 作用域。
>
> **全局作用域：**这个是默认的代码运行环境，一旦代码被载入，引擎最先进入的就是这个环境。
>
> **局部作用域：**当使用 *let* 或者 *const* 声明变量时，这些变量在一对花括号中存在局部作用域，只能够在花括号内部进行访问使用。
>
> **函数作用域：**当进入到一个函数的时候，就会产生一个函数作用域。函数作用域里面所声明的变量只在函数中提供访问使用。
>
> ***eval* 作用域：**当调用 *eval( )* 函数的时候，就会产生一个 *eval* 作用域。



### 28. *undefined==null* 返回的结果是什么？*undefined* 与 *null* 的区别在哪？

> 参考答案：
>
> 返回 *true*。
>
> 这两个值都表示“无”的意思。
>
> 通常情况下， 当我们试图访问某个不存在的或者没有赋值的变量时，就会得到一个 *undefined* 值。*Javascript* 会自动将声明是没有进行初始化的变量设为 *undifined*。
>
> 而 *null* 值表示空，*null* 不能通过 *Javascript* 来自动赋值，也就是说必须要我们自己手动来给某个变量赋值为 *null*。

> 解析：
>
> 那么为什么 *JavaScript* 要设置两个表示"无"的值呢？这其实是历史原因。
>
> *1995* 年 *JavaScript* 诞生时，最初像 *Java* 一样，只设置了 *null* 作为表示"无"的值。根据 *C* 语言的传统，*null* 被设计成可以自动转为*0*。
>
> 但是，*JavaScript* 的设计者，觉得这样做还不够，主要有以下两个原因。
>
> 1. *null* 像在 *Java* 里一样，被当成一个对象。但是，*JavaScript* 的数据类型分成原始类型（*primitive*）和合成类型（*complex*）两大类，作者觉得表示"无"的值最好不是对象。
> 2. *JavaScript* 的最初版本没有包括错误处理机制，发生数据类型不匹配时，往往是自动转换类型或者默默地失败。作者觉得，如果 *null* 自动转为 *0*，很不容易发现错误。
>
> 因此，作者又设计了一个 *undefined*。
>
> **这里注意：先有 *null* 后有 *undefined* 出来，*undefined* 是为了填补之前的坑。**
>
> *JavaScript* 的最初版本是这样区分的：
>
> *null* 是一个表示"无"的对象（空对象指针），转为数值时为 *0*；
>
> 典型用法是：
>
> - 作为函数的参数，表示该函数的参数不是对象。
>
> - 作为对象原型链的终点。
>
> *undefined* 是一个表示"无"的原始值，转为数值时为 *NaN*。
>
> 典型用法是：
>
> - 变量被声明了，但没有赋值时，就等于 *undefined*。 
> - 调用函数时，应该提供的参数没有提供，该参数等于 *undefined*。
> - 对象没有赋值的属性，该属性的值为 *undefined*。
> - 函数没有返回值时，默认返回 *undefined*。



### 29. 写一个函数判断变量类型

> 参考答案：
>
> ```js
> function getType(data){
>     let type = typeof data;
>     if(type !== "object"){
>         return type
>     }
>     return Object.prototype.toString.call(data).replace(/^\[object (\S+)\]$/,'$1')
> }
> function Person(){}
> console.log(getType(1)); // number
> console.log(getType(true)); // boolean
> console.log(getType([1,2,3])); // Array
> console.log(getType(/abc/)); // RegExp
> console.log(getType(new Date)); // Date
> console.log(getType(new Person)); // Object
> console.log(getType({})); // Object
> ```



### 30. *js* 的异步处理函数

> 参考答案：
>
> 在最早期的时候，*JavaScript* 中要实现异步操作，使用的就是 *Callback* 回调函数。
>
> 但是回调函数会产生回调地狱（*Callback Hell*）
>
> 之后 ES6 推出了 *Promise* 解决方案来解决回调地狱的问题。不过，虽然 *Promise* 作为 *ES6* 中提供的一种新的异步编程解决方案，但是它也有问题。比如，代码并没有因为新方法的出现而减少，反而变得更加复杂，同时理解难度也加大。
>
> 之后，就出现了基于 *Generator* 的异步解决方案。不过，这种方式需要编写外部的执行器，而执行器的代码写起来一点也不简单。当然也可以使用一些插件，比如 *co* 模块来简化执行器的编写。
>
> *ES7* 提出的 *async* 函数，终于让 *JavaScript* 对于异步操作有了终极解决方案。
>
> 实际上，*async* 只是生成器的一种语法糖而已，简化了外部执行器的代码，同时利用 *await* 替代 *yield*，*async* 替代生成器的`*`号。



### 31. *defer* 与 *async* 的区别

> 参考答案：
>
> 按照惯例，所有 *script* 元素都应该放在页面的 *head* 元素中。这种做法的目的就是把**所有外部文件（*CSS* 文件和 *JavaScript* 文件）的引用都放在相同的地方**。可是，在文档的 *head* 元素中包含所有 *JavaScript* 文件，意味着必须等到全部 *JavaScript* 代码都被下载、解析和执行完成以后，才能开始呈现页面的内容（浏览器在遇到 *body* 标签时才开始呈现内容）。
>
> 对于那些需要很多 *JavaScript* 代码的页面来说，这无疑会导致浏览器在呈现页面时出现明显的延迟，而延迟期间的浏览器窗口中将是一片空白。为了避免这个问题，现在 ***Web* 应用程序一般都全部 *JavaScript* 引用放在 *body* 元素中页面的内容后面**。这样一来，在解析包含的 *JavaScript* 代码之前，页面的内容将完全呈现在浏览器中。而用户也会因为浏览器窗口显示空白页面的时间缩短而感到打开页面的速度加快了。
>
> 有了 *defer* 和 *async* 后，这种局面得到了改善。
>
> ***defer* （延迟脚本）**
>
> 延迟脚本：*defer* 属性只适用于外部脚本文件。
>
> 如果给 *script* 标签定义了*defer* 属性，这个属性的作用是表明脚本在执行时不会影响页面的构造。也就是说，脚本会被延迟到整个页面都解析完毕后再运行。因此，如果 *script* 元素中设置了 *defer* 属性，相当于告诉浏览器立即下载，但延迟执行。
>
> ***async*（异步脚本）**
>
> 异步脚本：*async* 属性也只适用于外部脚本文件，并告诉浏览器立即下载文件。
>
> **但与 *defer* 不同的是：标记为 *async* 的脚本并不保证按照指定它们的先后顺序执行。**
>
> 所以总结起来，两者之间最大的差异就是在于脚本下载完之后何时执行，显然 *defer* 是最接近我们对于应用脚本加载和执行的要求的。
>
> *defer* 是立即下载但延迟执行，加载后续文档元素的过程将和脚本的加载并行进行（异步），但是脚本的执行要在所有元素解析完成之后，*DOMContentLoaded* 事件触发之前完成。*async* 是立即下载并执行，加载和渲染后续文档元素的过程将和 *js* 脚本的加载与执行并行进行（异步）。



### 32. 浏览器事件循环和任务队列

> 参考答案：
>
> *JavaScript* 的异步机制由事件循环和任务队列构成。
>
> *JavaScript* 本身是单线程语言，所谓异步依赖于浏览器或者操作系统等完成。*JavaScript* 主线程拥有一个执行栈以及一个任务队列，主线程会依次执行代码，当遇到函数时，会先将函数入栈，函数运行完毕后再将该函数出栈，直到所有代码执行完毕。
>
> 遇到异步操作（例如：*setTimeout、Ajax*）时，异步操作会由浏览器(*OS*)执行，浏览器会在这些任务完成后，将事先定义的回调函数推入主线程的任务队列(*task queue*)中,当主线程的执行栈清空之后会读取任务队列中的回调函数,当任务队列被读取完毕之后,主线程接着执行,从而进入一个无限的循环，这就是事件循环。



### 33. 原型与原型链 （美团 19年）

> 参考答案：
>
> - 每个对象都有一个 `__proto__ ` 属性，该属性指向自己的原型对象
> - 每个构造函数都有一个 `prototype ` 属性，该属性指向实例对象的原型对象
> - 原型对象里的 `constructor` 指向构造函数本身
>
> 如下图：
>
> <img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-08-12-081402.png" alt="image-20210812161401493" style="zoom: 45%;" />
>
> 每个对象都有自己的原型对象，而原型对象本身，也有自己的原型对象，从而形成了一条原型链条。
>
> 当试图访问一个对象的属性时，它不仅仅在该对象上搜寻，还会搜寻该对象的原型，以及该对象的原型的原型，依次层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾。



### 34. 作用域与作用域链 （美团 19年）

> 参考答案：
>
> 作用域是在运行时代码中的某些特定部分中变量，函数和对象的可访问性。换句话说，作用域决定了代码区块中变量和其他资源的可见性。*ES6* 之前 *JavaScript* 没有块级作用域，只有全局作用域和函数作用域。*ES6* 的到来，为我们提供了块级作用域。
>
> 作用域链指的是作用域与作用域之间形成的链条。当我们查找一个当前作用域没有定义的变量（自由变量）的时候，就会向上一级作用域寻找，如果上一级也没有，就再一层一层向上寻找，直到找到全局作用域还是没找到，就宣布放弃。这种一层一层的关系，就是作用域链 。



### 35. 闭包及应用场景以及闭包缺点 （美团 19年）

> 参考答案：
>
> 闭包的应用场景：
>
> 1. 匿名自执行函数
> 2. 结果缓存
> 3. 封装
> 4. 实现类和继承
>
> 闭包的缺点：
>
> 因为闭包的作用域链会引用包含它的函数的活动对象，导致这些活动对象不会被销毁，因此会占用更多的内存。



### 36. 原始值与引用值 （美团 19年）

> 参考答案：
>
> **原始值是表示 *JavaScript* 中可用的数据或信息的最底层形式或最简单形式。**简单类型的值被称为原始值，是因为它们是**不可细化**的。
>
> 也就是说，数字是数字，字符是字符，布尔值是 *true* 或 *false*，*null* 和 *undefined* 就是 *null* 和 *undefined*。这些值本身很简单，不能够再进行拆分。由于原始值的数据大小是固定的，所以**原始值的数据是存储于内存中的栈区里面的。**
>
> 在 *JavaScript* 中，对象就是一个引用值。因为对象可以向下拆分，拆分成多个简单值或者复杂值。**引用值在内存中的大小是未知的，因为引用值可以包含任何值，而不是一个特定的已知值，所以引用值的数据都是存储于堆区里面。**
>
> 最后总结一下两者的区别：
>
> 1. 访问方式
>    - 原始值：访问到的是值
>    - 引用值：访问到的是引用地址
> 2. 比较方式
>    - 原始值：比较的是值
>    - 引用值：比较的是地址
>
> 3. 动态属性
>    - 原始值：无法添加动态属性
>    - 引用值：可以添加动态属性
> 4. 变量赋值
>    - 原始值：赋值的是值
>    - 引用值：赋值的是地址



### 37. 描述下列代码的执行结果

```js
const first = () => (new Promise((resolve, reject) => {
    console.log(3);
    let p = new Promise((resolve, reject) => {
        console.log(7);
        setTimeout(() => {
            console.log(1);
        }, 0);
        setTimeout(() => {
            console.log(2);
            resolve(3);
        }, 0)
        resolve(4);
    });
    resolve(2);
    p.then((arg) => {
        console.log(arg, 5); // 1 bb
    });
    setTimeout(() => {
        console.log(6);
    }, 0);
}))
first().then((arg) => {
    console.log(arg, 7); // 2 aa
    setTimeout(() => {
        console.log(8);
    }, 0);
});
setTimeout(() => {
    console.log(9);
}, 0);
console.log(10);
```

> 参考答案：
>
> 3
> 7
> 10
> 4 5
> 2 7
> 1
> 2
> 6
> 9
> 8



### 38. 如何判断数组或对象（美团 19年）

> 参考答案：
>
> 1. 通过 *instanceof* 进行判断
>
> ```js
> var arr = [1,2,3,1];
> console.log(arr instanceof Array) // true
> ```
>
> 2. 通过对象的 *constructor* 属性
>
> ```js
> var arr = [1,2,3,1];
> console.log(arr.constructor === Array) // true
> ```
>
> 3. *Object.prototype.toString.call(arr)*
>
> ```js
> console.log(Object.prototype.toString.call({name: "jerry"}));//[object Object]
> console.log(Object.prototype.toString.call([]));//[object Array]
> ```
>
> 4. 可以通过 *ES6* 新提供的方法 *Array.isArray( )*
>
> ```js
> Array.isArray([]) //true
> ```



### 39. 对象深拷贝与浅拷贝，单独问了 *Object.assign*（美团 19年）

> 参考答案：
>
> - **浅拷贝**：只是拷贝了基本类型的数据，而引用类型数据，复制后也是会发生引用，我们把这种拷贝叫做浅拷贝（浅复制）
>
>     浅拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存。
>
> - **深拷贝**：在堆中重新分配内存，并且把源对象所有属性都进行新建拷贝，以保证深拷贝的对象的引用图不包含任何原有对象或对象图上的任何对象，拷贝后的对象与原来的对象是完全隔离，互不影响。
>
> 
>
> *Object.assign* 方法可以把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。但是 *Object.assign* 方法进行的是浅拷贝，拷贝的是对象的属性的引用，而不是对象本身。



### 40. 说说 *instanceof* 原理，并回答下面的题目（美团 19年）

```js
function A(){}
function B(){}
A.prototype = new B(); 
let a = new A(); 
console.log(a instanceof B) // true of false ?
```

> 参考答案：
>
> 答案为 *true*。
>
> *instanceof* 原理：
>
> *instanceof*  用于检测一个对象是否为某个构造函数的实例。
>
> 例如：*A instanceof B*
> *instanceof* 用于检测对象 *A* 是不是 *B* 的实例，而检测是基于原型链进行查找的，也就是说 *B* 的 *prototype* 有没有在对象 *A* 的\__*proto*__ 原型链上，如果有就返回 *true*，否则返回 *false*


### 41. 内存泄漏（美团 19 年）

> 参考答案：
>
> 内存泄漏（*Memory Leak*）是指程序中己动态分配的堆内存由于某种原因程序未释放或无法释放，造成系统内存的浪费，导致程序运行速度减慢甚至系统崩溃等严重后果。
>
> *Javascript* 是一种高级语言，它不像 *C* 语言那样要手动申请内存，然后手动释放，*Javascript* 在声明变量的时候自动会分配内存，普通的类型比如 *number*，一般放在栈内存里，对象放在堆内存里，声明一个变量，就分配一些内存，然后定时进行垃圾回收。垃圾回收的任务由 *JavaScript* 引擎中的垃圾回收器来完成，它监视所有对象，并删除那些不可访问的对象。
>
> 基本的垃圾回收算法称为**“标记-清除”**，定期执行以下“垃圾回收”步骤:
>
> - 垃圾回收器获取根并**“标记”**(记住)它们。
> - 然后它访问并“标记”所有来自它们的引用。
> - 然后它访问标记的对象并标记它们的引用。所有被访问的对象都被记住，以便以后不再访问同一个对象两次。
> - 以此类推，直到有未访问的引用(可以从根访问)为止。
> - 除标记的对象外，所有对象都被删除。



### 42. *ES6* 新增哪些东西？让你自己说（美团 19 年）

> 参考答案：
>
> *ES6* 新增内容众多，这里列举出一些关键的以及平时常用的新增内容：
>
> 1. 箭头函数
> 2. 字符串模板
> 3. 支持模块化（*import、export*）
> 4. 类（*class、constructor、extends*）
> 5. *let、const* 关键字
> 6. 新增一些数组、字符串等内置构造函数方法，例如 *Array.from*、*Array.of* 、*Math.sign*、*Math.trunc* 等
> 7. 新增一些语法，例如扩展操作符、解构、函数默认参数等
> 8. 新增一种基本数据类型 *Symbol*
> 9. 新增元编程相关，例如 *proxy*、*Reflect*
> 10. *Set* 和 *Map* 数据结构
> 11. *Promise*
> 12. *Generator* 生成器


