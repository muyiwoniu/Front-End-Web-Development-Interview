# *CSS3* 变形



## 经典真题



- 请简述一下 *CSS3* 中新增的变形如何使用？



## *CSS3* 变形相关知识



### 变形介绍



*CSS2.1* 中的页面都是静态的，多年来，*Web* 设计师依赖于图片、*Flash* 或者 *JavaScript* 才能完成修改页面的外观。*CSS3* 改变了设计师这种思维，借助 *CSS3* 可以轻松的倾斜、缩放、移动以及翻转元素。



*2012* 年 *9* 月，*W3C* 组织发布了 *CSS3* 变形工作草案。允许 *CSS* 把元素变成 *2D* 或者 *3D* 空间，这其实就是 *CSS3* 的 *2D* 变形和 *3D* 变形。



*CSS3* 变形是一些效果的集合，比如平移、旋转、缩放和倾斜效果，每个效果通过变形函数（*transform function*）来实现。在此之前，要想实现这些效果，必须依赖图片、*Flash* 或者 *JavaScript* 才能完成，而现在仅仅使用纯 *CSS* 就能够实现，大大的提高了开发效率以及页面的执行效率。



变形效果要通过变形函数来实现，语法如下：

```css
transform: none|transform-functions;
```

那么 *CSS3* 中为我们提供了哪些变形函数呢？

这里我们整体可以划分出 *3* 大类：

- 具有 *X/Y* 的函数：*translateX、translateY、sclaeX、scaleY、skewX、skewY*
- *2D* 变形函数：*translate、sclae、rotate、skew、matrix*
- *3D* 变形函数：*rotateX、rotateY、rotate3d、translateZ、translate3d、scaleZ、scale3d、matrix3d*



此时，你可能已经做好了逐一击破每个变形函数的思想准备了。

别急，在介绍每个变形函数之前，我们先来了解一下变形相关的属性。



### 变形属性



1. ***transform* 属性**



第一个首当其冲的就是 *transform* 属性，该属性所对应的属性值就是一系列的变形函数，例如：

```css
transform: scale(1.5)
```

上面的代码中，我们设置了 *transform* 属性，属性值为 *scale* 变形函数。



2. ***transform-origin* 属性**

接下来第二个是 *transform-origin* 属性，该属性用于设置元素的中心点位置。该属性语法如下：

```css
transform-origin: x-axis y-axis z-axis;
```

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-080037.png" alt="image-20210919160036555" style="zoom:50%;" />



为了演示此属性的作用，我们先透支一点后面的知识。

这里我们以 *rotate* 为例。*rotate* 是变形函数中的一个，作用是旋转元素，其语法如下：

```css
rotate(angle)
```

定义 *2D* 旋转，在参数中规定角度。

来看一个例子：

```html
<div></div>
```

```css
div{
  width: 150px;
  height: 150px;
  background-color: red;
  margin: 200px;
  transition: all 1s;
}
div:hover{
  transform: rotate(45deg);
}
```

效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-080641.gif" alt="2021-09-19 16.06.26" style="zoom:50%;" />

在上面的示例中，我们设置 *div* 鼠标 *hover* 的时候进行变形，旋转 *45* 度，为了更加平滑，我们加入了 *transition* 过渡效果。

我们观察整个元素的旋转中心点，是在元素的最中央。



接下来我们可以使用 *transform-origin* 来修改这个中心点的位置。例如：

```css
div{
  width: 150px;
  height: 150px;
  background-color: red;
  margin: 200px;
  transition: all 1s;
  transform-origin: bottom left;
  /* 修改元素的中心点位置 */
}
div:hover{
  transform: rotate(45deg);
}
```

效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-081002.gif" alt="2021-09-19 16.09.46" style="zoom:50%;" />

在上面的代码中，我们修改了元素的中心点位置为 *bottom、left*，也就是左下角。可以看到因为元素的中心点位置发生了变化，旋转的方式也随之发生了改变。



*transform-origin* 的属性值除了像上面一样设置关键词以外，也可以是百分比、*em、px* 等具体的值。

*transform-origin* 的第三个参数是定义 *Z* 轴的距离，这在后面介绍 *3D* 变形时再来介绍。



3. ***transform-style* 属性**



*transform-style* 属性是 *3D* 空间一个重要属性，指定了嵌套元素如何在 *3D* 空间中呈现。语法如下：

