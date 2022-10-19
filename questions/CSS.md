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
