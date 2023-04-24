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



### 7. 如何用 *css* 或 *js* 实现多行文本溢出省略效果，考虑兼容性

> 参考答案：
>
> CSS 实现方式
>
> 单行：
>
> ```css
> overflow: hidden;
> text-overflow:ellipsis;
> white-space: nowrap;
> ```
>
> 多行：
>
> ```css
> display: -webkit-box;
> -webkit-box-orient: vertical;
> -webkit-line-clamp: 3; //行数
> overflow: hidden;
> ```
>
> 兼容：
>
> ```css
> p{position: relative; line-height: 20px; max-height: 40px;overflow: hidden;}
> p::after{content: "..."; position: absolute; bottom: 0; right: 0; padding-left: 40px;
> background: -webkit-linear-gradient(left, transparent, #fff 55%);
> background: -o-linear-gradient(right, transparent, #fff 55%);
> background: -moz-linear-gradient(right, transparent, #fff 55%);
> background: linear-gradient(to right, transparent, #fff 55%);
> }
> ```
>
> JS 实现方式：
>
> - 使用split + 正则表达式将单词与单个文字切割出来存入words
> - 加上 '...'
> - 判断scrollHeight与clientHeight，超出的话就从words中pop一个出来



### 8. 居中为什么要使用 *transform*（为什么不使用 *marginLeft/Top*）（阿里）

> 参考答案：
>
> transform 属于合成属性（composite property），对合成属性进行 transition/animation 动画将会创建一个合成层（composite layer），这使得被动画元素在一个独立的层中进行动画。通常情况下，浏览器会将一个层的内容先绘制进一个位图中，然后再作为纹理（texture）上传到 GPU，只要该层的内容不发生改变，就没必要进行重绘（repaint），浏览器会通过重新复合（recomposite）来形成一个新的帧。
>
> top/left属于布局属性，该属性的变化会导致重排（reflow/relayout），所谓重排即指对这些节点以及受这些节点影响的其它节点，进行CSS计算->布局->重绘过程，浏览器需要为整个层进行重绘并重新上传到 GPU，造成了极大的性能开销。



### 9. 介绍下粘性布局（*sticky*）（网易）

> 参考答案：
>
> position 中的 sticky 值是 CSS3 新增的，设置了 sticky 值后，在屏幕范围（viewport）时该元素的位置并不受到定位影响（设置是top、left等属性无效），当该元素的位置将要移出偏移范围时，定位又会变成fixed，根据设置的left、top等属性成固定位置的效果。
>
> sticky 属性值有以下几个特点：
>
> - 该元素并不脱离文档流，仍然保留元素原本在文档流中的位置。
> - 当元素在容器中被滚动超过指定的偏移值时，元素在容器内固定在指定位置。亦即如果你设置了top: 50px，那么在sticky元素到达距离相对定位的元素顶部50px的位置时固定，不再向上移动。
> - 元素固定的相对偏移是相对于离它最近的具有滚动框的祖先元素，如果祖先元素都不可以滚动，那么是相对于viewport来计算元素的偏移量



### 10. 说出 *space-between* 和 *space-around* 的区别？（携程）

> 参考答案：
>
> 这个是 *flex* 布局的内容，其实就是一个边距的区别，按水平布局来说，`space-between`是两端对齐，在左右两侧没有边距，而`space-around`是每个 子项目左右方向的 margin 相等，所以两个item中间的间距会比较大。
>
> 如图所示：
>
> <img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-11-03-010805.png" alt="image-20210816161354713" style="zoom:50%;" />




### 11. *CSS3* 中 *transition* 和 *animation* 的属性分别有哪些（哔哩哔哩）

> 参考答案：
>
> *transition* 过渡动画：
>
> - *transition-property*：指定过渡的 *CSS* 属性
> - *transition-duration*：指定过渡所需的完成时间
> - *transition-timing-function*：指定过渡函数
> - *transition-delay*：指定过渡的延迟时间
>
> *animation* 关键帧动画：
>
> - *animation-name*：指定要绑定到选择器的关键帧的名称
> - *animation-duration*：动画指定需要多少秒或毫秒完成
> - *animation-timing-function*：设置动画将如何完成一个周期
> - *animation-delay*：设置动画在启动前的延迟间隔
> - *animation-iteration-count*：定义动画的播放次数
> - *animation-direction*：指定是否应该轮流反向播放动画
> - *animation-fill-mode*：规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式
> - *animation-play-state*：指定动画是否正在运行或已暂停