```css
transform-style: flat | preserve-3d;
```

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-091723.png" alt="image-20210919171722656"  />

其中 *flat* 为默认值。

需要注意的是该属性需要设置在父元素上面，使其变成一个真正的 *3D* 图形。

当然光看属性说明是非常模糊的，一点都不直观，我们直接来看一个例子。

```html
<div class="box">
  <div class="up">上</div>
  <div class="down">下</div>
  <div class="left">左</div>
  <div class="right">右</div>
  <div class="forword">前</div>
  <div class="back">后</div>
</div>
```

```css
.box {
  width: 250px;
  height: 250px;
  border: 1px dashed red;
  margin: 100px auto;
  position: relative;
  border-radius: 50%;
  transform-style: preserve-3d;
  animation: gun 8s linear infinite;
}

.box>div {
  width: 100%;
  height: 100%;
  position: absolute;
  text-align: center;
  line-height: 250px;
  font-size: 60px;
  color: #daa520;
}

.left {
  background-color: rgba(255, 0, 0, 0.3);
  transform-origin: left;
  transform: rotateY(90deg) translateX(-125px);
}

.right {
  background: rgba(0, 0, 255, 0.3);
  transform-origin: right;
  /* 变换*/
  transform: rotateY(90deg) translateX(125px);
}
.forward {
  background: rgba(255, 255, 0, 0.3);
  transform: translateZ(125px);
}
.back {
  background: rgba(0, 255, 255, 0.3);
  transform: translateZ(-125px);
}
.up {
  background: rgba(255, 0, 255, 0.3);
  transform: rotateX(90deg) translateZ(125px);
}
.down {
  background: rgba(99, 66, 33, 0.3);
  transform: rotateX(-90deg) translateZ(125px);
}
@keyframes gun {
  0% {
    transform: rotateX(0deg) rotateY(0deg);
  }

  100% {
    transform: rotateX(360deg) rotateY(360deg);
  }
}
```

效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-092030.gif" alt="2021-09-19 17.20.08" style="zoom:50%;" />

上面的 *CSS* 代码不用具体去关心，我们只看在 *box* 元素上面添加了一句 *transform-style: preserve-3d*，表示 *box* 里面的子元素都以 *3D* 的形式呈现。如果我们把这行代码去除掉或者值修改为 *flat*，效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-092331.gif" alt="2021-09-19 17.23.06" style="zoom:50%;" />

怎么样？是不是非常直观，一下子就知道 *transform-style* 属性的作用是什么了。该属性就是指定子元素是在 *3D* 空间还是 *2D* 平面中显示。



4. ***perspective* 属性**



*perspective* 属性用于设置查看者的位置，可以将可视内容映射到一个视锥上，继而投到一个 *2D* 视平面上。如果不指定该属性，则 *Z* 轴空间中所有点将平铺到同一个 *2D* 视平面中，并且在变换结果中将不存在景深概念。



简单理解，就是视距，用来设置用户和元素 *3D* 空间 *Z* 平面之间的距离。而其效应由他的值来决定，值越小，用户与 *3D* 空间 *Z* 平面距离越近，视觉效果更令人印象深刻；反之，值越大，用户与 *3D* 空间 *Z* 平面距离越远，视觉效果就很小。



<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-094151.png" alt="image-20210919174151244" style="zoom:50%;" />



注意当为元素定义 *perspective* 属性时，其子元素会获得透视效果，而不是元素本身。

我们还是来看一个直观的例子来了解这个属性的作用。例如：

```html
<div class="container">
  <div class="item"></div>
</div>
```

```css
.container{
  width: 500px;
  height: 500px;
  border: 1px solid;
  margin: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.item{
  width: 150px;
  height: 150px;
  background-color: red;
  animation: rotateAnimation 5s infinite;
}
@keyframes rotateAnimation {
  0%{
    transform: rotateY(0deg);
  }
  100%{
    transform: rotateY(360deg);
  }
}
```

效果：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-095136.gif" alt="2021-09-19 17.51.16" style="zoom:50%;" />

在上面的代码中，我们虽然设置了 *div.item* 沿着 *Y* 轴进行旋转，但是由于没有设置 *perspective* 视距，所以看上去就像是 *div* 盒子在宽度伸缩一样，*3D* 效果并不明显。

此时我们可以给父元素 *div.container* 设置 *perspective* 视距，例如：

```css
.container{
  ...
  perspective: 1200px;
}
```

