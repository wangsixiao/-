# -
数组基础学习+类数组解析+数组扁平化（flat）

之前做个一个关于react的todo分享，里面用了reduce函数，有同事问为什么用这个函数，因为是参考的实例，说不出个所以然，在这专门学习总结一下数组使用，在平时的开发过程中，处理数组或者类数组的机会还是很多的，因此对循环的方法也做一个对比。
### 1、基础篇
###### 创建数组

> Array构造函数：`let arr = new Array()`
> 数组字面量表示法：`let arr = []`【推荐】

###### 检测数组
> value `instanceof` Array 
> `Array.isArray`(value)【推荐】

instanceof操作符存在一个问题，它假定只有一个全局环境。如果一个网页中有多个框架，比如iframe，每个框架有各自的全局环境，当一个框架的数组传到另一个框架中，instanceof检测会返回false，因为全局环境不同，Array构造函数版本也不同。
Array.isArray()比较通用，不区别是哪个全局环境
###### 转换字符串

```javascript
	let arr1=['qwe','asd','zxc']
	arr1.toString() // "qwe,asd,zxc"；
	arr1.toLocaleString() // "qwe,asd,zxc" 标准时间  四位数以上每3位以逗号分隔
	arr1.valueOf() // 原数组
	arr1.join("&") // "qwe&asd&zxc"
```

    

> `数组转字符串，默认是使用toString方法`

###### 插入、删除
```javascript
	let arr = ['green']
	arr.push("red") // 2
	arr.unshift('blue') // 3 arr: ["blue", "green", "red"]
	arr.pop() // red
	arr.shift() // blue arr: ["green"]
```
插入：`push()`【栈方法：数组末端】、`unshfit()`【队列方法：数组前端】（`返回数组长度`）
删除：`pop()`【栈方法：取末端值】、`shift()`【队列方法：取前端值】（`返回所取值`，数组改变）
###### 排序
> `sort()、reverse()`

这两方法都是先使用toString()方法将数组转换为字符串，若是比较‘10’和‘5’，字符串10是比5小的，因此，请看下图方法：
```javascript
	function compare(v1,v2){
		return v1 - v2 // 倒叙的话，换成 v2-v1
	}
	let arrs = [1,0,5,3,14,10]
	arrs.sort(compare) //  [0, 1, 3, 5, 10, 14]
```
### 2、方法篇
###### 操作方法
```javascript
	let arr = [1,2,3,4,5,6]
	arr.concat(11) // 新数组：[1,2,3,4,5,6,11]，不传参数时，复制arr数组返回
	arr.slice(1,4) // 新数组：[2,3,4]，slice(起始位置，结束位置)
	arr.splice(2,0,12) // 返回：[]，原数组：[1,2,12,3,4,5,6]（插入）
	arr.splice(2,1) // 返回：[12]，原数组：[1,2,3,4,5,6]（删除）
	arr.splice(2,2,12) // 返回：[3,4]，原数组：[1,2,12,5,6](替换)
```

> `concat`()【不修改原数组】  
> `slice`(起始位置，结束位置[不包含]) 【不修改原数组】
> `splice`(删除项位置，删除项数，插入项)【修改原数组，可实现插入，删除，替换】

###### 位置方法
   > `indexOf()` 、`lastIndexOf()`

使用全等`===`操作符比较，未找到返回`-1`
###### 迭代方法

> `ervery()、some()、map()、forEach()、filter()` 
> 参数：`(item, index, array)`

forEach()没有返回值，与使用for循环迭代数组一样，当然，map也可以像forEach只作循环使用，不返回值，相对来说，map速度要更快一点，并且map会分配内存空间存储新数组并返回。
map与forEach方法对比：

> map 方法常用于根据当前数组生成新数组，侧重生成新数组 
> forEach 方法常用于对数组的每一项都执行一个方法，侧重对数组项执行操作