### 12. 隐藏页面中的某个元素的方法有哪些？

> 参考答案：
>
> 1. 隐藏类型
>
> 屏幕并不是唯一的输出机制，比如说屏幕上看不见的元素（隐藏的元素），其中一些依然能够被读屏软件阅读出来（因为读屏软件依赖于可访问性树来阐述）。为了消除它们之间的歧义，我们将其归为三大类：
>
> - 完全隐藏：元素从渲染树中消失，不占据空间。
> - 视觉上的隐藏：屏幕中不可见，占据空间。
> - 语义上的隐藏：读屏软件不可读，但正常占据空。
>
> **完全隐藏**
>
> (1) display 属性
>
> ```css
> display: none;
> ```
>
> (2) hidden 属性
> HTML5 新增属性，相当于 display: none
>
> ```html
> <div hidden>
> </div>
> ```
>
> **视觉上的隐藏**
>
> (1) 设置 posoition 为 absolute 或 fixed，通过设置 top、left 等值，将其移出可视区域。
>
> ```css
> position:absolute;
> left: -99999px;
> ```
>
> (2) 设置 position 为 relative，通过设置 top、left 等值，将其移出可视区域。
>
> ```css
> position: relative;
> left: -99999px;
> height: 0
> ```
>
> (3) 设置 margin 值，将其移出可视区域范围（可视区域占位）。
>
> ```js
> margin-left: -99999px;
> height: 0;
> ```
>
> 语义上隐藏
>
> *aria-hidden 属性*
>
> 读屏软件不可读，占据空间，可见。
>
> ```js
> <div aria-hidden="true">
> </div>
> ```



### 13. 层叠上下文

> 参考答案：
>
> **层叠上下文概念**
>
> 在 *CSS2.1* 规范中，每个盒模型的位置是三维的，分别是平面画布上的 *X* 轴，*Y* 轴以及表示层叠的 *Z* 轴。
>
> 一般情况下，元素在页面上沿 *X* 轴 *Y* 轴平铺，我们察觉不到它们在 *Z* 轴上的层叠关系。而一旦元素发生堆叠，这时就能发现某个元素可能覆盖了另一个元素或者被另一个元素覆盖。
>
> **层叠上下文触发条件**
>
> - *HTML* 中的根元素 *HTML* 本身就具有层叠上下文，称为“根层叠上下文”。
> - 普通元素设置 *position* 属性为非 *static* 值并设置 *z-index* 属性为具体数值，产生层叠上下文
> - *CSS3* 中的新属性也可以产生层叠上下文
>
> **层叠顺序**
>
> “层叠顺序”（*stacking order*）表示元素发生层叠时按照特定的顺序规则在 *Z* 轴上垂直显示。
>
> 说简单一点就是当元素处于同一层叠上下文内时如何进行层叠判断。
>
> 具体的层叠等级如下图：
>
> <img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-11-03-010804.png" alt="image-20210816174742449" style="zoom:50%;" />



### 14. 分析比较 *opacity: 0、visibility: hidden、display: none* 优劣和适用场景

> 参考答案：
>
> 1. display: none (不占空间，不能点击)（场景，显示出原来这里不存在的结构）
> 2. visibility: hidden（占据空间，不能点击）（场景：显示不会导致页面结构发生变动，不会撑开）
> 3. opacity: 0（占据空间，可以点击）（场景：可以跟transition搭配）




### 15. 讲一下*png8、png16、png32*的区别，并简单讲讲 *png* 的压缩原理