效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-095428.gif" alt="2021-09-19 17.54.08" style="zoom:50%;" />

很明显，加入  *perspective* 视距后，*3D* 旋转效果更佳的真实。

关于 *perspective* 的取值，接受一个长度单位大于 *0*，其单位不能为百分比的值。大致能够分为如下 *3* 种情况：

- *none* 或者不设置：没有 *3D* 空间。
- 取值越小：*3D* 效果越明显，也就是眼睛越靠近真 *3D*。
- 取值无穷大或者为 *0*：与取值为 *none* 的效果一样。



5. ***perspective-origin* 属性**



如果理解了上面的 *perspective* 属性，那么这个 *perspective-origin* 就非常好理解了，该属性用来决定 *perspective* 属性的源点角度。

其语法如下：

```css
perspective-origin: x-axis y-axis;
```

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-124302.png" alt="image-20210919204301259"  />

举个例子：

```css
.container{
  ...
  perspective: 600px;
  perspective-origin: top;
}
```

效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-124520.gif" alt="2021-09-19 20.45.00" style="zoom:50%;" />

由于我们设置的 *perspective-origin* 的值为 *top*，所以会呈现一种俯视的效果。如果将其值修改为 *bottom*，则会是仰视的即视感，如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-124646.gif" alt="2021-09-19 20.46.34" style="zoom:50%;" />



6. ***backface-visibility* 属性**



*backface-visibility* 属性决定元素旋转背面是否可见。对于未旋转的元素，该元素的正面面向观看者。当其旋转 *180* 度时会导致元素的背面面向观众。

该属性是设置在旋转的元素上面，语法如下：

```css
backface-visibility: visible|hidden;
```

![image-20210919205231265](https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-125232.png)

来看一个具体的例子：

```css
.item{
  ...
  backface-visibility: hidden;
}
```

效果：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-133845.gif" alt="2021-09-19 21.38.25" style="zoom:50%;" />

在上面的代码中，我们在子元素 *div.item* 上设置了 *backface-visibility: hidden*，当此元素旋转 *180* 度到背面时，我们可以发现此时是无法看到背面的。



### *2D* 变形



介绍完 *CSS3* 中变形的相关属性后，接下来我们就该来看一下具体的变形函数了。

整个 CSS3 为我们提供了相当丰富的变形函数，有 *2D* 的，有 *3D* 的。这里我们先来看 *2D* 的变形函数。



#### *2D* 位移



*2D* 位移对应有 *3* 个变形函数，分别是 *translate、translateX、translateY*

用法也非常简单，*translate* 方法从其当前位置移动元素（根据为 *X* 轴和 *Y* 轴指定的参数）。

```css
div {
  transform: translate(50px, 100px);
}
```

上面的例子把 *div* 元素从其当前位置向右移动 *50* 个像素，并向下移动 *100* 个像素：效果如下：



![Translate](https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-134634.png)



#### *2D* 缩放



*2D* 缩放对应的也有 *3* 个变形函数，分别是 *sclae、sclaeX、sclaeY*

该方法能够按照倍数放大或缩小元素的大小（根据给定的宽度和高度参数）。默认值为 *1*，小于这个值就是缩小，大于这个值就是放大。

```css
div {
  transform: scale(2, 3);
}
```

上面的例子把 *div* 元素增大为其原始宽度的两倍和其原始高度的三倍，效果如下：



![Scale](https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-135001.png)





#### *2D* 旋转



*2D* 旋转对应的只有 *1* 个变形函数 *rotate*，这个我们在前面也已经用过了。

该变形函数只接受一个值代表旋转的角度值，取值可正可负，正值代表顺时针旋转，负值代表逆时针旋转。

```css
div {
  transform: rotate(20deg);
}
```

上面的例子把 *div* 元素顺时针旋转 *20* 度，效果如下：



![Rotate](https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-135300.png)



#### *2D* 倾斜



*2D* 倾斜对应的变形函数也是 *3* 个，分别是 *skew、skewX、skewY*

语法如下：

```css
skew(ax, ay)
```

- *ax*：指定元素水平方向（*X* 轴方向）倾斜角度
- *ay*：指定元素垂直方向（*Y* 轴方向）倾斜角度

```html
<div></div>
```

```css
div{
  width: 150px;
  height: 150px;
  background-color: red;
  margin: 150px;
  transition: all 1s;
}
div:hover{
  transform: skew(20deg);
}
```

