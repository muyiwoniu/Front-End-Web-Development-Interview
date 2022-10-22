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

### 3. CSS3 中有哪些新特性
新增各种 CSS 选择器（:not(.input)：所有 class 不是“input”的节点）
圆角 （border-radius:8px)
多列布局 （multi-column layout）
阴影和反射 （Shadoweflect）
文字特效 （text-shadow）
文字渲染 （Text-decoration）
线性渐变 （gradient）
旋转 （transform）
增加了旋转、缩放、定位、倾斜、动画、多背景

### 4. 对 CSSSprites 的理解
CSSSprites（精灵图），将一个页面涉及到的所有图片都包含到一张大图中去，然后利用 CSS 的 background-image，background-repeat，background-position 属性的组合进行背景定位。
- 优点：
利用 CSS Sprites 能很好地减少网页的 http 请求，从而大大提高了页面的性能，这是 CSS Sprites 最大的优点；
CSS Sprites 能减少图片的字节，把 3 张图片合并成 1 张图片的字节总是小于这 3 张图片的字节总和。
- 缺点：
在图片合并时，要把多张图片有序的、合理的合并成一张图片，还要留好足够的空间，防止板块内出现不必要的背景。在宽屏及高分辨率下的自适应页面，如果背景不够宽，很容易出现背景断裂；
CSSSprites 在开发的时候相对来说有点麻烦，需要借助 photoshop 或其他工具来对每个背景单元测量其准确的位置。
维护方面：CSS Sprites 在维护的时候比较麻烦，页面背景有少许改动时，就要改这张合并的图片，无需改的地方尽量不要动，这样避免改动更多的 CSS，如果在原来的地方放不下，又只能（最好）往下加图片，这样图片的字节就增加了，还要改动 CSS。