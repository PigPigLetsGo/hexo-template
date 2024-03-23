---
title: JS进阶-深入面向对象
categories:
   - [计算机学科,web,js,JS进阶]
tags:
   - 计算机学科
   - web
   - js
---

## 深入面向对象

### 1 编程思想

-  面向过程
-  面向对象

#### 1.1 面向过程

目标：从生活例子了解什么是面向过程

>  -  <font title=red>面向过程</font>：就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候再一个一个的依次调用就可以了。
>  -  <font>举个栗子</font>：蛋炒饭
>
>  1.  蒸米饭
>  2.  炒鸡蛋
>  3.  放点，胡萝卜丁，洋葱，香肠，红椒
>  4.  最后将它们依次调用 就得出了 香喷喷的 ==蛋炒饭==.
>
>  <font>面向过程</font>，就是按照我们分析好了的步骤，按照步骤解决问题。

---

#### 1.2 面向对象 (oop)

目标：从生活例子了解什么是面向对象编程

>  -  <font title=red>面向对象</font>：是把事务分解成为一个个对象，然后由对象之间分工合作。
>  -  <font>举个栗子</font>：盖浇饭
>
>  1.  下面的配菜都提前准备好了然后直接调用就好了

![image-20230809091454087](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308090914759.png)

>  -  在面向对象程序开发思想中，每一个对象都是功能中心，具有明确分工。
>  -  面向对象编程具有灵活，代码可复用，容易维护和开发的优点，更适合多人合作的大型软件项目。
>  -  <font>面向对象的特性</font>：
>     -  封装性
>     -  继承性
>     -  多态性

![image-20230809092546938](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308090925578.png)

**继承**：继承自拖拉机，实现了扫地的接口

**封装**：无需知道如何运作，开动即可

**动态**：平时扫地，天热当风扇

**重用**：没用额外动力，重复利用了发动机能量

 **多线程**：多个扫把同时工作

**低耦合**：扫把可以换成拖把而无须改动

**组件编程**：每个配件都是可单独利用的工具

**适配器模式**：无需造发动机，继承自拖拉机，只取动力方法

**代码托管**：无需管理垃圾，直接扫到路边即可

---

#### 1.3 面向过程&面向对象 对比

##### 1.3.1 面向过程编程

**优点**：

性能比面向对象高，适合跟硬件联系很紧密的东西，例如单片机就采用的面向过程面呈。

**缺点**：

没有面向对象易维护，易复用，易扩展

##### 1.3.2 面向对象编程 (oop)

**优点**：

易维护，易复用，易扩展，由于面向对象有封装，继承，多态的特性，可以设计出低耦合的系统，使系统 更加灵活，更加易于维护

**缺点**：

性能比面向过程低

---

### 2 构造函数

>  -  封装是面向对象思想中比较重要的一部分，JS面向对象可以通过==构造函数实现的封装==。
>  -  同样的将变量和函数组合到了一起并通过==this实现数据的共享==，所不同的是借助==构造函数创建出来的实例对象之间是彼此不影响==的

```js
function Star(uname , age) {
   this.uname = uname
   this.age = age
   this.sing = function() {
      console.log('我会唱歌')
   }
}
//实例对象，获得了构造函数中封装的所有逻辑
const ldh = new Star('刘德华' , 18)
const zxy = new Star('张学友' , 19)
console.log(ldh === zxy)// false
console.log(ldh.sing === zxy.sing)// false
```



<blockquote alt=danger>
	<div>
      <p>
         <span><font title=red>注意</font>：</span>
      </p>
      <ul>
         <li>
         	<p>
               <span>封装是面向对象思想中比较重要的一部分，JS面向对象可以通过<span alt=solid>构造函数实现的封装</span></span>
            </p>
         </li>
          <li>
         	<p>
               <span><span alt=wavy>前面我们学过的构造函数方法很好用</span>，但是<font title=red>存在浪费内存的问题</font></span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>


![image-20230809100029870](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091000680.png)



<blockquote alt=success>
	<div>
      <p>
         <span><font>总结</font>：</span>
      </p>
      <ol>
         <li>
         	<p>
               <span>构造函数体现了面向对象的<span alt=solid>封装特性</span></span>
            </p>
         </li>
         <li>
         	<p>
               <span><span alt=wavy>构造函数实例创建的对象彼此独立，互不影响</span></span>
            </p>
         </li>
      </ol>
   </div>
</blockquote>



<blockquote alt=info>
	<div>
      <p>
         <span>我们希望<span alt=solid>所有的对象使用同一个函数，这样就比较节省内存</span>，那么我们要怎么做呢？</span>
      </p>
      <p>
         <span>我们就需要用到<font title=red>原型了</font> 下面学习原型</span>
      </p>
   </div>
</blockquote>


### 3 原型

<blockquote alt=info>
	<div>
      <p>
         <span><font>学习路径</font>：</span>
      </p>
      <ul>
         <li>
         	<p>
               <span>原型</span>
            </p>
         </li>
         <li>
         	<p>
               <span>constructor属性</span>
            </p>
         </li>
         <li>
         	<p>
               <span>对象原型</span>
            </p>
         </li>
         <li>
         	<p>
               <span>原型继承</span>
            </p>
         </li>
         <li>
         	<p>
               <span>原型链</span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>

#### 3.1 原型(prototype)

目标：能够利用==原型对象== 实现 ==方法 共享==.

>  -  构造函数通过==原型分配的函数==是所有对象所<font title=red>共享的</font>。
>  -  JavaScript规定，<font title=red>每一个构造函数都有一个prototype属性</font>，指向另一个对象，所以我们也称为<font title=red>原型对象</font>.
>  -  这个对象可以<font title=red>挂载函数</font>，对象实例化不会多次创建<span alt=solid>原型</span>上函数，<span alt=wavy>节约内存</span>.
>  -  ==我们可以把那些不变的方法，直接定义在 prototype 对象上^不要直接写在构造函数中^，这样所有对象的实例就可以共享这些方法==。
>  -  ==构造函数和原型对象中的 this 都指向 实例化的对象==。

![image-20230809102822018](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091028554.png)

```js
// 1. 公共的属性写到 构造函数里面
function Star(uname, age) {
    this.uname = uname
    this.age = age
    // 构造函数实例对象都是独立的彼此互不影响但是 多个对象就会有多个不同地址的函数 浪费内存
    //不建议将函数写在构造函数中
    // this.sing = function () {
    //     console.log('唱歌')
    // }
}
// 2. 公共的函数写到 原型对象 身上
Star.prototype.sing = function() {
    console.log('唱歌')
}
const ldh = new Star('刘德华', 18)
const zxy = new Star('张学友', 19)
ldh.sing()// 唱歌
zxy.sing()// 唱歌
console.log(ldh.sing === zxy.sing)
```

![image-20230809103215391](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091032780.png)

