# *let、var、const* 的区别



## 经典真题



- *let const var* 的区别？什么是块级作用域？如何用？



## 声明变量关键字汇总



在 *JavaScript* 中，一共存在 *3* 种声明变量的方式：

- *var*
- *let*
- *const*

之所以有 *3* 种方式，这是由于历史原因造成的。最初声明变量的关键字就是 *var*，但是为了解决作用域的问题，所以后面新增了 *let* 和 *const* 的方式。



### 作用域



首先我们来了解一下作用域。

*ES5* 中的作用域有：全局作用域、函数作用域，*ES6* 中新增了块级作用域。块作用域由 { } 包括，*if*  语句和  *for*  语句里面的 { } 也属于块作用域。

关于作用域的更多内容，可以参阅《作用域和作用域链》章节。



### *var* 关键字



1. 没有块级作用域的概念

```js
//Global Scope
{
  var a = 10;
}
console.log(a);  //10
```

上面代码中，在 *Global Scope*（全局作用域）中，且在 *Block Scope*（块级作用域） { } 中，*a* 输出结果为 *10*，由此可以看出 *var* 声明的变量不存在 *Block Scope* 的概念



2. 有全局作用域、函数作用域的概念

```js
//Global Scope
var a = 10;
function checkscope(){
    //Local Scope
    var b = 20;
    console.log(a);  //10
    console.log(b);  //20
}
checkscope();
console.log(b);  //ReferenceError: b is not defined
```

上面代码中，在 *Global Scope* 中用 *var* 声明了 *a*，在 *checkscope* 函数中的 *Local Scope*（本地作用域、函数作用域）中打印出了 *10*，但是在 *Global Scope* 中打印的变量 *b* 报错了。



3. 不初始化值默认为 *undefined*

```js
//Global Scope
var a;
console.log(a);  //undefined
```

上面代码中，在 *Global Scope* 中用 *var* 声明了 *a*，但没有初始化值，它的值默认为 *undefined*，这里是 *undefined* 是 *undefined* 类型，而不是字符串。



4. 存在变量提升

```js
//Global Scope
console.log(a);  //undefined
var a = 10;

checkscope();
function checkscope(){
    //Local Scope
    console.log(a);  //undefined
    var a;
}
```

上面代码中，先打印了 *a*，然后用 *var* 声明变量 *a*。变量提升是因为 *js* 需要经历编译和执行阶段。而 *js* 在编译阶段的时候，会搜集所有的变量声明并且提前声明变量。

可以将这个过程形象地想象成所有的声明（变量）都会被“移动”到各自作用域的最顶端，这个过程被称为提升。

至于 *checkscope* 函数中的变量 *a* 为什么输出 *undefined*，可以参阅《作用域和作用域链》章节。



5. 全局作用域用 *var* 声明的变量会挂载到 *window* 对象下

```js
//Global Scope
var a = 10;
console.log(a);  //10
console.log(window.a);  //10
console.log(this.a);  //10
```

上面代码中，打印出了 *3* 个 *10*，访问 *a* 和 *window.a* 或是 *this.a* 都是等价的。

举个例子：比如我要访问 *location* 对象，使用 *location* 可以访问，使用 *window.location* 也可以访问，只不过 *window* 对象可以省略不写，就像 *new Array( )* 和 *new window.Array( )* 是等价的。



6. 同一作用域中允许重复声明

```js
//Global Scope
var a = 10;
var a = 20;
console.log(a);  //20

checkscope();
function checkscope(){
    //Local Scope
    var b = 10;
    var b = 20;
    console.log(b);  //20
}
```

上面代码中，在 *Global Scope* 中声明了 *2* 次 *a*，以最后一次声明有效，打印为 *20*。同理，在 *Local Scope* 也是一样的。



### *let* 关键字



1. 有块级作用域的概念

```js
{
   //Block Scope
   let a = 10;
}
console.log(a);  //ReferenceError: a is not defined
```

上面代码中，打印 *a* 报错，说明存在 *Block Scope* 的概念。



2. 不存在变量提升

```js
{
  //Block Scope
  console.log(a);  //ReferenceError: Cannot access 'a' before initialization
  let a = 10;
}
```

上面代码中，打印 *a* 报错：无法在初始化之前访问。说明不存在变量提升。



3. 暂时性死区

```js
{
  //Block Scope
  console.log(a);  //ReferenceError: Cannot access 'a' before initialization
  let a = 20;
}

if (true) {
  //TDZ开始
  console.log(a);  //ReferenceError: Cannot access 'a' before initialization

  let a; //TDZ结束
  console.log(a);  //undefined

  a = 123;
  console.log(a);  //123
}
```

