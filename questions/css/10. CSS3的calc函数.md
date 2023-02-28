# *CSS3* 的 *calc* 函数



## 经典真题



- *CSS* 的计算属性知道吗？



## *CSS3* 中的 *calc* 函数



*calc* 是英文单词 *calculate*（计算）的缩写，是 *CSS3* 的一个新增的功能。

*MDN* 的解释为可以用在任何长度、数值、时间、角度、频率等处，语法如下：

```css
/* property: calc(expression) */
width: calc(100% - 80px);
```



可以用常见的 + - * / 符号来进行运算，但需要注意的是 + 和 - 必须用空格隔开，原因很简单，如果 - 号和计算的数字挨在一起，则浏览器在解析时会认为这可能是一个负值。



例如：

```css
width: calc(100% -8px); /* 这样会出错,结果为0 */
```

```css
width: calc(100% - 8px); /* 当 + -  符号用空格隔开时,运算成功 */
```



接下来我们来看一下 *calc* 函数的具体使用示例，如下：

```html
<div class="container">
  <div class="item"></div>
</div>
```

```css
* {
  margin: 0;
  padding: 0;
}

.container{
  width: 500px;
  height: 250px;
  background-color: skyblue;
  margin: 10px;
  position: relative;
}
.item{
  width: 100px;
  height: 100px;
  background-color: pink;
  position: absolute;
  left: calc(50% - 50px);
  top: calc(50% - 50px);
}
```



效果：



<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2021-09-14-094033.png" alt="image-20210914174033014" style="zoom:50%;" />



更多关于 *calc* 函数信息可以参阅：*https://developer.mozilla.org/zh-CN/docs/Web/CSS/calc()*



## 真题解答



- *CSS* 的计算属性知道吗？

> 参考答案：
>
> 即 *calc( )* 函数，主要用于指定元素的长度，支持所有 *CSS* 长度单位，运算符前后都需要保留一个空格。
>
> 比如： *width: calc(100% - 50px);*



-*EOF*-
