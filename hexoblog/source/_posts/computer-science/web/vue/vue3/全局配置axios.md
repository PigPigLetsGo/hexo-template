---
title: vue3-全局配置axios与使用
categories: 
    - [计算机学科,web,vue,vue3]
tags:
    - vue3
    - 计算机学科
---

## vue3全局配置axios与使用

### 为什么要全局配置axios

在实际项目开发中，几乎每个组件中都会用到 axios 发起数据请求。此时会遇到如下两个问题：

① 每个组件中都需要导入 axios（代码臃肿）

② 每次发请求都需要填写完整的请求路径（不利于后期的维护）

### vue2与vue3配置axios的不同之处

在vue2中会习惯性的把axios挂载到全局，以方便在各个组件或页面中使用this.$http请求接口。但是在vue3中取消了Vue.prototype，在全局挂载方法和属性时，需要使用官方提供的globalPropertiesAPI。

### 第一步：导入axios并进行挂载

```js
const app = createApp(App)

import axios from "axios"

app.config.globalProperties.$http = axios
```

### 第二步：在组件中获取getCurrentInstance实例，使用实例中已挂载的$http方法

```js
import { onMounted, getCurrentInstance} from "vue";

const {proxy} = getCurrentInstance()
onMounted(async()=>{
  
  const res=await proxy.$http.get('api/v1/topics')

  console.log(res)
})
```