> 参考答案：
>
> PNG图片主要有三个类型，分别为 PNG 8/ PNG 24 / PNG 32。
>
> - `PNG 8`：PNG 8中的8，其实指的是8bits，相当于用2^8（2的8次方）大小来存储一张图片的颜色种类，2^8等于256，也就是说PNG 8能存储256种颜色，一张图片如果颜色种类很少，将它设置成PNG 8得图片类型是非常适合的。
> - `PNG 24`：PNG 24中的24，相当于3乘以8 等于 24，就是用三个8bits分别去表示 R（红）、G（绿）、B（蓝）。R(0-255),G(0-255),B(0-255)，可以表达256乘以256乘以256=16777216种颜色的图片，这样PNG 24就能比PNG 8表示色彩更丰富的图片。但是所占用的空间相对就更大了。
> - `PNG 32`：PNG 32中的32，相当于PNG 24 加上 8bits的透明颜色通道，就相当于R（红）、G（绿）、B（蓝）、A（透明）。R(0~255),G(0~255),B(0~255),A(0~255)。比PNG 24多了一个A（透明），也就是说PNG 32能表示跟PNG 24一样多的色彩，并且还支持256种透明的颜色，能表示更加丰富的图片颜色类型。
>
> PNG图片的压缩，分两个阶段：
>
> - `预解析（Prediction）`：这个阶段就是对png图片进行一个预处理，处理后让它更方便后续的压缩。
> - `压缩（Compression）`：执行Deflate压缩，该算法结合了 LZ77 算法和 Huffman 算法对图片进行编码。



### 16. 说说渐进增强和优雅降级

> 参考答案：
>
> 这并不是一个新的概念，其实就是以前提到的"向上兼容"和"向下兼容"。渐进增强相当于向上兼容，优雅降级相当于向下兼容。向下兼容指的是高版本支持低版本，或者说后期开发的版本能兼容早期开发的版本。
>
> 在确定用户群体的前提下，渐进增强：针对低版本浏览器进行页面构建，保证基本功能，再针对高级浏览器进行效果、交互等改进和追加功能，达到更好的用户体验。优雅降级：一开始就构建完整的功能，再针对低版本浏览器进行兼容。区别：优雅降级是从复杂的现状开始并试图减少用户体验的供给，而渐进增强则是从一个基础的、能够起到作用的版本开始再不断扩充，以适应未来环境的需要。
>
> 绝大多少的大公司都是采用渐进增强的方式，因为业务优先，提升用户体验永远不会排在最前面。
>
> - 例如新浪微博网站这样亿级用户的网站，前端的更新绝不可能追求某个特效而不考虑低版本用户是否可用。一定是确保低版本到高版本的可访问性再渐进增强。
> - 如果开发的是一面面向青少面的软件或网站，你明确这个群体的人总是喜欢尝试新鲜事物，喜欢炫酷的特效，喜欢把软件更新至最新版本，这种情况再考虑优雅降级。



### 17. 介绍下 *positon* 属性

> 参考答案：
>
> position 属性主要用来定位，常见的属性值如下：
>
> - `absolute` 绝对定位，相对于 `static` 定位以外的第一个父元素进行定位。
>
> - `relative` 相对定位，相对于其自身正常位置进行定位。
>
> - `fixed` 固定定位，相对于浏览器窗口进行定位。
>
> - `static` 默认值。没有定位，元素出现在正常的流中。
>
> - `inherit` 规定应该从父元素继承 position 属性的值。
> - `sticky` 粘性定位，当元素在容器中被滚动超过指定的偏移值时，元素在容器内固定在指定位置。



### 18. 如何用 *CSS* 实现一个三角形

> 参考答案：
>
> 可以利用 border 属性
>
> 利用盒模型的 `border` 属性上下左右边框交界处会呈现出平滑的斜线这个特点，通过设置不同的上下左右边框宽度或者颜色即可得到三角形或者梯形。
>
> 如果想实现其中的任一个三角形，把其他方向上的 `border-color` 都设置成透明即可。
>
> 示例代码如下：
>
> ```html
> <div></div>
> ```
>
> ```css
> div{
> width: 0;
> height: 0;
> border: 10px solid red;
> border-top-color: transparent;
> border-left-color: transparent;
> border-right-color: transparent;
> }
> ```



### 19. 如何实现一个自适应的正方形