<blockquote alt=success>
	<div>
      <p>
         <span><font>总结</font>：</span>
      </p>
      <ul>
         <li>
         	<p>
               <span>公共的属性写到，构造函数里面</span>
            </p>
         </li>
         <li>
         	<p>
               <span>公共的函数写到 原型对象 身上</span>
            </p>
         </li>
      </ul>
      <hr>
      <ol>
         <li>
         	<p>
               <span>原型是什么？</span>
            </p>
            <ul>
               <li>
                  <p>
                     <span>一个对象，我们也称为<font>prototype</font>为<font>原型对象</font></span>
                  </p>
               </li>
            </ul>
         </li>
         <li>
         	<p>
               <span>原型的作用是什么？</span>
            </p>
            <ul>
               <li>
                  <p>
                     <span>共享方法</span>
                  </p>
               </li>
               <li>
                  <p>
                     <span>可以把那些<font>不变的方法</font>，直接定义在<font>prototype</font>对象上</span>
                  </p>
               </li>
            </ul>
         </li>
          <li>
         	<p>
               <span>构造函数和原型里面的this指向谁？</span>
            </p>
              <ul>
               <li>
                  <p>
                     <span><font title=red>实例化的对象</font></span>
                  </p>
               </li>
            </ul>
         </li>
      </ol>
   </div>
</blockquote>


>  两句话：
>
>  1.  所有的对象里面都有`__proto__`对象原型 指向原型`prototype`对象
>      -  `__proto__.constructor`指向构造函数
>  2.  所有的原型对象`(prototype)`里面都有`constructor`，指向 创建该原型对象的构造函数



#### 3.1.1 原型—this指向

目标：能够说出构造函数和原型对象中的this指向

>  ==构造函数==和==原型对象==中的==this==都==指向==，==实例化的对象==.

```js
let that
function Star(uname, age) {
    // 构造函数里面的 this 就是 实例对象 ldh
    that = this
    // console.log(this)
    this.uanem = uname
    this.age = age
}
let that1
// 原型对象里面的函数 this 指向的还是 实例对象 ldh
Star.prototype.sing = function() {
    that1 = this
    console.log('会唱歌')
}
// 实例对象ldh
const ldh = new Star('刘德华', 18)
ldh.sing()
// 判断两个 对象是否是同一个 对象
console.log(that === ldh) // true
console.log(that1 === ldh) // true
```



<blockquote alt=success>
	<div>
      <p>
         <span><font>总结</font>：</span>
      </p>
      <ol>
         <li>
         	<p>
               <span>构造函数和原型里面的this指向谁？</span>
            </p>
         </li>
      </ol>
      <ul>
         <li>
         	<p>
               <span>实例化的对象</span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>

#### 3.1.2 数组扩展案例—求最大值和数组求和

**代码**：

```js
// 自己定义 数组扩展方法  求和 和 最大值
// 1.我们定义的这个方法， 任何一个数组实例对象都可以使用
// 2.自定义的方法写到 数组.prototype 身上
const arr = [1, 2, 3]
// 最大值
// 数组实例.prototype原型.max 返回最大值
Array.prototype.max = function () {
    // 原型函数里面的this指向 实例对象arr 并 展开 this
    return Math.max(...this)
}
console.log(arr.max())// 3
// 求和函数
Array.prototype.sum = function () {
    // 原型函数里面的this指向 实例对象arr
    return this.reduce((prev, current) => prev + current, 0)
}
console.log(arr.sum())// 6
```



---

#### 3.2 constructor属性

**在哪里**？每个==原型对象==里面都有个`constructor`属性(`constructor`构造函数)

**作用**：该属性<font title=red>指向</font>该==原型对象==的<font title=red>构造函数，简单理解，就是指向我的爸爸，我是有爸爸的孩子</font>.

![image-20230809111807683](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091118245.png)

**代码**：

```js
// 构造函数
function Star() {}
// 实例化对象
const ldh = new Star()
// 只要是原型对象里面默认都有一个 constructor属性并且指向 Star
console.log(Star.prototype)//constructor : ƒ Star()
// 判断原型里面的构造函数属性是否与 Star构造函数是相同的
console.log(Star.prototype.constructor === Star)// true
```



**使用场景**：

>  如果有==多个方法的对象==，我们可以给==原型对象采取对象形式赋值==。
>
>  但是这样就==会覆盖构造函数原型对象原来的内容==，这样修改后的 原型对象 `constructor` 就==不再指向当前构造函数了==此时，我们可以在==修改后的原型对象中==，==添加一个 `constructor` 指向原来的构造函数==。

**以下两种方式的区别**：

```java
Star.prototype.sing = function () {}
//-----------------------------------
Star.prototype = {
    sing: function() {
        console.log('唱')
    }
}
```



**代码**：

```js
// 构造函数
function Star() { }
// 这么写 比较麻烦
// 追加的不会覆盖原型中的constructor属性的指向
Star.prototype.sing = function () {
    console.log('唱')
}
Star.prototype.dance = function () {
    console.log('跳')
}
Star.prototype.warp = function () {
    console.log('warp')
}
Star.prototype.qiu = function () {
    console.log('篮球')
}
// 实例化对象
const ldh = new Star()
ldh.sing()
ldh.dance()
ldh.warp()
ldh.qiu()
console.log(Star.prototype)
```



**结果**：

![image-20230809113309022](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091133419.png)

```js
// 构造函数
function Star() { }
// 赋值的会覆盖原型中原有的constructor属性指向
// 这种方式属于是将一个新的对象赋值给了prototype不再是之前的也不知道是谁的this了
Star.prototype = {
    sing: function() {
        console.log('唱')
    },
    dance: function() {
        console.log('跳')
    },
    warp: function() {
        console.log('warp')
    },
    qiu: function() {
        console.log('篮球')
    }
}
// 实例化对象
const ldh = new Star()
ldh.sing()
ldh.dance()
ldh.warp()
ldh.qiu()
console.log(Star.prototype)
```



**结果**：

![image-20230809113434852](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091134190.png)

#### 3.2.1 重新指回创建原型对象的构造函数

**代码**：

```js
// 构造函数
function Star() { }
// 赋值的会覆盖原型中原有的constructor属性指向
Star.prototype = {
    // 重新指回创造这个原型对象的 构造函数
    constructor: Star,
    sing: function() {
        console.log('唱')
    },
    dance: function() {
        console.log('跳')
    },
    warp: function() {
        console.log('warp')
    },
    qiu: function() {
        console.log('篮球')
    }
}
// 实例化对象
const ldh = new Star()
ldh.sing()
ldh.dance()
ldh.warp()
ldh.qiu()
console.log(Star.prototype)
```

**结果**：

![image-20230809114108545](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091141707.png)

<blockquote alt=success>
	<div>
      <p>
         <span><font>总结</font>：</span>
      </p>
      <ol>
         <li>
         	<p>
               <span>constructor属性的作用是什么？</span>
            </p>
         </li>
      </ol>
      <ul>
         <li>
         	<p>
               <span><span alt=wavy>指向该原型对象的构造函数</span></span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>


#### 3.3 对象原型

>  思考：
>
>  ==构造函数==可以创建==实例对象==，构造函数还==有一个====原型对象==，==一些公共的属性==或者==方法放到==这个==原型对象身上==，但是 ==为啥实例对象可以访问原型对象里面的属性和方法呢==？

![image-20230809114739934](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091147172.png)

<blockquote alt=info>
	<div>
      <p>
        <span><font>对象原型</font>：</span> 
      </p>
      <p>
         <span><font title=red>对象都会有一个属性</font> <font>__proto__</font> 指向构造函数的 <font>prototype</font> 原型对象，之所以我们对象可以使用构造函数<font>prototype</font>原型对象的属性和方法，就是因为对象有<font>__proto__</font>原型的存在。对象里面的原型所以叫对象原型</span>
      </p>
   </div>
</blockquote>

