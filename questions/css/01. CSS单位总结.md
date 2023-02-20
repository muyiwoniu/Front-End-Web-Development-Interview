# *CSS* 单位总结



## 经典真题



- *px* 和 *em* 的区别 



## *CSS* 中的哪些单位



首先，在 *CSS* 中，单位分为两大类，**绝对长度单位**和**相对长度单位**。



### 绝对长度单位



我们先来说这个，绝对长度单位最好理解，和我们现实生活中是一样的。在我们现实生活中，常见的长度单位有米（*m*）、厘米（*cm*）、毫米（*mm*），每一种单位的长度都是固定，比如 *5cm*，你走到任何地方 *5cm* 的长度都是一致的



例如：



```html
<div class="container"></div>
```

```css
.container{
  width: 5cm;
  height: 5cm;
  background-color: pink;
}
```

在上面的代码中，我们设置了盒子的宽高都是 *5cm*，这里用的就是绝对长度单位。



常见的绝对单位长度如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-14-073818.png" alt="image-20210914153818508" style="zoom:50%;" />

这些值中的大多数在用于打印时比用于屏幕输出时更有用。例如，我们通常不会在屏幕上使用 *cm*。

惟一一个经常使用的值，估计就是 *px*(像素)。



### 相对长度单位



相对长度单位相对于其他一些东西，比如父元素的字体大小，或者视图端口的大小。使用相对单位的好处是，经过一些仔细的规划，我们可以使文本或其他元素的大小与页面上的其他内容相对应。



下表列出了 *web* 开发中一些最有用的单位。



![image-20210914154021389](https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-14-074021.png)



上面的单位中，常用的有 *em、rem、vw、vh*，其中 *vw* 和 *vh* 代表的是视口的宽度和高度，例如：



```html
<div class="container"></div>
```

```css
*{
  margin: 0;
  padding: 0;
}
.container {
  width: 50vw;
  height: 100vh;
  background-color: pink;
}
```

在上面的代码中，我们设置了容器的宽度为 *50vw*，也就是占视口的一半，而高度我们设置的是 *100vh*，就是占满整个视图。



接下来来看一下 *em* 和 *rem*。

*em* 和 *rem* 相对于 *px* 更具有灵活性，他们是相对长度单位，意思是长度不是定死了的，更适用于响应式布局。

对于 *em* 和 *rem* 的区别一句话概括：***em* 相对于父元素，*rem* 相对于根元素。**



来看关于 *em* 和 *rem* 示例。

*em* 示例

```html
<div>
  我是父元素div
  <p>
    我是子元素p
    <span>我是孙元素span</span>
  </p>
</div>
```

```css
* {
  margin: 0;
  padding: 0;
}

div {
  font-size: 40px;
  width: 10em;
  /* 400px */
  height: 10em;
  outline: solid 1px black;
  margin: 10px;
}

p {
  font-size: 0.5em;
  /* 20px */
  width: 10em;
  /* 200px */
  height: 10em;
  outline: solid 1px red;
}

span {
  font-size: 0.5em;
  width: 10em;
  height: 10em;
  outline: solid 1px blue;
  display: block;
}
```

效果：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-14-075220.png" alt="image-20210914155219732" style="zoom:50%;" />



 

*rem* 示例



*rem* 是全部的长度都相对于根元素，根元素是谁？

那就是 *html*元素。通常做法是给 *html* 元素设置一个字体大小，然后其他元素的长度单位就为 *rem*。

例如：

```html
<div>
  我是父元素div
  <p>
    我是子元素p
    <span>我是孙元素span</span>
  </p>
</div>
```

```css
* {
  margin: 0;
  padding: 0;
}

html {
  font-size: 10px;
}

div {
  font-size: 4rem;
  /* 40px */
  width: 30rem;
  /* 300px */
  height: 30rem;
  /* 300px */
  outline: solid 1px black;
  margin: 10px;
}

p {
  font-size: 2rem;
  /* 20px */
  width: 15rem;
  /* 150px */
  height: 15rem;
  /* 150px */
  outline: solid 1px red;
}

span {
  font-size: 1.5rem;
  width: 10rem;
  height: 10rem;
  outline: solid 1px blue;
  display: block;
}
```



所以当用 *rem* 做响应式时，直接在媒体中改变 *html* 的 *font-size*，此时用 *rem* 作为单位的元素的大小都会相应改变，很方便。

看到这里我想大家都能够更深刻的体会了 *em* 和 *rem* 的区别了，其实就是参照物不同。



## 真题解答



- *px* 和 *em* 的区别 

> 参考答案：
>
> *px* 即 *pixel* 像素，是相对于屏幕分辨率而言的，是一个绝对单位，但是具有一定的相对性。因为在同一设备上每个设备像素所代表的物理长度是固定不变的（绝对性），但在不同设备间每个设备像素所代表的物理长度是可以变化的（相对性）。
>
> *em* 是相对长度单位，具体的大小要相对于父元素来计算，例如父元素的字体大小为 *40px*，那么子元素 *1em* 就代表字体大小和父元素一样为 *40px*，*0.5em* 就代表字体大小为父元素的一半即 *20px*。



-*EOF*-

