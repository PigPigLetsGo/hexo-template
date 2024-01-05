---
title: JS进阶
categories:
   - [计算机学科,web,js,JS进阶]
tags:
   - 计算机学科
   - web
   - js
   - JS进阶
---

# JS进阶

## 学习目标

>  1.  掌握作用域等概念加深对JS理解
>  2.  学习ES6新特性让代码书写更加简洁便利

## 作用域进阶

**目标**：了解作用域对程序执行的影响及作用域链的查找机制，使用闭包函数创建隔离作用域避免全局变量污染。

>  -  作用域 (scope) 规定了变量能够被访问的 “范围”，离开了这个 “范围” 变量便不能被访问
>  -  **作用域分为**：
>     -  局部作用域
>     -  全局作用域

### 1.1 局部作用域

局部作用域分为<font title=red>函数作用域</font>和<font title=red>块作用域</font>。

#### 1 函数作用域

在==函数内部声明的变量==只能在函数内部被访问，==外部无法直接访问==。

```js
<script>
	function getSum() {
   	// 函数内部是函数作用域 属于局部变量
   	const num = 10
	}
	console.log(num)// 此处报错，函数外部不能使用局部作用域变量
</script>
```



>  <font>总结</font>：
>
>  1.  ==函数内部声明的变量==，在==函数外部无法被访问==.
>  2.  函数的==参数==也是==函数内部的局部变量==.
>  3.  ==不同函数内部==声明的==变量无法互相访问==.
>  4.  ==函数执行完毕后==，函数内部的==变量实际被清空了==^垃圾回收机制^.

#### 2 块作用域

在JavaScript中使用 { } 包裹的代码称为==代码块==，代码块内部声明的变量外部将 [<font title=red>有可能^可能指的是使用var声明变量的块作用域^</font>] 无法被访问。

**使用let 来声明变量** 

```js
for(let t = 1;t <= 6;t ++) {
   //t 只能在改代码块中被访问
   console.log(t)// 正常
}
//超出了t的作用域
console.log(t)// 报错，会产生块作用域
```



**使用const 来声明变量** 

```js
for(let t = 1;t <= 6;t ++) {
   //t 只能在改代码块中被访问
   console.log(t)// 正常
}
//超出了t的作用域
console.log(t)// 报错，会产生块作用域
```



**使用var 来声明变量** 

```js
for(var t = 1;t <= 6;t ++) {
   //t 只能在改代码块中被访问
   console.log(t)// 正常
}
//超出了t的作用域
console.log(t)// 正常，var声明的变量不会产生作用域。
```



>  1.  ==let==声明的变量会==产生块作用域==，==var不会产生块作用域==.
>  2.  ==const==声明的常量也会==产生块作用域==.
>  3.  ==不同代码块==之间的变量==无法互相访问==.
>  4.  推荐使用==let== 或 ==const==.

>  <font>总结</font>
>
>  1.  局部作用域分为哪两种？
>      -  函数作用域 函数内部
>      -  块级作用域 { }
>  2.  局部作用域声明的变量外部能使用吗？
>      -  不能

### 1.2 全局作用域

`<script>`标签和`.js`文件的 [<font title=red>最外层</font>] 就是所谓的==全局作用域==，在此声明的变量在==函数内部也可以被访问==。

<span alt=solid>全局作用域中声明的变量，任何其它作用域都可以被访问</span>.

```js
<script>
	//全局作用域
   //全局作用域下声明了num变量
   const num = 10
	function fn() {
      //函数内部可以使用全局作用域的变量
      console.log(num)
   }
 	// 此处全局作用域
	console.log(num)// 可以正常访问
</script>
```



<blockquote alt="danger">
	<p>
      <font title=red>
      	注意：
      </font>
   </p>
   <p>
		1.  为window对象动态添加的属性默认也是全局的，不推荐！
   </p>
   <p>
		2.  函数中未使用任何关键字声明的变量为全局变量，不推荐！！
   </p>
   <p>
		3.  尽可能少的声明全局变量，防止全局变量被污染。
   </p>
</blockquote>

**总结**：

1.  全局作用域有哪些？
    -  `<script>`标签内部
    -  `.js`文件
2.  全局作用域声明的变量其它作用域能使用吗
    -  可以

### 1.3 作用域链

==作用域链==本质上是底层的 ==变量查找机制==.

-  在函数被执行时，会==优先查找当前函数作用域中查找变量==
-  如果==当前作用域查找不到==则会依次==逐级查找父级作用域==直到全局作用域

```js
<script>
   //全局作用域
   let a = 1
	let b = 2
   //局部作用域
   function f() {
      let a = 1
      //局部作用域
      function() {
         a = 2
         console.log(a)
      }
      g()// 调用g函数
   }
	f()// 调用f函数
</script>
```



**查找顺序**：`g` —> `f` —> `global` 

>  <font>总结</font>：
>
>  1.  嵌套关系的作用域串联起来形成了作用域链
>  2.  相同作用域中按着==从小到大==的规则查找变量
>  3.  <span alt=wavy>子作用域能够访问父作用域，父级作用域无法访问子级作用域</span>.

### 垃圾回收机制

>   垃圾回收机制(Garbage Collection)简称<font title=red>GC</font>.

JS中==内存==的分配和回收都是==自动完成==的，内存在不使用的时候会被==垃圾回收器==自动回收

#### 内存的声明周期

JS环境中分配的内存，一般有如下==声明周期==：

>  1.  **内存分配**：当我们声明==变量==，==函数==，==对象==的时候，系统会自动为它们==分配内存==.
>  2.  **内存使用**：即读写内存，也就是使用变量，函数等
>  3.  **内存回收**：使用完毕，由==垃圾回收器==自动回收不再使用的内存

```js
//为变量分配内存
const age = 18
//为对象分配内存
const obj = {
   age: 19
}
//为函数分配内存
function fn() {
   const age = 20
   console.log(age)
}
```



**说明**：

>  1.  全局变量一般不会回收 (关闭页面回收)
>  2.  一般情况下==局部变量的值==，不用了，会被==自动回收==掉

**内存泄漏**：程序中分配的内存由于某种原因程序==未释放==或==无法释放==叫做==内存泄漏==.

#### JS垃圾回收机制—算法说明

**堆栈空间分配区别**：

>  1.  栈 (操作系统)：由==操作系统自动分配释放==函数的参数值，局部变量等，基本数据类型放到栈里面。
>  2.  堆 (操作系统)：一般由程序员分配释放，若程序员不释放，由==垃圾回收机制==回收。==复杂数据类型==放到堆里面。

**下面介绍两种常见的浏览器**==垃圾回收算法==：==引用计数法== **和** ==标记清除法==.

##### 引用计数

IE采用的引用计数算法，定义 “==内存不再使用==” ，就是看一个==对象==是否有指向它的==引用==，==没有引用==了就==回收对象==.

**算法**：

>  1.  跟踪记录被==引用的次数==.
>  2.  如果被==引用了一次==，那么就==记录次数 1==，多次引用会 ==累加++==.
>  3.  如果==减少一个引用==就==减1 - -==.
>  4.  如果引用次数是==0==，则==释放内存==.

![image-20230806154152471](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308061541096.png)

但它却存在一个致命的问题：<font color=red>嵌套引用</font> (循环引用)

如果两个对象==互相引用==，尽管它们已经==不再使用==，垃圾回收器==不会进行回收==，==导致内存泄漏==.

```js
function fn() {
   let o1 = {}
   let o2 = {}
   o1.a = o2
   o2.a = o1
   return '引用计数无法回收'
}
fn()
```



因为它们的引用次数==永远不会是0==，这样的相互引用如果说<font title=red>很大量的存在</font>就==会导致大量的内存泄露==。

##### 标记清除法

<span alt=wavy>现代的浏览器已经不再使用引用计数算法了</span>。

现代浏览器通用的大多是基于 ==标记清楚算法== 的某些改进算法，总体思想都是一致的。

**核心**：

>  1.  标记清除算法将 “不再使用的对象”定义为 “==无法达到的对象==”
>  2.  就是从==根部== (在JS中就是全局对象) 出发==定时扫描内存中的对象==。凡是==能从根部到达的对象，都是还需要使用的==。
>  3.  那些==无法==由==根部出发 触及到==的 ==对象被标记==为==不再使用==，稍后进行==回收==。

**标记所有的引用** 

![image-20230806160645281](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308061606847.png)

![image-20230806160709067](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308061607509.png)

##### 总结

>  1.  ==标记清除法==核心思路是什么？
>      -  从==根部扫描==对象，==能查找到的就是使用的==，==查找不到的就要被回收==。

### 闭包

**目标**：能说出什么是闭包，闭包的作用以及注意事项

**概念**：一个==函数对周围状态的引用捆绑在一起==，==内层函数中访问到其外层函数的作用域==.

**简单理解**：==闭包== = ==内层==**函数** + ==外层==**函数**的==变量==.

**先看个简单代码**：

```js
function outer() {
   const a = 1//外层函数变量
   //                 + 
   function f() {//内层函数
      //           =  闭包
      console.log(a)
   }
   f()
}
outer()
```



![image-20230806162014826](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308061620199.png)

**闭包作用**：==封闭数据==，==提供操作==，==外部函数也可以访问函数内部的变量==。

**闭包的基本格式**：

```js
function outer() {
   let i = 1
   function fn() {
      console.log(i)
   }
   return fn //将fn返回给外部函数使用
}
//outer() =等价于= fn =等价于= function fn(){}
const fun = outer()
fun()// 1
//外层函数使用内部函数的变量
```



`outer() =等价于= fn =等价于= function fn(){}` 

**简约写法**：

```js
function outer() {
   let i = 1
   return function() {
      console.log(i)
   }
}
const fun = outer()
fun() // 调用fun   1
//外层函数使用内层函数的变量
```



