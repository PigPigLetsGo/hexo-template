---
title: alova
categories:
    - [tools,前端]
tags:
    - tools
    - web
---

# alova

## 改变axios的用法后，我的工作效率提升了3倍

实际场景下的请求问题

作为前端开发，网络请求肯定是我们经常要面对的事情，在前端请求中，axios和fetch API应该是我们最常用的请求工具了，它们在发送请求和接收响应数据已经做到了足够简单。

但在实际项目中，为了达到更好的用户体验，我们还需要考虑下面这几个因素：

1.  展示加载中的请求状态
2.  展示请求错误状态
3.  展示上传/下载文件的进度信息

上面这些都需要我们编写额外的代码，增加了不少的工作量，你的请求代码可能是下面这样的，我们以vue3代码为示例。

```js
const loading = ref(false);  
const data = ref({});  
const error = ref(null);  
const request = async () => { 
  try {  
    loading.value = true;  
    data.value = await axios.get('/xxx');  
  } catch (e) {  
    error.value = e;  
  }
  loading.value = false;  
};
onMounted(request);
```

如果面对大量的api，这工作量可想而知，想到这时就有点头疼啊。有没有一种方法可以自动帮我处理这些逻辑，让请求代码看起来更简洁呢？

解决

我们可以用封装的思路，把上面这些都封装为一个简单的use hook，就可以很好地解决了，封装后的代码大概是下面这样的。

```js
export const useRequest = (url) => {
  const loading = ref(false);
  const data = ref({});
  const error = ref(null);
  const request = async () => {
    try {
      loading.value = true;
      data.value = await axios.get(url);
    } catch (e) {
      error.value = e;
    }
    loading.value = false;
  }
  onMounted(request);

  return {
    loading,
    data,
    error
  };
}
```

这是一个最简单的use hook实现，它帮我们解决了请求模板代码的问题。当然你还可以使用use hook封装更多更高级的请求功能，而这些功能现在不必你自己封装了，使用[alova](https://link.juejin.cn/?target=https%3A%2F%2Falova.js.org)就可以了。

**alova 是一个轻量级的请求策略库，针对分页请求、表单提交、上传和下载文件等不同请求场景使用对应的请求模块，让开发者使用非常少量的代码就可以实现高可用性和高流畅性的请求功能，这意味着，你再也不需要自己绞尽脑汁编写请求优化代码，再也不需要自己维护请求数据和相关状态，你只需要选择并使用请求模块，设置参数后，alova 帮你搞定！**

在引入alova后，我的工作效率直接提高了3倍，强烈推荐给大家。

你可以将alova理解成是axios或fetch-api等请求工具的一种武器装备，将alova与请求工具配合使用将会让它们变得更加强大。

>  其实，alova底层依然依赖axios或fetch-api等请求函数进行请求，因此你仍然可以使用你喜欢的请求库。

以下是一个基于`vue3+axios+alova`的使用示例，alova将自动为你创建请求相关的，可以直接用于视图的响应式状态，创建一个vue3的项目架子，然后运行：

`npm i` 初始化项目

`npm i alova --save-dev` 下载alova

`npm i @alova/adapter-axios --save-dev` 下载alova配合axios使用的依赖

代码如下：

```js
<template>
  <div v-if="loading">Loading...</div>
  <div v-else-if="error">{{ error.message }}</div>
  <span v-else>responseData: {{ data }}</span>
</template>

<script setup>
import { createAlova, useRequest } from 'alova';
import VueHook from 'alova/vue';
import { axiosRequestAdapter } from '@alova/adapter-axios';

const alovaInstance = createAlova({
  statesHook: VueHook,

  // 设置使用axios作为请求工具
  requestAdapter: axiosRequestAdapter()
});

const {
  // 加载状态
  loading,

  // 响应数据
  data,

  // 错误信息，请求错误时才有值
  error
} = useRequest(alovaInstance.Get('/todoList'));
</script>
```

是不是非常nice！！！之前需要我们自己实现的各种功能，alova 都帮我们做了。

alova的github地址：https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Falovajs%2Falova

alova的官网地址：https://link.juejin.cn/?target=https%3A%2F%2Falova.js.org%2F

**总结**：

>  alova目前提供了8种请求策略，如果你希望在不同的请求场景下方便地实现特定的请求需求，那千万别错过它哦。alova 还提供了缓存管理、请求共享等更多功能，可以覆盖绝大多数请求场景。
>
>  所以，在你的下一个项目中，你也可以来试试 alova，一定能给你带来更愉快的开发体验!

