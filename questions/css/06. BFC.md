# *BFC*



## 经典真题



- 介绍下 *BFC* 及其应用
- 介绍下 *BFC、IFC、GFC* 和 *FFC*



## 搞懂各种 *FC*



一看到  *BFC、IFC、GFC* 和 *FFC*，大家可能会想到 *KFC*。



<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-13-054223.png" alt="image-20210913134223247" style="zoom:50%;" />



然而这里所说的 *xFC* 和 *KFC* 没有任何关系。



那么这些 *FC* 究竟是啥呢？



不着急，我们先搞懂一个，后面的陆陆续续也就融会贯通了。



我们首先就来看这个 *BFC*，英语全称 *Block formatting contexts*，翻译成中文就是“块级格式化上下文”。



简单来说，就是页面中的一块渲染区域，并且有一套属于自己的渲染规则，它决定了元素如何对齐内容进行布局，以及与其他元素的关系和相互作用。 当涉及到可视化布局的时候，*BFC* 提供了一个环境，*HTML* 元素在这个环境中按照一定规则进行布局。



再简短一点，那就是：***BFC* 是一个独立的布局环境，*BFC* 内部的元素布局与外部互不影响**



这就好比你在你自己家里面，你想怎么摆放你的家具都可以，你家的家具布局并不会影响邻居家的家具布局。



当然，虽然说 *BFC* 是一个独立的布局环境，里外不影响，但也不是意味着布局没有章法，基本的规矩还是要有的。



例如，*BFC* 的布局规则有如下几条：



1. 内部的 *Box* 会在垂直方向一个接着一个地放置。
2. *Box* 垂直方向上的距离由 *margin* 决定。属于同一个 *BFC* 的两个相邻的 *Box* 的 *margin* 会发生重叠。
3. 每个盒子的左外边框紧挨着包含块的左边框，即使浮动元素也是如此。
4. *BFC* 的区域不会与浮动 *Box* 重叠。
5. *BFC* 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。
6. 计算 *BFC* 的高度时，浮动子元素也参与计算。



诶？？



这好像在我们的 *body* 元素里面，盒子天然就是这样的布局规则呀？



没错，实际上在一个标准流中 *body* 元素就是一个天然的 *BFC*。



那么如果其他区域，我想单独设置成一个 *BFC*，该怎么办呢？这里记录了一些常见的方式：



| 元素或属性 | 属性值                     |
| ---------- | -------------------------- |
| 根元素     |                            |
| *float*    | *left、right*              |
| *postion*  | *absolute、fixed*          |
| *overflow* | *auto、scroll、hidden*     |
| *display*  | *inline-block、table-cell* |



> 上面只记录了一些常见的方式，完整的 *BFC* 触发方式可以参阅：*https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context*



那么块级格式化上下文有啥具体的用处呢？我们来看几个场景



1. 解决浮动元素令父元素高度坍塌的问题

```html
<div class="father">
   <div class="son"></div>
</div>
```

```css
.father{
  border: 5px solid;
}
.son{
  width: 100px;
  height: 100px;
  background-color: blue;
  float: left;
}
```

在上面的代码中，父元素的高度是靠子元素撑起来的，但是一旦我们给子元素设置了浮动，那么父元素的高度就塌陷了。如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-13-060809.png" alt="image-20210913140809007" style="zoom:50%;" />



此时我们就可以将父元素设置成一个 *BFC*，例如：

```css
.father{
  border: 5px solid;
  overflow: hidden; 
  /* 将父元素设置为一个 BFC */
}
.son{
  width: 100px;
  height: 100px;
  background-color: blue;
  float: left;
}
```

效果：可以看到由于父元素变成 *BFC*，高度并没有产生塌陷了，其原因是在计算 *BFC* 的高度时，浮动子元素也参与计算



<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-13-060948.png" alt="image-20210913140948390" style="zoom:50%;" />



2. 非浮动元素被浮动元素覆盖

```html
<div class="box1"></div>
<div class="box2"></div>
```

```css
.box1{
  width: 100px;
  height: 50px;
  background-color: red;
  float: left;
}
.box2{
  width: 50px;
  height: 50px;
  background-color: blue;
}
```

在上面的代码中，由于 *box1* 设置了浮动效果，所以会脱离标准流，自然而然 *box2* 会往上面跑，结果就被高度和自己一样的 *box1* 给挡住了。



<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-13-061556.png" alt="image-20210913141555490" style="zoom:50%;" />

接下来我们设置 *box2* 为 *BFC*，如下：

```css
.box1{
  width: 100px;
  height: 50px;
  background-color: red;
  float: left;
}
.box2{
  width: 50px;
  height: 50px;
  background-color: blue;
  overflow: hidden;
}
```