![image-20230809115311930](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091153140.png)

**代码**：

```js
// 构造函数
function Star() {}
// 实例化对象
const ldh = new Star()
// 打印实例化对象
console.log(ldh)
```

**结果**：

![image-20230809131512463](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091315944.png)

打印中，其中[[Prototype]] 就是 `__proto__` 看如下解释：

<blockquote alt=danger>
	<div>
      <p>
         <span><font title=red>注意：</font></span>
      </p>
      <ul>
         <li>
         	<p>
               <span>__proto__ 是JS非标准属性，所以有些浏览器不是显示的 __proto__ </span>
            </p>
         </li>
         <li>
         	<p>
               <span>[[prototype]] 和 __proto__ 意义相同，但是我们写的时候必须写 __proto__</span>
            </p>
         </li>
         <li>
         	<p>
               <span><span alt=solid>用来表明当前实例对象 指向哪个原型对象 prototype</span></span>
            </p>
         </li>
         <li>
         	<p>
               <span>__proto__ 对象原型里面也有一个<font>constructor</font>属性，<font title=red>指向创建该实例对象的构造函数</font></span>
            </p>
         </li>
         <li>
         	<p>
               <span>__proto__ 是只读的，不能修改！！！</span>
            </p>
         </li>
         <li>
         	<p>
               <span>对象原型 (_proto_) 指向原型对象 prototype</span>
            </p>
         </li>
      </ul>
   </div>
</blockquote>

**代码**：

```js
// 构造函数
function Star() {}
// 实例化对象
const ldh = new Star()
// 对象原型__proto__指向 构造函数的原型对象
console.log(ldh.__proto__ === Star.prototype)// true
// 对象原型里面有constructor指向 构造函数 Star 又指回来了!
console.log(ldh.__proto__.constructor === Star)// true
```



爸爸指向哥哥，哥哥的constructor指向爸爸，弟弟指向哥哥，弟弟的constructor指向爸爸。爸爸指向弟弟

![image-20230809133342890](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091333285.png)

<blockquote alt=success>
	<div>
      <p>
         <span><font>总结：</font></span>
      </p>
      <ol>
         <li>
         	<p>
               <span>prototype 是什么？哪里来的？</span>
            </p>
            <ul>
               <li>
                  <p>
                     <span>原型(原型对象)</span>
                  </p>
               </li>
               <li>
                  <p>
                     <span>构造函数都自动有原型</span>
                  </p>
               </li>
            </ul>
         </li>
         <li>
         	<p>
               <span>constructor属性在哪里？作用干啥的？</span>
            </p>
            <ul>
               <li>
                  <p>
                     <span><font title=red>prototype原型对象</font>和<font title=red>对象原型__proto__</font>里面都有</span>
                  </p>
               </li>
               <li>
                  <p>
                     <span>都<font title=red>指向</font>创建实例对象 / 原型的<font title=red>构造函数</font></span>
                  </p>
               </li>
            </ul>
         </li>
         <li>
         	<p>
               <span>__proto__ 属性在哪里？ 指向谁？</span>
            </p>
            <ul>
            <li>
            	<p>
                  <span>在实例对象里面</span>
               </p>
            </li>
            <li>
            	<p>
                  <span>指向原型 prototype</span>
               </p>
            </li>
         </ul>
         </li>
      </ol>
   </div>
</blockquote>
#### 3.4 原型继承

==继承==是==面向对象编程的另一个特征==，通过继承==进一步提升代码封装的程度==，JavaScript中大多是==借助原型对象实现继承的特性==。

>  龙生龙，凤生凤，老鼠的儿子会打洞 描述的正是继承的含义。

**我们来看个代码**：

```js
function Man() {
   this.head = 1
   this.eyes = 2
   this.legs = 2
   this.say = function() {}
   this.eat = function() {}
}
const pink = new Man()
```



```js
function Woman() {
 this.head = 1
   this.eyes = 2
   this.legs = 2
   this.say = function() {}
   this.eat = function() {}
   this.baby = function() {}
}
const red = new Woman()
```



##### 1 <font title=red>封装</font>—抽取公共部分

把男人和女人公共的部分抽取出来放到人类里面

```js
//人类
const Person = {
    eyes: 1,
    head: 2
}
//男人
function Man() {}
//女人
function Woman() {
   this.baby = function() {}
}
```



##### 2 问题——原因 

将 构造函数.prototype.constructor重新指回自己

**代码**：

```js
// 对象 公共属性或方法
const Person = {
    eyes: 1,
    head: 2
}
// 女人构造函数
function Woman() {}
// 原型对象继承Person对象 公共属性或方法
Woman.prototype = Person
// 指向自己构造函数
Woman.prototype.constructor = Woman
// 向女人 中 追加一个baby的 特有函数
Woman.prototype.baby = function() {
    console.log('小宝宝')
}
console.log(new Woman())
// 男人构造函数
function Man() {}
// 原型对象继承Person对象 公共属性或方法
Man.prototype = Person
// 指向自己构造函数
Man.prototype.constructor = Man
console.log(new Man())
```



![image-20230809170720906](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091707485.png)

**结果**：

<span alt=solid>注意，这里面的两个不同实例对象的constructor指向同一个实例方法</span>.

![image-20230809161325679](./JS%E8%BF%9B%E9%98%B6.assets/image-20230809161325679.png)



男人和女人==同时使用了同一个对象==，根据==引用类型的特点==，==它们指向同一个对象==，==修改一个就会都影响==。

![image-20230809163744490](./JS%E8%BF%9B%E9%98%B6.assets/image-20230809163744490.png)

![image-20230809163754261](./JS%E8%BF%9B%E9%98%B6.assets/image-20230809163754261.png)

>  <font>解决方式</font>：
>
>  1.  既然使用对象抽取公共属性的话，很多个实例都要使用而一个修改prototype就是全部修改，因为对象是引用数据类型
>  2.  那么就换成<span alt=solid>构造函数</span>，构造函数 每个实例要使用都需要new一下构造函数这样每个实例的prototype都是不同的，这样修改就不会影响其它的了。

```js
// 对象 公共属性或方法 不要使用对象 因为对象都引用了同一个内容一个修改会影响其它的
// const Person = {
//     eyes: 1,
//     head: 2
// }
// 使用构造函数 抽取公共的属性或方法
// 构造函数 new 出来的对象 结构一样 但是对象不一样
function Person() {
    this.eyes = 1,
    this.head = 2
}
// 女人构造函数
function Woman() {}
// 原型对象继承Person对象 公共属性或方法
Woman.prototype = new Person()
// 指向自己构造函数
Woman.prototype.constructor = Woman
// 向女人 中 追加一个baby的 特有函数
Woman.prototype.baby = function() {
    console.log('小宝宝')
}
console.log(new Woman())
// 男人构造函数
function Man() {}
// 原型对象继承Person对象 公共属性或方法
Man.prototype = new Person()
// 指向自己构造函数
Man.prototype.constructor = Man
console.log(new Man())
```

**结果**：

![image-20230809165135788](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091705280.png)

#### 3.5 原型链

基于==原型对象的继承使得不同构造函数的原型对象关联在一起==，并且这种==关联的关系是一种链状的结构==，我们将原型对象的链状结构关系称为==原型链==.

