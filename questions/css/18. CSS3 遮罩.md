# *CSS3* 遮罩



本文主要包含以下内容：



- *CSS3* 遮罩介绍
- 遮罩各属性介绍



## *CSS3* 遮罩介绍



*CSS mask* 遮罩属性的历史非常久远了，远到比 *CSS3 border-radius* 等属性还要久远，最早是出现在 *Safari* 浏览器上的，差不多可以追溯到 *2009* 年。

不过那个时候，遮罩只能作为实验性的属性，做一些特效使用。毕竟那个年代还是 *IE* 浏览器的时代，属性虽好，但价值有限。

但是如今情况却大有变化，除了 *IE* 浏览器不支持，*Firefox、Chrome、Edge* 以及移动端等都已经全线支持，其实际应用价值已不可同日而语。

尤其 *Firefox* 浏览器，从版本 *53* 开始，已经全面支持了 *CSS3 mask* 属性。并且 *mask* 属性规范已经进入候选推荐规范之列，会说以后进入既定规范标准已经是板上钉钉的事情，大家可以放心学习，将来必有用处。

![image-20211025225724975](https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-25-145725.png)

（图为 *caniuse* 上各浏览器对 *CSS mask* 的支持情况）



## 快速入门示例



下面，我们来看一个 *CSS mask* 的快速入门示例。

首先需要准备两张图片，图片素材如下：



一张 *jpg* 图片：*zelda.jpg*

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-25-151154.png" alt="image-20211025231154694" style="zoom: 50%;" />



一张 *png* 图片：*mask.png*，该 *png* 图片背景为透明（这里划重点）

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-25-151236.png" alt="image-20211025231236440" style="zoom:50%;" />



接下来准备我们的测试目录：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-25-151518.png" alt="image-20211025231518012" style="zoom:50%;" />





*index.html* 代码如下：

```html
<div class="mask"></div>
```

```css
* {
  margin: 0;
  padding: 0;
}

div {
  width: 1200px;
  height: 600px;
  outline: 1px solid;
  margin: 50px auto;
  background: url('./zelda.jpg') no-repeat center/cover;
}

/*  
  虽然 .mask 和 div 都是选择中的相同的元素
  这里为了单独观察 mask 相关设置，
  和 mask 不相关的属性设置放入到了 div 选择器中 
*/
.mask {
  -webkit-mask-image: url('./mask.png');
}
```

在上面的代码中，我们为 *div* 设置了一个铺满整个盒子的背景图，然后为该盒子设置了遮罩效果。由于 *mask.png* 无法占满整个盒子，所以出现了重复的效果，***mask.png* 遮罩图片透明的部分不会显示底部图片的信息，而非透明部分则会显示底层图片信息**。

效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-25-151805.png" alt="image-20211025231804573" style="zoom:50%;" />



除了设置透明的 *png* 图片，还可以设置透明的渐变。例如：

```css
.mask {
  -webkit-mask-image: linear-gradient(transparent 10%, white);
}
```

在上面的代码中，我们设置了一个从上到下的线性透明渐变，效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-013708.png" alt="image-20211026093708467" style="zoom:50%;" />



可以看到，无论是设置图片还是渐变，一定要有透明的部分，否则无法起到遮罩的效果，例如：

```css
.mask {
  -webkit-mask-image: linear-gradient(red,blue);
}
```

上面的代码中，我们设置的是一个红到蓝的渐变，没有任何的透明部分，所以遮罩层不会起作用，底图会原封不动的显示出来。



## 遮罩各属性介绍



在上面的快速入门示例中，我们用到的是 *mask-image* 属性，但是除了该属性外，*CSS mask* 还有很多其他的属性，如下：



- *mask-image*
- *mask-mode*
- *mask-repeat*
- *mask-position*
- *mask-clip*
- *mask-origin*
- *mask-size*
- *mask-type*
- *mask-composite*



