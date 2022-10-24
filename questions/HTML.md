# 大厂前端面试题精选

## HTML 部分

### 1. 对 HTML 语义化的理解
语义化是指根据内容的结构化（内容语义化），选择合适的标签（代码语义化）。通俗来讲就是用正确的标签做正确的事情。
语义化的优点如下：
对机器友好，带有语义的文字表现力丰富，更适合搜索引擎的爬虫爬取有效信息，有利于 SEO。除此之外，语义类还支持读屏软件，根据文章可以自动生成目录；
对开发者友好，使用语义类标签增强了可读性，结构更加清晰，开发者能清晰的看出网页的结构，便于团队的开发与维护。
常见的语义化标签：
```html
<!-- 头部 -->
<header></header>

<!-- 导航栏 -->
<nav></nav>

<!-- 区块（有语义化的 div） -->
<section></section>

<!-- 主要区域 -->
<main></main>

<!-- 主要内容 -->
<article></article>

<!-- 侧边栏 -->
<aside></aside>

<!-- 底部 -->
<footer></footer>

```

### 2. DOCTYPE(⽂档类型) 的作⽤
DOCTYPE 是 HTML5 中一种标准通用标记语言的文档类型声明，它的目的是告诉浏览器（解析器）应该以什么样（html 或 xhtml）的文档类型定义来解析文档，不同的渲染模式会影响浏览器对 CSS 代码甚⾄ JavaScript 脚本的解析。它必须声明在 HTML⽂档的第⼀⾏。
浏览器渲染页面的两种模式（可通过 document.compatMode 获取，比如，语雀官网的文档类型是 CSS1Compat）：
CSS1Compat：标准模式（Strick mode），默认模式，浏览器使用 W3C 的标准解析渲染页面。在标准模式中，浏览器以其支持的最高标准呈现页面。
BackCompat：怪异模式(混杂模式)(Quick mode)，浏览器使用自己的怪异模式解析渲染页面。在怪异模式中，页面以一种比较宽松的向后兼容的方式显示。

### 3. script 标签中 defer 和 async 的区别
如果没有 defer 或 async 属性，浏览器会立即加载并执行相应的脚本。它不会等待后续加载的文档元素，读取到就会开始加载和执行，这样就阻塞了后续文档的加载。
下图可以直观的看出三者之间的区别:
![script 标签中 defer 和 async 的区别](../assets/images/script-%E6%A0%87%E7%AD%BE%E4%B8%AD-defer-%E5%92%8C-async-%E7%9A%84%E5%8C%BA%E5%88%AB.jpg)
其中蓝色代表 js 脚本网络加载时间，红色代表 js 脚本执行时间，绿色代表 html 解析。
defer 和 async 属性都是去异步加载外部的 JS 脚本文件，它们都不会阻塞页面的解析，其区别如下：
执行顺序：多个带 async 属性的标签，不能保证加载的顺序；多个带 defer 属性的标签，按照加载顺序执行；
脚本是否并行执行：async 属性，表示后续文档的加载和执行与 js 脚本的加载和执行是并行进行的，即异步执行；defer 属性，加载后续文档的过程和 js 脚本的加载(此时仅加载不执行)是并行进行的(异步)，js 脚本需要等到文档所有元素解析完成之后才执行，DOMContentLoaded 事件触发执行之前。


