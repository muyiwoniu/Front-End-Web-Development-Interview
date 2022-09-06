# 大厂前端面试题精选

## Vue 部分

### 1. Vue 的基本原理
当一个 Vue 实例创建时， Vue 会遍历 data 中的属性 ， 用 Object.defineProperty （ vue3.0 使用 proxy ） 将它们转为 getter / setter，并且在内部追踪相关依赖，在属性被访问和修改时通知变化。 每个组件实例都有相应的 watcher 程序实例，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 setter 被调用时，会通知 watcher 重新计算，从而致使它关联的组件得以更新。![Vue 的基本原理](../assets/images/Vue%20%E7%9A%84%E5%9F%BA%E6%9C%AC%E5%8E%9F%E7%90%86.png)


### 2. 双向数据绑定的原理
Vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过 Object.defineProperty() 来劫持各个属性的 setter / getter，在数据变动时发布消息给订阅者，触发相应的监听回调。主要分为以下几个步骤：
1. 需要 observe 的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter 和 getter。这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化。
2. compile 解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图。
3. Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁，主要做的事情是: ①在自身实例化时往属性订阅器(dep)里面添加自己 ②自身必须有一个 update() 方法 ③待属性变动 dep.notice() 通知时，能调用自身的 update() 方法，并触发 Compile 中绑定的回调，则功成身退。
4. MVVM 作为数据绑定的入口，整合 Observer、Compile 和 Watcher 三者，通过 Observer 来监听自己的 model 数据变化，通过 Compile 来解析编译模板指令，最终利用 Watcher 搭起 Observer 和 Compile 之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据 model 变更的双向绑定效果。
![Vue 双向数据绑定的原理](../assets/images/Vue-%E5%8F%8C%E5%90%91%E6%95%B0%E6%8D%AE%E7%BB%91%E5%AE%9A%E7%9A%84%E5%8E%9F%E7%90%86.jpg)

### 3. MVVM、MVC、MVP 的区别
MVC、MVP 和 MVVM 是三种常见的软件架构设计模式，主要通过分离关注点的方式来组织代码结构，优化开发效率。
在开发单页面应用时，往往一个路由页面对应了一个脚本文件，所有的页面逻辑都在一个脚本文件里。页面的渲染、数据的获取，对用户事件的响应所有的应用逻辑都混合在一起，这样在开发简单项目时，可能看不出什么问题，如果项目变得复杂，那么整个文件就会变得冗长、混乱，这样对项目开发和后期的项目维护是非常不利的。
**1. MVC**
MVC 通过分离 Model、View 和 Controller 的方式来组织代码结构。
其中 View 负责页面的显示逻辑，Model 负责存储页面的业务数据，以及对相应数据的操作。并且 View 和 Model 应用了观察者模式，当 Model 层发生改变的时候它会通知有关 View 层更新页面。
Controller 层是 View 层和 Model 层的纽带，它主要负责用户与应用的响应操作，当用户与页面产生交互的时候，Controller 中的事件触发器就开始工作了，通过调用 Model 层，来完成对 Model 的修改，然后 Model 层再去通知 View 层更新。
![MVC](../assets/images/MVC.jpg)
**2. MVVM**
MVVM 分为 Model、View、ViewModel：
Model 代表数据模型，数据和业务逻辑都在 Model 层中定义；
View 代表 UI 视图，负责数据的展示；
ViewModel 负责监听 Model 中数据的改变并且控制视图的更新，处理用户交互操作；
Model 和 View 并无直接关联，而是通过 ViewModel 来进行联系的，Model 和 ViewModel 之间有着双向数据绑定的联系。因此当 Model 中的数据改变时会触发 View 层的刷新，View 中由于用户交互操作而改变的数据也会在 Model 中同步。
这种模式实现了 Model 和 View 的数据自动同步，因此开发者只需要专注于数据的维护操作即可，而不需要自己操作 DOM。
![MVVM](../assets/images/MVVM.jpg)
**3. MVP**
MVP 模式与 MVC 唯一不同的在于 Presenter 和 Controller。在 MVC 模式中使用观察者模式，来实现当 Model 层数据发生变化的时候，通知 View 层的更新。这样 View 层和 Model 层耦合在一起，当项目逻辑变得复杂的时候，可能会造成代码的混乱，并且可能会对代码的复用性造成一些问题。MVP 的模式通过使用 Presenter 来实现对 View 层和 Model 层的解耦。MVC 中的 Controller 只知道 Model 的接口，因此它没有办法控制 View 层的更新，MVP 模式中，View 层的接口暴露给了 Presenter，因此可以在 Presenter 中将 Model 的变化和 View 的变化绑定在一起，以此来实现 View 和 Model 的同步更新。这样就实现了对 View 和 Model 的解耦，Presenter 还包含了其他的响应逻辑。

