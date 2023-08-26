# *Promise* 相关面试题



*Promise* 是面试时大概率会被考察到的一个点，所以我们这里做一个 *Promise* 相关代码题的汇总。



题目 1

```js
const first = () => (new Promise((resolve, reject) => {
    console.log(3);
    let p = new Promise((resolve, reject) => {
        console.log(7);
        setTimeout(() => {
            console.log(1);
        }, 0);
        setTimeout(() => {
            console.log(2);
            resolve(3);
        }, 0)
        resolve(4);
    });
    resolve(2);
    p.then((arg) => {
        console.log(arg, 5); // 1 bb
    });
    setTimeout(() => {
        console.log(6);
    }, 0);
}))
first().then((arg) => {
    console.log(arg, 7); // 2 aa
    setTimeout(() => {
        console.log(8);
    }, 0);
});
setTimeout(() => {
    console.log(9);
}, 0);
console.log(10);
```

>参考答案：
>
>3
>7
>10
>4 5
>2 7
>1
>2
>6
>9
>8



题目 2

```js
new Promise((resolve, reject) => {
    reject(1);
    console.log(2);
    resolve(3);
    console.log(4);
}).then((res) => { console.log(res) })
    .catch(res => { console.log('reject1') })
try {
    new Promise((resolve, reject) => {
        throw 'error'
    }).then((res) => { console.log(res) })
        .catch(res => { console.log('reject2') })
} catch (err) {
    console.log(err)
}
```

>参考答案：
>
>2
>4
>reject1
>reject2



题目 3

```js
const promise = new Promise((resolve, reject) => {
    console.log(1);
    resolve();
    console.log(2);
    reject('error');
})
promise.then(() => {
    console.log(3);
}).catch(e => console.log(e))
console.log(4);
```

> 参考答案：
>
> 1
> 2
> 4
> 3



题目 4

```js
const p1 = () => (new Promise((resolve, reject) => {
	console.log(1);
	let p2 = new Promise((resolve, reject) => {
		console.log(2);
		const timeOut1 = setTimeout(() => {
			console.log(3);
			resolve(4);
		}, 0)
		resolve(5);
	});
	resolve(6);
	p2.then((arg) => {
		console.log(arg);
	});
}));
const timeOut2 = setTimeout(() => {
	console.log(8);
	const p3 = new Promise(reject => {
		reject(9);
	}).then(res => {
		console.log(res)
	})
}, 0)


p1().then((arg) => {
	console.log(arg);
});
console.log(10);
```

> 参考答案：
>
> 1
> 2
> 10
> 5
> 6
> 8
> 9
> 3



题目 5

顺序加载 *10* 张图片，图片地址已知，但是同时最多加载 *3* 张图片，要求用 *promise* 实现。