```java
	let arr = [1, 2, 3, 4, 5];
	let newArr1 = arr.forEach((num, index) => {
	    return num * 2;
	}); // undefined 返回无效
	let newArr2 = arr.map((num) => {
		return num * 3
	}) // [3, 6, 9, 12, 15]
	arr.forEach((num, index) => {
	    arr[index] = num * 2;
	}); // arr改变：[2,4,6,8,10]，替换成map亦可
	let newArr3 = arr.filter((num) => {
		return num%4 == 0
	}) // [4,8]
```
之前看到的列子，感觉写法很棒，放图：
![之前看到的列子，感觉写法很棒，放图](https://img-blog.csdnimg.cn/20190703104944741.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NDIzNDMx,size_16,color_FFFFFF,t_70)
###### 归并方法
此方法很是强大，后有实例
> `reduce(prev, cur, index, array)`
（前一个值，当前值，项的索引，数组对象）
### 3、类数组
什么是类数组，当初实习面试的时候被问到过，没回答上来。。。面试小哥很好心的教了教我，举了一个例，说传入函数的`arguments参数`和用js方法getElementsByTagName`获取的dom元素`，是`有length属性`的，但是它是`不具备数组所具有的方法`。
将可迭代的对象转换为数组
> `Array.from(foo)` (good)
> `[...foo]`(best)
> `Array.prototype.slice.call(arrayLike)`(just so so)

区别类数组与数组
Array.isArray、instanceof、constructor、toString
```java
	function test(){
		let arr = []
		console.log(arguments)
		console.log(Array.isArray(arguments))
		console.log(arguments instanceof Array)
		console.log(arguments.constructor === Array)
		console.log(Object.prototype.toString.call(arguments))
		console.log(Object.prototype.toString.call(arr))
	}
	test(1,2,3,4,5,6)
```
输出：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190703111627207.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NDIzNDMx,size_16,color_FFFFFF,t_70)
当你尝试在方法里arguments.push(7)，浏览器会报错Uncaught TypeError: arguments.push is not a function，arguments是不具备数组方法的，
类数组的好处就是把数组和对象拼接在一起。
### 4、实例篇
###### 1、将一个数组组合到另一个数组或者非数组中
```javascript
// 二维数组
let arr1 = [1,2];
let arr2 = [3,4];
let arr3 = 5
// 1️⃣
if(Array.isArray(arr1)){
	if(Array.isArray(arr2)){
		arr1.push.apply(arr1,arr2)
	}
}
// 2️⃣
arr1.concat(arr2) // [1, 2, 3, 4]
// 3️⃣
[arr3].concat(arr1) // [5, 1, 2, 3, 4]
```
###### 2、数组扁平化方法总结
将二维数组或者多维数组转换成一维数组

可直接使用flat方法：arr.flat(Infinity);

1）reduce方法
```javascript
	let arr = [[1,2],[3,4],[5,6]];
	let newArr = arr.reduce((prev, cur) => {
		return prev.concat(cur)
	})
```

```js
	let arr = [[1,2],[4,[5,[6,7]]]]
	let newArr = (arr) => {
		return arr.reduce((prev, cur) =>
			prev.concat(Array.isArray(cur) ? newArr(cur) : cur),[]
		)
	}
	console.log(newArr(arr))
	
```
2）使用字符串做处理

```js
let arr = [1, [2, [3, [4, 5]]], 6];
let str = JSON.stringify(arr); // "[1,[2,[3,[4,5]]],6]"
let newStr = str.replace(/(\[|\])/g, '')
newStr = '[' + newStr + ']'
JSON.parse(newStr)
```

3）在看数组实例时，看到有另外两种写法，真是牛逼，里面的例子也很经典，[附作者链接](https://blog.csdn.net/lj745280746/article/details/70880809)

```java
	// 偶然发现 arr.toString() 或 arr.join() => '1,2,3,4,5'
	// 于是可以写的简便些
	const arr = [1,[2,[3],4],5]
	arr.join()
		.split(',')
		.map(it => Number(it))
	
	// 网上搜了下，还可以这么写。。。
	const arr = [1, [2, [3], 4], 5]
	JSON.parse(`[${arr}]`)
```
4）看react源码的时候看到的
```java
function accumulateInto(current, next) {

  if (current == null) {
    return next;
  }

  // 将next添加到current中,返回一个包含他们两个的新数组
  // 如果next是数组,current不是数组,采用push方法,否则采用concat方法
  // 如果next不是数组,则返回一个current和next构成的新数组
  if (Array.isArray(current)) {
    if (Array.isArray(next)) {
      current.push.apply(current, next);
      return current;
    }
    current.push(next);
    return current;
  }

  if (Array.isArray(next)) {
    return [current].concat(next);
  }

  return [current, next];
}
```
