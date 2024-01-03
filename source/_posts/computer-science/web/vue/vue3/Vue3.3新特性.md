---
title: vue3-新特性
categories: 
    - [计算机学科,web,vue3]
tags:
    - web
    - vue3
    - 计算机学科
---

## defineOptions

>  <span alt='solid'>背景说明</span>：
>
>  有`<script setup>`之前，如果要定义props，emits可以轻而易举的添加一个与setup平级的属性。
>
>  但是有了`<script setup>`后，就没法这么干了setup属性已经没有了，自然无法添加与其平级的属性。
>
>  ---
>
>  为了解决这一问题，引入了<font title='red'>defineProps</font>与<font title='red'>defineEmits</font>这两个宏，但是只解决了<font title='red'>props</font>与<font title='red'>emits</font>这两个属性。
>
>  如果我们要定义组件的name或其它自定义的属性，还是得回到最原始的用法——再添加一个普通的<script>标签。
>
>  这样就会存在两个<script>标签。让人无法接受。

于是。。。就迎来了。

## Vue3.3新特性-defineOptions

所以在vue3.3中新引入了 <font title='red'>defineOptions </font>宏。顾名思义，主要是用来定义 <font title='red'>Options API </font>的选项。可以用 defineOptions 定义任意的选项，props，emits，expose，slots 除外 (因为这些可以使用 defineXXX来做到)

```js
<script setup>
	defineOptions({
   	name: 'Foo',
      // 不希望组件的根元素继承特性，如果希望设置为 true
   	inheritAttrs: false,
   	// ... 更多自定义属性
	})
</script>
```

## Vue3.3新特性-defineModel

在Vue3中，自定义组件上使用v-model，相当于传递一个modelValue属性，同时触发 update:modelValue事件

![image-20230830201543099](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308302015742.png)

我们需要先定义props，在定义emits。其中有许多重复的代码。如果需要修改此值，还需要手动调用emit函数。

![image-20230830203106641](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308302031546.png)

**代码演示**：

父组件

```html
<script setup>
  import MyInput from '@/components/my-input.vue'
  import { ref } from 'vue'
  const text = ref('123456')
</script>
<template>
<MyInput v-model="text"></MyInput>
{{ text }}
</template>
```

子组件

```html
<script setup>
    import { defineModel } from 'vue'
    const modeValue = defineModel()
    const emit = defineEmits(['update:modeValue'])
</script>
<template>
    <div>
        <input :value="modeValue"
        @input="e => modeValue = e.target.value " type="text"/>
    </div>
</template>
```

vite配置

```js
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue({
      // 配置开启defineModel
      script: {
        defineModel: true
      }
    }),
  ],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})
```

**效果**：

![image-20230830203747863](./Vue3.3%E6%96%B0%E7%89%B9%E6%80%A7.assets/image-20230830203747863.png)