```js
// 构造函数
function Person() {}
// 实例对象
const ldh = new Person()
// 判断实例对象的__proto__ 是否与 Person.prototype相同 实例对象__prto__是指向Person.prototype的
console.log(ldh.__proto__ === Person.prototype)// true
// __proto__：对象原型 指向 原型对象
// 原型对象 里面有一个 对象原型 指向的还是一个 原型对象 ,那么指向的是谁的 原型对象呢？
console.log(Person.prototype.__proto__)
// 判断原型对象的对象原型是否指向Object的原型对象
// 因为Person.prototype是自定义函数 往上走就是Object没有其它的了Object指向Object.prototype
console.log(Person.prototype.__proto__ === Object.prototype)// true
// Object的构造函数 指向 Object.prototype 而 Object.prototype.constructor 指向 它自己的构造函数
console.log(Object.prototype)
// Object的prototype.__proto__ 指向上一层但是上一层就没有了 所以为 null
console.log(Object.prototype.__proto__)// null
```

**结果**：

![image-20230809180028175](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091800621.png)

-  <span alt=wavy>只要是个对象就有`__proto__` </span>.

-  <span alt=wavy>只要是原型对象里面就有`constructor` </span>.

![image-20230809173938143](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091739595.png)

##### 3.5.1 原型链—查找规则

1.  当访问一个对象的属性 (包括方法) 时 ， 首先查找这个<font title=red>对象自身</font>有没有该属性。
2.  如果没有就查找它的原型 (也就是`__proto__`指向的<font title=red>prototype原型对象</font>)
3.  如果还没有就查找原型对象 的原型 (<font title=red>Object的原型对象</font>)
4.  依此类推一直找到Object为止 (null)
5.  `__proto__`对象原型的==意义==就在于==为对象成员查找机制提供一个方向，或者一条路线==.
6.  可以使用`instanceof`运算符用于检测构造函数的`prototype`属性==是否出现在某个实例对象的原型链上==.

**instanceof演示**：

```js
// 构造函数
function Person() { }
// 实例对象
const ldh = new Person()
console.log(ldh instanceof Person)// true
console.log(ldh instanceof Object)// true
console.log(ldh instanceof Array)// false ldh是一个对象它不属于数组
console.log([1, 2, 3] instanceof Array)// true []数组类型属于数组
console.log(Array instanceof Object)//万物皆对象
```



### 综合案例—模态框封装

目的：练习<font title=red>面向对象</font> 写插件 (模态框)

![image-20230809183332615](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091833091.png)

**分析需求**：

1.  多个模态框一样的，而且每次点击都会出来一个，怎么做呢？
    -  <font title=red>构造函数</font>。把模态框封装一个<font title=red>构造函数Modal</font>，每次new都会产出一个模态框，所以点击不同的按钮就是在做<font title=red>new模态框</font>，实例化。
2.  模态框有什么功能呢？打开功能 (显示)，关闭功能，而且每个模态框都包含着2个功能
    -  open功能 (打开)
    -  close功能 (关闭)

**问**：open和close方法，写到哪里？

<span alt=solid>构造函数Modal的<font title=red>原型对象上</font>，共享方法</span>.

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>面向对象封装消息提示</title>
  <style>
    .modal {
      width: 300px;
      min-height: 100px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
      border-radius: 4px;
      position: fixed;
      z-index: 999;
      left: 50%;
      top: 50%;
      transform: translate3d(-50%, -50%, 0);
      background-color: #fff;
    }

    .modal .header {
      line-height: 40px;
      padding: 0 10px;
      position: relative;
      font-size: 20px;
    }

    .modal .header i {
      font-style: normal;
      color: #999;
      position: absolute;
      right: 15px;
      top: -2px;
      cursor: pointer;
    }

    .modal .body {
      text-align: center;
      padding: 10px;
    }

    .modal .footer {
      display: flex;
      justify-content: flex-end;
      padding: 10px;
    }

    .modal .footer a {
      padding: 3px 8px;
      background: #ccc;
      text-decoration: none;
      color: #fff;
      border-radius: 2px;
      margin-right: 10px;
      font-size: 14px;
    }

    .modal .footer a.submit {
      background-color: #369;
    }
  </style>
</head>
<body>
  <button id="delete">删除</button>
  <button id="login">登录</button>

  <!-- <div class="modal">
    <div class="header">温馨提示 <i>x</i></div>
    <div class="body">您没有删除权限操作</div>
  </div> -->

  <script src="./js/模态框.js"></script>
</body>
</html>
```



```js
// 1.Modal 构造函数封装—模态框
function Modal(title = '加载中...', message = '...') {
    // 创建Modal 模态框盒子
    // 1.1 创建div标签
    this.modalBox = document.createElement('div')
    // 1.2 给div标签添加类名为 modal
    this.modalBox.className = 'modal'
    // 1.3 modal盒子内部填充2个div标签并且修改文字内容
    this.modalBox.innerHTML = `
    <div class="header">${title} <i>x</i></div>
    <div class="body">${message} </div>
    `
    console.log(this.modalBox)
}
// 2. 给构造函数原型对象 挂载 open方法
Modal.prototype.open = function () {
    // 先来判断页面中是否有modal盒子，如果有先删除，否则继续添加，防止重复打开同一个窗口
    const modal = document.querySelector('.modal')
    modal && modal.remove()
    // 注意这个方法不要用箭头函数 因为还需要用到this
    // 把刚才创建的modalBox显示到页面body中
    document.body.append(this.modalBox)
    // 要等着盒子显示出来，就可以绑定点击事件了 (关闭窗口事件)
    this.modalBox.querySelector('i').addEventListener('click',() => {
        // 这个地方需要用到箭头函数
        // 这个this指向 实例对象 箭头函数 沿用作用域链上一层找this 就是function函数 Modal了
        console.log(this)
        this.close()
    })
}
// 3. 给构造函数原型对象挂载close方法 
Modal.prototype.close = function() {
    // 将当前元素从DOM树中删除
    this.modalBox.remove()
}
// 点击 删除按钮
document.querySelector('#delete').addEventListener('click', () => {
    const del = new Modal('温馨提示', '您没有权限删除操作')
    del.open()
})
// 点击 登录按钮
document.querySelector('#login').addEventListener('click', () => {
    const log = new Modal('友情提示','您没有注册呢 ?')
    log.open()
})
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308091936673.gif)

## 深浅拷贝

-  浅拷贝

-  深拷贝

### 1 浅拷贝

首先浅拷贝和深拷贝只针对引用类型

**浅拷贝**：拷贝的是地址

**常见方法**：

1.  **拷贝对象**：Object.assgin() / 展开运算符`{…Obj}`  拷贝对象
2.  拷贝数组：Array.prototype.concat() 或者`[…arr]`  

```js
const obj = {
    uname: 'pink',
    age: 18,
    family: {
        baby: '小Pink',
        age: 3
    }
}
// 浅拷贝
const o = {...obj}
// 基本数据类型拷贝的是值，引用类型拷贝的是地址值 一个修改影响全部
console.log(o)// {uname: 'pink', age: 18}  family: {baby: '老pink', age: 3}
o.age = 20
o.family.baby = '老pink'
console.log(o)// {uname: 'pink', age: 20} family: {baby: '老pink', age: 3}
console.log(obj)// {uname: 'pink', age: 18} family: {baby: '老pink', age: 3}
```

**结果**：

![image-20230809203736792](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308092037020.png)