下面我们来针对每个属性进行介绍。



#### *mask-image*



该属性在上面的快速入门示例中我们已经体验过了，默认值为 *none*，表示没有遮罩图片。

可以设置的值为透明图片，或透明渐变。



#### *mask-repeat*



表示遮罩层是否允许重复，默认值为 *repeat* 允许重复，可选值与 *background-repeat* 相同。

```css
.mask {
  -webkit-mask-image: url('./mask.png');
  -webkit-mask-repeat: no-repeat;
}
```

上面的代码中，我们设置遮罩层的重复行为是 *x、y* 轴都不重复，效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-015113.png" alt="image-20211026095112536" style="zoom:50%;" />

（红框是为了表示整张图片的大小，并非浏览器实际显示情况）



#### *mask-position*



该属性用于设置遮罩层的位置，默认值为*0 0* 在最左上角，可选值与 *background-position* 相同。

```css
.mask {
  -webkit-mask-image: url('./mask.png');
  -webkit-mask-repeat: no-repeat;
  -webkit-mask-position: center;
}
```

效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-015730.png" alt="image-20211026095730053" style="zoom:50%;" />

（红框是为了表示整张图片的大小，并非浏览器实际显示情况）



#### *mask-size*



该属性用于设置遮罩层的大小，默认值为 *auto*，可选值与 *background-size* 相同，如下表：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-021622.png" alt="image-20211026101622232" style="zoom:50%;" />

例如：

```css
.mask {
  -webkit-mask-image: url('./mask.png');
  -webkit-mask-repeat: no-repeat;
  -webkit-mask-position: center;
  -webkit-mask-size: contain;
}
```

效果如下：

![image-20211026101900926](https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-021901.png)

（红框是为了表示整张图片的大小，并非浏览器实际显示情况）



#### *mask-origin*



默认值为 *border-box*，可选值与 *background-origin* 相同，可以设置如下的属性值：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-020115.png" alt="image-20211026100114147" style="zoom:50%;" />

下面为了显示其效果，我们需要稍微对其他样式做一些改变，如下：

```css
div {
  width: 1200px;
  height: 600px;
  border: 100px solid;
  margin: 50px auto;
  background: url('./zelda.jpg') no-repeat center/cover;
}

/*  
虽然 .mask 和 div 都是选择中的相同的元素
这里为了单独观察 mask 相关设置，
和 mask 不相关的属性设置放入到了 div 选择器中 
*/
.mask {
  -webkit-mask-image: url('./mask.png');
  -webkit-mask-repeat: no-repeat;
}
```

在上面的代码中，我们为该 *div* 设置了一个宽度为 *100px* 的 *border*，由于 *mask-origin* 的默认值为 *border-box*，所以我们可以看到如下的效果：

![image-20211026100927087](https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-020928.png)

（红框是为了表示整张图片的大小，并非浏览器实际显示情况）

此时如果设置 *mask-origin* 的值为 *centent-box*，遮罩层图像就会相对于内容框来定位。

```css
.mask {
  -webkit-mask-image: url('./mask.png');
  -webkit-mask-repeat: no-repeat;
  -webkit-mask-origin: content-box;
}
```

效果如下：

![image-20211026101238881](https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-021239.png)

（外层的黑框表示该 *div* 的 *border*，红框表示该 *div* 的内容区域，并非浏览器实际显示情况）



#### *mask-clip*



默认值为 *border-box*，可选值与 *background-clip* 相同，可以设置如下属性值：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-025300.png" alt="image-20211026105259969" style="zoom:50%;" />

我们同样为 *div* 设置一个宽度为 *100px* 的 *border*，由于默认值为 *border-box*，所以我们看到的效果如下：

![image-20211026100927087](https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-025412.png)

（红框是为了表示整张图片的大小，并非浏览器实际显示情况）



接下来设置 *mask-clip* 为 *content-box*

