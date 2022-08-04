# 大厂前端面试题精选

## React 部分

### 1. React 的事件和普通的 HTML 事件有什么不同？
区别：
对于事件名称命名方式，原生事件为全小写，react 事件采用小驼峰；
对于事件函数处理语法，原生事件为字符串，react 事件为函数；
react 事件不能采用 return false 的方式来阻止浏览器的默认行为，而必须要地明确地调用 preventDefault() 来阻止默认行为。
合成事件是 react 模拟原生 DOM 事件所有能力的一个事件对象，其优点如下：
兼容所有浏览器，更好的跨平台；
将事件统一存放在一个数组，避免频繁的新增与删除（垃圾回收）。
方便 react 统一管理和事务机制。
事件的执行顺序为原生事件先执行，合成事件后执行，合成事件会冒泡绑定到 document 上，所以尽量避免原生事件与合成事件混用，如果原生事件阻止冒泡，可能会导致合成事件不执行，因为需要冒泡到 document 上合成事件才会执行。

### 2. React 组件中怎么做事件代理？它的原理是什么？
React 基于 Virtual DOM 实现了一个 SyntheticEvent 层（合成事件层），定义的事件处理器会接收到一个合成事件对象的实例，它符合 W3C 标准，且与原生的浏览器事件拥有同样的接口，支持冒泡机制，所有的事件都自动绑定在最外层上。
在 React 底层，主要对合成事件做了两件事：
事件委派：React 会把所有的事件绑定到结构的最外层，使用统一的事件监听器，这个事件监听器上维持了一个映射来保存所有组件内部事件监听和处理函数。
自动绑定：React 组件中，每个方法的上下文都会指向该组件的实例，即自动绑定 this

### 3. React 高阶组件、Render props、hooks 有什么区别，为什么要不断迭代？
这三者是目前 react 解决代码复用的主要方式：
高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。具体而言，高阶组件是参数为组件，返回值为新组件的函数。
render props 是指一种在 React 组件之间使用一个值为函数的 prop 共享代码的简单技术，更具体的说，render prop 是一个用于告知组件需要渲染什么内容的函数 prop。
通常，render props 和高阶组件只渲染一个子节点。让 Hook 来服务这个使用场景更加简单。这两种模式仍有用武之地，（例如，一个虚拟滚动条组件或许会有一个 renderltem 属性，或是一个可见的容器组件或许会有它自己的 DOM 结构）。但在大部分场景下，Hook 足够了，并且能够帮助减少嵌套。
**1. HOC**
官方解释∶
高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而
形成的设计模式。
简言之，HOC 是一种组件的设计模式，HOC 接受一个组件和额外的参数（如果需要），返回一个新的组件。HOC 是纯函数，没有副作用。
```
// HOC 的定义
function withSubscription(WrappedComponent, selectData) {
    return class extends react.Component {
        constructor(props) {
            supper(props);
            this.state = {
                dara: selectData(DataSource, props)
            };
        }
        // 一些通用的逻辑处理
        render() {
            // ... 并使用新数据渲染被包装的组件！
            return <WrappedComponent data = { this.state.data } { ...this.props } />;
        }
    };
}

// 使用
const BlogPostWithSubScription = withSubscription(BlogPost, (DataSource, props) => DataSource.getBlogPost(props.id));
```
HOC 的优缺点∶
优点∶ 逻辑复用、不影响被包裹组件的内部逻辑。
缺点∶ hoc 传递给被包裹组件的 props 容易和被包裹后的组件重名，
进而被覆盖。
**2. Render prop**
官方解释∶
"render prop"是指一种在 React 组件之间使用一个值为函数的 prop 共享代码的简单技术。
具有 render prop 的组件接受一个返回 React 元素的函数，将 render 的渲染逻辑注入到组件内部。在这里，"render"的命名可以是任何其他有效的标识符。
```
// DataProvider 组件内部的渲染逻辑如下
class DataProvider extends React.Components {
    state = {
        name: 'Tom'
    }
    render(){
        return (
            <div>
                <p>共享数据组件自己内部的渲染逻辑</p>
                { this.props.render(this.state) }
            </div>
        );
    }
}

// 调用方式
<DataProvider render = {data => (
    <h1>Helo { data.name }</h1>
)} />
```
由此可以看到，render props 的优缺点也很明显∶
优点：数据共享、代码复用，将组件内的 state 作为 props 传递给调用者，将渲染逻辑交给调用者。
缺点：无法在 return 语句外访问数据、嵌套写法不够优雅。
**3. Hooks**
官方解释∶
Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。通过自定义 hook，可以复用代码逻辑。
```
// 自定义一个获取订阅数据的 HOOK
function useSubscription() {
    const data = DataSource.getComments();
    return [data];

}

function CommentList(props) {
    const {data} = props;
    const [subData] = useSubscription();
    ...
}

// 使用
<CommentList data = 'hello' />
```
以上可以看出，hook 解决了 hoc 的 prop 覆盖的问题，同时使用的方
式解决了 render props 的嵌套地狱的问题。hook 的优点如下∶
使用直观；
解决 hoc 的 prop 重名问题；
解决 render props 因共享数据 而出现嵌套地狱的问题；
能在 return 之外使用数据的问题。
需要注意的是：hook 只能在组件顶层使用，不可在分支语句中使用。
**总结∶**
Hoc、render props 和 hook 都是为了解决代码复用的问题，但是 hoc 和 render props 都有特定的使用场景和明显的缺点。hook 是 react16.8 更新的新的 API，让组件逻辑复用更简洁明了，同时也解决了 hoc 和 render props 的一些缺点。

### 4. Component, Element, Instance 之间有什么区别和联系？
元素：一个元素 element 是一个普通对象(plain object)，描述了对于一个 DOM 节点或者其他组件 component，你想让它在屏幕上呈现成什么样子。元素 element 可以在它的属性 props 中包含其他元素(译注:用于形成元素树)。创建一个 React 元素 element 成本很低。元素 element 创建之后是不可变的。
组件：一个组件 component 可以通过多种方式声明。可以是带有一个 render() 方法的类，简单点也可以定义为一个函数。这两种情况下，它都把属性 props 作为输入，把返回的一棵元素树作为输出。
实例：一个实例 instance 是你在所写的组件类 component class 中使用关键字 this 所指向的东西(译注:组件实例)。它用来存储本地状态和响应生命周期事件很有用。
函数式组件(Functional component)根本没有实例 instance。类组件(Class component)有实例 instance，但是永远也不需要直接创建一个组件的实例，因为 React 帮我们做了这些。