**闭包应用**：实现数据的<font title="red">私有</font>.

>  比如，我们要做个统计函数调用次数，函数调用一次，就 + +

**使用普通函数的形式**：

```js
//使用普通函数的形式
let i = 1
function fn() {
   i++
   console.log(`函数被调用${i}次`)
}
fn() //2
fn() //3
```



<blockquote alt="warn"><p><font title="red">但是，这个i是个全局变量，很容易被修改</font></p></blockquote>

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308061656547.gif)

**使用闭包函数的形式**：

```js
// 使用闭包的形式
function outer() {
    let i = 0
    return function () {
        i++
        console.log(`函数被调用了${i}次~~~`)
    }
}
const fun = outer()
```



<blockquote alt="success"><p>解决了修改全局变量的问题 ! 实现数据私有。</p></blockquote>

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308061704072.gif)

![image-20230806170926606](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308061709949.png)

>  从window最大的开始能找到fun，fun又能找到 outer函数内返回的 return function() 该函数内部使用到了外部函数的i 内部函数又能找到外部函数的 i ，所以 变量 i 不会被垃圾回收。

<blockquote alt="danger">
   <p>
		但是闭包存在一个问题就是：内存泄漏 ！   
   </p>
   <p>
      fun是全局作用域会一直被使用，所以一直都有一个指向关系直到 变量 i 不能被垃圾回收掉可能导致<font title="red">内存泄漏 ！</font>
   </p>
</blockquote>


#### 总结

>  1.  怎么理解闭包？
>      -  闭包 = 内层函数 + 外层函数的变量
>  2.  闭包的作用？
>      -  封闭数据，实现数据私有，外部函数也可以访问函数内部的变量
>      -  闭包很有用，因为它允许将函数与其所有操作的某些数据 (环境) 关联起来
>  3.  闭包可能引起的问题？
>      -  内存泄漏！！

### 变量提升(了解)

**目标**：了解什么是变量提升

变量提升是JavaScript中比较 “==奇怪==” 的现象，它允许在==变量声明之前即被访问== (仅存在于var声明变量)

<blockquote alt="info">
	<p>
      <font>说明</font>：
   </p>
   <p>
      JS初学者经常花很多时间才能习惯变量提升，还经常出现一些意想不到的bug，正因
   </p>
   <p>
      为如此，ES6引入了块级作用域，用let或者const声明变量，让代码写更加规范和人性化。
   </p>
</blockquote>

```js
// 1.把所有var声明的变量提升到 当前作用域的最前面
// 2.只提升声明，不提升赋值
var num //相当于将下面的num提升到了当前作用域最前面也就是这里了
console.log(num + '件')// undefined件 ，提升了num变量并没有赋值 否则应该会报错说 num is not definedat
var num = 10 
```



**结果**：

![image-20230806191723213](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308061917516.png)

```js
// console.log(num) 报错:  num is not definedat
function fun() {
    var num //var声明的变量只提升当前作用域的最前面外部并不能访问到
    console.log(num + '件') //undefined件
    var num = 10
}
fun()
```



**结果**：

![image-20230806192051726](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308061920853.png)

<blockquote alt="danger">
	<p>
      <font title=red>注意</font>
   </p>
   <p>
      1. 变量在未声明即被访问时会报语法错误
   </p>
   <p>
      2. 变量在var声明之前即被访问，变量的值为 undefined (因为存在变量提升的问题 !)
   </p>
   <p>
      3. let / const 声明的变量不存在变量提升
   </p>
   <p>
      4. 变量提升出现在相同作用域当中
   </p>
   <p>
      <font title=red>
      	5. 实际开发中推荐先声明再访问变量
      </font>
   </p>
</blockquote>

>  <font>总结</font>：
>
>  1.  用哪个关键字声明变量会有变量提升？
>      -  var
>  2.  变量提升是什么流程？
>      -  先把var变量提升到当前作用域最前面
>      -  只提升变量==声明==，不提升变量==赋值==.
>      -  然后依次执行代码
>
>  我们不建议使用var声明变量

## 函数进阶

目标：知道函数参数默认值，动态参数，剩余参数的使用细节，提升函数应用的灵活度，知道箭头函数的语法及与普通函数的差异。

### 1.1 函数提升

目标：能说出函数提升的过程

函数提升与变量提升比较类似，是指函数在声明之前即可被调用。

```js
// 1.会把所有函数声明提升到当前作用域的最前面
// 2.只提升函数声明，不提升函数调用
fun()// 函数提升
function fun() {
    console.log('函数提升')
}

// ------------------------------------
var fn

fn() //fn is not a function

var fn = function() {
    console.log('函数表达式')
}

//函数表达式，必须先声明并且赋值，后调用使用 否则报错
```



![image-20230806194738556](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308061947985.png)

>  <font>总结</font>：
>
>  1.  函数提升能够使函数的声明调用更灵活
>  2.  函数表达式不存在提升的现象
>  3.  函数提升出现在相同作用域当中

### 1.2 函数参数

函数参数的使用细节，能够提升函数应用的==灵活度==。

**学习路径**：

1.  动态参数
2.  剩余参数

#### 1.2.1 动态参数

`arguments`是函数内部内置的伪数组变量，它包含了调用函数时传入的所有实参

```js
//求和函数，计算所有参数的和
function sum() {
   //consoole.log(arguments) //arguments 动态参数 只存在于函数中
   let s = 0
   for(let i = 0;i < arguments.length;i ++){
      s += arguments[i]
   }
   console.log(s)
   //调用求和函数
   sum(5,10)// 两个参数
   sum(1,2,4)// 三个参数
}
```



>  <font>总结</font>：
>
>  1.  arguments是一个伪数组，只存在于函数中
>  2.  arguments的作用是动态获取函数的实参
>  3.  可以通过for循环依次得到传递过来的实参
>  4.  当不确定传递多少个实参的时候，我们怎么办？
>      -  arguments动态参数
>  5.  arguments是什么？
>      -  伪数组
>      -  它只存在函数中
>  6.  箭头函数中没有`arguments`.

#### 1.2.2 剩余参数

目标：能够使用剩余参数

剩余参数允许我们将一个不定数量的参数表示为一个<font title=red>真数组</font>.

```js
// ... 剩余参数 arr变量名
// 将前面的实参传给形参a,b剩余的给...剩余参数
function fun(a,b,...arr) {
    // a = 1
    console.log(a)
    // b = 2
    console.log(b)
    // Array[3]
    console.log(arr)
}
fun(1,2)
fun(1,2,3)
```



**结果**：

![image-20230806202027525](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308062020926.png)

>  <font>总结</font>：
>
>  1.  `...`是语法符号，置于最末^mo^函数形参之前，用于获取==多余==的实参
>  2.  借助`...`获取的剩余实参，是个 <font title=red>真数组</font>.

开发中，还是提倡多使用<font title=red>剩余参数</font>.

#### 总结

<blockquote alt=success>
	<div>
      <p>
         <span>1. 剩余参数主要的使用场景是?</span>
   	</p>
      <ul>
         <li>
         	<p>
					<span>用于获取多余的实参</span>
      		</p>
         </li>
         <li>
         	<p>
               <span>剩余参数只能用于函数中</span>
            </p>
         </li>
      </ul>
      <p>
         <span>2. 剩余参数和动态参数区别是什么？开发中提倡使用哪一个？</span>
   	</p>
      <ul>
         <li>
         	<p>
               <span>动态参数是伪数组</span>
   			</p>
         </li>
         <li>
         	<p>
               <span>剩余参数是<font>真数组</font></span>
            </p>
         </li>
         <li>
         	<p>
               <span>开发中使用剩余参数想必也是极好的。</span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>


### 展开运算符

目标：能够使用展开运算符并说出常用的场景

展开运算符(`...`)将一个数组进行展开

```js
const arr = [1,5,3,8,2]
console.log(...arr)//1 5 3 8 2
```



>  <font>说明</font>：
>
>  -  展开运算符==不会修改原数组==.

**典型运用场景**：求数组最大值 (最小值)，合并数组等。

```js
const arr = [1,2,3]
// 展开运算符，可以展开数组
// 使用Math.max求最大值的函数配合...展开运算符求出数组中的最大值
//...arr =等价于= 1,2,3
console.log(Math.max(...arr))// 3
// 使用Math.max求最大值的函数配合...展开运算符求出数组中的最小值
//...arr =等价于= 1,2,3
console.log(Math.min(...arr))// 1
```



**合并数组**：

```js
// 展开运算符，可以展开数组
const arr1 = [1,2,3]
const arr2 = [4,5,6]
// 使用展开运算 合并数组
const arr = [...arr1,...arr2]
console.log(arr)//[1, 2, 3, 4, 5, 6] 合并后的数组
```



### 箭头函数:star: (非常重要)

目标：能够熟悉箭头函数==不同写法==.

**目的**：引入箭头函数的目的是更==简短==的函数写法并且==不绑定this==，箭头函数的语法比函数表达式更简洁

**使用场景**：箭头函数更适用于那些本来==需要匿名函数的地方==.

<blockquote alt=info>
	<p>
      <font>学习路径：</font>
   </p>
   <p>
      1. 基本语法
   </p>
   <p>
      2. 箭头函数参数
   </p>
   <p>
      3. 箭头函数 this
   </p>
</blockquote>

#### 1 基本语法

**语法1**：基本写法

```js
// 箭头函数 声明类型 函数名 = (参数列表) => {方法体}
const fn = () => {}
```



**语法2**：只有一个参数时可以省略小括号

```js
//x是形参
const fn = x => {
   console.log(x) // 1
}
fn(1)
```



**语法3**：如果函数体==只有一行代码==，可以==省略====方法体==写到一行上，并且无需写`return`直接返回