> 参考答案：
>
> ```js
> const baseUrl = 'http://img.aizhifou.cn/';
> const urls = ['1.png', '2.png', '3.png', '4.png', '5.png','6.png', '7.png', '8.png', '9.png', '10.png'];
> const loadImg = function (url, i) {
> 	return new Promise((resolve, reject) => {
> 		try {
> 			// 加载一张图片
> 			let image = new Image();
> 			image.onload = function () {
> 				resolve(i)
> 			}
> 			image.onerror = function () {
> 				reject(i)
> 			};
> 			image.src = baseUrl + url;
> 		} catch (e) {
> 			reject(i)
> 		}
> 	})
> }
> function startLoadImage(urls, limits, endHandle) {
> 	// 当前存在的promise队列
> 	let promiseMap = {};
> 	// 当前索引对应的加载状态，无论成功，失败都会标记为true，格式： {0: true, 1: true, 2: true...}
> 	let loadIndexMap = {};
> 	// 当前以及加载到的索引，方便找到下一个未加载的索引，为了节省性能，其实可以不要
> 	let loadIndex = 0;
> 	const loadAImage = function () {
> 		// 所有的资源都进入了异步队列
> 		if (Object.keys(loadIndexMap).length === urls.length) {
> 			// 所有的资源都加载完毕，或者进入加载状态，递归结束
> 			const promiseList = Object.keys(promiseMap).reduce((arr, item) => {arr.push(promiseMap[item]); return arr}, [])
> 			Promise.all(promiseList).then(res => {
> 				// 这里如果没有加载失败，就会在所有加载完毕后执行，如果其中某个错误了，这里的结果就不准确，不过这个不是题目要求的。
> 				console.log('all');
> 				endHandle && endHandle()
> 			}).catch((e) => {
> 				console.log('end:' + e);
> 			})
> 		} else {
> 			// 遍历，知道里面有3个promise
> 			while (Object.keys(promiseMap).length < limits) {
> 				for (let i = loadIndex; i < urls.length; i++) {
> 					if (loadIndexMap[i] === undefined) {
> 						loadIndexMap[i] = false;
> 						promiseMap[i] = loadImg(urls[i], i);
> 						loadIndex = i;
> 						break;
> 					}
> 				}
> 			}
> 			// 获取当前正在进行的promise列表，利用reduce从promiseMap里面获取
> 			const promiseList = Object.keys(promiseMap).reduce((arr, item) => {arr.push(promiseMap[item]); return arr}, [])
> 			Promise.race(promiseList).then((index) => {
> 				// 其中一张加载成功，删除当前promise，让PromiseList小于limit，开始递归，加载下一张
> 				console.log('end:' + index);
> 				loadIndexMap[index] = true;
> 				delete promiseMap[index];
> 				loadAImage();
> 			}).catch(e => {
> 				// 加载失败也继续
> 				console.log('end：' + e);
> 				loadIndexMap[e] = true;
> 				delete promiseMap[e];
> 				loadAImage();
> 			})
> 		}
> 	}
> 	loadAImage()
> }
> 
> startLoadImage(urls, 3)
> ```



题目 6

```js
Promise.resolve(1)
.then(2)
.then(Promise.resolve(3))
.then(console.log)
```

> 参考答案：
>
> 1



题目 7 

```js
var promise = new Promise(function(resolve, reject){
  setTimeout(function() {
    resolve(1);
  }, 3000)
})
```

请问这三种有何不同？

```js
// 1
promise.then(() => {
  return Promise.resolve(2);
}).then((n) => {
  console.log(n)
});

// 2
promise.then(() => {
  return 2
}).then((n) => {
  console.log(n)
});

// 3
promise.then(2).then((n) => {
  console.log(n)
});
```

> 参考答案：
>
> 1. 输出2。Promise.resolve 就是一个 Promise 对象就相当于返回了一个新的 Promise 对象。然后在下一个事件循环里才会去执行 then
> 2. 输出2。和上一点不一样的是，它不用等下一个事件循环。
> 3. 输出1。then 和 catch 期望接收函数做参数，如果非函数就会发生 Promise 穿透现象，打印的是上一个 Promise 的返回。



题目 8

```js
let a;
const b = new Promise((resolve, reject) => {
  console.log('promise1');
  resolve();
}).then(() => {
  console.log('promise2');
}).then(() => {
  console.log('promise3');
}).then(() => {
  console.log('promise4');
});

a = new Promise(async (resolve, reject) => {
  console.log(a);
  await b;
  console.log(a);
  console.log('after1');
  await a
  resolve(true);
  console.log('after2');
});

console.log('end');
```

> 参考答案：
>
> promise1 undefined end promise2 promise3 promise4 Promise { pending } after1



题目 9

```js
const promise = new Promise((resolve, reject) => {
  resolve('success1');
  reject('error');
  resolve('success2');
});

promise
  .then((res) => {
    console.log('then: ', res);
  })
  .catch((err) => {
    console.log('catch: ', err);
  });

```

> 参考答案：
>
> then:  success1
>
> 简单题，牢牢记住 Promise 对象的状态只能被转移一次，resolve('success1') 时状态转移到了 fullfilled 。后面 reject 就调用无效了，因为状态已经不是 pending。



题目 10

```js
Promise.resolve()
    .then(() => {
        return new Error('error!!!')
    })
    .then((res) => {
        console.log('then: ', res)
    })
    .catch((err) => {
        console.log('catch: ', err)
    })
```

> 参考答案：
>
> 没有抛出错误和异常，只是 return 了一个对象，哪怕他是 Error 对象，那自然也是正常走 then 的链式调用下去了，不会触发 catch。



-*EOF*-