```css
.mask {
  -webkit-mask-image: url('./mask.png');
  -webkit-mask-repeat: no-repeat;
  -webkit-mask-clip: content-box;
}
```

由于 *mask-origin* 默认值为 *border-box*，所以遮罩层以边框盒来定位，之后我们设置了 *mask-clip* 为 *content-box*，表示以内容框来进行裁剪。效果如下：

![image-20211026105532851](https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-025533.png)

（外层的黑框表示该 *div* 的 *border*，红框表示该 *div* 的内容区域，并非浏览器实际显示情况）



#### *mask-mode*



*mask-mode* 属性默认值为 *match-source*，作用是根据资源的类型自动采用合适的遮罩模式。

例如，如果遮罩效果使用的是 *SVG* 中的 \<*mask*> 元素，则此时的 *mask-mode* 属性的值为 *luminance*，表示基于亮度来判断是否要进行遮罩。

如果是其他场景，则计算值是 *alpha*，表示基于透明度判断是否要进行遮罩。

因此 *mask-mode* 可选值为 *alpha、luminance、match-source*。

使用搜索引擎搜索遮罩素材的时候，往往搜索的结果都是白底的 *JPG* 图片，因此使用默认的遮罩模式是没有预期的遮罩效果的。此时就非常适合设置遮罩模式为 *luminance*。



另外，目前仅 *Firefox* 浏览器支持 *mask-mode* 属性，*Chrome* 浏览器并不提供支持，但是可以使用非标准的 *mask-source-type* 属性来进行替代（没有私有前缀）。

下面来看一个简单的示例。首先我们需要扩充我们的遮罩素材，准备了一张 *mask2.jpg* 的遮罩图片，该素材首先是 *jpg* 格式的，其次并没有透明区域，仅有一些白底区域。

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-032617.png" alt="image-20211026112617175" style="zoom:50%;" />

接下来设置如下的代码：

```css
.mask {
  -webkit-mask-image: url('./mask2.jpg');
  -webkit-mask-repeat: no-repeat;
  -webkit-mask-position: center;
  mask-mode: luminance;
}
```

在上面的代码中，我们设置 *mask-mode* 的值为 *luminance*，表示基于亮度来判断是否进行遮罩。

在 *Firefox* 浏览器中的效果如下：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-10-26-032916.png" alt="image-20211026112915034" style="zoom:50%;" />

（红框是为了表示整张图片的大小，并非浏览器实际显示情况）



#### *mask-type*



*mask-type* 属性的功能和 *mask-mode* 属性类似，都是设置不同的遮罩模式，但还是有一个很大的区别，就是 *mask-type* 属性只能作用于 *SVG* 元素上，因为其本质上是由 *SVG* 属性演变而来的，*Chrome* 等浏览器也都支持该属性。而 *mask-mode* 是一个针对所有元素类型的 *CSS* 属性，*Chrome* 等浏览器并不支持该属性，目前仅只有 *Firefox* 浏览器对其提供支持。



由于 *mask-type* 属性只能作用于 *SVG* 元素上，因此默认值表现为 *SVG* 元素默认遮罩模式，也就是默认值是 *luminance* 亮度遮罩模式。如果需要支持透明度遮罩模式，可以这么设置：

```css
mask-type: alpha;
```



#### *mask-composite*



*mask-composite* 属性表示同时使用多张图片进行遮罩时的合成方式。默认值为 *add*，可选值为 *add、subtract、intersect、exclude*。

- *mask-composite: add*：表示遮罩累加，这是默认值
- *mask-composite: subtract*：表示遮罩相减，也就是遮罩图片重合的区域不显示，这就意味着，遮罩层图片越多，遮罩区域越小。
- *mask-composite: intersect*：表示遮罩相交，也就是遮罩图片重合的区域才显示遮罩。
- *mask-composite: exclude*：表示遮罩排除，也就是遮罩图片重合的区域会被当作透明。



-*EOF*-