> 参考答案：
>
> **方法1：利用 CSS3 的 vw 单位**
>
> `vw` 会把视口的宽度平均分为 100 份
>
> ```
> .square {
>  width: 10vw;
>  height: 10vw;
>  background: red;
> }
> ```
>
> **方法2：利用 margin 或者 padding 的百分比计算是参照父元素的 width 属性**
>
> ```
> .square {
>  width: 10%;
>  padding-bottom: 10%; 
>  height: 0; // 防止内容撑开多余的高度
>  background: red;
> }
> ```



### 20. 如何实现三栏布局

> 参考答案：
>
> 三栏布局是很常见的一种页面布局方式。左右固定，中间自适应。实现方式有很多种方法。
>
> **第一种：flex**
>
> ```
> <div class="container">
>  <div class="left">left</div>
>  <div class="main">main</div>
>  <div class="right">right</div>
> </div>
> .container{
>  display: flex;
> }
> .left{
>  flex-basis:200px;
>  background: green;
> }
> .main{
>  flex: 1;
>  background: red;
> }
> .right{
>  flex-basis:200px;
>  background: green;
> }
> ```
>
> **第二种：position + margin**
>
> ```
> <div class="container">
>  <div class="left">left</div>
>  <div class="right">right</div>
>  <div class="main">main</div>
> </div>
> body,html{
>  padding: 0;
>  margin: 0;
> }
> .left,.right{
>  position: absolute;
>  top: 0;
>  background: red;
> }
> .left{
>  left: 0;
>  width: 200px;
> }
> .right{
>  right: 0;
>  width: 200px;
> }
> .main{
>  margin: 0 200px ;
>  background: green;
> }
> ```
>
> **第三种：float + margin**
>
> ```
> <div class="container">
>  <div class="left">left</div>
>  <div class="right">right</div>
>  <div class="main">main</div>
> </div>
> body,html{
>  padding:0;
>  margin: 0;
> }
> .left{
>  float:left;
>  width:200px;
>  background:red;
> }
> .main{
>  margin:0 200px;
>  background: green;
> }
> .right{
>  float:right;
>  width:200px;
>  background:red;
> }
> ```



### 21. *import* 和 *link* 区别

> 参考答案：
>
> 1. 从属关系区别
>
> `@import`是 CSS 提供的语法规则，只有导入样式表的作用；`link`是HTML提供的标签，不仅可以加载 CSS 文件，还可以定义 RSS、rel 连接属性等。
>
> 2. 加载顺序区别
>
> 加载页面时，`link`标签引入的 CSS 被同时加载；`@import`引入的 CSS 将在页面加载完毕后被加载。
>
> 3. 兼容性区别
>
> `@import`是 CSS2.1 才有的语法，故只可在 IE5+ 才能识别；`link` 标签作为 HTML 元素，不存在兼容性问题。
>
> 4. DOM可控性区别
>
> 可以通过 JS 操作 DOM ，插入`link`标签来改变样式；由于DOM方法是基于文档的，无法使用`@import`的方式插入样式。



### 22. 说说你对 *BFC* 的理解

> 参考答案：
>
> 块级格式化上下文 *Block Formatting Context* 是 CSS 规范中一个概念，决定元素的内容如何渲染以及与其他元素的关系。存在5条规则：
>
> 1. *BFC* 有隔离作用，内部元素不受外部元素影响，反之亦然。
> 2. 一个元素只能存在于一个 *BFC* 中，如果能同时存在于两个 *BFC* 中，那么就违反了 *BFC* 的隔离原则。
> 3. *BFC* 内的元素按正常流排列，元素间的间隙由元素外边距控制。
> 4. *BFC* 中的内容不会与外面的浮动元素重叠。
> 5. 计算 *BFC* 的高度需要包括 *BFC* 内的浮动子元素的高度。



### 23. 清除浮动的方法

> 参考答案：
>
> - clear 清除浮动（添加空div法）在浮动元素下方添加空div，并给该元素写css样式： {clear:both;height:0;overflow:hidden;}
>
> - 给浮动元素父级设置高度
>
> - 父级同时浮动（需要给父级同级元素添加浮动）
>
> - 父级设置成inline-block，其margin: 0 auto居中方式失效
>
> - 给父级添加overflow:hidden 清除浮动方法
>
> - 万能清除法 after 伪类清浮动（现在主流方法，推荐使用）