### 4. MVVM 的优缺点?
优点: 
分离视图（View）和模型（Model），降低代码耦合，提⾼视图或者逻辑的重⽤性: ⽐如视图（View）可以独⽴于 Model 变化和修改，⼀个 ViewModel 可以绑定不同的"View"上，当 View 变化的时候 Model 可以不变，当 Model 变化的时候 View 也可以不变。你可以把⼀些 视图逻辑放在⼀个 ViewModel⾥⾯，让很多 view 重⽤这段视图逻辑提⾼可测试性: ViewModel 的存在可以帮助开发者更好地编写测试代码
⾃动更新dom: 利⽤双向绑定，数据更新后视图⾃动更新，让开发者从繁琐的⼿动 dom 中解放
缺点: Bug 很难被调试: 因为使⽤双向绑定的模式，当你看到界⾯异常了，有可能是你 View 的代码有 Bug，也可能是 Model 的代码有问题。数据绑定使得⼀个位置的 Bug 被快速传递到别的位置，要定位原始出问题的地⽅就变得不那么容易了。另外，数据绑定的声明是指令式地写在 View 的模版当中的，这些内容是没办法去打断点 debug 的。⼀个⼤的模块中 model 也会很⼤，虽然使⽤⽅便了也很容易保证了数据的⼀致性，当时⻓期持有，不释放内存就造成了花费更多的内存对于⼤型的图形应⽤程序，视图状态较多，ViewModel 的构建和维护的成本都会⽐较⾼。

### 5. 说一下 Vue 的生命周期
Vue 实例有⼀个完整的⽣命周期，也就是从开始创建、初始化数据、编译模版、挂载 Dom -> 渲染、更新 -> 渲染、卸载 等⼀系列过程，称这是 Vue 的⽣命周期。
1. beforeCreate（创建前）：数据观测和初始化事件还未开始，此时 data 的响应式追踪、event/watcher 都还没有被设置，也就是说不能访问到 data、computed、watch、methods 上的方法和数据。
2. created（创建后）：实例创建完成，实例上配置的 options 包括 data、computed、watch、methods 等都配置完成，但是此时渲染的节点还未挂载到 DOM，所以不能访问到 $el 属性。
3. beforeMount（挂载前）：在挂载开始之前被调用，相关的 render 函数首次被调用。实例已完成以下的配置：编译模板，把 data 里面的数据和模板生成 html。此时还没有挂载 html 到页面上。
4. mounted（挂载后）：在 el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用。实例已完成以下的配置：用上面编译好的 html 内容替换 el 属性指向的 DOM 对象。完成模板中的 html 渲染到 html 页面中。此过程中进行 ajax 交互。
5. beforeUpdate（更新前）：响应式数据更新时调用，此时虽然响应式数据更新了，但是对应的真实 DOM 还没有被渲染。
6. updated（更新后）：在由于数据更改导致的虚拟 DOM 重新渲染和打补丁之后调用。此时 DOM 已经根据响应式数据的变化更新了。调用时，组件 DOM 已经更新，所以可以执行依赖于 DOM 的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。
7. beforeDestroy（销毁前）：实例销毁之前调用。这一步，实例仍然完全可用，this 仍能获取到实例。
8. destroyed（销毁后）：实例销毁后调用，调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务端渲染期间不被调用。

另外还有 keep-alive 独有的生命周期，分别为 activated 和 deactivated。用 keep-alive 包裹的组件在切换时不会进行销毁，而是缓存到内存中并执行 deactivated 钩子函数，命中缓存渲染后会执行 activated 钩子函数。

