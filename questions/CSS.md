# 大厂前端面试题精选

## CSS 部分

### 1. display 的 block、inline 和 inline-block 的区别
**block**：会独占一行，多个元素会另起一行，可以设置 width、height、margin 和 padding 属性；
**inline**：元素不会独占一行，设置 width、height 属性无效。但可以设置水平方向的 margin 和 padding 属性，不能设置垂直方向的 padding 和 margin；
**inline-block**：将对象设置为 inline 对象，但对象的内容作为 block 对象呈现，之后的内联对象会被排列在同一行内。
对于行内元素和块级元素，其特点如下：
1. 行内元素
设置宽高无效；
可以设置水平方向的 margin 和 padding 属性，不能设置垂直方向的 padding 和 margin；
不会自动换行；
2. 块级元素
可以设置宽高；
设置 margin 和 padding 都有效；
可以自动换行；
多个块状，默认排列从上到下。

### 2. link 和 @import 的区别
两者都是外部引用 CSS 的方式，它们的区别如下：
link 是 XHTML 标签，除了加载 CSS 外，还可以定义 RSS 等其他事务；@import 属于 CSS 范畴，只能加载 CSS。
link 引用 CSS 时，在页面载入时同时加载；@import 需要页面网页完全载入以后加载。
link 是 XHTML 标签，无兼容问题；@import 是在 CSS2.1 提出的，低版本的浏览器不支持。
link 支持使用 Javascript 控制 DOM 去改变样式；而 @import 不支持。