效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-140231.gif" alt="2021-09-19 22.02.15" style="zoom:50%;" />



### *2D* 矩阵



虽然 *CSS3* 为我们提供了上述的变形函数方便我们进行元素的变形操作，但是毕竟函数个数有限，有些效果是没有提供的，例如镜像翻转的效果。此时就轮到 *2D* 矩阵函数 *matrix* 登场了。



*matrix* 有六个参数：

```css
matrix(a,b,c,d,e,f)
```

六个参数对应的矩阵：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-141132.png" alt="image-20210919221131755" style="zoom:50%;" />

这六个参数组成的矩阵与原坐标矩阵相乘计算坐标。计算方式如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-19-141105.png" alt="image-20210919221104828" style="zoom:50%;" />

什么意思呢 ？*x* 和 *y* 是元素中每一个像素的初始原点的坐标，而 *x'* 和 *y'* 是通过矩阵变化后得到的新原点坐标。通过中间 *3 x 3* 变换矩阵，对原先的坐标施加变换，从而得到新的坐标。

*x' = ax+cy+e*，表示变换后的**水平**坐标

*y' = bx+dy+f*，表示变换后的**垂直**位置



在 *CSS3* 中，上面我们所介绍的所有 *2D* 变形函数都能够通过这个 *matrix* 矩阵函数来替代。



**矩阵实现偏移**



我们首先来看通过矩阵实现偏移效果。

偏移效果前后 *x、y* 与 *x'、y'* 所对应的坐标公式为：

```
x' = x + 偏移量
y' = y + 偏移量
```

套用上面的公式那么各个参数的取值就应该是：

```
a = 1; b = 0;
c = 0; d = 1;
e = x 偏移量; f = y 偏移量
x' = ax+cy+e = 1x + 0y + x 偏移量 = x + x 偏移量
y' = bx+dy+f = 0x + 1y + y 偏移量 = y + y 偏移量
```

所以换成 *matrix* 函数就应该是：

```
matrix(1, 0, 0, 1, x 偏移量, y 偏移量)
```

下面来做一个测试：

```html
<div></div>
```

```css
div{
  width: 150px;
  height: 150px;
  background-color: red;
  margin: 150px;
  transition: all 1s;
}
div:hover{
  /* transform: translate(100px, 200px); */
  transform: matrix(1, 0, 0, 1, 100, 200);
}
```

在上面的示例中，使用 *translate* 和 *matrix* 两个变形函数的效果一致。



**矩阵实现缩放**



缩放之后  *x、y* 与 *x'、y'* 所对应的坐标公式为：

```
x' = x * x 缩放倍数
y’ = y * y 缩放倍数
```

套用上面的公式那就是：

```
a = x缩放倍数; b = 0;
c = 0; d = y 缩放倍数;
e = 0; f = 0;
x' = ax+cy+e = x缩放倍数 * x + 0y + 0 = x缩放倍数 * x
y' = bx+dy+f = 0x + y 缩放倍数 * y + 0 = y 缩放倍数 * y
```

所以换成 *matrix* 函数就应该是：

```css
matrix(x 缩放倍数, 0, 0, y 缩放倍数, 0, 0);
```

例如：

```html
<div></div>
```

```css
div{
  width: 150px;
  height: 150px;
  background-color: red;
  margin: 150px;
  transition: all 1s;
}
div:hover{
  /* transform: scale(2, 2); */
  transform: matrix(2, 0, 0, 2, 0, 0);
}
```

上面的代码我们分别使用 *scale* 和矩阵实现了相同的缩放效果。



**矩阵实现旋转**



旋转需要实现

```
水平倾斜角度 =  - 垂直倾斜角度
```

旋转我们用到的变形函数是 *rotate(θ)*，其中 *θ* 为旋转的角度。套用上面的公式：

```
x' = xcosθ - ysinθ + 0 = xcosθ - ysinθ;
y' = xsinθ + ycosθ + 0 = xsinθ + ycosθ
```

转换为 *matrix* 的代码为：

```css
matrix(cos(θ), sin(θ), -sin(θ), cos(θ), 0, 0)
```

例如：

```html
<div></div>
```

```css
div{
  width: 150px;
  height: 150px;
  background-color: red;
  margin: 150px;
  transition: all 1s;
}
div:hover{
  /* transform: rotate(45deg); */
  transform: matrix(0.7, 0.7, -0.7, 0.7, 0, 0);
}
```