### 6. slot 是什么？有什么作用？原理是什么？
slot 又名插槽，是 Vue 的内容分发机制，组件内部的模板引擎使用 slot 元素作为承载分发内容的出口。插槽 slot 是子组件的一个模板标签元素，而这一个标签元素是否显示，以及怎么显示是由父组件决定的。slot 又分三类，默认插槽，具名插槽和作用域插槽。
默认插槽：又名匿名插槽，当 slot 没有指定 name 属性值的时候，默认显示插槽，一个组件内只有有一个匿名插槽。
具名插槽：带有具体名字的插槽，也就是带有 name 属性的 slot，一个组件可以出现多个具名插槽。
作用域插槽：默认插槽、具名插槽的一个变体，可以是匿名插槽，也可以是具名插槽，该插槽的不同点是在子组件渲染作用域插槽时，可以将子组件内部的数据传递给父组件，让父组件根据子组件的传递过来的数据决定如何渲染该插槽。
实现原理：当子组件 vm 实例化时，获取到父组件传入的 slot 标签的内容，存放在 vm.\$slot 中，默认插槽为 vm.\$slot.default，具名插槽为 vm.\$slot.xxx，xxx 为插槽名，当组件执行渲染函数时候，遇到 slot 标签，使用 $slot 中的内容进行替换，此时可以为插槽传递数据，若存在数据，则可称该插槽为作用域插槽。

### 7. $nextTick 原理及作用
Vue 的 nextTick 其本质是对 JavaScript 执行原理 EventLoop 的一种应用。
nextTick 的核心是利用了如 Promise 、MutationObserver 、setImmediate 、setTimeout 的原生 JavaScript 方法来模拟对应的微/宏任务的实现，本质是为了利用 JavaScript 的这些异步回调任务队列来实现 Vue 框架中自己的异步回调队列。
nextTick 不仅是 Vue 内部的异步队列的调用方法，同时也允许开发者在实际项目中使用这个方法来满足实际应用中对 DOM 更新数据时机的后续逻辑处理。
nextTick 是典型的将底层 JavaScript 执行原理应用到具体案例中的示例，引入异步更新队列机制的原因：
如果是同步更新，则多次对一个或多个属性赋值，会频繁触发 UI/DOM 的渲染，可以减少一些无用渲染。
同时由于 VirtualDOM 的引入，每一次状态发生变化后，状态变化的信号会发送给组件，组件内部使用 VirtualDOM 进行计算得出需要更新的具体的 DOM 节点，然后对 DOM 进行更新操作，每次更新状态后的渲染过程需要更多的计算，而这种无用功也将浪费更多的性能，所以异步渲染变得更加至关重要。
Vue 采用了数据驱动视图的思想，但是在一些情况下，仍然需要操作 DOM。有时候，可能遇到这样的情况，DOM1 的数据发生了变化，而 DOM2 需要从 DOM1 中获取数据，那这时就会发现 DOM2 的视图并没有更新，这时就需要用到了 nextTick 了。
由于 Vue 的 DOM 操作是异步的，所以，在上面的情况中，就要将 DOM2 获取数据的操作写在 $nextTick 中。
```js
this.nextTick(() => {
    // 获取数据的操作...
})
```
所以，在以下情况下，会用到 nextTick：
在数据变化后执行的某个操作，而这个操作需要使用随数据变化而变化的 DOM 结构的时候，这个操作就需要方法在 nextTick() 的回调函数中。
在 vue 生命周期中，如果在 created() 钩子进行 DOM 操作，也一定要放在 nextTick() 的回调函数中。
因为在 created() 钩子函数中，页面的 DOM 还未渲染，这时候也没办法操作 DOM，所以，此时如果想要操作 DOM，必须将操作的代码放在 nextTick() 的回调函数中。

### 8. Vue 单页应用与多页应用的区别
概念：
SPA 单页面应用（SinglePage Web Application），指只有一个主页面的应用，一开始只需要加载一次 js、css 等相关资源。所有内容都包含在主页面，对每一个功能模块组件化。单页应用跳转，就是切换相关组件，仅仅刷新局部资源。
MPA 多页面应用 （MultiPage Application），指有多个独立页面的应用，每个页面必须重复加载 js、css 等相关资源。多页应用跳转，需要整页资源刷新。
区别：
| 对比项 \ 模式 | SPA | MPA |
|---|---|---|
| 结构 | 一个主页面 + 许多模块的组件 | 许多完整的页面 |
| 体验 | 页面切换快，体验佳；当初次加载文件过多时，需要做相关的调优 | 页面切换慢，网速慢的时候，体验尤其不好 |
| 资源文件 | 组件公用的资源只需要加载一次 | 每个页面都要自己加载公用的资源 |
| 适用场景 | 对体验度和流畅度有较高要求的应用，不利于 SEO（可借助 SSR 优化 SEO） | 适用于对 SEO 要求较高的应用 |
| 过渡动画 | Vue 提供了 transition 的封装组件，容易实现 | 很难实现 |
| 内容更新 | 相关组件的切换，即局部更新 | 整体 HTML 的切换，重复 HTTP 请求 |
| 路由模式 | 可以使用 hash，也可以使用 history | 普通链接跳转 |
| 数据传递 | 因为单页面，使用全局变量就好（Vue） | cookie、localStorage 等缓存方案，URL 参数，调用接口保存等 |
| 相关成本 | 前期开发成本较高，后期维护较为容易 | 前期开发成本低，后期维护比较麻烦，因为可能一个功能需要改很多地方 |