>  如果是简单数据类型拷贝==值==，引用数据类型拷贝的是==地址==(简单理解：如果是==单层对象，没问题==，如果有==多层就有问题==)

>  <font>总结</font>：
>
>  1.  直接赋值和浅拷贝有什么区别？
>      -  直接赋值的方法，只要是对象，都会互相影响，因为是直接拷贝对象栈里面的地址
>      -  浅拷贝如果是一层对象，不互相影响，如果出现多层对象拷贝还是会互相影响
>  2.  浅拷贝怎么理解？
>      -  拷贝对象之后，里面的属性值是简单数据类型直接拷贝值
>      -  如果属性值是引用数据类型则拷贝的是地址值

### 2 深拷贝

首先浅拷贝和深拷贝只针对引用类型

**深拷贝**：拷贝的是==对象==，==不是地址==.

>  <font>常见方法</font>：
>
>  1.  通过递归实现深拷贝
>  2.  lodash / cloneDeep
>  3.  通过JSON.stringify() 实现

**常见方法**：

#### 1 通过递归实现深拷贝

##### 递归介绍

>  <font>函数递归</font>：
>
>  <span alt=solid>如果一个函数在内部可以调用其本身，那么这个函数就是递归函数</span>.
>
>  -  简单理解：函数内部自己调自己，这个函数就是递归函数
>  -  递归函数的作用和循环效果类似
>  -  由于递归很容易发生 “栈溢出” 错误 (stack ooverflow)，所以 <font title=red>必须要加退出条件 return</font>。

![image-20230809204658767](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308092047083.png)

```js
let i = 0
function fn() {
    i++
    console.log(`调用第${i}次`)
    if (i === 10)
        return
    fn()
}
fn()
```

**效果**：

![image-20230809205327608](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308092053955.png)

**函数递归** 

利用递归函数实现 setTimeout 模拟 setInterval 效果

```html
<body>
    <div></div>
   <script src="./js/模拟定时器.js"></script> 
</body>
```



```js
function fn() {
    document.querySelector('div').innerHTML = new Date().toLocaleString()
    setTimeout(fn,1000)
}
fn()
```



##### 1.1 通过递归实现深拷贝 (精简版)

```js
const o = {}
function deepCopy(newObj , oldObj) {
   for(let k in oldObj) {
      if(oldObj[k] instanceof Array) {
         newObj[k] = []
         deepCopy(newObj[k] , oldObj[k])
      }else if(newObj[k] , oldObj[k]) {
         newObj[k] = {}
         deepCopy(newObj[k] , oldObj[k]
      }else {
     		newObj[k] = oldObj[k]             
     }
   }
}
```



```js
const o = {}
function deepCopy(newObj , oldObj) {
   for(let k in oldObj) {
      newObj[k] = oldObj[k]
   }
}
deepCopy(o,obj)
```



```js
const o = {}
function deepCopy(newObj , oldObj) {
   for(let k in oldObj) {
      if(oldObj[k] instanceof Array) {
         newObj[k] = []
         deepCopy(newObj[k] , oldObj[k])
      }else if(oldObj[k] instanceof Object) {
         newObj[k] = {}
         deepCopy(newObj[k] , oldObj[k])
      }else {
         newObj[k] = oldObj[k]
      }
   }
}
```



```js
const o = {}
function deepCopy(newObj , oldObj) {
   for(let k in oldObj) {
      if(oldObj[k] instanceof Array) {
         newObj[k] = []
         deepCopy(newObj[k] , oldObj[k])
      }else if(oldObj[k] instanceof Object) {
         newObj[k] = {}
         deepCopy(newObj[k] , oldObj[k])
      }else {
         newObj[k] = oldObj[k]
      }
   }
}
```



**综合代码**：

```js
const obj = {
    uname: 'pink',
    age: 18,
    famiyl: {
        uname: '老pink',
        age: 29
    },
    hobby: ['唱', '跳', 'wrap']
}
const o = {}
// 拷贝函数
function deepCopy(newObj, oldObj) {
    for (let k in oldObj) {
        // 处理数组的拷贝问题
        // 判断属性值是否为数组类型的
        // 万物皆对象，先筛选数组 再 对象 
        if (oldObj[k] instanceof Array) {
            // 新数组中传入属性名 默认赋值类型为数组
            newObj[k] = []
            // 递归调用自己传入 属性名 ， 属性值 传入函数中
            deepCopy(newObj[k], oldObj[k])
        } else if (oldObj[k] instanceof Object) { // 千万不要将对象的判断写在前面因为数组也是对象它会将数组当对象处理
            newObj[k] = {}
            deepCopy(newObj[k], oldObj[k])
        } else {
            //k : uname , age | oldOb[k] 'pink' , 18
            // newObj[k] 等价于 o.uname , o.age
            newObj[k] = oldObj[k]
        }
    }
}
// 调用函数 传入实参 两个对象  新对象  旧对象
// 功能： 将旧对象的属性名属性值 拷贝到 新对象中
deepCopy(o, obj)
// 基本数据类型修改后不会影响到另一个对象
o.age = 100
o.famiyl.uname = '老老老pink'
o.hobby[0] = '练习两年半'
console.log(o)
console.log(obj)
```

**结果**：

![image-20230810095909996](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101029120.png)

![image-20230810102915988](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101029744.png)

#### 2 js库lodash里面cloneDeep内部实现了深拷贝 (常用):star: 

**使用步骤**：

1.  引入lodash.min.js文件

    ```js
    <script src="./js/lodash.min.js"></script> 
    ```

2.  开始调用lodash里面的函数进行使用

```js
const obj = {
    uname: 'pink',
    age: 18,
    family: {
        uname: '老pink',
        age: 20
    },
    arr: ['唱','跳','wrap','篮球']
}
// 调用函数进行深拷贝
const o = _.cloneDeep(obj)
o.uname = '小pink'
o.family.uname = '老老老老pink'
o.arr[0] = '鸡哥'
console.log(o)
console.log(obj)
```

**结果**：

![image-20230810111311696](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101113200.png)

#### 3 使用JSON的方式实现深拷贝

```js
const obj = {
    uname: 'pink',
    age: 18,
    hobby: ['唱歌','跳舞','wrap'],
    family: {
        uname: '老pink'
    }
}
// 把对象转换为JSON字符串
const json = JSON.stringify(obj)
console.log(json)
// 再将转换为字符串的JSON数据解析为 对象 此时它们的 地址 和 引用的地址都不同了
const o = JSON.parse(json)
o.uname = '小pink'
o.hobby[0] = 'ikun'
o.family.uname = '老老老pink'
console.log(o)
console.log(obj)
```

**图解**：

![image-20230810111855025](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101118380.png)

**效果**：

![image-20230810111918889](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101119177.png)

>  <span alt=solid>总结</span>：
>
>  1.  实现深拷贝三种方式？
>      -  自己利用递归函数书写深拷贝
>      -  利用js库lodash里面的`_.cloneDeep()` 
>      -  利用JSON字符串转换

## 异常处理

-  throw 抛异常
-  try / catch 捕获异常
-  debugger

了解JavaScript中程序异常处理的方法，提升代码运行的健壮性。

### 1 throw 抛异常

异常处理是指预估代码执行过程中==可能发生的错误==，然后最大程度的==避免错误的发生导致整个程序无法继续运行==。

```js
function counter(x, y) {
    if (!x || !y)
        //throw '参数不能为空'
        throw new Error('参数不能为空')
    return x + y
}
counter()
```