### 24. 说说选择器的权重计算方式

> 参考答案：
>
> *!important* 最高，\* 为0，行内样式 A 组加一，id 选择器 B 组加一，类、伪类、属性选择器 C 组加一，元素、伪元素 D 组加一。
>
> <img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-11-03-10806.png" alt="image-20210816194530798" style="zoom:50%;" />
>
> 其中 A 组权重值为 1000，B 组权重值为 100，C 组权重值为 10，D 组权重值为 1。



### 25. *css reset* 和 *normalize.css* 有什么区别？

> 参考答案：
>
> - 两者都是通过重置样式，保持浏览器样式的一致性
>
> - 前者几乎为所有标签添加了样式，后者保持了许多浏览器样式，保持尽可能的一致
>
> - 后者修复了常见的桌面端和移动端浏览器的 bug：包含了 HTML5 元素的显示设置、预格式化文字的 font-size 问题、在 IE9 中 SVG 的溢出、许多出现在各浏览器和操作系统中的与表单相关的 bug。
>
> - 前者中含有大段的继承链
>
> - 后者模块化，文档较前者来说丰富



### 26. 说说两种盒模型以及区别

> 参考答案：
>
> 盒模型也称为框模型，就是从盒子顶部俯视所得的一张平面图，用于描述元素所占用的空间。它有两种盒模型，W3C盒模型和IE盒模型（IE6以下，不包括IE6以及怪异模式下的IE5.5+）
>
> 理论上两者的主要区别是二者的盒子宽高是否包括元素的边框和内边距。当用CSS给给某个元素定义高或宽时，IE盒模型中内容的宽或高将会包含内边距和边框，而W3C盒模型并不会。



### 27. 如何避免重绘或者重排？

> 参考答案：
>
> 1. **集中改变样式**
>
> 我们往往通过改变 class 的方式来集中改变样式
>
> ```
> // 判断是否是黑色系样式
> const theme = isDark ? 'dark' : 'light'
> 
> // 根据判断来设置不同的class
> ele.setAttribute('className', theme)
> ```
>
> 2. **使用 DocumentFragment**
>
> 我们可以通过createDocumentFragment创建一个游离于DOM树之外的节点，然后在此节点上批量操作，最后插入DOM树中，因此只触发一次重排
>
> ```
> var fragment = document.createDocumentFragment();
> 
> for (let i = 0;i<10;i++){
>   let node = document.createElement("p");
>   node.innerHTML = i;
>   fragment.appendChild(node);
> }
> 
> document.body.appendChild(fragment);
> ```
>
> 3. **提升为合成层**
>
> 将元素提升为合成层有以下优点：
>
> - 合成层的位图，会交由 GPU 合成，比 CPU 处理要快
> - 当需要 repaint 时，只需要 repaint 本身，不会影响到其他的层
> - 对于 transform 和 opacity 效果，不会触发 layout 和 paint
>
> 提升合成层的最好方式是使用 CSS 的 will-change 属性：
>
> ```
> #target {
>   will-change: transform;
> }
> ```



### 28. 如何触发重排和重绘？

> 参考答案：
>
> 任何改变用来构建渲染树的信息都会导致一次重排或重绘：
>
> - 添加、删除、更新DOM节点
> - 通过display: none隐藏一个DOM节点-触发重排和重绘
> - 通过visibility: hidden隐藏一个DOM节点-只触发重绘，因为没有几何变化
> - 移动或者给页面中的DOM节点添加动画
> - 添加一个样式表，调整样式属性
> - 用户行为，例如调整窗口大小，改变字号，或者滚动。



### 29. 重绘与重排的区别？

> 参考答案：
>
> - 重排: 部分渲染树（或者整个渲染树）需要重新分析并且节点尺寸需要重新计算，表现为重新生成布局，重新排列元素
> - 重绘: 由于节点的几何属性发生改变或者由于样式发生改变，例如改变元素背景色时，屏幕上的部分内容需要更新，表现为某些元素的外观被改变
>
> 单单改变元素的外观，肯定不会引起网页重新生成布局，但当浏览器完成重排之后，将会重新绘制受到此次重排影响的部分
>
> 重排和重绘代价是高昂的，它们会破坏用户体验，并且让UI展示非常迟缓，而相比之下重排的性能影响更大，在两者无法避免的情况下，一般我们宁可选择代价更小的重绘。
>
> 『重绘』不一定会出现『重排』，『重排』必然会出现『重绘』。