### 9. Vue 中封装的数组方法有哪些，其如何实现页面更新?
在 Vue 中，对响应式处理利用的是 Object.defineProperty 对数据进行拦截，而这个方法并不能监听到数组内部变化，数组长度变化，数组的截取变化等，所以需要对这些操作进行 hack，让 Vue 能监听到其中的变化。
Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新。这些被包裹过的方法包括：
- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()
那 Vue 是如何实现让这些数组方法实现元素的实时更新的呢，下面是 Vue 中对这些方法的封装：
```js
// 缓存数组原型
const arrayProto = Array.prototype;
// 实现 arrayMethods.__proto__ === Array.prototype
export const arrayMethods = Object.create(arrayProto);
// 需要进行功能拓展的方法
const methodsToPatch = [
    "push",
    "pop",
    "shift",
    "unshift",
    "splice",
    "sort",
    "reverse"
];
/**
 * Intercept mutating methods and emit events
 */
methodsToPatch.forEach(function (method) {
    // 缓存原生数组方法
    const original = arrayProto[method];
    def(arrayMethods, method, function mutator(...args) {
        // 执行并缓存原生数组功能
        const result = original.apply(this, args);
        // 响应式处理
        const ob = this.__ob__;
        let inserted;
        switch (method) {
            // push、unshift 会新增索引，所以要手动 observer
            case "push":
            case "unshift":
                inserted = args;
                break;
                // splice 方法，如果传入了第三个参数，也会有索引加入，也要手动 observer
            case "splice":
                inserted = args.slice(2);
                break;
        }
        // 获取插入的值，并设置响应式监听
        if (inserted) ob.observeArray(inserted);
        // notify change
        ob.dep.notify(); // 通知依赖更新
        // 返回原生数组方法的执行结果
        return result;
    });
});

```
简单来说就是，重写了数组中的那些原生方法，首先获取到这个数组的__ob__，也就是它的 Observer 对象，如果有新的值，就调用 observeArray 继续对新的值观察变化（也就是通过 target__proto__ == arrayMethods 来改变了数组实例的型），然后手动调用 notify，通知渲染 watcher，执行 update。

### 10. delete 和 Vue.delete 删除数组的区别
delete 只是被删除的元素变成了 empty / undefined 其他的元素的键值还是不变。
Vue.delete 直接删除了数组，改变了数组的键值。

### 11. Vue data 中某一个属性的值发生改变后，视图会立即同步执行重新渲染吗？
不会立即同步执行重新渲染。Vue 实现响应式并不是数据发生变化之后 DOM 立即变化，而是按一定的策略进行 DOM 的更新。Vue 在更新 DOM 时是异步执行的。只要侦听到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。 
如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作是非常重要的。然后，在下一个的事件循环 tick 中，Vue 刷新队列并执行实际（已去重的）工作。

### 12. 简述 mixin、extends 的覆盖逻辑
1. mixin 和 extends
mixin 和 extends 均是用于合并、拓展组件的，两者均通过 mergeOptions 方法实现合并。
mixins 接收一个混入对象的数组，其中混入对象可以像正常的实例对象一样包含实例选项，这些选项会被合并到最终的选项中。Mixin 钩子按照传入顺序依次调用，并在调用组件自身的钩子之前被调用。extends 主要是为了便于扩展单文件组件，接收一个对象或构造函数。
| 属性名称 | 合并策略 | 对应合并函数 |
|---|---|---|
| data | mixins / extends 只会将自己有的但是组件上没有的内容混合到组件上，重复定义默认使用组件上的。如果 data 里的值是对象，将递归内部对象继续按照该策略合并 | mergeDataOrFn, mergeData |
| provide | 同上 | mergeDataOrFn, mergeData |
| props | mixins / extends 只会将自己有的但组件上没有的内容混合到组件上 | extend |
| methods | 同上 | extend |
| inject | 同上 | extend |
| computed | 同上 | extend |
| 组件、过滤器、指令属性 | 同上 | extend |
| el | 同上 | defaultStrat |
| propsData | 同上 | defaultStrat |
| watch | 合并 watch 监控的回调方法。执行顺序是先 mixins / extends 里 watch 定义的回调，然后是组建的回调 | strats.watch |
| HOOKS 生命周期钩子 | 同一种钩子的回调函数会被合并成数组。执行顺序是先 mixins / extends 里定义的钩子函数，然后才是组件里定义的 | mergeHook |
2. mergeOptions 的执行过程
规范化选项（normalizeProps、normalizelnject、normalizeDirectives）
对未合并的选项，进行判断
```js
if (!child._base) {
    if (child.extends) {
        parent = mergeOptions(parent, child.extends, vm);
    }
    if (child.mixins) {
        for (let i = 0, l = child.mixins.length; i < l; i++) {
            parent = mergeOptions(parent, child.mixins[i], vm);
        }
    }
}
```