### 4. 行内元素有哪些？块级元素有哪些？ 空(void)元素有那些？
行内元素有：a b span img input select strong；
块级元素有：div ul ol li dl dt dd h1 h2 h3 h4 h5 h6 p；
空元素，即没有内容的 HTML 元素。空元素是在开始标签中关闭的，也就是空元素没有闭合标签：
常见的有：
```html
<!-- 换⾏标签，通常⽤于⽂本格式换⾏ -->
<br />

<!-- 显示为一条水平线 -->
<hr /> 

<!-- ⽤于为基于 Web 的表单创建交互式控件，以便接受来⾃⽤户的数据。 -->
<input /> 

<!-- 代表⽂档中的⼀个图像。 -->
<img />

<!-- 指定了外部资源与当前⽂档的关系。这个元素的使⽤⽅法包括为导航定义关系框架。这个元素经常⽤来链接 css ⽂件。 -->
<link />

<!-- 元素表⽰那些不能由其它 HTML 元相关元素 (<base>, <link>, <script>, <style> 或 <title>) 之⼀表⽰的任何元数据信息。 --> 
<meta />

```
鲜见的有：
```html
<!-- 定义表格中的列，并⽤于定义所有公共单元格上的公共语义。它通常位于 `<colgroup>` 元素内。 --> 
<col />  定义表格中的列，并⽤于定义所有公共单元格上的公共语义。它通常位于 `<colgroup>` 元素内。

<!-- 在图⽚上定义⼀个热点区域。 --> 
<area />

<!-- <source> 标签为媒介元素（比如 <video> 和 <audio>）定义媒介资源。 --> 
<source />

<!-- <track> 标签为诸如 video 元素之类的媒介规定外部文本轨道。 --> 
<track />

<!-- 它定义了⼀个特定区域，另⼀个  HTML  ⽂档可以在⾥⾯展⽰。 ( 已废弃 )  --> 
<frame />

<!-- 指定⽤于⼀个⽂档中包含的所有相对 URL 的基本 URL。 --> 
<base />

<!-- ⽤来设置⽂档的默认字体⼤⼩。（⽬前已废弃） --> 
<basefont />

<!-- IE 浏览器中设置⽹页背景⾳乐的元素。  --> 
<bgsound />

<!-- ⽤于表⽰⼀个外部应⽤或交互式内容的集合点，换句话说，就是⼀个插件。 --> 
<embed />

<!-- 定义了 <object> 的参数。 --> 
<param />

<!-- ⼀个⽂本中的位置，其中浏览器可以选择来换⾏，虽然它的换⾏规则可能不会在这⾥换⾏。 --> 
<wbr />

<!-- 使浏览器显⽰⼀个对话框，提⽰⽤户输⼊单⾏⽂本。 --> 
<isindex />

<!-- ⼀个过时的 HTML 元素,  它使下⼀个 web 设计⼯具能够为其定位点⽣成⾃动名称标签。它是由该 web 编辑⼯具⾃动⽣成的，不需要⼿动调整或输⼊。这个元素的区别是成为第⼀个元素,  成为⼀个 " 丢失的标签 " 被淘汰的官⽅公共 DTD 的 HTML 版本。 --> 
<nextid /> 

<!-- 为了⽅便⽣成密钥材料和提交作为 [HTML form] 的⼀部分的公钥 . 这种机制被⽤于设计基于 Web 的证书管理系统。( 已废弃 )  --> 
<keygen />

<!-- 将起始标签后⾯的任何东西渲染为纯⽂本，不会解释为  HTML 。它没有闭合标签，因为任何后⾯的东西都会看做纯⽂本。( 已废弃 )  --> 
<plaintext />

<!-- 它可以向页⾯插⼊间隔。它由 Netscape 设计，⽤于实现单像素布局图像的相同效果，Web 设计师⽤它来向页⾯添加空⽩，⽽不需要实际使⽤图⽚。（已废弃） --> 
<spacer />

```

### 5. 浏览器是如何对 HTML5 的离线储存资源进行管理和加载？
在线的情况下，浏览器发现 html 头部有 manifest 属性，它会请求 manifest 文件，如果是第一次访问页面 ，那么浏览器就会根据 manifest 文件的内容下载相应的资源并且进行离线存储。如果已经访问过页面并且资源已经进行离线存储了，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的 manifest 文件与旧的 manifest 文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，就会重新下载文件中的资源并进行离线存储。
离线的情况下，浏览器会直接使用离线存储的资源。

### 6. Canvas 和 SVG 的区别
**1. SVG**：
SVG 可缩放矢量图形（Scalable Vector Graphics）是基于可扩展标记语言 XML 描述的 2D 图形的语言，SVG 基于 XML 就意味着 SVG DOM 中的每个元素都是可用的，可以为某个元素附加 Javascript 事件处理器。在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。
其特点如下：
    - 不依赖分辨率
    - 支持事件处理器
    - 最适合带有大型渲染区域的应用程序（比如谷歌地图）
    - 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）
    - 不适合游戏应用
**2. Canvas**：
Canvas 是画布，通过 Javascript 来绘制 2D 图形，是逐像素进行渲
染的。其位置发生改变，就会重新进行绘制。
其特点如下：
    - 依赖分辨率
    - 不支持事件处理器
    - 弱的文本渲染能力
    - 能够以 .png 或 .jpg 格式保存结果图像
    - 最适合图像密集型的游戏，其中的许多对象会被频繁重绘。

注：矢量图，也称为面向对象的图像或绘图图像，在数学上定义为一系列由线连接的点。矢量文件中的图形元素称为对象。每个对象都是一个自成一体的实体，它具有颜色、形状、轮廓、大小和屏幕位置等属性。

### 7. 说一下 HTML5 drag API
**dragstart**：事件主体是被拖放元素，在开始拖放被拖放元素时触发。
**darg**：事件主体是被拖放元素，在正在拖放被拖放元素时触发。
**dragenter**：事件主体是目标元素，在被拖放元素进入某元素时触发。
**dragover**：事件主体是目标元素，在被拖放在某元素内移动时触发。
**dragleave**：事件主体是目标元素，在被拖放元素移出目标元素是触发。
**drop**：事件主体是目标元素，在目标元素完全接受被拖放元素时触发。
**dragend**：事件主体是被拖放元素，在整个拖放操作结束时触发。