### 30. 如何优化图片

> 参考答案：
>
> 1. 对于很多装饰类图片，尽量不用图片，因为这类修饰图片完全可以用 CSS 去代替。
>
> 2. 对于移动端来说，屏幕宽度就那么点，完全没有必要去加载原图浪费带宽。一般图片都用 CDN 加载，可以计算出适配屏幕的宽度，然后去请求相应裁剪好的图片。
>
> 3. 小图使用 *base64* 格式
>
> 4. 将多个图标文件整合到一张图片中（雪碧图）
>
> 5. 选择正确的图片格式：
>
> - 对于能够显示 WebP 格式的浏览器尽量使用 WebP 格式。因为 WebP 格式具有更好的图像数据压缩算法，能带来更小的图片体积，而且拥有肉眼识别无差异的图像质量，缺点就是兼容性并不好
> - 小图使用 PNG，其实对于大部分图标这类图片，完全可以使用 SVG 代替
> - 照片使用 JPEG



### 31. 描述一下渐进增强和优雅降级之间的不同

>参考答案：
>
>渐进增强 progressive enhancement：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。
>
>优雅降级 graceful degradation：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。
>
>区别：优雅降级是从复杂的现状开始，并试图减少用户体验的供给，而渐进增强则是从一个非常基础的，能够起作用的版本开始，并不断扩充，以适应未来环境的需要。降级（功能衰减）意味着往回看；而渐进增强则意味着朝前看，同时保证其根基处于安全地带。



### 32. 如何在页面上实现一个圆形的可点击区域？

>参考答案：
>
>首先使用 *CSS3* 新增的 *border-radius* 属性来将一个区域变成圆形，然后再使用 *JS* 来绑定事件。



### 33. 什么是渐进式渲染（*progressive rendering*）？

>参考答案：
>
>渐进式渲染是用于提高网页性能（尤其是提高用户感知的加载速度），以尽快呈现页面的技术。
>
>在以前互联网带宽较小的时期，这种技术更为普遍。如今，移动终端的盛行，而移动网络往往不稳定，渐进式渲染在现代前端开发中仍然有用武之地。
>
>一些举例：
>
>- 图片懒加载——页面上的图片不会一次性全部加载。当用户滚动页面到图片部分时，JavaScript 将加载并显示图像。
>- 确定显示内容的优先级（分层次渲染）——为了尽快将页面呈现给用户，页面只包含基本的最少量的 CSS、脚本和内容，然后可以使用延迟加载脚本或监听`DOMContentLoaded`/`load`事件加载其他资源和内容。
>- 异步加载 HTML 片段——当页面通过后台渲染时，把 HTML 拆分，通过异步请求，分块发送给浏览器。



### 34. *CSS3* 新增了那些东西？

>参考答案：
>
>*CSS3* 新增东西众多，这里列举出一些关键的新增内容：
>
>- 选择器
>- 盒子模型属性：*border-radius、box-shadow、border-image*
>- 背景：*background-size、background-origin、background-clip*
>- 文本效果：*text-shadow、word-wrap*
>- 颜色：新增 *RGBA，HSLA* 模式
>- 渐变：线性渐变、径向渐变
>- 字体：*@font-face*
>- 2D/3D转换：*transform、transform-origin*
>- 过渡与动画：*transition、@keyframes、animation*
>- 多列布局
>- 媒体查询


### 35. 实现一根只有 *1px* 的长线怎么实现?

>参考答案：
>
>实现的方式很多，下面是一种参考方案：
>
>```css
>.line {
>width: 100%;
>height: 1px;
>overflow: hidden;
>font-size: 0px; 
>border-bottom: dashed 1px #ccc;
>}
>```
>
>```html
><div class="line"></div>
>```



### 36. *box-sizing* 有什么作用？