```js
// 箭头函数 声明类型 函数名 = (参数列表) => {方法体}
// 只有一个形参的函数 小括号可以省略 
// 只有一行代码时，可以省略大括号和return，有返回值
const fun = x => x + x
// 调用箭头函数并传入实参
console.log(fun(1)) 
```



**更加简介的事件阻止默认行为的方式**：

```js
//更加简介的语法
const form = document.querySelector('form')
form.addEventListener('click',ev => ev.preventDefault())
```



**语法4**：加括号的函数体返回对象字面量表达式 (箭头函数可以直接返回一个==对象==)

-  返回对象的函数体格式：`({复杂数据类型})` 必须再用小括号包起来

```js
//返回一个对象 不能直接使用{}需要配合（）使用，因为{}和函数{}冲突
const fn1 = uname => ({uname: uname})
console.log(fn1('pink老师'))
```



<blockquote alt=success>
	<p>
      <font>总结</font>：
   </p>
   <p>
      1. 箭头函数属于表达式函数，因此<font>不存在函数提升</font>
   </p>
   <p>
      2. 箭头函数只有一个参数时可以省略小括号 ()
   </p>
   <p>
      3. 箭头函数函数体只有一行代码时，可以省略方法体{}，还有返回值 return
   </p>
   <p>
      4. 加括号的函数体返回对象字面量表达式 格式必须是 ({复杂数据类型}) 因为 {} 与函数 {} 冲突
   </p>
</blockquote>


#### 2 箭头函数参数

<blockquote alt=info>
	<p>
      1. 普通函数有 arguments 动态参数
   </p>
   <p>
      2. 箭头函数没有arguments 动态参数，但是有 剩余参数 …args
   </p>
</blockquote>
**箭头函数求和** 

```js
// 利用箭头函数求和
// 参数列表使用剩余参数 来接收
const getSum = (...arr) => {
    let sum = 0
    // 便利剩余参数 累加 数组内的元素值
    for (let i = 0; i < arr.length; i++)
        sum += arr[i]
    // 返回结果
    return sum
}
console.log(getSum(2, 3))// 5
```



<blockquote alt=success>
   <div>
      <p>
         <span><font>总结</font>：</span>
   	</p>
      <p>
         <span>1. 箭头函数里面有 arguments 动态参数吗？可以使用什么参数？</span>
      </p>
      <ul>
         <li>
            <p>
               <span>没有arguments动态参数</span>
            </p>
         </li>
         <li>
         	<p>
               <span>可以使用 ... 剩余参数</span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>



#### 3 箭头函数this

在箭头函数出现之前，每一个新函数根据它是被<font title=red>如何调用的</font>来定义这个函数的this值，==非常令人讨厌==。

==箭头函数不会创建自己的this==，它只会从自己的作用域链的上一层沿用this。

```js
//以前this的指向：谁调用我，我就是谁
console.log(this)// 此处为 window
const sayHi = function() {
   cnosole.log(this) // 普通函数指向调用者 此处为window
}
sayHi()
// 函数的完整调用方式：window.sayHi()
btn.addEventListener('click',function() {
   console.log(this) // 当前this 指向 btn
})
```



在开发中 [使用箭头函数前需要考虑函数中this的值] ，事件回调函数使用箭头函数时，this为全局的window，因此<font title=red>DOM事件回调函数为了简便，还是不太推荐使用箭头函数</font>。

```js
<script>
   // DOM 节点
	const btn = document.querySelector('.btn')
	// 箭头函数 此时 this 指向了 window
	btn.addEventListener('click',() => {
      console.log(this)
   })

	//普通函数 此时 this 指向了 DOM 对象
	btn.addEventListener('click',function() {
      console.log(this)
   })
</script>
```



<blockquote alt=success>
	<div>
      <font>总结</font>：
      <p>
         <span>1. 箭头函数里面有this吗？</span>
      </p>
      <ul>
         <li>
            <p>
               <span><font title=red>箭头函数不会创建自己的this</font>，它只会从自己的作用域链的上一层沿用this</span>
            </p>
         </li>
      </ul>
      <p>
         <span>2. DOM事件回调函数推荐使用箭头函数吗？</span>
      </p>
      <ul>
         <li>
         	<p>
               <span><font title=red>不太推荐，特别是需要用到this的时候</font></span>
            </p>
         </li>
         <li>
         	<p>
               <span>事件回调函数使用箭头函数时，this为全局的window</span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>

## 解构赋值

-  数组解构
-  对象解构

目标：知道解构的语法及分类，使用解构简洁语法快速为变量赋值

**打比方**：

获取数组中的最大值，最小值，平均值。

方式一：

```js
const arr = [100,60,80]
console.log(arr[0])//最大值
console.log(arr[1])//最小值
console.log(arr[2])//平均值
```



方式二：

```js
const arr = [100,60,80]
const max = arr[0]
const min = arr[1]
const avg = arr[2]
console.log(max)// 最大值 100
console.log(min)// 最小值 60
console.log(avg)// 平均值 80
```



以上要么不好记忆^对于新手而言^，要么书写麻烦，此时可以使用==解构赋值==的方法让代码更简洁，如下代码：

```js
const [max,min,avg] = [100,60,80]
console.log(max)// 最大值 100
console.log(min)// 最小值 60
console.log(avg)// 平均值 80
```



### 1.1 数组解构

==数组解构==是将数组的==单元值^指的是数组元素[1,2,3]^快速批量赋值==给一系列==变量^[a,b,c]批量声明的变量^==的==简洁语法==。

<blockquote alt=info>
	<div>
      <p>
         <span><font>基本语法</font>：</span>
      </p>
      <p>
         <span>1. 赋值运算符 = 左侧的 [ ] 用于批量声明变量，右侧数组的单元值将被赋值给左侧的变量</span>
      </p>
      <p>
         <span>2. 变量的顺序对应数组单元值的位置依次进行赋值操作</span>
      </p>
   </div>
</blockquote>

```js
// 普通数组
const arr = [1,2,3]
//---------------------------------
// 两种语法格式
// 批量声明变量a,b,c 同时将普通数组的元素对应下标赋值给 批量声明的变量中
const [a,b,c] = arr
// 或者
// const [a,b,c] = [1,2,3]
//---------------------------------
// const [a,b,c] = arr 类似于如下：
// const a = arr[0]
// const b = arr[1]
// const c = arr[2]
//---------------------------------
console.log(`a = ${a}`)// a = 1
console.log(`b = ${b}`)// b = 2
console.log(`c = ${c}`)// c = 3
```



**典型应用交换2个变量的值**：

<blockquote alt=danger>
	<div>
      <p>
         <span>一定注意变量b = 3; 后面有一个分号跟解构数组语法连起来的 可以换行写</span>
      </p>
   </div>
</blockquote>

```js
//============交换变量============
let a = 1
let b = 3;[a, b] = [b, a]
console.log(`交换后的变量: a = ${a},b = ${b}`) a = 3,b = 1
//或者这么写================================================
let a = 1
let b = 3
console.log(`交换前的变量: a = ${a},b = ${b}`)
;[a, b] = [b, a] //必须使用 ； 分号将 [] 与它之前的代码分隔开来否则就会报错 为什么报错 下面有解释
console.log(`交换后的变量: a = ${a},b = ${b}`)
```



<blockquote alt=info>
	<div>
      <p>
         <span>冒泡排序的简洁写法—使用 解构数组方式来进行变量的交换 省去一个中间人。</span>
      </p>
   </div>
</blockquote>

```js
const arr = [2, 6, 4, 3, 5, 1]
for (let i = 0; i < arr.length; i++)
    for (let j = 0; j < arr.length - i - 1; j++)
        if (arr[j] > arr[j + 1])
            [arr[j + 1], arr[j]] = [arr[j], arr[j + 1]]
console.log(arr)
```



#### 实参数量与形参数量问题

```js
// 变量多 单元值少
const [a,b,c] = [1,2]
console.log(a) // 1
console.log(b) // 2
console.log(c) // undefined
// 变量少 单元值多
const [a1,b1,c1] = [1,2,3,4]
console.log(a1) // 1
console.log(b1) // 2
console.log(c1) // 3
```



<blockquote alt=info>
	<div>
      <p>
         <span>1. 变量多 单元值少</span>
      </p>
      <ul>
         <li>
         	<p>
               <span>多余的变量没有接收到 单元值的值 默认为 undefined</span>
            </p>
         </li>
      </ul>
      <p>
         <span>2. 变量少 单元值多</span>
      </p>
      <ul>
         <li>
         	<p>
               <span>多余的单元值，没有变量来接收 可以使用  剩余参数来使用</span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>

##### 1 利用剩余参数解决多余的单元值

```js
// 变量少 单元值多
const [a1,b1,c1,...d] = [1,2,3,4]
console.log(a1) // 1
console.log(b1) // 2
console.log(c1) // 3
console.log(d) // [4] 得到一个 真数组
```



##### 2 防止有undefined 传递 单元值的情况，可以为变量设置默认值

```js
// 变量多 单元值少
const [a = a | 0,b = b | 0,c = 0] = [1,2]
console.log(a) // 1
console.log(b) // 2
console.log(c) // 0
```



##### 3 按需导入赋值

```js
// 按需导入赋值
//	        ↓ 忽略一位
const [a,b, ,d] = [1,2,3,4]
console.log(a)// 1
console.log(b)// 2
console.log(d)// 4
```



##### 4 支持多维数组的解构

```js
// 多维数组演示
const arr = [1,2,[3,4,[5,6]]]
console.log(arr[0])//1
console.log(arr[2][2][0])//5
// 使用数组解构
const [a,b,[c,d,[e,f]]] = [1,2,[3,4,[5,6]]]
console.log(a)// 1
console.log(b)// 2
console.log(c)// 3
console.log(d)// 4
console.log(e)// 5
console.log(f)// 6
```



#### 分号解释