效果：由于 *BFC* 的区域不会与浮动 *box* 重叠，所以即使 *box1* 因为浮动脱离了标准流，*box2* 也不会被 *box1* 遮挡



<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-13-061805.png" alt="image-20210913141805543" style="zoom:50%;" />



基于此特点，我们就可以制作两栏自适应布局，方法就是给固定栏设置固定宽度，给不固定栏开启 *BFC*。

```html
<div class="left">导航栏</div>
<div class="right">这是右侧</div>
```

```css
*{
  margin: 0;
  padding: 0;
}
.left {
  width: 200px;
  height: 100vh;
  background-color: skyblue;
  float: left;
}

.right {
  width: calc(100% - 200px); 
  height: 100vh;
  background-color: yellowgreen;
}
```

效果：在上面的代码中，我们要设置两栏布局，左边栏宽度固定，右边栏自适应。结果我们发现右侧出现了空白

究其原因就是右侧区域与浮动盒子重叠了

![image-20210913143033581](https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-13-063034.png)



修改 *.right* 部分的代码，添加 *overflow:hidden* 使其成为一个 *BFC*：

```css
.right {
  width: calc(100% - 200px); 
  height: 100vh;
  background-color: yellowgreen;
  overflow: hidden;
}
```



效果：因为 *BFC* 的区域不会与浮动 *Box* 重叠，所以右侧空白没有了

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-13-063330.png" alt="image-20210913143330616" style="zoom:50%;" />



3. 外边距垂直方向重合的问题

*BFC* 还能够解决 *margin* 折叠的问题，例如：

```html
<div class="box1"></div>
<div class="box2"></div>
```

```css
* {
  margin: 0;
  padding: 0;
}

.box1{
  width: 100px;
  height: 100px;
  background-color: red;
  margin-bottom: 10px;
}
.box2{
  width: 100px;
  height: 100px;
  background-color: blue;
  margin-top: 10px;
}
```

效果：



<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-13-063850.png" alt="image-20210913143849932" style="zoom:50%;" />





此时我们可以在 box2 外部再包含一个 div，并且将这个 div 设置为 *BFC*，如下：

```html
<div class="box1"></div>
<div class="container">
  <div class="box2"></div>
</div>
```

```css
* {
  margin: 0;
  padding: 0;
}

.box1{
  width: 100px;
  height: 100px;
  background-color: red;
  margin-bottom: 10px;
}
.box2{
  width: 100px;
  height: 100px;
  background-color: blue;
  margin-top: 10px;
}
.container{
  overflow: hidden;
}
```



效果：



<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-13-064106.png" alt="image-20210913144106258" style="zoom:50%;" />



*OK*，到这里你应该明白什么是 *BFC* 以及 *BFC* 的触发规则和其使用场景了。

明白了 *BFC*，那么其他的 *IFC、GFC* 和 *FFC* 也就大同小异了。



- *IFC*（*Inline formatting context*）：翻译成中文就是“行内格式化上下文”，也就是一块区域以行内元素的形式来格式化
- *GFC*（*GrideLayout formatting contexts*）：翻译成中文就是“网格布局格式化上下文”，将一块区域以 *grid* 网格的形式来格式化
- *FFC*（*Flex formatting contexts*）：翻译成中文就是“弹性格式化上下文“，将一块区域以弹性盒的形式来格式化



> 更多关于格式化上下文的内容，可以参阅 *MDN*：
>
> *BFC*：*https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context*
>
> *IFC*：*https://developer.mozilla.org/zh-CN/docs/Web/CSS/Inline_formatting_context*



## 真题解答



- 介绍下 *BFC* 及其应用

> 参考答案：
>
> 所谓 *BFC*，指的是一个独立的布局环境，*BFC* 内部的元素布局与外部互不影响。
>
> 触发 *BFC* 的方式有很多，常见的有：
>
> - 设置浮动
> - *overflow* 设置为 *auto、scroll、hidden*
> - *positon* 设置为 *absolute、fixed*
>
> 常见的 *BFC* 应用有：
>
> - 解决浮动元素令父元素高度坍塌的问题
> - 解决非浮动元素被浮动元素覆盖问题
> - 解决外边距垂直方向重合的问题



- 介绍下 *BFC、IFC、GFC* 和 *FFC*

> 参考答案：
>
> - *BFC*：块级格式上下文，指的是一个独立的布局环境，*BFC* 内部的元素布局与外部互不影响。
> - *IFC*：行内格式化上下文，将一块区域以行内元素的形式来格式化。
> - *GFC*：网格布局格式化上下文，将一块区域以 *grid* 网格的形式来格式化
> - *FFC*：弹性格式化上下文，将一块区域以弹性盒的形式来格式化



-*EOF*-