>参考答案：
>
>*box-sizing* 是用于告诉浏览器如何计算一个元素是总宽度和总高度，主要用来切换标准盒模型和 IE 盒子。
>
>- 标准盒模型 box-sizing: content-box
>  content-box:
>  width  = content width;
>  height = content height
>
>- IE盒模型      box-sizing: border-box
>  border-box:
>  width  = border + padding + content width
>  heigth = border + padding + content heigth



### 37. *img* 标签在页面中有 *1px* 的边框，怎么处理

> 参考答案：
>
> ```css
> img{
> 	border: 0;
> }
> ```



### 38. 如何绝对居中（不用定位）（看书网）

> 参考答案：
>
> 方法一：
>
> 使用 *flex* 弹性盒来绝对居中
>
> 方法二：
>
> 设置 *margin-left* 为 *calc(50% - 50px);*



### 39. 我有5个div在一行，我要让div与div直接间距10px且最左最右两边的div据边框10px，同时在我改变窗口大小时，这个10px不能变，div根据窗口改变大小（长天星斗）

> 参考答案：
>
> ```css
> *{
> margin: 0;
> }
> .container {
> display: flex;
> }
> .container>div{
> outline: 1px solid;
> margin: 0 5px;
> flex-grow: 1;
> }
> 
> .container>div:first-child {
> margin-left: 10px;
> }
> .container>div:last-child {
> margin-right: 10px;
> }
> ```
>
> ```html
> <div class="container">
> <div>1</div>
> <div>2</div>
> <div>3</div>
> <div>4</div>
> <div>5</div>
> </div>
> ```


### 40. 什么是响应式设计

> 参考答案：
>
> 响应式设计简而言之，就是一个网站能够兼容多个终端——而不是为每个终端做一个特定的版本。
>
> 优点：
>
> - 面对不同分辨率设备灵活性强
> - 能够快捷解决多设备显示适应问题
>
> 缺点：
>
> 兼容各种设备工作量大，效率低下
>
> 代码累赘，会出现隐藏无用的元素，加载时间加长
>
> 其实这是一种折中性质的设计解决方案，多方面因素影响而达不到最佳效果
>
> 一定程度上改变了网站原有的布局结构，会出现用户混淆的情况
>
> 具体步骤：
>
> - 第一步：meta 标签
>
> 为了适应屏幕，多数的移动浏览器会把HTML网页缩放到设备屏幕的宽度。你可以使用meta标签的viewport属性来设置。下面的代码告诉浏览器使用设备屏幕宽度作为内容的宽度，并且忽视初始的宽度设置。这段代码写在 `<head>`里面
>
> ```
> <meta name="viewport" content="width=device-width, initial-scale=1.0">
> ```
>
> IE8及以下的浏览器不支持media query。你可以使用 media-queries.js 或 respond.js 。这样IE就能支持media query了。
>
> ```
> <!--[if lt IE 9]> <script src="http://css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js"></script> <![endif]-->
> ```
>
> - 第二步：HTML 结构
>
> 这个例子里面，有header、content、sidebar和footer等基本的网页布局。
>
> header 有固定的高180px，content 容器的宽是600px，sidebar的宽是300px。
>
> - 第三步：Media Queries
>
> CSS3 media query 响应式网页设计的关键。它像一个 if 语句，告诉浏览器如何根据特定的屏幕宽口来加载网页。
>
> 下面是一个媒体查询示例代码：
>
> ```js
> @media screen and (max-width: 300px) {
>     body {
>         background-color:lightblue;
>     }
> }
> ```
>
> 如果文档宽度小于 300 像素则修改背景演示(background-color)


### 41. 如何做响应式？

> 参考答案：
>
> 可以使用 *CSS3* 新增的媒体查询。
>
> 媒体查询的语法如下：
>
> ```css
> @media mediatype and|not|only (media feature) { CSS-Code;}
> ```


### 42. *bootstrap* 响应式的原理是什么

> 参考答案：
>
> *bootstrap* 使用的是栅格布局。栅格布局的实现原理，是通过定义容器大小，平分 *12* 份，再调整内外边距，最后结合媒体查询，就制作出了强大的响应式网格系统。



### 43. 块级元素转行内元素除了 *display:inline* 还有什么？ 

> 参考答案：
>
> *display:inline-block*