上面代码中，使用 *let* 声明的变量 *a*，导致绑定这个块级作用域，所以在 *let* 声明变量前，打印的变量 *a* 报错。

这是因为使用 *let/const* 所声明的变量会存在暂时性死区。

什么叫做暂时性死区域呢？

*ES6* 标准中对 *let/const* 声明中的解释 [第13章](https://link.segmentfault.com/?enc=K6pZVwgVNQb0IBQ9LTOuJg%3D%3D.p07UoPCGl5RslJ9ZnW9Nr36NFqs2pU%2FnSfWZUPIH3S1TUXzWdj22pH0lUMFVGVUwJkDpSHrYe8uKlYek%2FK4HBDYkJhc%2Fe2xiWo5V6teR%2BXY%3D)，有如下一段文字：

> *The variables are created when their containing Lexical Environment is instantiated but may not be accessed inany way until the variable’s LexicalBinding is evaluated.*



翻译成人话就是：

> 当程序的控制流程在新的作用域（*module、function* 或 *block* 作用域）进行实例化时，在此作用域中用 *let/const* 声明的变量会先在作用域中被创建出来，但因此时还未进行词法绑定，所以是不能被访问的，如果访问就会抛出错误。因此，在这运行流程进入作用域创建变量，到变量可以被访问之间的这一段时间，就称之为暂时死区。



再简单理解就是：

>*ES6* 规定，*let/const* 命令会使区块形成封闭的作用域。若在声明之前使用变量，就会报错。
>总之，在代码块内，使用 *let/const* 命令声明变量之前，该变量都是不可用的。
>这在语法上，称为 **“暂时性死区”**（ *temporal dead zone*，简称 ***TDZ***）。



其实上面不存在变量提升的例子中，其实也是暂时性死区，因为它有暂时性死区的概念，所以它压根就不存在变量提升了。



4. 同一块作用域中不允许重复声明

```js
{
  //Block Scope
  let A;
  var A;  //SyntaxError: Identifier 'A' has already been declared
}
{
  //Block Scope
  var A;
  let A;  //SyntaxError: Identifier 'A' has already been declared
}
{
  //Block Scope
  let A;
  let A;  //SyntaxError: Identifier 'A' has already been declared
}
```



### *const* 关键字



1. 必须立即初始化，不能留到以后赋值

```js
// Block Scope 
const a; // SyntaxError: Missing initializer in const declaration } 
```

上面代码中，用 *const* 声明的变量 *a* 没有进行初始化，所以报错。



2. 常量的值不能改变

```js
//Block Scope 
{
  const a = 10; 
	a = 20; // TypeError: Assignment to constant variable
}
```

上面代码中，用 *const* 声明了变量 *a* 且初始化为 *10*，然后试图修改 *a* 的值，报错。 

*const* 实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。



### 特点总结



- *var* 关键字

1. 没有块级作用域的概念
2. 有全局作用域、函数作用域的概念
3. 不初始化值默认为 *undefined*
4. 存在变量提升
5. 全局作用域用 *var* 声明的变量会挂载到 *window* 对象下
6. 同一作用域中允许重复声明



- *let* 关键字

1. 有块级作用域的概念
2. 不存在变量提升
3. 暂时性死区
5. 同一块作用域中不允许重复声明



- *const* 关键字

1. 与 *let* 特性一样，仅有 *2* 个差别
2. 区别 1：必须立即初始化，不能留到以后赋值
3. 区别 2：常量的值不能改变



## 真题解答



- *let const var* 的区别？什么是块级作用域？如何用？

>参考答案：
>
>1. *var* 定义的变量，没有块的概念，可以跨块访问, 不能跨函数访问，有变量提升。
>2. *let* 定义的变量，只能在块作用域里访问，不能跨块访问，也不能跨函数访问，无变量提升，不可以重复声明。
>3. *const* 用来定义常量，使用时必须初始化(即必须赋值)，只能在块作用域里访问，而且不能修改，无变量提升，不可以重复声明。
>
>最初在 *JS* 中作用域有：全局作用域、函数作用域。没有块作用域的概念。
>
>*ES6* 中新增了块级作用域。块作用域由 { } 包括，*if* 语句和 *for* 语句里面的 { } 也属于块作用域。
>
>在以前没有块作用域的时候，在 *if* 或者 *for* 循环中声明的变量会泄露成全局变量，其次就是 { } 中的内层变量可能会覆盖外层变量。块级作用域的出现解决了这些问题。



-*EOF*- 