上面的代码使用了 *rotate* 和 *matrix* 来实现旋转 *45* 度的效果。其中 *sin45* 和 *cos45* 的值都为 *0.7*。



**矩阵实现倾斜**



*skew(θx, θy)* 将一个元素按指定的角度在 *X* 轴和 *Y* 轴上倾斜。

倾斜对应的公式为：

```
x' = x + ytan(θx) + 0 = x + ytan(θx)
y' = xtan(θy) + y + 0 = xtan(θy) + y
```

转换为 *matrix* 的代码为：

```css
matrix(1,tan(θy),tan(θx),1,0,0)
```

例如：

```html
<div></div>
```

```css
div {
  width: 150px;
  height: 150px;
  background-color: red;
  margin: 150px;
  transition: all 1s;
}

div:hover {
  /* transform: skew(20deg); */
  transform: matrix(1, 0, 0.4, 1, 0, 0);
}
```

上面的示例中分别使用 *skew* 和矩阵 *matrix* 实现了一致的倾斜效果。



**矩阵实现镜像变形**



前面介绍的效果，*CSS3* 中都提供了对应的变形函数，但是矩阵真正发挥威力是在没有对应的变形函数时，例如这里要讲的镜像变形。

我们先来看一下各种镜像变化 *x、y* 与 *x'、y'* 所对应的关系：

水平镜像，就是 *y* 坐标不变，*x* 坐标变负

```
x' = -x;
y' = y;
```

所以：

```
a = -1; b = 0; 
c = 0; d = 1; 
e = 0; f = 0;
```

具体示例如下：

```html
<div></div>
```

```css
div {
  width: 300px;
  height: 200px;
  margin: 150px;
  transition: all 1s;
  background: url('./ok.png') no-repeat;
  background-position: center;
  background-size: contain;
}

div:hover {
  transform: matrix(-1, 0, 0, 1, 0, 0);
}
```

效果：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-20-011654.gif" alt="2021-09-20 09.16.38" style="zoom:50%;" />



垂直镜像，就是 *x* 坐标不变，*y* 坐标变负

```
x' = x;
y' = -y;
```

所以：

```
a = 1; b = 0; 
c = 0; d = -1; 
e = 0; f = 0;
```

例如：

```css
...
div:hover {
  transform: matrix(1, 0, 0, -1, 0, 0);
}
```

效果：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-20-011934.gif" alt="2021-09-20 09.19.11" style="zoom:50%;" />



水平镜像 + 倒立就是 y 坐标变负，x 坐标变负

```
x' = -x;
y' = -y;
```

所以：

```
a = -1; b = 0; 
c = 0; d = -1; 
e = 0; f = 0;
```

例如：

```css
...
div:hover {
  transform: matrix(-1, 0, 0, -1, 0, 0);
}
```

效果：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-20-013005.gif" alt="2021-09-20 09.29.45" style="zoom:50%;" />

*90* 度旋转 + 镜像就是：

```
x' = y;
y' = x;
```

所以：

```
a = 0; b = 1; 
c = 1; d = 0; 
e = 0; f = 0;
```

例如：

```css
...
div:hover {
  transform: matrix(0, 1, 1, 0, 0, 0);
}
```

效果：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-20-013140.gif" alt="2021-09-20 09.31.24" style="zoom:50%;" />

*-90* 度旋转 + 镜像就是：

```
x' = -y;
y' = -x;
```

所以：

```
a = 0; b = -1; 
c = -1; d = -0; 
e = 0; f = 0;
```

例如：

```css
...
div:hover {
  transform: matrix(0, -1, -1, 0, 0, 0);
}
```

效果：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-20-013304.gif" alt="2021-09-20 09.32.47" style="zoom:50%;" />



通过上面一系列的示例，我们可以发现，使用矩阵 *matrix* 函数确实更佳灵活，能够写出各种变形效果。



### *3D* 变形



使用二维变形能够改变元素在水平和垂直轴线，然而还有一个轴沿着它，可以改变元素。使用三维变形，可以改变元素在 *Z* 轴位置。

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-20-024408.png" alt="image-20210920104407743" style="zoom: 50%;" />

三维变换使用基于二维变换的相同属性，如果熟悉二维变换就会发现，*3D* 变形的功能和 *2D* 变换的功能类似。CSS3 中的 3D 变换只要包含以下几类：