**结果**：

![image-20230810112858201](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101129281.png)

>  <span alt=solid>总结</span>：
>
>  1.  throw 抛出异常信息，==程序也会终止执行==.
>  2.  throw 后面跟的是==错误提示信息==.
>  3.  Error 对象，配合 throw 使用，能够设置==更详细的错误信息==.
>
>  ---
>
>  1.  抛出异常我们用哪个关键字？它会终止程序吗？
>      -  throw 关键字
>      -  <font title=red>会终止程序</font>.
>  2.  抛出异常经常和谁配合使用？
>      -  Error 对象，配合throw 使用

---

### 2 try / catch 捕获错误信息

我们可以通过`try/catch`==捕获错误信息== (==浏览器提供==的错误信息) <span alt=dotted>try 试试</span>   <span alt=dotted>catch 拦住</span>    <span alt=dotted>finally 最后</span>.

```js
<body>
    <p>123</p>
    <script src="../js/try/catch捕获异常.js"></script>
</body>
```



```js
function fn() {
    try {
        // 可能发生错误的代码 放到try中 试试
        const p = document.querySelector('.p')
        p.style.color = 'red'
        // 捕获可能发生的异常
        // catch参数列表中 随意写一个变量名称 它是由浏览器提供的错误信息
    } catch (err) {
        // 调用错误信息的属性 message
        console.log(err.message)
        // 抛出异常后会终止后面的代码
        throw new Error('选择器 获取不到指定的标签')
        // 上面抛出了异常程序终止下面的打印不会执行
        console.log('hello')
        //finally 无论是否发生异常 都会执行代码块
    } finally {
        console.log('world !')
    }
}
fn()
```



**结果**：

![image-20230810115318837](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101153181.png)

>  <span alt="solid">总结</span>:
>
>  1.  `try...catch`用于捕获错误信息
>  2.  将预估可能发生错误的代码写在`try`代码块中
>  3.  如果`try`代码块中出现错误后，会执行`catch`代码块，并拦截到错误信息
>  4.  `finally `不管是否有错误，都会执行
>
>  ---
>
>  1.  捕获异常我们用哪3个关键字？可能会出现的错误代码写到谁里面
>      -  `try catch finally`
>      -  `try`
>  2.  怎么调用错误信息？
>      -  利用`catch`的参数

### 3 debugger

```js
function fn() {
    try {
        // 可能发生错误的代码 放到try中 试试
        const p = document.querySelector('.p')
        p.style.color = 'red'
        // 捕获可能发生的异常
        // catch参数列表中 随意写一个变量名称 它是由浏览器提供的错误信息
    } catch (err) {
        console.log(err.message)
        // 抛出异常后会终止后面的代码
        // 相当于打了个断点
        debugger
        // 调用错误信息的属性 message
        throw new Error('选择器 获取不到指定的标签')
        // 上面抛出了异常程序终止下面的打印不会执行
        console.log('hello')
        //finally 无论是否发生异常 都会执行代码块
    } finally {
        console.log('world !')
    }
}
fn()
```



**效果**：

![image-20230810131346686](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101313189.png)

## 处理this

-  this 指向
-  改变 this

this 是 JavaScript 最具 “魅惑” 的知识点，不同的应用场合 this 的取值可能会与意想不到的结果，在此我们对以往学习过的关于 [this 默认的取值] 情况进行归纳和总结。

目标：了解函数中 this 在不同场景下的默认值，知道动态指定函数 this 值的方法

**学习路径**：

1.  普通函数 this 指向
2.  箭头函数 this 指向 

### 1 this 指向—普通函数

目标：能说出普通函数的 this 指向

普通函数的调用方式决定了 this 的值，即 [谁调用 this 的值指向谁]

#### 普通函数

```js
// 普通函数
function sayHi() {
   console.log(this)// window 
}
// 函数表达式
const sayHello = function() {
   console.log(this)// window 
}
// 函数的调用方式决定了 this 的值
sayHi()// window
window.sayHi()// window

// 普通对象
const user = {
   uname: '小明',
   walk: function() {
      console.log(this)// user
   }
}
// 动态为 user 添加方法
user.sayHi = sayHi // user
user.sayHello = sayHello // user
// 函数调用方式，决定了 this 的值
user.sayHi()// user
user.sayHello()// user
```



```js
<script>
	'use strict' // 开启严格模式 下面的函数中获取this就是 undefined 了
	function fn() {
      console.log(this) // undefined
   }
	fn()
</script>
```

<span alt=solid>普通函数没有明确调用者时 this 值为 window，严格模式下没有调用者时 this 的值为 undefined</span>.

>  总结：
>
>  1.  普通函数 this 指向我们怎么记忆？
>      -  [谁调用 this 的值指向谁]
>  2.  普通函数严格模式下指向谁？
>      -  严格模式下指向 `undefined` 

### 1.1 this 指向—箭头函数

目标：能说出箭头函数的 this 指向

>  箭头函数中的 this 与普通函数完全不同，也不受调用方式的影响，事实上<span alt=solid>箭头函数中并不存在 this!</span>
>
>  1.  箭头函数会默认帮我们绑定外层 this 的值，所以在箭头函数中 this 的值和外层的 this 是一样的
>  2.  箭头函数中的 this 引用的就是最近作用域中的 this
>  3.  向外层作用域中，一层一层查找 this ， 直到有 this 的定义

```js
const sayHi = () => {
    console.log(this)// window
} 
sayHi()// window.sayHi()
const user = {
    uname: 'pink',
    //到了user的作用域中 而user是 window调用的
    walk: () => {
        // 从上一层作用域开始找
        console.log(this)// window
    }
}
user.walk()
```



#### 注意情况1：

在开发中 [使用箭头函数前需要考虑函数中this的值] ，事件回调函数使用箭头函数时，this 为全局的window因此DOM事件回调函数==如果里面需要==DOM对象的this，<span alt=solid>则不推荐使用箭头函数</span>.

```js
// 获取元素对象
const button = document.querySelector('button')
// 箭头函数此时 this 指向了 window
button.addEventListener('click',() => console.log(this))// window
// 普通函数 此时 this 指向了 DOM对象 button
button.addEventListener('click',function() {console.log(this)}) // button
```



#### 注意情况2：

同样由于箭头函数 this 的原因，基于原型的面向对象也==不推荐采用箭头函数==.

```js
function Person() {}
// 原型对象上 添加了 箭头函数
Person.prototype.walk = () => {
    console.log(this)// window
}
const man = new Person()
man.walk()
```



>  <span alt=solid>总结</span>：
>
>  1.  函数内不存在 this ，沿用上一级的
>      -  过程：向外层作用域中，一层一层查找 this ，直到有 this 的定义为止
>  2.  <font>不适用</font>.
>      -  ==构造函数==，==原型函数==，==dom事件函数==等等
>  3.  <font>适用</font>.
>      -  需要使用==上层 this== 的地方
>  4.  <span alt=wavy>使用正确的话，它会在很多地方带来方便，后面我们会大量使用慢慢体会</span>.

### 2 改变this

JavaScript 中还允许指定函数中 this 的指向，有3个方法可以动态指定普通函数中 this 的指向

1.  `call()` 
2.  `apply()` 
3.  `bind()` 

#### 1 call()—了解

使用call方法调用函数，同时指定被调用函数中 this 的值

###### 语法：