<blockquote alt=danger>
	<div>
      <p>
         <span><font title=red>注意：JS前面必须加分号情况</font></span>
      </p>
   </div>
</blockquote>

1.  立即执行函数

    ```js
    (function() {})();
    ;(function() {})()
    ```

    

2.  数组解构

    ```js
    //数组开头的，特别是前面有语句的一定注意加分号 前面带；是表示结束的意思
    ;[a,b] = [a,b]
    ```

    

![image-20230807101435985](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308071014090.png)

<blockquote alt=success>
	<div>
      <p>
         <span><font>总结</font>：</span>
      </p>
      <P>
         <span>1. 数组解构赋值的作用是什么？</span>
      </P>
      <ul>
         <li>
         	<p>
               <span>是将数组的单元值快速<font title=red>批量赋值</font>给一系列变量<font title=red>的简洁语法</font></span>
            </p>
         </li>
      </ul>
      <p>
         <span>2. JS前面有哪两种情况需要加分号？</span>
      </p>
      <ul>
         <li>
         	<p>
               <span>立即执行函数</span>
            </p>
         </li>
         <li>
         	<p>
               <span>数组解构</span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>

#### 练习

**独立完成数组解构赋值** 

>  需求1：
>
>  有个数组：`const ps = ['张三','李四','刘桑']` 
>
>  解构为变量：zs，ls，ls
>
>  需求2：
>
>  请将最大值和最小值函数返回值解构为max和min两个变量
>
>  ```js
>  function getValue() {
>     return [100,60]
>  }
>  //要求max变量里面存100 min 变量里面存 60
>  ```



**题一**：

```js
const arr = ['张三','李四','刘桑']
const [zs,ls,ls1] = arr
console.log(zs) // 张三
console.log(ls) // 李四
console.log(ls1) // 刘桑
```



**题二**：

```js
function getValue() {
   return [100,60]
}
const [max,min] = getValue()
console.log(`最大值 = ${max}`) // 100
console.log(`最小值 = ${min}`) // 60
```



### 1.2 对象解构 (非常的重要 ！)

==对象解构==是将对象==属性==和==方法==快速==批量赋值给==一系列==变量==的==简洁语法==.

<blockquote alt=info>
	<div>
      <p>
         <span><font>1. 基本语法</font>：</span>
      </p>
      <p>
         <span>1. 赋值运算符 = 左侧的 { } 用于批量声明变量，右侧对象的属性值将被赋值给左侧的变量</span>
      </p>
      <p>
         <span>2. 对象属性的值将被赋值与属性名<font title=red>相同的</font>变量</span>
      </p>
      <p>
         <span>3. 注意解构的<font title=red>变量名不要和外面的变量名冲突否则报错</font></span>
      </p>
      <p>
         <span>4. 对象中找不到与变量名一致的属性时变量值为 <font title=red>undefined</font></span>
      </p>
   </div>
</blockquote>

#### 1 基本语法：

```js
// 创建普通对象
const obj1 = {
    unames: '小花',
    ages: 18
}

// 对象解构
const {unames,ages} = obj1

// 打印输出
console.log(unames) // 小花
console.log(ages) // 18
```



**更简洁的写法**：

```js
// ==========普通调用方式==========
// 普通对象
const obj = {
    uname: '张三',
    age: 18
}
//          麻烦 不方便
// obj.uname
// obj.age

// ==========对象解构——解构语法==========
const {uname , age} = {uname: '李四' , age: 20}
// 等价于 const uname = obj.uname
// 要求 声明变量的名称 要和 对象属性名一致
console.log(uname) // 李四
console.log(age) // 20

// ==========对象解构中函数this的指向==========

// 使用  对象解构的话 this 指向的是window
const {uname1 , sayHi} = {uname1: '王刚' , sayHi: function() {console.log(this)}}
console.log(uname1) // 王刚
// 必须在上面声明sayHi才能调用否则报错
sayHi() // window对象
```



#### 2 为对象解构变量名重新改名，防止变量名冲突

```js
const {uname , age} = {uname: '李四' , age: 20}
// ==========对象解构的变量名 可以重新改名 防止变量名冲突==========
const {uname: username , age: userage} = {uname: '李四' , age: 20}
console.log(username) // 李四
console.log(userage) // 20
```



**冒号表示** “什么值：赋值给谁” 

### 1.3 数组对象解构

```js
// 数组对象解构
// 数组对象
const pig = [{
    uname: '佩奇',
    age: 6
}]
// 解构 数组对象
const [{uname , age}] = pig
console.log(uname)// 佩奇
console.log(age)// 6
```



### 1.4 多级对象解构

```js
// 多级对象
const pig = {
    uname: '佩奇',
    family: {
        mother: '猪妈妈',
        father: '猪爸爸',
        sister: '乔治'
    },
    age: 6
}
// 多级对象 解构
const { uname, age, family: { mother, father, sister } } = pig
console.log(uname)// 佩奇
console.log(age)// 16
console.log(mother)// 猪妈妈
console.log(father)// 猪爸爸
console.log(sister)// 乔治
```



### 1.5 多级数组对象 解构

```js
// 多级数组对象
const pigs = [
    {
        uname: '佩奇',
        family: {
            mother: '猪妈妈',
            father: '猪爸爸',
            sister: '乔治'
        },
        age: 6
    },
    {
        uname: '大佩奇',
        family: {
            mother: '大猪妈妈',
            father: '大猪爸爸',
            sister: '大乔治'
        },
        age: 12
    }
]
// 多级 数组对象 解构
const [{ uname, age, family: { mother, father, sister } }, { uname: uname1, age: age1, family: { mother: mother1, father: father1, sister: sister1 } }] = pigs
console.log(uname)// 佩奇
console.log(age)// 6
console.log(mother)// 猪妈妈
console.log(father)// 猪爸爸
console.log(sister)// 乔治
console.log('======================')
console.log(uname1)// 大佩奇
console.log(age1)// 12
console.log(mother1)// 大猪妈妈
console.log(father1)// 大猪爸爸
console.log(sister1)// 大乔治
```

### 练习案例：

```js
// 1. 这是后台传递过来的数据
const msg = {
    "code": 200,
    "msg": "获取新闻列表成功",
    "data": [
        {
            "id": 1,
            "title": "5G商用自己，三大运用商收入下降",
            "count": 58
        },
        {
            "id": 2,
            "title": "国际媒体头条速览",
            "count": 56
        },
        {
            "id": 3,
            "title": "乌克兰和俄罗斯持续冲突",
            "count": 1669
        },
    ]
}
// ==========================请不看下面的代码完成上面的 数据解构==========================
// 形参接收到msg的对象数组后 将其进行 解构 得到msg中data的数据 数据解构 得到data的数据并改名为myData传入函数中
function render({ data: myData }) {
    // 参数列表中{data:myData} 等价于 const {data:myData} = arr
    console.log(myData)
    // 解构 数组对象
    const [{ id, title, count }, { id: id1, title: title1, count: count1 }, { id: id2, title: title2, count: count2 }] = myData
    console.log(`id: ${id}`)
    console.log(`title: ${title}`)
    console.log(`count: ${count}`)
    console.log('---------------------')
    console.log(`id: ${id1}`)
    console.log(`title: ${title1}`)
    console.log(`count: ${count1}`)
    console.log('---------------------')
    console.log(`id: ${id2}`)
    console.log(`title: ${title2}`)
    console.log(`count: ${count2}`)
}
// 将msg 对象数组 传入函数实参
render(msg)
```

## 遍历数组forEach方法 (重点)

-  forEach( ) 方法用于调用数组的每个元素，并将元素传递给回调函数
-  **主要使用场景**：<span alt=wavy>遍历数组的每个元素</span>.

>  <span alt=solid>就是增强的for循环，主要适用于 遍历 数组对象 ，数组对象越是复杂 用 forEach越是明显，但是用来遍历普通的数组并不推荐，普通数组还是用for来得比较轻松</span>.

**语法**：

```js
被遍历的数组.forEach(function(当前数组元素，当前元素索引号) {
   // 函数体
})
```



**例如**：

```js
const arr = ['red', 'pink', 'green']
arr.forEach(function (item, index) {
    console.log(item)// 依次打印数组中的每个元素
    console.log(index)// 依次打印数组元素对应的下标
})
```



<blockquote alt=danger>
	<div>
      <p>
         <span><font title=red>注意</font>：</span>
      </p>
      <p>
         <span>1. forEach主要是遍历数组</span>
      </p>
      <p>
         <span>2. 参数中获取<span alt=solid>当前数组元素</span>是必须要写的，<span alt=dotted>当前元素索引号</span>可选。</span>
      </p>
      <p>
         <span>3. <font title=red>不返回新的数组 ，只用于遍历</font></span>
      </p>
   </div>
</blockquote>

### 练习—渲染商品案例

**效果**：

![image-20230807154019723](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308071540714.png)

html与js