- ***3D* 位移**：包括 *translateZ* 和 *translate3d* 两个变形函数。
- ***3D* 旋转**：包括 *rotateX、rotateY、rotateZ* 和 *rotate3d* 这四个变形函数。
- ***3D* 缩放**：包括 *scaleZ* 和 *sclae3d* 两个变形函数。
- ***3D* 矩阵**：和 *2D* 变形一样，也有一个 *3D* 矩阵功能函数 *matrix3d*



#### *3D* 位移

我们直接来看合成变形函数 *translate3d*，其语法如下：

```css
translate3d(tx, ty, tz)
```

- *tx*：在 *X* 轴的位移距离。
- *ty*：在 *Y* 轴的位移距离。
- *tz*：在 *Z* 轴的位移距离。值越大，元素离观察者越近，值越小，元素离观察者越远

来看一个具体的示例：

```html
<div class="container">
  <div class="item"></div>
</div>
```

```css
.container{
  width: 400px;
  height: 400px;
  border: 1px solid;
  margin: 150px;
  display: flex;
  justify-content: center;
  align-items: center;
  perspective: 1000px;
}
.item {
  width: 300px;
  height: 200px;
  transition: all 1s;
  background: url('./ok.png') no-repeat;
  background-position: center;
  background-size: contain;
}

.item:hover {
  transform: translate3d(100px, 100px, -500px)
}
```

在上面的代码中，我们设置 *div.item* 被 *hover* 的时候进行 *3D* 位移，也就是 *X、Y、Z* 轴同时进行移动。注意这里要设置父元素的透视效果，也就是设置 *perspective* 值，否则看不出 *Z* 轴的移动效果。效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-20-024039.gif" alt="2021-09-20 10.40.24" style="zoom:50%;" />



#### *3D* 旋转



在三维变形中，可以让元素在任何轴旋转，对应的变形函数有 *rotateX、rotateY、rotateZ* 以及 *rotate3d*。



其中 *rotate3d* 就是前面 *3* 个变形函数的复合函数。不过出了 *x、y、z* 这三条轴的参数以外，还接受第四个参数 *a*，表示旋转角度。

```css
rotate3d(x, y, z, a)
```

- *x*：可以是 *0* 到 *1* 之间的数值，表示旋转轴 *X* 坐标方向的矢量。
- *y*：可以是 *0* 到 *1* 之间的数值，表示旋转轴 *Y* 坐标方向的矢量。
- *z*：可以是 *0* 到 *1* 之间的数值，表示旋转轴 *Z* 坐标方向的矢量。
- *a*：表示旋转角度。正的角度值表示顺时针旋转，负值表示逆时针旋转。

下面我们以 *rotateX* 变形函数为例：

```html
<div class="container">
  <div class="item"></div>
</div>
```

```css
.container{
  width: 400px;
  height: 400px;
  border: 1px solid;
  margin: 150px;
  display: flex;
  justify-content: center;
  align-items: center;
  perspective: 1000px;
}
.item {
  width: 150px;
  height: 150px;
  background-color: red;
  transition: all 1s;
}

.item:hover {
  transform: rotateX(45deg)
}
```

效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-21-132328.gif" alt="2021-09-21 21.23.14" style="zoom:50%;" />



#### *3D* 缩放



*3D* 缩放主要有 *sclaeZ* 和 *scale3d*，其中 *scale3d* 就是 *scaleX*、*scaleY* 以及 *scaleZ* 的复合变形函数。其语法如下：

```css
scale(sx, sy, sz)
```

- *sx*：*X* 轴上的缩放比例
- *sy*：*Y* 轴上的缩放比例
- *sz*：*Z* 轴上的缩放比例

但是 *scaleX* 和 *scaleY* 变形效果很明显，但是 *scaleZ*  却基本看不出有什么效果。原因很简单，*scaleZ* 是 *Z* 轴上面的缩放，也就是厚度上面的变化，所以如果不是立方体结构，基本上是看不出来 *Z* 轴上面的缩放效果的。



一般来讲，我们不会将 *scaleZ* 和 *scale3d* 单独使用，因为 *scaleZ* 和 *scale3d* 这两个变形函数在单独使用时没有任何效果，需要配合其他的变形函数一起使用时才会有效果。

这里我们以前面那个立方体为例，如下：

