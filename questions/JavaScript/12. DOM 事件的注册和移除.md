# *DOM* 事件的注册和移除



## 经典真题



- 总结一下 *DOM* 中如何注册事件和移除事件



## *DOM* 注册事件



使用 *JavaScript* 为 *DOM* 元素注册事件的方式有多种。但是并不是一开始就设计了多种方式，而是随着技术的发展，发展前一种方式有所缺陷，所以设计了新的 *DOM* 元素注册事件的方式。



这里我们就一起来总结一下 *DOM* 中注册事件的方式有哪些。



### *HTML* 元素中注册事件



*HTML* 元素中注册的事件，又被称之为行内事件监听器。这是在浏览器中处理事件最原始的方法。

具体的示例如下：

```html
<button onclick="test('张三')">点击我</button>
```

```js
function test(name) {
  console.log(`我知道你已经点击了，${name}`);
}
```

在上面的代码中，我们为 *button* 元素添加了 *onclick* 属性，然后绑定了一个名为 *test* 的事件处理器。

在 *JavaScript* 中只需要书写对应的 *test* 事件处理函数即可。

但是有一点需要注意，就是这种方法已经过时了，原因如下：

- *JavaScript* 代码与 *HTML* 标记混杂在一起，破坏了结构和行为分离的理念。
- 每个元素只能为每种事件类型绑定一个事件处理器。
- 事件处理器的代码隐藏于标记中，很难找到事件是在哪里声明的。

但是如果是做简单的事件测试，那么这种写法还是非常方便快捷的。



### *DOM0* 级方式注册事件



这种方式是首先取到要为其绑定事件的元素节点对象，然后给这些节点对象的事件处理属性赋值一个函数。

这样就可以达到 *JavaScript* 代码和 *HTML* 代码相分离的目的。

具体的示例如下：

```html
<button id="test">点击我</button>
```

```js
var test = document.getElementById("test");
test.onclick = function(){
  console.log("this is a test");
}
```

这种方式虽然相比 *HTML* 元素中注册事件有所改进，但是它也有一个缺点，那就是它依然存在每个元素只能绑定一个函数的局限性。

下面我们尝试使用这种方式为同一个元素节点绑定 *2* 个事件，如下：

```js
var test = document.getElementById("test");
test.onclick = function(){
  console.log("this is a test");
}
test.onclick = function(){
  console.log("this is a test,too");
}
```

当我们为该 *DOM* 元素绑定 *2* 个相同类型的事件时，后面的事件处理函数就会把前面的事件处理函数给覆盖掉。



### *DOM2* 级方式注册事件



*DOM2* 级再次对事件的绑定方式进行了改进。

*DOM2* 级通过 *addEventListener* 方法来为一个 *DOM* 元素添加多个事件处理函数。

该方法接收 *3* 个参数：事件名、事件处理函数、布尔值。

如果这个布尔值为 *true*，则在捕获阶段处理事件，如果为 *false*，则在冒泡阶段处理事件。若最后的布尔值不填写，则和 *false* 效果一样，也就是说默认为 *false*，在冒泡阶段进行事件的处理。



接下来我们来看下面的示例：这里我们为 *button* 元素绑定了 *2* 个事件处理程序，并且 *2* 个事件处理程序都是通过点击来触发。

```js
var test = document.getElementById("test");
test.addEventListener("click", function () {
  console.log("this is a test");
}, false);
test.addEventListener("click", function () {
  console.log("this is a test,too");
}, false);
```

在上面的代码中，我们通过 *addEventListener* 为按钮绑定了 *2* 个点击的事件处理程序，*2* 个事件处理程序都会执行。

另外需要注意的是，在 *IE* 中和 *addEventListener* 方法与之对应的是 *attachEvent* 方法。



## *DOM* 移除事件



通过 *DOM0* 级来添加的事件，删除的方法很简单，只需要将 *DOM* 元素的事件处理属性赋值为 *null* 即可。

例如：

```js
var test = document.getElementById("test");
test.onclick = function(){
  console.log("this is a test");
  test.onclick = null;
}
```

在上面的代码中，我们通过 *DOM0* 级的方式为 *button* 按钮绑定了点击事件，但是在事件处理函数中又移除了该事件。所以该事件只会生效一次。



如果是通过 *DOM2* 级来添加的事件，我们可以使用 *removeEventLister* 方法来进行事件的删除。

需要注意的是，如果要通过该方法移除**某一类事件类型的一个事件**的话，在通过 *addEventListener* 来绑定事件时的写法就要稍作改变。

先单独将绑定函数写好，然后 *addEventListener* 进行绑定时第 *2* 个参数传入要绑定的函数名即可。

示例如下：

```js
var test = document.getElementById("test");
//DOM 2级添加事件
function fn1() {
  console.log("this is a test");
  test.removeEventListener("click", fn1); // 只删除第一个点击事件
}
function fn2() {
  console.log("this is a test,too");
}
test.addEventListener("click", fn1, false);
test.addEventListener("click", fn2, false);
```

在上面的代码中，我们为 *button* 元素绑定了两个 *click* 事件，之后在第一个事件处理函数中，对 *fn1* 事件处理函数进行了移除。所以第一次点击时，*fn1* 和 *fn2* 都会起作用，之后因为 *fn1* 被移除，所以只会 *fn2* 有作用。



## 真题解答



- 总结一下 *DOM* 中如何注册事件和移除事件

> 参考答案：
>
> 注册事件的方式常见的有 *3* 种方式：
>
> - *HTML* 元素中注册的事件：这种方式又被称之为行内事件监听器。这是在浏览器中处理事件最原始的方法。
>
> - *DOM0* 级方式注册事件：这种方式是首先取到要为其绑定事件的元素节点对象，然后给这些节点对象的事件处理属性赋值一个函数。
>
> - *DOM2* 级方式注册事件：*DOM2* 级通过 *addEventListener* 方法来为一个 *DOM* 元素添加多个事件处理函数。
>
>   该方法接收 *3* 个参数：事件名、事件处理函数、布尔值。
>
>   如果这个布尔值为 *true*，则在捕获阶段处理事件，如果为 *false*，则在冒泡阶段处理事件。若最后的布尔值不填写，则和 *false* 效果一样，也就是说默认为 *false*，在冒泡阶段进行事件的处理。
>
> 关于移除注册的事件，如果是 *DOM0* 级方式注册的事件，直接将值设置为 *null* 即可。如果是 *DOM2* 级注册的事件，可以使用 *removeEventListener* 方法来移除事件。



-*EOF*-

