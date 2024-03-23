---
title: vue3-什么是Pinia
categories: 
    - [计算机学科,web,vue,vue3]
tags:
    - vue3
    - 计算机学科
---

## 什么是Pinia

Pinia是Vue的最新 <font title='red'>状态管理工具</font>，是Vuex的<font title='red'>替代品</font>。

>  <span alt='solid'>优势</span>：
>
>  1.  提供更加简单的API (去掉了mutation)
>  2.  提供符合，组合式风格的API (和Vue3新语法统一)
>  3.  去掉了 modules的概念，每一个 store 都是一个独立的模块
>  4.  配合 TypeScript 更加友好，提供可靠的类型推断

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308302044414.png" alt="image-20230830204405854" style="zoom:80%;" />

## 手动添加Pinia到Vue项目

在实际开发项目的时候，关于Pinia的配置，可以在项目创建时自定添加现在我们初次学习，从零开始：

1.  使用Vite创建一个空的Vue3项目

    `npm create vue@latest`  | 或者使用`pnpm create vue` 

2.  按照<font title='red'>官方文档</font> 安装Pinia到项目中

    `npm install pinia --save-dev` | `pnpm add pinia -D` 

    安装完成后再package.json中查看

    ![image-20230918075003833](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309180750304.png)

3.  创建一个pinia(根存储) 并将其传递给应用程序，在main.js中进行配置注册Pinia

    ```js
    import './assets/main.css'
    
    import { createApp } from 'vue'
    import App from './App.vue'
    
    import {createPinia} from 'pinia'
    const app = createApp(App)
    app.use(createPinia)
    app.mount('#app')
    ```

4.  定义一个Store

    在src目录中创建store/index.js 配置如下内容

    >  在深入了解核心概念之前，我们需要知道Store是使用`defineStore()`定义的，并且它需要一个唯一名称，作为第一个参数传递：

    ```js
    import { defineStore } from 'pinia'
    
    // useStore 可以是 useUser、useCart 之类的任何东西
    // 第一个参数是应用程序中 store 的唯一 id
    export const useStore = defineStore('main', {
      // other options...
    })
    ```

    >  这个name，也称为id，是必要的，Pinia使用它来将store连接到dvetools。将返回的函数命名为`useXxx` 是跨可组合项的约定，以使其符合你的使用习惯

**代码演示**：

main.js

```js
import './assets/main.css'

import { createPinia } from 'pinia'
const pinia = createPinia()
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)
app.use(pinia)
app.mount('#app')
```

store/index.js

```js
import { ref, computed } from 'vue'
import { defineStore } from 'pinia'

// useStore可以是useUser,useCart 之类的任何东西
// 第一个参数是应用程序中 store 的唯一 id
// 定义store
// defineStore(仓库的唯一标识, () => { ... })
export const useStore = defineStore('main', () => {
  // other options ...
  // 声明数据state - count
  const count = ref(100)
  // 声明操作数据的方法action(普通函数)组合式写法没有action选项，普通函数就是action
  const addCount = () => count.value++
  const subCount = () => count.value--
  // 声明基于数据派生的计算属性getters(computed)
  // 利用computed函数去实现的，你需要getters你就提供一个computed
  const double = computed(() => count.value * 2)
  // 声明第二个数据state - msg 如果这个模块除了count还有其它数据
  const msg = ref('hello,world')
  // 将数据暴漏出去
  return {
    count,
    msg,
    addCount,
    subCount,
    double
  }
})
```

<span alt='solid'>目录结构</span>：

```
|-src
|--components
|---icons
|---son1.vue
|---son2.vue
|--store
|---index.js
|--App.vue
|--main.js
```

父组件

```html
<script setup>
  import Son1Com from '@/components/Son1Com.vue'
  import Son2Com from '@/components/Son2Com.vue'
  import { useCounterStore } from '@/store/counter.js'
  // 不要对counterStore做对象解构会丢失数据的响应式
  const counterStore = useCounterStore()
  console.log(counterStore)
</script>
<template>
  <div>
    <h3>
      <!-- 访问store数据的话直接访问 -->
      App.vue根组件 - {{ counterStore.count }}
      - {{ counterStore.msg }}
    </h3>
    <Son1Com></Son1Com>
    <Son2Com></Son2Com>
  </div>
</template>
<style scoped>

</style>
```

子组件1

```html
<script setup>
  import { useCounterStore } from '@/store/counter.js'
  // 不要对counterStore做对象解构会丢失数据的响应式
  const counterStore = useCounterStore()
</script>
<template>
    <div>我是son1 - {{ counterStore.count }} - {{ counterStore.double }}<button @click="counterStore.addCount">+</button></div>
</template>
<style scoped>

</style>
```

子组件2

```html
<script setup>
  import { useCounterStore } from '@/store/counter.js'
  // 不要对counterStore做对象解构会丢失数据的响应式
  const counterStore = useCounterStore()
</script>
<template>
    <div>我是son2 - {{ counterStore.count }}<button @click="counterStore.subCount">-</button></div>
</template>
<style scoped>

</style>
```

效果：

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308311009499.gif" alt="test" style="zoom:50%;" />