```js
<body>
  <div class="list">
    <!-- <div class="item">
      <img src="" alt="">
      <p class="name"></p>
      <p class="price"></p>
    </div> -->
  </div>
  <script>
    const goodsList = [
      {
        id: '4001172',
        name: '称心如意手摇咖啡磨豆机咖啡豆研磨机',
        price: '289.00',
        picture: 'https://yanxuan-item.nosdn.127.net/84a59ff9c58a77032564e61f716846d6.jpg',
      },
      {
        id: '4001594',
        name: '日式黑陶功夫茶组双侧把茶具礼盒装',
        price: '288.00',
        picture: 'https://yanxuan-item.nosdn.127.net/3346b7b92f9563c7a7e24c7ead883f18.jpg',
      },
      {
        id: '4001009',
        name: '竹制干泡茶盘正方形沥水茶台品茶盘',
        price: '109.00',
        picture: 'https://yanxuan-item.nosdn.127.net/2d942d6bc94f1e230763e1a5a3b379e1.png',
      },
      {
        id: '4001874',
        name: '古法温酒汝瓷酒具套装白酒杯莲花温酒器',
        price: '488.00',
        picture: 'https://yanxuan-item.nosdn.127.net/44e51622800e4fceb6bee8e616da85fd.png',
      },
      {
        id: '4001649',
        name: '大师监制龙泉青瓷茶叶罐',
        price: '139.00',
        picture: 'https://yanxuan-item.nosdn.127.net/4356c9fc150753775fe56b465314f1eb.png',
      },
      {
        id: '3997185',
        name: '与众不同的口感汝瓷白酒杯套组1壶4杯',
        price: '108.00',
        picture: 'https://yanxuan-item.nosdn.127.net/8e21c794dfd3a4e8573273ddae50bce2.jpg',
      },
      {
        id: '3997403',
        name: '手工吹制更厚实白酒杯壶套装6壶6杯',
        price: '99.00',
        picture: 'https://yanxuan-item.nosdn.127.net/af2371a65f60bce152a61fc22745ff3f.jpg',
      },
      {
        id: '3998274',
        name: '德国百年工艺高端水晶玻璃红酒杯2支装',
        price: '139.00',
        picture: 'https://yanxuan-item.nosdn.127.net/8896b897b3ec6639bbd1134d66b9715c.jpg',
      },
    ]

    // 1.声明一个空串
    let str = ''
    // 2.遍历数组
    goodsList.forEach(function (item, index) {
      // 对象解构
      const { name, picture, price } = item
      // 拼接字符串
      str += `
          <div class="item">
            <img src="${picture}" alt="">
            <p class="name">${name}</p>
            <p class="price">${price}</p>
          </div>
      `
    })
    // 添加到盒子中
    const list = document.querySelector('.list').innerHTML = str
  </script>
</body>
```

css

```css
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    .list {
      width: 990px;
      margin: 0 auto;
      display: flex;
      flex-wrap: wrap;
      padding-top: 100px;
    }

    .item {
      width: 240px;
      margin-left: 10px;
      padding: 20px 30px;
      transition: all .5s;
      margin-bottom: 20px;
    }

    .item:nth-child(4n) {
      margin-left: 0;
    }

    .item:hover {
      box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.2);
      transform: translate3d(0, -4px, 0);
      cursor: pointer;
    }

    .item img {
      width: 100%;
    }

    .item .name {
      font-size: 18px;
      margin-bottom: 10px;
      color: #666;
    }

    .item .price {
      font-size: 22px;
      color: firebrick;
    }

    .item .price::before {
      content: "¥";
      font-size: 14px;
    }
  </style>
```

## 综合案例—价格筛选

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308071647839.gif)

需要用到filter方法，先学习一下filter方法

### 筛选数组filter方法 (重点)

-  filter() 筛选数组

-  flter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素
-  **主要使用场景**：<font title=red>筛选数组符合条件的元素</font>，并返回筛选之后元素的新数组
-  **返回值**：返回数组，包含了符合条件的所有元素。如果没有符合条件的元素则返回空数组
-  **参数**：currentValue必须写，index 可选
-  因为返回新数组，所以不会影响原数组

![image-20230807160454114](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308071604621.png)

**基本语法**：

```js
被遍历的数组.filter(function (currentValue , index) {
   return 筛选条件
})
```



**例如**：

```js
const arr = [10,20,30]
// 使用filter过滤数组中不符合条件的元素 item是元素,index是索引
const newArr = arr.filter(function(item,index) {
    // 返回 大于20 的元素 过滤后 返回一个 真数组
    return item > 20
    // filter中只能写 比较运算符其它无效
    // return item + 20
})
console.log(newArr)
```



**简写**：

```js
const arr = [10,20,30]
// 使用filter过滤数组中不符合条件的元素 item是元素,index是索引
const newArr = arr.filter(item => item > 20)
console.log(newArr)
```



<blockquote alt=success>
	<div>
      <p>
         <span><font>总结</font>：</span>
      </p>
      <p>
         <span>1. return 过滤条件只能使用 比较运算符 其它无效</span>
      </p>
      <p>
         <span>2. 过滤返回的是 真数组</span>
      </p>
   </div>
</blockquote>

### 案例代码：

```html
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    .list {
      width: 990px;
      margin: 0 auto;
      display: flex;
      flex-wrap: wrap;
    }

    .item {
      width: 240px;
      margin-left: 10px;
      padding: 20px 30px;
      transition: all .5s;
      margin-bottom: 20px;
    }

    .item:nth-child(4n) {
      margin-left: 0;
    }

    .item:hover {
      box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.2);
      transform: translate3d(0, -4px, 0);
      cursor: pointer;
    }

    .item img {
      width: 100%;
    }

    .item .name {
      font-size: 18px;
      margin-bottom: 10px;
      color: #666;
    }

    .item .price {
      font-size: 22px;
      color: firebrick;
    }

    .item .price::before {
      content: "¥";
      font-size: 14px;
    }

    .filter {
      display: flex;
      width: 990px;
      margin: 0 auto;
      padding: 50px 30px;
    }

    .filter a {
      padding: 10px 20px;
      background: #f5f5f5;
      color: #666;
      text-decoration: none;
      margin-right: 20px;
    }

    .filter a:active,
    .filter a:focus {
      background: #05943c;
      color: #fff;
    }
  </style>
</head>

<body>
  <div class="filter">
    <a data-index="1" href="javascript:;">0-100元</a>
    <a data-index="2" href="javascript:;">100-300元</a>
    <a data-index="3" href="javascript:;">300元以上</a>
    <a href="javascript:;">全部区间</a>
  </div>
  <div class="list">
    <!-- <div class="item">
      <img src="" alt="">
      <p class="name"></p>
      <p class="price"></p>
    </div> -->
  </div>
  <script>
    // 2. 初始化数据
    const goodsList = [
      {
        id: '4001172',
        name: '称心如意手摇咖啡磨豆机咖啡豆研磨机',
        price: '289.00',
        picture: 'https://yanxuan-item.nosdn.127.net/84a59ff9c58a77032564e61f716846d6.jpg',
      },
      {
        id: '4001594',
        name: '日式黑陶功夫茶组双侧把茶具礼盒装',
        price: '288.00',
        picture: 'https://yanxuan-item.nosdn.127.net/3346b7b92f9563c7a7e24c7ead883f18.jpg',
      },
      {
        id: '4001009',
        name: '竹制干泡茶盘正方形沥水茶台品茶盘',
        price: '109.00',
        picture: 'https://yanxuan-item.nosdn.127.net/2d942d6bc94f1e230763e1a5a3b379e1.png',
      },
      {
        id: '4001874',
        name: '古法温酒汝瓷酒具套装白酒杯莲花温酒器',
        price: '488.00',
        picture: 'https://yanxuan-item.nosdn.127.net/44e51622800e4fceb6bee8e616da85fd.png',
      },
      {
        id: '4001649',
        name: '大师监制龙泉青瓷茶叶罐',
        price: '139.00',
        picture: 'https://yanxuan-item.nosdn.127.net/4356c9fc150753775fe56b465314f1eb.png',
      },
      {
        id: '3997185',
        name: '与众不同的口感汝瓷白酒杯套组1壶4杯',
        price: '108.00',
        picture: 'https://yanxuan-item.nosdn.127.net/8e21c794dfd3a4e8573273ddae50bce2.jpg',
      },
      {
        id: '3997403',
        name: '手工吹制更厚实白酒杯壶套装6壶6杯',
        price: '99.00',
        picture: 'https://yanxuan-item.nosdn.127.net/af2371a65f60bce152a61fc22745ff3f.jpg',
      },
      {
        id: '3998274',
        name: '德国百年工艺高端水晶玻璃红酒杯2支装',
        price: '139.00',
        picture: 'https://yanxuan-item.nosdn.127.net/8896b897b3ec6639bbd1134d66b9715c.jpg',
      },
    ]

    const filter = document.querySelector('.filter')
    filter.addEventListener('click', ev => {
      // const id = ev.target.dataset.index
      // 对象解构 对象名：对象解构
      const { tagName, dataset: { index } } = ev.target
      //将goodsList赋值给arr默认渲染全部的商品
      let arr = goodsList
      if (tagName === "A") {
        if (index === '1')
          arr = goodsList.filter(item => item.price > 0 && item.price <= 100)
        else if (index === '2')
          arr = goodsList.filter(item => item.price >= 100 && item.price <= 300)
        else if (index === '3')
          arr = goodsList.filter(item => item.price >= 300)
        // 渲染函数
        render(arr)
      }
    })
    // 渲染函数 封装
    function render(arr) {
      let str = ''
      arr.forEach(function (item, index) {
        const { name, price, picture } = item
        str +=
          `
        <div class="item">
          <img src="${picture}" alt="">
          <p class="name">${name}</p>
          <p class="price">${price}</p>
        </div>
        `
      })
      document.querySelector('.list').innerHTML = str
    }
    render(goodsList)

  </script>
</body>
```



## 深入对象

**学习路径**：

-  创建对象三种方式
-  构造函数
-  实例成员 & 静态成员

### 1.1 创建对象三种方式

目标：了解创建对象有三种方式

#### 1 利用对象字面量创建对象

```js
const o = {
   name: '佩奇'
}
```



#### 2 利用new Object 创建对象

```js
const o = new Object({name: '佩奇'})
console.log(o) // {name: '佩奇'}
```



### 1.2 构造函数

**利用构造函数创建对象  (自定义构造函数创建对象)** 

目标：能够利用构造函数创建对象

>  -  构造函数：是一种<font>特殊</font>的函数，主要用来<font>初始化对象</font>。
>  -  使用场景：常规的 `{...}` 语法允许创建一个对象。比如我们创建了佩奇的对象 ，继续创建乔治的对象还需要重新写一遍，此时可以通过<font title=red>构造函数</font>来<font title=red>快速创建多个类似的对象</font>。

