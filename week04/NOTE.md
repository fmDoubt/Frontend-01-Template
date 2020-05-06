## 宿主接受js代码的三种方式

1. 普通代码

   ```
   <script>
   var a = 1;
   a++;
   </script>
   ```

2. setTimeout

   ```
   <script>
   setTimeout(function(){}, 1000)
   </script>
   ```

3. module

   ```
   <script type="module">
   var a = 1;
   a++;
   </script>
   ```

## 宏任务与微任务
```
async function afoo(){
	console.log("-1")
	await new Promise(resolve => resolve())
	console.log("-2")
	await new Promise(resolve => resolve())
	console.log("-0.5")
}

new Promise(resolve => (console.log("0"), resolve())).
then(() => (console.log("1"),
			new Promise(resolve => resolve())
				.then(() => console.log("1.5")) ))

setTimeout(function(){
	console.log("2")
	new Promise(resolve => resolve()).then(console.log("3"))
}, 0)
console.log("4")
console.log("5")
afoo()
// 结果
0
4
5
-1
1
-2
1.5
-0.5
Promise {<resolved>: undefined}
2
3

宏任务：
	0、4、5、-1 入队1，-2
	1  入队1.5
	-2 入队-0.5
	1.5
	-0.5
微任务：
2
3
```

```
async function async1() {
    console.log('async1 start');
    await async2();
    // 用分号来理解，分号后没有回调函数
    console.log('async1 end');
}
async function async2() {
    console.log('async2');
}
async1();
new Promise(function (resolve) {
    console.log('promise1');
    resolve();
}).then(function () {
    console.log('promise2');
});
// 结果
async1 start
async2
promise1
async1 end
promise2
Promise {<resolved>: undefined}

// 在safari下
async1 start
async2
promise1
promise2
async1 end
Promise {status: "pending"}
```
一个宏任务里的同步代码也可以理解为微任务，只不过比宏任务里异步代码微任务先入队

标准里关于微任务的描述是8.4 Jobs and Job Queues
