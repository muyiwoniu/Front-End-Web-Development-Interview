# *import* 指令



## 经典真题



- *CSS* 引用的方式有哪些？*link* 和 *@import* 的区别？



## 来看看 *import* 指令是啥



*import* 指令是用来导入 *CSS* 样式的。



什么？导入样式不是已经有 *link* 标签了么？



没错，*link* 标签可以导入外部 *CSS* 样式，*import* 仍然可以导入外部 *CSS* 样式。



我们首先来看一下 *import* 的基本用法



1. 在 *HTML* 文件中导入外部样式

```html
<style>
  @import url('./index.css');
</style>
```

要在 *HTML* 源代码直接应用 *@import* 引入外部 *CSS* 文件，须要将 *@import* 放入 *style* 标签



2. 在 *CSS* 文件中引入另一个 *CSS* 文件

```css
@import url('./index.css');
/* 后面书写其他样式 */
```

除了 *HTML* 源代码中使用 *style* 标签来运用 *@import*，在 *CSS* 文件代码中依旧可以或许使用 *@import*，这个时候就不须要 *style* 标签，而是直接应用 *@import* 就可，这样便可实现一个（多个）*CSS* 文件中引入套入别的一个（多个）*CSS* 文件。



3. *@import* 规则还支持媒体查询，因此可以允许依赖媒体的导入

```css
@import "printstyle.css" print;
/* 只在媒体为 print 时导入 "printstyle.css" 样式表 */
```

```css
@import "mobstyle.css" screen and (max-width: 768px);
/* 只在媒体为 screen 且视口最大宽度 768 像素时导入 "mobstyle.css" 样式表 */
```



看完了 *@import* 的基本使用后，接下来我们来看一下它和 *link* 的区别：



1. ***link* 属于 *HTML* 标签，而 *@import* 完全是 *CSS* 提供的一种方式。**

   *link* 标签除了可以加载 *CSS* 外，还可以做很多其它的事情，比如定义 *RSS*，定义 *rel* 连接属性等，*@import* 就只能加载 *CSS* 了。

2. **加载顺序的差别。**

   比如，在 *a.css* 中使用 *import* 引用 *b.css*，只有当使用当使用 *import* 命令的宿主 *css* 文件 *a.css* 被下载、解析之后，浏览器才会知道还有另外一个 *b.css* 需要下载，这时才去下载，然后下载后开始解析、构建 *render tree* 等一系列操作.

3. **兼容性的差别。**

   由于 *@import* 是 *CSS2.1* 提出的所以老的浏览器不支持，*@import* 只有在 *IE5* 以上的才能识别，而 *link* 标签无此问题。

4. **当使用 *JS* 控制 *DOM* 去改变样式的时候，只能使用 *link* 标签，因为 *@import* 不是 *DOM* 可以控制的**。

   对于可换皮肤的网站而言，可以通过改变 *link* 标签这两个的 *href* 值来改变应用不用的外部样式表，但是对于 *import* 是无法操作的，毕竟不是标签。



另外，从性能优化的角度来讲，尽量要避免使用 *@import*。

使用 *@import* 引入 *CSS* 会影响浏览器的并行下载。使用 *@import* 引用的 *CSS* 文件只有在引用它的那个 *CSS* 文件被下载、解析之后，浏览器才会知道还有另外一个 *CSS* 需要下载，这时才去下载，然后下载后开始解析、构建 *Render Tree* 等一系列操作。

多个 *@import* 会导致下载顺序紊乱。在 *IE* 中，*@import* 会引发资源文件的下载顺序被打乱，即排列在 *@import* 后面的 *JS* 文件先于 *@import* 下载，并且打乱甚至破坏 *@import* 自身的并行下载。



## 真题解答



- *CSS* 引用的方式有哪些？*link* 和 *@import* 的区别？

> 参考答案：
>
> *CSS* 引用的方式有：
>
> - 外联，通过 *link* 标签外部链接样式表
> - 内联，在 *head* 标记中使用 *style* 标记定义样式
> - 嵌入，在元素的开始标记里通过 *style* 属性定义样式
>
> *link* 和 *@import* 的区别：
>
> 1. ***link* 属于 *HTML* 标签，而 *@import* 完全是 *CSS* 提供的一种方式。**
>
>    *link* 标签除了可以加载 *CSS* 外，还可以做很多其它的事情，比如定义 *RSS*，定义 *rel* 连接属性等，*@import* 就只能加载 *CSS* 了。
>
> 2. **加载顺序的差别。**
>
>    比如，在 *a.css* 中使用 *import* 引用 *b.css*，只有当使用当使用 *import* 命令的宿主 *css* 文件 *a.css* 被下载、解析之后，浏览器才会知道还有另外一个 *b.css* 需要下载，这时才去下载，然后下载后开始解析、构建 *render tree* 等一系列操作.
>
> 3. **兼容性的差别。**
>
>    由于 *@import* 是 *CSS2.1* 提出的所以老的浏览器不支持，*@import* 只有在 *IE5* 以上的才能识别，而 *link* 标签无此问题。
>
> 4. **当使用 *JS* 控制 *DOM* 去改变样式的时候，只能使用 *link* 标签，因为 *@import* 不是 *DOM* 可以控制的**。
>
>    对于可换皮肤的网站而言，可以通过改变 *link* 便签这两个的 *href* 值来改变应用不用的外部样式表，但是对于 *import* 是无法操作的，毕竟不是标签。



-*EOF*-