![image-20230807170529419](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308071705310.png)

<blockquote alt=info>
	<div>
      <p>
         <span>构造函数在技术上是<font>常规函数</font></span>
      </p>
      <p>
         <span>不过有两个<font title=red>约定</font>：</span>
      </p>
      <p>
         <span>1. 它们的命名以<font title=red>大写字母开头</font></span>
      </p>
      <p>
         <span>2. 它们只能由 "<font>new</font>" 操作符来执行</span>
      </p>
   </div>
</blockquote>

-  **构造函数语法**：大写字母开头的函数

**创建构造函数**：

```js
// 创建一个 构造函数
function Pig(uname, age, gender) {
    this.uname = uname
    this.age = age
    this.gender = gender
}
// new 关键字调用 构造函数 传入实参赋值
// new Pig(参数列表)
// const Peppa 接收创建的对象
const Peppa = new Pig('佩奇', 6, '女')
const George = new Pig('乔治', 3, '男')
const Mum = new Pig('猪妈妈', 30, '女')
const Dad = new Pig('猪爸爸', 32, '男')
console.log(Peppa)
console.log(George)
console.log(Mum)
console.log(Dad)
```



==效果==：

![image-20230807172651411](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308071726698.png)

<blockquote alt=warn>
	<div>
      <p>
         <span>说明：</span>
      </p>
      <ol>
         <li>
         	<p>
               <span>使用 new 关键字调用函数的行为被称为<font title=red>实例化</font></span>
            </p>
         </li>
         <li>
         	<p>
               <span>实例化构造函数时没有参数时 可以省略 ()</span>
            </p>
         </li>
         <li>
         	<p>
               <span>构造函数内部无需写 return，返回值即为新创建的对象</span>
            </p>
         </li>
         <li>
         	<p>
               <span>构造函数内部的 return 返回的值<font title=red>无效</font>，所以不要写 return</span>
            </p>
         </li>
         <li>
         	<p>
               <span>new Object() new Date() 也是实例化构造函数</span>
            </p>
         </li>
      </ol>
   </div>
</blockquote>
<blockquote alt=success>
	<div>
      <p>
         <span><font>总结</font>：</span>
      </p>
      <ol>
         <li>
         	<p>
               <span>构造函数的作用是什么？怎么写呢？</span>
            </p>
            <ul>
               <li>
                  <p>
                     <span>构造函数是来快速<font title=red>创建多个</font>类似的<font title=red>对象</font></span>
                  </p>
               </li>
            </ul>
         </li>
          <li>
         	<p>
               <span>new 关键字调用函数的行为被称为？</span>
            </p>
             <ul>
               <li>
                  <p>
                     <span>实例化</span>
                  </p>
               </li>
            </ul>
         </li>
         <li>
         	<p>
               <span>构造函数内部需要写 return 吗，返回值是什么？</span>
            </p>
            <ul>
               <li>
                  <p>
                     <span>不需要</span>
                  </p>
               </li>
               <li>
                  <p>
                     <span>构造函数自动返回创建的新的对象</span>
                  </p>
               </li>
            </ul>
         </li>
      </ol>
   </div>
</blockquote>


#### 1.2.1 实例化执行过程

<blockquote alt=info>
	<div>
      <p>
         <span><font>说明</font>：</span>
      </p>
      <ol>
         <li>
         	<p>
               <span>创建新对象</span>
            </p>
         </li>
         <li>
         	<p>
               <span>构造函数this指向新对象</span>
            </p>
         </li>
         <li>
         	<p>
               <span>执行构造函数代码，修改this，添加新的属性</span>
            </p>
         </li>
         <li>
         	<p>
               <span>返回新对象</span>
            </p>
         </li>
      </ol>
   </div>
</blockquote>

```js
// 1.创建构造函数
function Pig(uname) {
   this.uname = uname
}
// 2.new 关键字 调用函数
// new Pig('佩奇')
// 接收创建的对象
const peppa = new Pig('佩奇')
console.log(peppa) // {name: '佩奇'}
```



![image-20230807175138441](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308071751121.png)

### 1.3 实例成员&静态成员

#### 1.3.1 实例成员：

通过构造函数创建的对象称为实例对象，<font title=red>实例对象中</font>的<font title=red>属性和方法</font>称为<font title=red>实例成员</font> (实例属性和实例方法)

```js
// 实例成员和静态成员
// 1.实例成员：实例对象上的属性和方法属于实例成员
function Pig(uname) {
    this.uname = uname
}
const peppa = new Pig('佩奇')
peppa.uname = '小猪佩奇今年6岁'
const George = new Pig('乔治')
console.log(peppa)// Pig {uname: '小猪佩奇今年6岁'}
console.log(George)// Pig {uname: '乔治'}
console.log(peppa === George) // false
```



<blockquote alt=info>
	<div>
      <p>
         <span><font>说明</font>：</span>
      </p>
      <ol>
         <li>
         	<p>
               <span>为构造函数传入参数，创建结构相同但值<font title=red>不同的对象</font></span>
            </p>
         </li>
         <li>
         	<p>
               <span>为构造函数创建的实例对象<font title=red>彼此独立</font>互不影响</span>
            </p>
         </li>
      </ol>
   </div>
</blockquote>

#### 1.3.2 静态成员

<font title=red>构造函数</font>的属性和方法被称为<font title=red>静态成员</font> (静态属性和静态方法)

```js
// 构造函数
function Person(uname , age) {
   // 省略实例成员
}
// 静态属性
Person.eyers = 2
Person.arms = 2
// 静态方法
Person.walk = function() {
   console.log('^_^人都会走路...')
   // this指向Person
   console.log(this.eyes)
}
```



<blockquote alt=info>
	<div>
      <p>
         <span><font>说明</font>：</span>
      </p>
      <ol>
         <li>
         	<p>
               <span>静态成员只能构造函数来访问</span>
            </p>
         </li>
         <li>
         	<p>
               <span>静态方法中的this指向构造函数</span>
            </p>
         </li>
      </ol>
   </div>
</blockquote>

比如 `Date.now()` `Math.PI` `Math.random()` 

<blockquote alt=success>
	<div>
      <P>
         <span><font>总结</font>：</span>
      </P>
      <ol>
         <li>
         	<p>
               <span>实例成员 (属性和方法) 写在谁身上？</span>
            </p>
            <ul>
               <li>
                  <p>
                     <span><font title=red>实例对象</font>的属性和方法即为实例成员</span>
                  </p>
               </li>
               <li>
                  <p>
                     <span>实例对象<font title=red>相互独立</font>，<font title=red>实例成员</font>当前实例对象使用</span>
                  </p>
               </li>
            </ul>
         </li>
         <li>
         	<p>
               <span>静态成员 (属性和方法) 写在谁身上？</span>
            </p>
            <ul>
               <li>
                  <p>
                     <span><font title=red>构造函数</font>的属性和方法被称为静态成员</span>
                  </p>
               </li>
               <li>
                  <p>
                     <span>静态成员只能构造函数访问</span>
                  </p>
               </li>
            </ul>
         </li>
      </ol>
   </div>
</blockquote>


## 内置构造函数

>  学习路径:
>
>  -  Object
>  -  Array
>  -  String
>  -  Number

在JavaScript中<font title=red>最主要</font>的数据类型有6种：

**基本数据类型**：

-  字符串，数值，布尔，undefined，null

**引用类型**：

-  对象

**但是我们会发现有些特殊情况**：

```js
//普通 字符串
const str = 'pink'
//普通 整数类型
console.log(str.length) // 4
const num = 12
console.log(num.toFixed(2)) // 12.00
// 实际上js底层完成的 把简单数据类型包装为了引用数据类型
new String('pink')
new Number(12)
```

其实==字符串==，==数值==，==布尔==，等基本类型==也有专门的构造函数==，这些我们称为==包装类型==。

<span alt=wavy>JS中几乎==所有==的数据都可以基于构成函数==创建==</span>。

### JS提供的内置构造函数

#### 引用类型

-  Object，Array，RegEp，Date等

#### 包装类型

-  String，Number，Boolean等

### 1 Object

Object是内置的构造 函数，用于==创建普通对象==。

```js
//通过构造函数创建普通对象
const user = new Object({uname: '小明' , age: 15})
```

推荐使用==字面量==方式声明对象，而不是Object构造函数。

学习三个常用静态方法 (静态方法就是==只有构造函数Object可以调用==的)

**获取对象里面的属性和值怎么做**？

**以前的方法**：

```js
//想要获取对象里面的属性和值怎么做？
const o = {uname: '佩奇' , age: 6}
for(let k in o) {
   console.log(k)// 属性 uname age
   console.log(o[k])// 值 佩奇 6
}
```

**新的方法**。

-  **作用**：Object.keys(obj) 静态方法获取对象中 ==所有属性名== (键)

-  **作用**：Object.values(obj) 静态方法获取对象中 ==所有属性值== (值)

-  **作用**：Object.assign(空对象，拷贝对象) 静态方法常用于==对象拷贝==.

   -  **使用场景**：给一个对象==追加属性==.

   -  **语法**Object.assign：

      ```js
      //==========对象的拷贝==========
      const obj1 = {}
      // 将obj拷贝到obj1中
      Object.assign(obj1, obj)
      //==========对象追加属性==========
      Object.assign(obj1, { gender: '女' })
      console.log(obj1)
      ```

      

-  **语法**Object.keys & Object.values：

```js
const obj = { uname: '佩奇', age: 6 }
// keys 和 values 返回的是一个 真数组
// 获取对象的所有属性名
console.log(Object.keys(obj))// ['uname', 'age']
// 获取对象的所有属性值
console.log(Object.values(obj))// ['佩奇', 6]
```