`fun.call(thisArg,arg1,arg2,...)` 

-  `thisArg`：在fun函数运行时指定的 this 值
-  `arg2,arg2`：传递的其它参数

<span alt=solid>返回值就是函数的返回值，因为它就是调用函数</span>.

```js
const obj = {
    uname: 'ikun'
}
function fn(x, y) {
    // 逻辑中断 默认赋值
    x = x | 0
    y = y | 0
    console.log(this)// obj
    console.log(x + y)
}
// 1.调用函数
// 1.2 可以传递实参
// 2.改变this指向
fn.call(obj, 1, 2)
```



**结果**：

![image-20230810141447040](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101414250.png)

#### 2 apply()—理解

使用 apply 方法调用函数，同时指定被调用函数中 this 的值

###### 语法：

`fun.apply(thisArg,[argsArray])` 

-  `thisArg`：在fun函数运行时指定的 this 值
-  `argsArray`：传递的值，必须包含在 ==数组== 里面

<span alt=solid>返回值就是函数的返回值，因为它就是调用函数</span>.

因此apply主要==跟数组有关系==，比如使用 `Math.max()` 求数组的最大值

```js
const obj = {
    age: 18
}
function fn(x, y) {
    console.log(this)// obj
    console.log(x + y)// 3
}
// 1.调用函数
// 2.改变this指向
// 3.返回值,本身就是在调用函数，所以返回值就是函数返回值
fn.apply(obj, [1, 2])
```



**结果**：

![image-20230810142707106](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101427538.png)

>  思考：
>
>  1.  call和apply的区别是？
>      -  都是调用函数，都能改变 this 指向
>      -  参数不一样，apply传递的必须是数组，但是形参接收写法和变量一样

#### 3 bind()—重点

bind() 方法不会调用函数。但是能改变函数内部 this 指向

###### 语法：

`fun.bind(thisArg,arg1,arg2,...)` 

-  `thisArg`：在fun函数运行时指定的this值
-  `arg1,arg2`：传递的其它参
-  返回由指定的this值和初始化参数改造的 ==原函数拷贝 (新函数)==.
-  因此当我们只是想改变 this 指向，并且不想调用这个函数的时候，可以使用 bind ，比如改变定时器内部的 this 指向。

```js
const obj = {
    age: 18
}
function fn() {
    console.log(this)// window
}
// 1.bind不会调用函数
// 2.能改变this指向
// 3.返回值是个函数，但是这个函数里面的this是更改过的
const fun = fn.bind(obj)
fun()// obj
```



##### 定时器—点击按钮后2秒恢复

```js
// 需求，有一个按钮 点击里面就禁用，2秒之后就开启
const button = document.querySelector('button')
button.addEventListener('click', function () {
    // 禁用元素
    this.disabled = true
    let that = this
    // 定时器里面也可以改为使用箭头函数 因为箭头函数this沿用上一层就是button对象的this
    setTimeout(function () {
        //window.button.disabled = false 无效
        // 在这个函数里面，我们要由原来的window改为button
        this.disabled = false
        // 打印查看 定时器内部函数的this 跟 单击事件里面的 this是否相同 单击事件的this 是buttn按钮对象
        console.log(this === that)
        // bind(this) 里面的this就是单击事件函数中的this也就是 button对象
    }.bind(this), 2000)
})
```

>  <span alt=solid>call,apply,bind总结</span>：
>
>  -  相同点：
>     -  都可以改变函数内部 this 的指向。
>  -  区别点：
>     -  call和apply 会调用函数，并且改变函数内部 this 的指向
>     -  call和apply 传递的参数不一样 call 传递参数`arg1,arg2,...`形式 apply必须数组形式传递`[arg1,arg2,...]` 
>     -  <span alt=solid>bind 不会调用函数，可以改变函数内部 this 的指向</span>.
>  -  主要应用场景：
>     -  call 调用函数并且可以传递参数
>     -  apply 经常跟数组有关系，比如借助于数学对象实现数组最大值最小值
>     -  <span alt=solid>bind 不调用函数，但是还会改变 this 指向，比如改变定时器内部的 this 指向</span>。

## 防抖 (debounce)

**防抖**：单位时间内，频繁触发事件，<font title=red>只执行最后一次</font>.

![image-20230810153242050](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101532326.png)

**举个栗子**：<span alt=solid>王者荣耀回城</span>，只要被打断就需要重新来

廉颇残血了 想要 回家补补血，这时鲁班突然冒出来打了 廉颇一下，廉颇的回城施法被打断了，生气的廉颇就需要重新点一下回城，重新等待3秒的 施法时间。

![image-20230810153426277](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101534660.png)

**使用场景**：

-  ==搜索框搜索输入==。只需用户==最后==一次输入完，再发送请求
-  手机号，邮箱验证 ==输入检测==。

### 案例—利用防抖来处理—鼠标滑过盒子显示文字+1

**要求**：鼠标再盒子上移动，里面的数字就会变化+1

#### 1 使用 loadsh库来实现 防抖

1.引入lodash库的js文件

```js
  <style>
    .box {
      width: 500px;
      height: 500px;
      background-color: #ccc;
      color: #fff;
      text-align: center;
      font-size: 100px;
    }
  </style>
  <script src="./js/lodash.min.js"></script>
```



2.编写js代码：

```js
<body>
  <div class="box"></div>
  <script>
    // 利用防抖实现性能优化
    // 需求：鼠标在盒子上移动，里面的数字就会 + 1
    const box = document.querySelector('.box')
    let i = 1
    // 鼠标移动函数
    function mouseMove() {
      box.innerHTML = i++
      // 如果里面存在大量消耗性能的代码，比如dom操作，比如数据处理，可能造成卡顿

    }
    // 添加鼠标移动监听事件
    // box.addEventListener('mousemove', mouseMove)
    // 利用lodash库实现防抖—500毫秒之后才 + 1
    // 语法：_.debounce(fun,时间)
    box.addEventListener('mousemove', _.debounce(mouseMove, 500))
  </script>
</body>
```



**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101602532.gif)

### 2 手写防抖函数

**核心思路**：

防抖的核心就是利用定时器 (setTimeout) 来实现

1.  声明一个定时器 ==变量==。
2.  当鼠标每次滑动都先判断 ==是否有定时器==了，如果有定时器先 ==清除以前==的定时器
3.  如果没有定时器则 ==开启== 定时器 ，记得==存到变量==里面
4.  在==定时器里面调用==要执行的函数

```js
// 利用防抖实现性能优化
// 需求：鼠标在盒子上移动，里面的数字就会 + 1
const box = document.querySelector('.box')
let i = 1
// 鼠标移动函数
function mouseMove() {
   box.innerHTML = i++
   // 如果里面存在大量消耗性能的代码，比如dom操作，比如数据处理，可能造成卡顿

}
// 手写防抖函数
// 1.  声明一个定时器 变量。
// 2.  当鼠标每次滑动都先判断 是否有定时器了，如果有定时器先 清除以前的定时器
// 3.  如果没有定时器则 开启 定时器 ，记得存到变量里面
// 4.  在定时器里面调用要执行的函数
function debounce(fn, t) {
   let timer
   // return 返回一个匿名函数
   return function () {
      // 判断是否有定时器了 有则函数 
      if (timer) clearTimeout(timer)
      timer = setTimeout(function () {
         fn()// 调用传入的函数
      }, t)
   }
}
// debounce(mouseMOve, 500)相当于 function() {定时器} 当鼠标再次移动就调用了返回的函数
box.addEventListener('mousemove', debounce(mouseMove, 500))
```



