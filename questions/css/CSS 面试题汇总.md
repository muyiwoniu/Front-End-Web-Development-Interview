# *CSS* 面试题汇总



### 1.  介绍下 *BFC* 及其应用

> 参考答案：
>
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



### 2. 介绍下 *BFC、IFC、GFC* 和 *FFC*

> 参考答案：
>
> - *BFC*：块级格式上下文，指的是一个独立的布局环境，*BFC* 内部的元素布局与外部互不影响。
> - *IFC*：行内格式化上下文，将一块区域以行内元素的形式来格式化。
> - *GFC*：网格布局格式化上下文，将一块区域以 *grid* 网格的形式来格式化
> - *FFC*：弹性格式化上下文，将一块区域以弹性盒的形式来格式化



### 3. *flex* 布局如何使用？

> 参考答案：
>
> flex 是 Flexible Box 的缩写，意为"弹性布局"。指定容器display: flex即可。
>
> 容器有以下属性：flex-direction，flex-wrap，flex-flow，justify-content，align-items，align-content。
>
> - flex-direction属性决定主轴的方向；
> - flex-wrap属性定义，如果一条轴线排不下，如何换行；
> - flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap；
> - justify-content属性定义了项目在主轴上的对齐方式。
> - align-items属性定义项目在交叉轴上如何对齐。
> - align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
>
> 项目（子元素）也有一些属性：order，flex-grow，flex-shrink，flex-basis，flex，align-self。
>
> - order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。
> - flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
> - flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
> - flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。
> - flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
> - align-self 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性。默认值为 *auto*，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。



### 4. 怎么让一个 *div* 水平垂直居中

> 参考答案：
>
> 水平垂直居中有好多种实现方式，主要就分为两类不定宽高和定宽高 以在 *body* 下插入一个 *div* 为例
>
> **定宽高** 
>
> 使用定位 + *margin*
>
> ```js
> element.style {
>  position: absolute;
>  left: 50%;
>  top: 50%;
>  margin-left: -250px;
>  margin-top: -250px;
>  width: 500px;
>  height: 500px;
>  background: yellow;
>  z-index: 1;
> }
> ```
>
> 使用定位 + *transfrom*
>
> ````js
> element.style {
>  position: absolute;
>  left: 50%;
>  top: 50%;
>  width: 500px;
>  height: 500px;
>  background: yellow;
>  z-index: 1;
>  transform: translate3d(-50%,-50%,0);
> }
> ````
>
> **不定宽高**
>
> 不定宽高的方法基本都适用于定宽高的情况 这里把 *div* 的宽高按照内容展开，使用定位 + *transform* 同样是适用的
>
> ```js
> element.style {
>  position: absolute;
>  left: 50%;
>  top: 50%;
>  background: yellow;
>  z-index: 1;
>  transform: translate3d(-50%,-50%,0);
> }
> ```



### 5. 分析比较 *opacity: 0、visibility: hidden、display: none* 优劣和适用场景。

> 参考答案：
>
> - 结构： display:none: 会让元素完全从渲染树中消失，渲染的时候不占据任何空间, 不能点击， visibility: hidden:不会让元素从渲染树消失，渲染元素继续占据空间，只是内容不可见，不能点击 opacity: 0: 不会让元素从渲染树消失，渲染元素继续占据空间，只是内容不可见，可以点击
>
> - 继承： display: none和opacity: 0：是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示。 visibility: hidden：是继承属性，子孙节点消失由于继承了hidden，通过设置visibility: visible;可以让子孙节点显式。
>
> - 性能： displaynone : 修改元素会造成文档回流,读屏器不会读取display: none元素内容，性能消耗较大 visibility:hidden: 修改元素只会造成本元素的重绘,性能消耗较少读屏器读取visibility: hidden元素内容 opacity: 0 ： 修改元素会造成重绘，性能消耗较少



### 6. 已知如下代码，如何修改才能让图片宽度为 *300px* ？注意下面代码不可修改。

```js
<img src="1.jpg" style="width:480px!important;”>
```

> 参考答案：
>
> **CSS 方法** 
>
> - *max-width:300px;* 覆盖其样式
> - *transform: scale(0.625)* 按比例缩放图片； 
> - 利用 *CSS* 动画的样式优先级高于 *!important* 的特性
>
> **JS 方法** 
>
> - *document.getElementsByTagName("img")[0].setAttribute("style","width:300px!important;")*