<blockquote alt=danger>
	<div>
      <p>
         <span><font title=red>注意</font>：返回的是一个 真数组</span>
      </p>
   </div>
</blockquote>
### 2 Array

Array是内置的构造函数，用于==创建数组==.

```js
const arr = new Array(3,5)
console.log(arr)// [3,5]
```



创建数组建议使用<font title=red>字面量</font>创建，不用Array构造函数创建

###### 1 数组常见==实例方法==—核心方法

| 方法    | 作用                            | 说明                                                         |
| ------- | ------------------------------- | ------------------------------------------------------------ |
| forEach | <font title=red>遍历</font>数组 | 不返回数组，经常用于<font title=red>查找遍历数组元素</font>  |
| filter  | <font title=red>过滤</font>数组 | <font title=red>返回新数组</font>，返回的是<font title=red>筛选满足条件</font>的数组元素 |
| map     | <font title=red>迭代</font>数组 | <font title=red>返回新数组</font>，返回的是<font title=red>处理之后</font>的数组元素，想要使用返回的新数组 |
| reduce  | <font title=red>累计器</font>   | 返回累计处理的结果，经常用于<font title=red>求和</font>等    |

**理解图**：

![image-20230808091415006](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308080914757.png)

#### reduce

-  **作用**：==reduce== 返回<font title=red>累计处理的结果</font>，经常用于<font title=red>求和等</font>.
-  **基本语法**：

```js
const arr = [1,2,3,4]
```



```js
arr.reduce(function() {},起始值)
```



```js
arr.reduce(function(上一次值，当前值) {},起始值)
```



-  **参数**：

   1 如果<font title=red>有起始值</font>，则把初始值==累加==到里面

```js
// arr.reduce(function (上一次的值, 当前的值) { }, 初始值) 有没有初始值 会影响到最后的计算结果
const arr = [1, 5, 8]
// 1. 没有初始值
const total = arr.reduce(function (prev, current) {
    return prev + current
})
// 没有初始值 直接将数组的所有元素进行累加
console.log(`没有初始值: ${total}`)// 14
// 2. 有初始值
const initTotal = arr.reduce(function (prev, current) { 
    return prev + current
}, 10)
// 有初始值 将数组所有元素进行累加并加上自己的初始值
console.log(`有初始值: ${initTotal}`)// 24
```



**使用箭头函数写法**：

```js
//===============箭头函数==============
const totalJ = arr.reduce((prev, current) => prev + current, 10)
console.log(`箭头函数: ${totalJ}`)// 24
```



<blockquote alt=warn>
	<div>
      <ul>
         <li>
         	<p>
               <span>reduce执行过程：</span>
            </p>
         </li>
      </ul>
      <ol>
         <li>
         	<p>
               <span>如果<font title=red>没有起始值</font>，则<font tite=red>上一次值以</font>数组的<font title=red>第一个数组元素的值</font></span>
            </p>
         </li>
         <li>
         	<p>
               <span>每一次循环，把<font title=red>返回值</font>给做为，下一次循环的<font title=red>上一次值</font></span>
            </p>
         </li>
          <li>
         	<p>
               <span>如果<font title=red>有起始值</font>，则 起始值做为<font title=red>上一次值</font></span>
            </p>
         </li>
      </ol>
   </div>
</blockquote>

**分析过程表**：对比下面代码和表

```js
const arr = [1, 5, 8]
const total = arr.reduce(function (prev, current) {
    return prev + current
})
```



| 上一次值 | 当前值 | 返回值 | (没有起始值的情况) |
| -------- | ------ | ------ | ------------------ |
| 1        | 5      | 6      | 第一次循环         |
| 6        | 8      | 14     | 第二次循环         |

| 上一次值   | 当前值 | 返回值 | (有起始值的情况 比方 10) |
| ---------- | ------ | ------ | ------------------------ |
| 起始值(10) | 1      | 11     | 第一次循环               |
| 11         | 5      | 16     | 第二次循环               |
| 16         | 8      | 24     | 第三次循环               |

<blockquote alt=success>
	<div>
      <p>
         <span><font>总结</font>：</span>
      </p>
      <ol>
         <li>
         	<p>
               <span>对数组中所有的元素进行累加计算 返回求和结果</span>
            </p>
         </li>
         <li>
         	<p>
               <span>有，没有起始值会影响到 最后的计算结果，起始值第一次循环在上一次值中进行运算</span>
            </p>
         </li>
      </ol>
   </div>
</blockquote>

##### 案例—求员工的薪资

```js
const arr = [
    {
        uanme: '张三',
        salary: 10000
    },
    {
        uanme: '李四',
        salary: 10000
    },
    {
        uanme: '王五',
        salary: 10000
    }
]
// 解构 方式
const [{ salary, salary: salary1, salary: salary2 }] = arr
const number = [salary, salary1, salary2]
const total = number.reduce((prev, current) => prev + current)
console.log(`总工资方式一：${total}`)// 总工资方式一：3000
// 初始值 方式
// 上一次值 如果有 起始值 则为 起始值，当前值是一个对象 调用 salary进行计算 0 + 10000
//   _______________________________________.
// / 。。。    + ---- +         ___________...
// + ---------|计算过程|---------___________ +
// | 上一次值 | 当前值 | 结果   | ===========||
// |    0    | 10000 | 10000  | 第一次计算 + |
// |  10000  | 10000 | 20000  | 第二次计算 + |
// |  20000  | 10000 | 30000  | 第三次计算 + |
// +----------------------------------------+
const total2 = arr.reduce((prev, current) => prev + current.salary, 0)
console.log(`总工资方式二：${total2}`)// 总工资方式二：30000
```



---

#### 2.1 数组常见方法—其它方法

>  1.  实例方法==join==数组元素拼接为字符串，返回字符串 (重点)
>  2.  实例方法==find==查找元素，返回符合测试条件的第一个数组元素值，如果没有符合条件的则返回==undefined==( 重点)
>  3.  实例方法==every==检测数组所有元素是否都符合指定条件，如果**所有元素**都通过检测都返回true，否则返回false (重点)
>  4.  实例方法==some==检测数组中的元素是否满足指定条件，**如果数组中有**元素满足条件返回true，否则返回false
>  5.  实例方法==concat==合并两个数组，返回生成新数组
>  6.  实例方法==sort==对原数组单元值排序
>  7.  实例方法==splice==删除或替换原数组单元
>  8.  实例方法==reverse==反转数组
>  9.  实例方法==findIndex==查找元素的索引值

![image-20230808113520614](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308081135529.png)

##### 1 find

```js
const arr = [
    {
        uname: '小米',
        price: 1999
    },
    {
        uname: '华为',
        price: 3999
    },
    {
        uname: '小米',
        price: 2000
    }
]
// 查找uname为小米的对象 并返回,注意：find是找到后直接返回而不是遍历数组的全部对象
// 第二个小米不会输出 找到第一个小米后就直接返回了
const newArr = arr.find(item => item.uname === '小米')
console.log(newArr)// {uname: '小米', price: 1999}
```



<blockquote alt=info>
	<div>
      <ul>
         <li>
         	<p>
               <span>查找数组中的某一个满足条件的元素并返回一个伪数组</span>
            </p>
         </li>
         <li>
         	<p>
               <span>查找时只要找到满足条件的直接返回 不会再继续进行查找 所以返回的是第一个满足条件的</span>
            </p>
         </li>
         <li>
         	<p>
               <span>如果找不到符合条件的 则返回 <font title=red>undefined</font></span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>

##### 2 every

```js
// every 检测数组所有元素是否都符合条件，都符合返回true 其中一个不符合返回false
const arr = [10, 20, 30]
const flag = arr.every(item => item >= 10)
console.log(flag)// true
//---------------------------------------------------------------------------
const arr = [10, 20, 30]
const flag = arr.every(item => item >= 20)
console.log(flag)// false
```



<blockquote alt=info>
	<div>
      <ul>
         <li>
         	<p>
               <span>检测数组的所有元素，检测是否都满足条件</span>
            </p>
         </li>
         <li>
         	<p>
               <span>若全部都满足条件则返回true，若其中一个不满足则返回false</span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>



##### 3 some

```js
// some 检测数组所有元素是否有其中一个符合条件，有 则返回true, 都没有(一个也没有)则返回false
const flag1 = arr.some(item => item >= 20)
console.log(flag1)// true
```



<blockquote alt=info>
	<div>
      <ul>
         <li>
         	<p>
               <span>检测数组中的所有元素，检测是否有其中一个满足条件</span>
            </p>
         </li>
         <li>
         	<p>
               <span>若其中一个满足条件则返回 true，若全都不满足条件则返回 false</span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>

---

### 3 String

在JavaScript中的==字符串==，==数值==，==布尔==具有对象的使用特征，如具有属性和方法

```js
//字符串类型
const str = 'hellow world!'
//统计字符的长度 (字符数量)
console.log(str.length)
//数值类型
coonst price = 12.345
//保留两位小数
price.toFixed(2)
```

之所以具有对象特征的原因是字符串，数值，布尔类型数据是JavaScript底层使用Object构造函数 “包装” 来的，被称为 <font title=red>包装类型</font>.

##### String 常见实例方法

>  1.  实例属性==`length`==用来获取字符串的长度 (重点)
>  2.  实例方法==`split('分隔符')`==用来以特定的规则将字符串拆分成数组 (重点)
>  3.  实例方法==`substring(需要截取的第一个字符的索引[，结束的索引号])`==用于字符串截取 (重点)
>  4.  实例方法==`startsWith(检测字符串[，检测位置索引号])`==检测是否以某字符开头 (重点)
>  5.  实例方法==`includes(搜索的字符串[，检测位置索引号])`==判断一个字符串是否包含在另一个字符串中，根据情况返回 布尔值 (重点)
>  6.  实例方法==`toUppreCase(str)`==用于将字母转换成大写
>  7.  实例方法==`toLowerCase(str)`==用于将字母转换成小写
>  8.  实例方法==`indexOf(str)`==获取某个字符的下标号
>  9.  实例方法==`endWith`==检测是否以某字符结尾
>  10.  实例方法==`replace`==用于替换字符串，支持正则匹配
>  11.  实例方法==`math`==用于查找字符串，支持正则匹配