## 节流—throttle

**节流**：单位时间内，频繁触发事件，==只执行一次==.

![image-20230810161935226](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101619481.png)

**举个栗子**：

-  ==王者荣耀技能冷却==，期间无法继续释放技能
-  ==和平精英98k== 换子弹期间不能射击

-  **使用场景**：
   -  **高频事件**：鼠标移动==`mousemove`==，页面尺寸缩放 ==resize==，滚动条滚动 ==scroll等等==.

**要求**：鼠标在盒子上移动， 不管移动多少次，每隔 500ms 才 + 1

**实现方式**：

1.  ==lodash== 提供的==节流函数==来处理
2.  ==手写==一个==节流函数==来处理

### 1 使用lodash方式实现节流

```js
  <style>
    .box {
      width: 500px;
      height: 500px;
      background-color: #ccc;
      color: #fff;
      text-align: center;
      font-size: 100px;
    }
  </style>
  <script src="./js/lodash.min.js"></script>
</head>

<body>
  <div class="box"></div>
  <script>
    // 利用防抖实现性能优化
    // 需求：鼠标在盒子上移动，里面的数字就会 + 1
    const box = document.querySelector('.box')
    let i = 1
    // 鼠标移动函数
    function mouseMove() {
      box.innerHTML = i++
      // 如果里面存在大量消耗性能的代码，比如dom操作，比如数据处理，可能造成卡顿
    }
    // 利用lodash库实现节流，500毫秒之后才+ 1
    // 语法: _.throttle(fun,时间)
    box.addEventListener('mousemove', _.throttle(mouseMove, 500))
  </script>
</body>
```



**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101631716.gif)

### 手写节流函数

核心也是使用定时器实现 setTimeout 实现

**核心思路**：

1.  声明一个定时器 ==变量==.
2.  当鼠标每次滑动都先判断 ==是否有定时器了==，如果有定时器则==不开启==新定时器
3.  如果没有定时器则==开启==定时器，记得==存到变量==里面
    1.  定时器里面==调用==执行的函数
    2.  定时器里面要把定时器==清空==.

```js
<body>
  <div class="box"></div>
  <script>
    // 利用防抖实现性能优化
    // 需求：鼠标在盒子上移动，里面的数字就会 + 1
    const box = document.querySelector('.box')
    let i = 1
    // 鼠标移动函数
    function mouseMove() {
      box.innerHTML = i++
      // 如果里面存在大量消耗性能的代码，比如dom操作，比如数据处理，可能造成卡顿
    }
    // 利用lodash库实现节流，500毫秒之后才+ 1
    // 语法: _.throttle(fun,时间)
    function throttle() {
      let timer = null
      if (!timer)
        timer = setTimeout(function () {
          fn()
          //清空定时器 
          //思考这里为什么不使用 clearTimeout来清除定时器 而是赋值为 null呢?
          timer = null
        }, t)
    }
    box.addEventListener('mousemove', throttle(mouseMove, 500))
  </script>
</body>
```



### 定时器的冷知识

```js
let timer = null
timer = setTimeout(function() {
    clearTimeout(timer)
    console.log(timer)// 1
},1000)
// 在setTimeout中是无法删除定时器的，
// 因为定时器还在运作，所以使用 timer = null 而不是clearTimeout(timer)
```

定时器还在运作的期间是无法删除的，一般不要在定时器的内部使用clearTimeout来删除当前的定时器可以删除其它的

## 防抖和节流总结

| 性能优化 | 说明                                     | 使用场景                                         |
| -------- | ---------------------------------------- | ------------------------------------------------ |
| 防抖     | 单位时间内，频繁触发事件，只执行最后一次 | 搜索框搜索输入，手机号，邮箱验证输入检测         |
| 节流     | 单位时间内，频繁触发事件，只执行一次     | 高频事件：鼠标移动，页面尺寸缩放，滚动条滚动等等 |

## 综合案例—记录上次视频播放位置

**分析**：

**两个事件**：

1.  ontimeupdate 事件在 视频/音频(audio/video)当前的播放位置发送改变时触发
2.  onloadeddata 事件在当前帧的数据加载完成且还没有足够的数据播放 视频/音频 (audio / video) 的下一帧时触发

##### 演示ontimeupdate事件

```html
<video src="https://v.itheima.net/LapADhV6.mp4" controls></video>
```



```js
  <script>
    // 获取元素 要对视频进行操作
    const video = document.querySelector('video')
    // 当视频/音频 当前播放的位置发送改变时触发事件
    video.ontimeupdate = function() {
      console.log('123')
    }
  </script>
```



**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308101700465.gif)

**谁需要节流**？

ontimeupdate，触发频次太高了，我们可以设定1秒钟触发一次

**思路**：

1.  在ontimeupdate事件触发的时候，每隔1秒钟，就记录当前时间到本地存储
2.  下次打开页面，onloadeddata 事件触发，就可以从本地存储取出时间，让视频从取出的时间播放，如果没有就默认为 0
3.  获得当前时间：`video.currentTime` 

```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="referrer" content="never" />
  <title>综合案例</title>
  <style>
    * {
      padding: 0;
      margin: 0;
      box-sizing: border-box;
    }

    .container {
      width: 1200px;
      margin: 0 auto;
    }

    .video video {
      width: 100%;
      padding: 20px 0;
    }

    .elevator {
      position: fixed;
      top: 280px;
      right: 20px;
      z-index: 999;
      background: #fff;
      border: 1px solid #e4e4e4;
      width: 60px;
    }

    .elevator a {
      display: block;
      padding: 10px;
      text-decoration: none;
      text-align: center;
      color: #999;
    }

    .elevator a.active {
      color: #1286ff;
    }

    .outline {
      padding-bottom: 300px;
    }
  </style>
</head>

<body>
  <div class="container">
    <div class="header">
      <a href="http://pip.itcast.cn">
        <img src="https://pip.itcast.cn/img/logo_v3.29b9ba72.png" alt="" />
      </a>
    </div>
    <div class="video">
      <video src="https://v.itheima.net/LapADhV6.mp4" controls></video>
    </div>
    <div class="elevator">
      <a href="javascript:;" data-ref="video">视频介绍</a>
      <a href="javascript:;" data-ref="intro">课程简介</a>
      <a href="javascript:;" data-ref="outline">评论列表</a>
    </div>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
  <script>
    // 获取元素 要对视频进行操作
    const video = document.querySelector('video')
    // 当视频/音频 当前播放的位置发送改变时触发事件
    video.ontimeupdate = _.throttle(() => {
      console.log(video.currentTime) //获得当前视频时间
      // 本地存储视频时间
      localStorage.setItem('currentTime', video.currentTime)
    }, 1000)
    // 打开页面触发事件，就从本地存储里面取出记录的时间，赋值给 video.currentTime
    // 注意打开页面触发事件 ，而不是刷新页面触发事件 刷新页面不触发事件
    video.onloadeddata = () => {
      // 防止用户第一次打开本地没有currentTime获取就是undefined 使用 || 逻辑中断
      // 将本地存储播放的视频 赋值给视频
      video.currentTime = localStorage.getItem('currentTime') || 0
    }
  </script>
</body>
</html>
```