```html
<div class="box">
  <div class="up">上</div>
  <div class="down">下</div>
  <div class="left">左</div>
  <div class="right">右</div>
  <div class="forword">前</div>
  <div class="back">后</div>
</div>
```

```css
.box {
  width: 250px;
  height: 250px;
  border: 1px dashed red;
  margin: 100px auto;
  position: relative;
  border-radius: 50%;
  transform-style: preserve-3d;
  transition: all 1s;
  transform: rotateX(45deg) rotateY(45deg);
}

.box:hover{
  transform: rotateX(45deg) rotateY(45deg) scaleZ(.5);
}

.box>div {
  width: 100%;
  height: 100%;
  position: absolute;
  text-align: center;
  line-height: 250px;
  font-size: 60px;
  color: #daa520;
}

.left {
  background-color: rgba(255, 0, 0, 0.3);
  transform-origin: left;
  transform: rotateY(90deg) translateX(-125px);
}

.right {
  background: rgba(0, 0, 255, 0.3);
  transform-origin: right;
  /* 变换*/
  transform: rotateY(90deg) translateX(125px);
}

.forward {
  background: rgba(255, 255, 0, 0.3);
  transform: translateZ(125px);
}

.back {
  background: rgba(0, 255, 255, 0.3);
  transform: translateZ(-125px);
}

.up {
  background: rgba(255, 0, 255, 0.3);
  transform: rotateX(90deg) translateZ(125px);
}

.down {
  background: rgba(99, 66, 33, 0.3);
  transform: rotateX(-90deg) translateZ(125px);
}
```

效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-21-143413.gif" alt="2021-09-21 22.33.57" style="zoom:50%;" />

### *3D* 矩阵



*CSS3* 中的 *3D* 矩阵比 *2D* 矩阵复杂，从二维到三维，在矩阵里 3\*3 变成 4\*4，即 *9* 到 *16*。

对于 *3D* 矩阵而言，本质上很多东西与 *2D* 是一致的，只是复杂程度不一样而已。

对于 *3D* 缩放效果，其矩阵如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-21-145039.png" alt="image-20210921225039133" style="zoom:50%;" />

```css
matrix3d(sx, 0, 0, 0, 0, sy, 0, 0, 0, 0, sz, 0, 0, 0, 0, 1)
```

倾斜是二维变形，不能在三维空间变形。元素可能会在 *X* 轴和 *Y* 轴倾斜，然后转化为三维，但它们不能在 *Z* 轴倾斜。



这里举几个 *3D* 矩阵的例子：

*translate3d(tx,ty,tz)* 等价于 *matrix3d(1,0,0,0,0,1,0,0,0,0,1,0,tx,ty,tz,1)*

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-21-145544.png" alt="image-20210921225544059" style="zoom:50%;" />

*scale3d(sx,sy,sz)* 等价于 *matrix3d(sx,0,0,0,0,sy,0,0,0,0,sz,0,0,0,0,1)*

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-21-145618.png" alt="image-20210921225617731" style="zoom:50%;" />

*rotate3d(x,y,z,a)* 真是得搬出高中数学书好好复习一下了，第四个参数 *alpha* 用于 *sc* 和 *sq* 中

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-21-145731.png" alt="image-20210921225731310" style="zoom:50%;" />

等价于...你自己从上到下，从左到右依次将参数搬入 *matrix3d* 中吧。

当然除非是库函数需要，否则我严重怀疑是否会有人放着 *rotate3d* 不用，偏要去挑战用 *matrix3d* 模拟 *rotate3d*。



## 真题解答



- 请简述一下 *CSS3* 中新增的变形如何使用？

> 参考答案：
>
> 在 *CSS3* 中的变形分为 *2D* 变形和 *3D* 变形。
>
> 整体可以划分出 *3* 大类：
>
> - 具有 *X/Y* 的函数：*translateX、translateY、sclaeX、scaleY、skewX、skewY*
> - *2D* 变形函数：*translate、sclae、rotate、skew、matrix*
> - *3D* 变形函数：*rotateX、rotateY、rotate3d、translateZ、translate3d、scaleZ、scale3d、matrix3d*
>
> 另外，还有一些重要的变形属性，例如：
>
> - ***transform* 属性**
> - ***transform-origin* 属性**
> - ***transform-style* 属性**
> - ***perspective* 属性**
> - ***perspective-origin* 属性**
> - ***backface-visibility* 属性**



-*EOF*-