###### 1 split

```js
const str = 'pink,red,green'
// split使用特定的方式对数组进行切割并返回一个切割后的数组 , split返回一个 真数组
console.log(str.split(','))//['pink', 'red', 'green']
const str1 = '2023-07-08'
console.log(str1.split('-'))//['2023', '07', '08']
```



###### 2 substring

```js
// 字符串截取
const str = 'wecomtonewyourk!!!'
// 从字符串下标7开始 到最后 没有指定结束位置默认为直到该字符串的最后
console.log(str.substring(7))// newyourk
// 包头不包尾
console.log(str.substring(7,9))// ne
```



###### 3 startsWith

```js
const str = 'wecomtonewyourk!!!'
//上面字符串是以w开头的，第二个参数不写默认以开头开始判断
console.log(str.startsWith('w'))// true
console.log(str.startsWith('e'))// false
// 从下标2的位置开始
console.log(str.startsWith('comto',2))// true
```



###### 4 incudes

```js
const str = 'wecomtonewyourk!!!'
//判断上面字符串中是否包含your字符串
console.log(str.includes('your'))// true
// 第二个参数是指定从索引12开始但是12超过了y了所以就不包含your了
console.log(str.includes('your',12))// false
```



#### 练习—显示赠品练习

请将下面字符串渲染到准备好的p标签内部，结构必须如坐下图所示，展示效果如右图所示：

```js
const gift = '50g茶叶，清洗球'
```



>  思路：
>
>  1.  把字符串拆分为数组，这样两个赠品就拆分开了 用哪个方法？ `split('')` 
>  2.  利用==map==遍历数组，同时把数组元素生成到span里面，并且返回
>  3.  因为返回的是数组，所以需要 转换为 字符串，用哪个方法？` join` 

**代码**：

```js
const gift = '50g的茶叶,清洗球'
// 以,将数组进行分割，map遍历split返回的数组并进行处理返回新的数组 再使用join将数组的内容以特定形式进行拼接
const str = gift.split(',').map(item => `
    <span>赠品:【${item}】</span><br>
`).join('')
// 将内容添加到div中
const div = document.querySelector('div').innerHTML = str
```



**效果**：

![image-20230808164254967](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308081642453.png)

### 4 Number

Number是==内置==的==构造函数==，用于==创建数值==.

>  <font>常用方法</font>：
>
>  toFixed() 设置保留小数位的长度

```js
//数值类型
const price = 12.345
//保留两位小数 四舍五入
console.log(price.toFixed(2))// 12.35
```



### 购物车展示案例:star: 

```js
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    .list {
      width: 990px;
      margin: 100px auto 0;
    }

    .item {
      padding: 15px;
      transition: all .5s;
      display: flex;
      border-top: 1px solid #e4e4e4;
    }

    .item:nth-child(4n) {
      margin-left: 0;
    }

    .item:hover {
      cursor: pointer;
      background-color: #f5f5f5;
    }

    .item img {
      width: 80px;
      height: 80px;
      margin-right: 10px;
    }

    .item .name {
      font-size: 18px;
      margin-right: 10px;
      color: #333;
      flex: 2;
    }

    .item .name .tag {
      display: block;
      padding: 2px;
      font-size: 12px;
      color: #999;
    }

    .item .price,
    .item .sub-total {
      font-size: 18px;
      color: firebrick;
      flex: 1;
    }

    .item .price::before,
    .item .sub-total::before,
    .amount::before {
      content: "¥";
      font-size: 12px;
    }

    .item .spec {
      flex: 2;
      color: #888;
      font-size: 14px;
    }

    .item .count {
      flex: 1;
      color: #aaa;
    }

    .total {
      width: 990px;
      margin: 0 auto;
      display: flex;
      justify-content: flex-end;
      border-top: 1px solid #e4e4e4;
      padding: 20px;
    }

    .total .amount {
      font-size: 18px;
      color: firebrick;
      font-weight: bold;
      margin-right: 50px;
    }
  </style>
</head>

<body>
  <div class="list">
    <!-- <div class="item">
      <img src="https://yanxuan-item.nosdn.127.net/84a59ff9c58a77032564e61f716846d6.jpg" alt="">
      <p class="name">称心如意手摇咖啡磨豆机咖啡豆研磨机 <span class="tag">【赠品】10优惠券</span></p>
      <p class="spec">白色/10寸</p>
      <p class="price">289.90</p>
      <p class="count">x2</p>
      <p class="sub-total">579.80</p>
    </div> -->
  </div>
  <div class="total">
    <div>合计：<span class="amount">1000.00</span></div>
  </div>
  <script>
    const goodsList = [
      {
        id: '4001172',
        name: '称心如意手摇咖啡磨豆机咖啡豆研磨机',
        price: 289.9,
        picture: 'https://yanxuan-item.nosdn.127.net/84a59ff9c58a77032564e61f716846d6.jpg',
        count: 2,
        spec: { color: '白色' }
      },
      {
        id: '4001009',
        name: '竹制干泡茶盘正方形沥水茶台品茶盘',
        price: 109.8,
        picture: 'https://yanxuan-item.nosdn.127.net/2d942d6bc94f1e230763e1a5a3b379e1.png',
        count: 3,
        spec: { size: '40cm*40cm', color: '黑色' }
      },
      {
        id: '4001874',
        name: '古法温酒汝瓷酒具套装白酒杯莲花温酒器',
        price: 488,
        picture: 'https://yanxuan-item.nosdn.127.net/44e51622800e4fceb6bee8e616da85fd.png',
        count: 1,
        spec: { color: '青色', sum: '一大四小' }
      },
      {
        id: '4001649',
        name: '大师监制龙泉青瓷茶叶罐',
        price: 139,
        picture: 'https://yanxuan-item.nosdn.127.net/4356c9fc150753775fe56b465314f1eb.png',
        count: 1,
        spec: { size: '小号', color: '紫色' },
        gift: '50g茶叶,清洗球'
      }
    ]
	 // map遍历返回处理后的数据
    document.querySelector('.list').innerHTML = goodsList.map(item => {
      // 对象解构
      let { name, price, picture, count, gift, spec } = item
      //获取 spec对象 所有属性值并以/拼接
      const text = Object.values(spec).join('/')
      // 求出一个商品的总价
      let subTotal = ((price  * 100 * count) / 100).toFixed(2)
      // 处理赠品模块
      // 三目运算符判断是否存在gift如果不存在则gift为undefined执行 '' 如果存在执行前面的操作
      // gift内容以 , 分割 返回数组 用map遍历 处理数据后 返回数组 并join拼接为 字符串
      const  str = gift ? gift.split(',').map(item => `<span class="tag">【赠品】${item}</span>`).join('') : ''
      // 返回所有数据都处理后的结果
      return `
            <div class="item">
              <img src="${picture}" alt="">
              <p class="name">${name} ${str}</p>
              <p class="spec">${text} </p>
              <p class="price">${price.toFixed(2)}</p>
              <p class="count">x${count}</p>
              <p class="sub-total">${subTotal}</p>
            </div>
          `
    }).join('') // 拼接为字符串
    // 合计模块
    document.querySelector('.amount').innerHTML = goodsList.reduce((prev,current) => prev + current.count * current.price,0).toFixed(2)
  </script>
</body>
```

**效果**：

![image-20230809090121616](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308090901457.png)

## 练习-完成以下需求

```js
const spec = {size: '40cm*40cm',color: '黑色'}
```

请将size和color里面的值拼接为字符串之后，写道div标签里面，展示内容如下：

![image-20230808140020322](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308081400848.png)

>  <font>思路</font>：获得所有的属性值，然后拼接字符串就可以了
>
>  1.  获得所有属性值是：`Object.values(arr)` 返回的是 真数组
>  2.  拼接数组是 `join('/')`这样就可以转换为字符串了

```js
const arr = { size: '40*40', color: '黑色' }
// 获取div元素对象，Object.values获取对象所有的属性值返回一个真数组，使用join进行拼接以'/'来拼接 最后innerHTML到div元素内容中
const div = document.querySelector('div').innerHTML = Object.values(arr).join('/')
```


```html
<div></div>
```


```less
body {
    margin: 0;
    padding: 0;
    -webkit-tab-heiglight-color: transparent;
    box-sizing: border-box;
    min-width: 370px;
    background-color: #fff;
    height: 1000px;
    div {
        position: sticky;
        top: 10px;
        left: 100px;
        text-align: center;
        line-height: 2;
        color: #ccc;
        width: 125px;
        height: 36px;
        border: 1px #ccc solid;
    }
}
```

## 伪数组转换为真数组

>  伪数组==盘点==：
>
>  1.  querySelectorAll() 获取所有的子元素对象
>  2.  getElements带s的都是伪数组
>  3.  children 获得所有子元素节点
>  4.  arguments 动态参数
>  5.  find查找满足条件的第一个元素并返回一个伪数组
>
>  统统可以使用 静态方法： Array.from(伪数组) 来进行转换为 真数组

静态方法 Array.from()

```html
<ul>
   <li>1</li>
   <li>2</li>
   <li>2</li>
</ul>
```

```js
// 获取ul中所有的li返回一个伪数组
const lis = document.querySelectorAll('ul li')
// lis.pop() 伪数组不能使用函数
console.log(lis)
// 将伪数组转换为真数组
const newLis = Array.from(lis)
// 可以使用pop函数 移动并返回 数组最后一个元素
const val = newLis.pop()
console.log(newLis)
console.log(val)
```
