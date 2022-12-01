# 大厂前端面试题精选

## React 部分

### 1. React 的事件和普通的 HTML 事件有什么不同？
区别：
对于事件名称命名方式，原生事件为全小写，react 事件采用小驼峰；
对于事件函数处理语法，原生事件为字符串，react 事件为函数；
react 事件不能采用 return false 的方式来阻止浏览器的默认行为，而必须要地明确地调用 preventDefault() 来阻止默认行为。
合成事件是 react 模拟原生 DOM 事件所有能力的一个事件对象，其优点如下：
兼容所有浏览器，更好的跨平台；
将事件统一存放在一个数组，避免频繁的新增与删除（垃圾回收）[[React 17 中移除了 “event pooling（事件池）”](https://zh-hans.reactjs.org/blog/2020/08/10/react-v17-rc.html#no-event-pooling)]。
方便 react 统一管理和事务机制。
在 React 16 或更早版本中，事件的执行顺序为原生事件先执行，合成事件后执行，合成事件会冒泡绑定到 document 上，所以尽量避免原生事件与合成事件混用，如果原生事件阻止冒泡，可能会导致合成事件不执行，因为需要冒泡到 document 上合成事件才会执行。在 React 17 中，React 将不再向 document 附加事件处理器。而会将事件处理器附加到渲染 React 树的根 DOM 容器中。

### 2. React 组件中怎么做事件代理？它的原理是什么？
React 基于 Virtual DOM 实现了一个 SyntheticEvent 层（合成事件层），定义的事件处理器会接收到一个合成事件对象的实例，它符合 W3C 标准，且与原生的浏览器事件拥有同样的接口，支持冒泡机制，所有的事件都自动绑定在最外层上。
在 React 底层，主要对合成事件做了两件事：
事件委派：React 会把所有的事件绑定到结构的最外层，使用统一的事件监听器，这个事件监听器上维持了一个映射来保存所有组件内部事件监听和处理函数。
自动绑定：React 组件中，每个方法的上下文都会指向该组件的实例，即自动绑定 this 为当前组件。

### 3. React 高阶组件、Render props、hooks 有什么区别，为什么要不断迭代？
这三者是目前 react 解决代码复用的主要方式：
高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。具体而言，高阶组件是参数为组件，返回值为新组件的函数。
render props 是指一种在 React 组件之间使用一个值为函数的 prop 共享代码的简单技术，更具体的说，render prop 是一个用于告知组件需要渲染什么内容的函数 prop。
通常，render props 和高阶组件只渲染一个子节点。让 Hook 来服务这个使用场景更加简单。这两种模式仍有用武之地，（例如，一个虚拟滚动条组件或许会有一个 renderltem 属性，或是一个可见的容器组件或许会有它自己的 DOM 结构）。但在大部分场景下，Hook 足够了，并且能够帮助减少嵌套。
**1. HOC**
官方解释∶
高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。
简言之，HOC 是一种组件的设计模式，HOC 接受一个组件和额外的参数（如果需要），返回一个新的组件。HOC 是纯函数，没有副作用。
```js
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
```js
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
由此可以看到，render props 的优缺点也很明显：
优点：数据共享、代码复用，将组件内的 state 作为 props 传递给调用者，将渲染逻辑交给调用者。
缺点：无法在 return 语句外访问数据、嵌套写法不够优雅。
**3. Hooks**
官方解释∶
Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。通过自定义 hook，可以复用代码逻辑。
```js
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
以上可以看出，hook 解决了 hoc 的 prop 覆盖的问题，同时使用的方式解决了 render props 的嵌套地狱的问题。hook 的优点如下∶
使用直观；
解决 hoc 的 prop 重名问题；
解决 render props 因共享数据而出现嵌套地狱的问题；
能在 return 之外使用数据的问题。
需要注意的是：hook 只能在组件顶层使用，不可在分支语句中使用。
**总结∶**
Hoc、render props 和 hook 都是为了解决代码复用的问题，但是 hoc 和 render props 都有特定的使用场景和明显的缺点。hook 是 react16.8 更新的新的 API，让组件逻辑复用更简洁明了，同时也解决了 hoc 和 render props 的一些缺点。

### 4. Component, Element, Instance 之间有什么区别和联系？
元素：一个元素 element 是一个普通对象(plain object)，描述了对于一个 DOM 节点或者其他组件 component，你想让它在屏幕上呈现成什么样子。元素 element 可以在它的属性 props 中包含其他元素(译注:用于形成元素树)。创建一个 React 元素 element 成本很低。元素 element 创建之后是不可变的。
组件：一个组件 component 可以通过多种方式声明。可以是带有一个 render() 方法的类，简单点也可以定义为一个函数。这两种情况下，它都把属性 props 作为输入，把返回的一棵元素树作为输出。
实例：一个实例 instance 是你在所写的组件类 component class 中使用关键字 this 所指向的东西(译注:组件实例)。它用来存储本地状态和响应生命周期事件很有用。
函数式组件(Functional component)根本没有实例 instance。类组件(Class component)有实例 instance，但是永远也不需要直接创建一个组件的实例，因为 React 帮我们做了这些。

### 5. React.createClass 和 extends Component 的区别有哪些？
React.createClass 和 extends Component 的区别主要在于：
1. 语法区别
createClass 本质上是一个工厂函数，extends 的方式更加接近最新的 ES6 规范的 class 写法。两种方式在语法上的差别主要体现在方法的定义和静态属性的声明上。
createClass 方式的方法定义使用逗号, 隔开，因为 creatClass 本质上是一个函数，传递给它的是一个 Object；而 class 的方式定义方法时务必谨记不要使用逗号隔开，这是 ES6 class 的语法规范。
2. propType 和 getDefaultProps
React.createClass：通过 proTypes 对象和 getDefaultProps() 方法来设置和获取 props。
React.Component：通过设置两个属性 propTypes 和 defaultProps
3. 状态的区别
React.createClass：通过 getInitialState()方法返回一个包含初始值的对象
React.Component：通过 constructor 设置初始状态
4. this 区别
React.createClass：会正确绑定 this
React.Component：由于使用了 ES6，这里会有些微不同，属性并不会自动绑定到 React 类的实例上。
5. Mixins
React.createClass：使用 React.createClass 的话，可以在创建组件时添加一个叫做 mixins 的属性，并将可供混合的类的集合以数组的形式赋给 mixins。
如果使用 ES6 的方式来创建组件，那么 React mixins 的特性将不能被使用了。

### 6. React 如何判断什么时候重新渲染组件？
组件状态的改变可以因为 props 的改变，或者直接通过 setState 方法改变。组件获得新的状态，然后 React 决定是否应该重新渲染组件。只要组件的 state 发生变化，React 就会对组件进行重新渲染。这是因为 React 中的 shouldComponentUpdate 方法默认返回 true，这就是导致每次更新都重新渲染的原因。
当 React 将要渲染组件时会执行 shouldComponentUpdate 方法来看它是否返回 true（组件应该更新，也就是重新渲染）。所以需要重写 shouldComponentUpdate 方法让它根据情况返回 true 或者 false 来告诉 React 什么时候重新渲染什么时候跳过重新渲染。

### 7. React 中可以在 render 访问 refs 吗？为什么？
不可以，render 阶段 DOM 还没有生成，无法获取 DOM。DOM 的获取需要在 pre-commit 阶段和 commit 阶段：
![React 的生命周期](../assets/images/React-%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.jpg)

### 8. React setState 调用之后发生了什么？是同步还是异步？
1. React 中 setState 后发生了什么
在代码中调用 setState 函数之后，React 会将传入的参数对象与组件当前的状态合并，然后触发调和过程(Reconciliation)。经过调和过程，React 会以相对高效的方式根据新的状态构建 React 元素树并且着手重新渲染整个 UI 界面。
在 React 得到元素树之后，React 会自动计算出新的树与老树的节点差异，然后根据差异对界面进行最小化重渲染。在差异计算算法中，React 能够相对精确地知道哪些位置发生了改变以及应该如何改变，这就保证了按需更新，而不是全部重新渲染。
如果在短时间内频繁 setState。React 会将 state 的改变压入栈中，在合适的时机，批量更新 state 和视图，达到提高性能的效果。
2. setState 是同步还是异步的
假如所有 setState 是同步的，意味着每执行一次 setState 时（有可能一个同步代码中，多次 setState），都重新 vnode diff + dom 修改，这对性能来说是极为不好的。如果是异步，则可以把一个同步代码中的多个 setState 合并成一次组件更新。所以默认是异步的，但是在一些情况下是同步的。
setState 并不是单纯同步/异步的，它的表现会因调用场景的不同而不同。在源码中，通过 isBatchingUpdates 来判断 setState 是先存进 state 队列还是直接更新，如果值为 true 则执行异步操作，为 false 则直接更新。
异步：在 React 可以控制的地方，就为 true，比如在 React 生命周期事件和合成事件中，都会走合并操作，延迟更新的策略。
同步：在 React 无法控制的地方，比如原生事件，具体就是在 addEventListener 、setTimeout、setInterval 等事件中，就只能同步更新。
一般认为，做异步设计是为了性能优化、减少渲染次数：
setState设计为异步，可以显著的提升性能。如果每次调用 setState 都进行一次更新，那么意味着 render 函数会被频繁调用，界面重新渲染，这样效率是很低的；最好的办法应该是获取到多个更新，之后进行批量更新；
如果同步更新了 state，但是还没有执行 render 函数，那么 state 和 props 不能保持同步。state 和 props 不能保持一致性，会在开发中产生很多的问题。

### 9. React 组件的 state 和 props 有什么区别？
1. props
props 是一个从外部传进组件的参数，主要作为就是从父组件向子组件传递数据，它具有可读性和不变性，只能通过外部组件主动传入新的 props 来重新渲染子组件，否则子组件的 props 以及展现形式不会改变。
2. state
state 的主要作用是用于组件保存、控制以及修改自己的状态，它只能在 constructor 中初始化，它算是组件的私有属性，不可通过外部访问和修改，只能通过组件内部的 this.setState 来修改，修改 state 属性会导致组件的重新渲染。
3. 区别
props 是传递给组件的（类似于函数的形参），而 state 是在组件内被组件自己管理的（类似于在一个函数内声明的变量）。
props 是不可修改的，所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。
state 是在组件中创建的，一般在 constructor 中初始化 state。
state 是多变的、可以修改，每次 setState 都异步更新的。

### 10. React 中的 props 为什么是只读的？
this.props 是组件之间沟通的一个接口，原则上来讲，它只能从父组件流向子组件。React 具有浓重的函数式编程的思想。
提到函数式编程就要提一个概念：纯函数。它有几个特点：
给定相同的输入，总是返回相同的输出。
过程没有副作用。
不依赖外部状态。
this.props 就是汲取了纯函数的思想。props 的不可变性就保证相同的输入，页面显示的内容是一样的，并且不会产生副作用。

### 11. React 中怎么检验 props？验证 props 的目的是什么？
React 为我们提供了 PropTypes 以供验证使用。当我们向 Props 传入的数据无效（向 Props 传入的数据类型和验证的数据类型不符）就会在控制台发出警告信息。它可以避免随着应用越来越复杂从而出现的问题。并且，它还可以让程序变得更易读。
```jsx
import PropTypes from 'prop-type';

class Greeting extends React.Component {
    render() {
        return (
            <h1>Hello, {this.props.name}</h1>
        );
    }
}

Greeting.propType = {
    name: PropType.string
};
```
当然，如果项目中使用了 TypeScript，那么就可以不用 PropTypes 来校验，而使用 TypeScript 定义接口来校验 props。

### 12. React 废弃了哪些生命周期？为什么？
被废弃的三个函数都是在 render 之前，因为 fber 的出现，很可能因为高优先级任务的出现而打断现有任务导致它们会被执行多次。另外的一个原因则是，React 想约束使用者，好的框架能够让人不得已写出容易维护和扩展的代码，这一点又是从何谈起，可以从新增加以及即将废弃的生命周期分析入手
1. componentWillMount
首先这个函数的功能完全可以使用 componentDidMount 和 constructor 来代替，异步获取的数据的情况上面已经说明了，而如果抛去异步获取数据，其余的即是初始化而已，这些功能都可以在 constructor 中执行，除此之外，如果在 willMount 中订阅事件，但在服务端这并不会执行 willUnMount 事件，也就是说服务端会导致内存泄漏，所以 componentWilIMount 完全可以不使用，但使用者有时候难免因为各种各样的情况在 componentWilMount中做一些操作，那么 React 为了约束开发者，干脆就抛掉了这个 API。
2. componentWillReceiveProps
在老版本的 React 中，如果组件自身的某个 state 跟其 props 密切相关的话，一直都没有一种很优雅的处理方式去更新 state，而是需要在 componentWilReceiveProps 中判断前后两个 props 是否相同，如果不同再将新的 props 更新到相应的 state 上去。这样做一来会破坏 state 数据的单一数据源，导致组件状态变得不可预测，另一方面也会增加组件的重绘次数。类似的业务需求也有很多，如一个可以横向滑动的列表，当前高亮的 Tab 显然隶属于列表自身的时，根据传入的某个值，直接定位到某个 Tab。为了解决这些问题，React 引入了第一个新的生命周期：getDerivedStateFromProps。它有以下的优点∶
    - getDSFP 是静态方法，在这里不能使用 this，也就是一个纯函数，开发者不能写出副作用的代码。
    - 开发者只能通过 prevState 而不是 prevProps 来做对比，保证了 state 和 props 之间的简单关系以及不需要处理第一次渲染时 prevProps 为空的情况。
    - 基于第一点，将状态变化（setState）和昂贵操作（tabChange）区分开，更加便于 render 和 commit 阶段操作或者说优化。
3. componentWillUpdate
与 componentWillReceiveProps 类似，许多开发者也会在 componentWillUpdate 中根据 props 的变化去触发一些回调。但不论是 componentWilReceiveProps 还是 componentWilUpdate，都有可能在一次更新中被调用多次，也就是说写在这里的回调函数也有可能会被调用多次，这显然是不可取的。与 componentDidMount 类似，componentDidUpdate 也不存在这样的问题，一次更新中 componentDidUpdate 只会被调用一次，所以将原先写在 componentWillUpdate 中的回调迁移至 componentDidUpdate 就可以解决这个问题。
另外一种情况则是需要获取 DOM 元素状态，但是由于在 fber 中，render 可打断，可能在 willMount 中获取到的元素状态很可能与实际需要的不同，这个通常可以使用第二个新增的生命函数的解决 getSnapshotBeforeUpdate(prevProps, prevState)
4. getSnapshotBeforeUpdate(prevProps, prevState)
返回的值作为 componentDidUpdate 的第三个参数。与 willMount 不同的是，getSnapshotBeforeUpdate 会在最终确定的 render 执行之前执行，也就是能保证其获取到的元素状态与 didUpdate 中获取到的元素状态相同。官方参考代码：
```jsx
class ScrollingList extends React.Component {
    constructor(props) {
        super(props);
        this.listRef = React.createRef();
    }

    getSnapshotBeforeUpdate(prevProps, prevState) {
        // 是否在 list 中添加新的 items ?
        // 捕获滚动位置，以便稍后调整滚动位置
        if (prevProps.list.length < this.props.list.length) {
            const list = this.listRef.current;
            return list.scrollHeight - list.scrollTop;
        }
        return null;
    }

    componentDidUpdate(prevProps, prevState, snapshot) {
        // 如果 snapshot 有值，说明刚添加了新的 items,
        // 调整滚动位置使得这些新 items 不会将旧的 items 推出视图。
        // （这里的 snapshot 是 getSnapshotBeforeUpdate 的返回值）
        if (snapshot !== null) {
            const list = this.listRef.current;
            list.scrollTop = list.scrollHeight - snapshot;
        }
    }

    render() {
        return (
            <div ref={this.listRef}>{/* ...contents... */}</div>
        );
    }
}
```