### 13. 对 Vue 组件化的理解
1. 组件是独立和可复用的代码组织单元。组件系统是 Vue 核心特性之一，它使开发者使用小型、独立和通常可复用的组件构建大型应用；
2. 组件化开发能大幅提高应用开发效率、测试性、复用性等；
3. 组件使用按分类有：页面组件、业务组件、通用组件；
4. vue 的组件是基于配置的，我们通常编写的组件是组件配置而非组件，框架后续会生成其构造函数，它们基于 VueComponent，扩展于 Vue；
5. vue 中常见组件化技术有：属性 prop，自定义事件，插槽等，它们主要用于组件通信、扩展等；
6. 合理的划分组件，有助于提升应用性能；
7. 组件应该是高内聚、低耦合的；
8. 遵循单向数据流的原则。

### 14. 子组件可以直接改变父组件的数据吗？
子组件不可以直接改变父组件的数据。这样做主要是为了维护父子组件的单向数据流。每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值。如果这样做了，Vue 会在浏览器的控制台中发出警告。
Vue 提倡单向数据流，即父级 props 的更新会流向子组件，但是反过来则不行。这是为了防止意外的改变父组件状态，使得应用的数据流变得难以理解，导致数据流混乱。如果破坏了单向数据流，当应用复杂时，debug 的成本会非常高。
只能通过 $emit 派发一个自定义事件，父组件接收到后，由父组件修改。

### 15. Vue 子组件和父组件执行顺序
加载渲染过程：
1. 父组件 beforeCreate 
2. 父组件 created 
3. 父组件 beforeMount 
4. 子组件 beforeCreate 
5. 子组件 created 
6. 子组件 beforeMount 
7. 子组件 mounted 
8. 父组件 mounted 

更新过程：
1. 父组件 beforeUpdate 
2. 子组件 beforeUpdate 
3. 子组件 updated 
4. 父组件 updated 

销毁过程：
1. 父组件 beforeDestroy 
2. 子组件 beforeDestroy 
3. 子组件 destroyed 
4. 父组件 destroyed

### 16. created 和 mounted 的区别
created:在模板渲染成 html 前调用，即通常初始化某些属性值，然后再渲染成视图。
mounted:在模板渲染成 html 后调用，通常是初始化页面完成后，再对 html 的 dom 节点进行一些需要的操作。

### 17. 一般在哪个生命周期请求异步数据
我们可以在钩子函数 created、beforeMount、mounted 中进行调用，因为在这三个钩子函数中，data 已经创建，可以将服务端端返回的数据进行赋值。
推荐在 created 钩子函数中调用异步请求，因为在 created 钩子函数中调用异步请求有以下优点：
- 能更快获取到服务端数据，减少页面加载时间，用户体验更好；
- SSR 不支持 beforeMount、mounted 钩子函数，放在 created 中有助于一致性。

### 18. keep-alive 中的生命周期哪些
keep-alive 是 Vue 提供的一个内置组件，用来对组件进行缓存——在组件切换过程中将状态保留在内存中，防止重复渲染 DOM。
如果为一个组件包裹了 keep-alive，那么它会多出两个生命周期： deactivated、activated。
同时，beforeDestroy 和 destroyed 就不会再被触发了，因为组件不会被真正销毁。
当组件被换掉时，会被缓存到内存中、触发 deactivated 生命周期；
当组件被切回来时，再去缓存里找这个组件、触发 activated 钩子函数。