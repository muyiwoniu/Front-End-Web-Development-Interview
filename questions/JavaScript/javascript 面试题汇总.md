# *JavaScript* 面试题汇总

### 1. 根据下面 *ES6* 构造函数的书写方式，要求写出 *ES5* 的

```js
class Example { 
  constructor(name) { 
    this.name = name;
  }
  init() { 
    const fun = () => { console.log(this.name) }
    fun(); 
  } 
}
const e = new Example('Hello');
e.init();
```

> 参考答案：
>
> ```js
> function Example(name) {
>       'use strict';
>       if (!new.target) {
>            throw new TypeError('Class constructor cannot be invoked without new');
>       }
>       this.name = name;
> }
> 
> Object.defineProperty(Example.prototype, 'init', {
>       enumerable: false,
>       value: function () {
>            'use strict';
>            if (new.target) {
>                throw new TypeError('init is not a constructor');
>            }
>            var fun = function () {
>                console.log(this.name);
>            }
>            fun.call(this);
>       }
> })
> ```
>

> 解析：
>
> 此题的关键在于是否清楚 *ES6* 的 *class* 和普通构造函数的区别，记住它们有以下区别，就不会有遗漏：
> 
>   1. *ES6* 中的 *class* 必须通过 *new* 来调用，不能当做普通函数调用，否则报错
>   
>       因此，在答案中，加入了 *new.target* 来判断调用方式
>   
>   2. *ES6* 的 *class* 中的所有代码均处于严格模式之下
> 
>    因此，在答案中，无论是构造函数本身，还是原型方法，都使用了严格模式
> 
>   3. *ES6* 中的原型方法是不可被枚举的
>   
>       因此，在答案中，定义原型方法使用了属性描述符，让其不可枚举
>    
>    4. 原型上的方法不允许通过 *new* 来调用
>    
>       因此，在答案中，原型方法中加入了 *new.target* 来判断调用方式