## action异步实现

**编写方式**：异步action函数的写法和<font title='red'>组件中获取异步数据的写法完全一致</font>。

**接口地址**：http://geek.itheima.net/v1_0/channels

**需求**：在Pinia中获取频道列表数据并把数据渲染App组件的模版中

![image-20230831101326740](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308311013165.png)

**代码演示**：

store/index.js

```js
import { defineStore } from 'pinia'
import { ref } from 'vue'
import axios from 'axios'
// 规范名：use 仓库名 Store 的组合来命名仓库名
export const useChannelStore = defineStore('channel', () => {
    // 声明数据，声明一个空数组
    const channelList = ref([])
    // 声明操作数据的方法 异步axios
    const getList = async () => {
        // 对象解构
        const {data: {data}} = await axios.get('http://geek.itheima.net/v1_0/channels')
        // 将请求获取到的数据赋值给声明 的空数组
        channelList.value = data.channels
        console.log(data.channels)
    }
    // 暴漏出数据
    return {
        channelList,
        getList
    }
})
```

父组件

```html
<script setup>
  import Son1Com from '@/components/Son1Com.vue'
  import Son2Com from '@/components/Son2Com.vue'
  import { storeToRefs } from 'pinia'
   // counter还是上面的例子中的仓库，store中定义了两个仓库命名不同
  import { useCounterStore } from '@/store/counter.js'
  import { useChannelStore } from '@/store/channel.js'
  const counterStore = useCounterStore()
  const channelStore = useChannelStore()
  // 不要对counterStore做对象解构会丢失数据的响应式
  // 如果希望结构之后还是保持对应的响应式对Store在解构的时候用一个方法storeToRefs这个方法需要从pinia中导入一下
  const { count, msg} = storeToRefs(counterStore)
  const { channelList } = storeToRefs(channelStore)
  // 因为是个函数所以不需要数据的响应式，数据的响应式在空数组上，直接对象解构拿到
  const { getList } = channelStore
</script>
<template>
  <div>
    <h3>
      <!-- 访问store数据的话直接访问 -->
      App.vue根组件 - {{ count }}
      - {{ msg }}
    </h3>
    <Son1Com></Son1Com>
    <Son2Com></Son2Com>
    <hr>
    <button @click="getList">获取频道数据</button>
    <ul>
      <li v-for="item in channelList" :key="item.id">{{ item.name }}</li>
    </ul>
  </div>
</template>
<style scoped>

</style>
```

效果

![image-20230831111442463](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308311114981.png)

## Pinia的调试

Vue官方的<font title='red'>dev-tools调试工具</font> 对Pinia直接支持，可以直接进行调试

![image-20230831111542048](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308311115432.png)

## Pinia持久化插件

官方文档：https://prazdevs.github.io/pinia-plugin-persistedstate/zh/

1.  安装插件 `pinia-plugin-persistedstate` 

    `npm i pinia-plugin-persistedstate` 

2.  mian.js使用

    ```js
    import persist from 'pinia-plugin-persistedstate'
    ...
    app.use(createPinia().use(persist))
    ```

3.  store仓库中，persist: true 开启

    

**代码演示**：

main.js

```js
import { createApp } from 'vue'
import { createPinia } from 'pinia'
// 导入持久化的插件
import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'
import App from './App.vue'
const pinia = createPinia() //创建pinia实例
// 创建根实例
const app = createApp(App)
app.use(pinia)// pinia插件的安装配置
app.use(pinia.use(piniaPluginPersistedstate))
app.mount('#app')// 视图的挂载
```

conter.js

```js
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'
// 定义store
// defineStore(仓库的唯一标识, () => { ... })
export const useCounterStore = defineStore('counter', () => {
    // 声明数据state - count
    const count = ref(100)
    // 声明操作数据的方法action(普通函数) 组合式写法没有action选项 普通函数就是action
    const addCount = () => count.value++
    const subCount =() => count.value--
    // 声明基于数据派生的计算属性getters (computed)
    // 利用computed函数去实现的,你需要getters你就提供一个computed
    const double = computed(() => count.value * 2)
    // 声明第二个数据state - msg 如果这个模块除了count还有其它数据
    const msg = ref('hello,world')
    // 将数据暴漏出去
    return {
        count,
        msg,
        addCount,
        subCount,
        double
    }
}, {
    persist: { //开启当前模块的持久化
        // 指定key的名称
        key: 'dk-keykeykey',
        // 指定要持久化的数据,其它的不持久化
        paths: ['count']
    } 
})
```

效果：

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308311135658.png" alt="image-20230831113535324" style="zoom:50%;" />

>  <span alt='solid'>总结</span>：
>
>  1.  pinia是用来做什么的？
>
>      新一代的状态管理工具，替代vuex
>
>  2.  pinia中还需要mutation吗？
>
>      不需要，action即支持同步也支持异步
>
>  3.  pinia如何实现getters
>
>      componentd计算属性函数
>
>  4.  pinia产生的store如何解构赋值数值保持响应式？
>
>      storeToRefs
>
>  5.  pinia如何快速实现持久化？
>
>      pinia-plugin-persistedstate