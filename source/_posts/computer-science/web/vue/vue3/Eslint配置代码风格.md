---
title: vue3-Eslint配置代码风格
categories: 
    - [计算机学科,web,vue,vue3]
tags:
    - web
    - vue3
    - 计算机学科
---

## Eslint配置代码风格

**环境同步**：

1.  安装了插件ESlint，开启保存自动修复
2.  禁用了插件Prettier，并关闭保存自动格式化

```js
//ESlint插件 + Vscode配置  实现自动格式化修复
"editor.codeActionsOnSave": {
   "source.fixAll": true
},
"editor.formatOnSaven": false
```



**配置文件**`.eslintrc.cjs` 

1.  prettier风格配置 https://prettier.io 
    1.  单引号
    2.  不使用分号
    3.  每行宽度至多 80 字符
    4.  不加对象 | 数组最后逗号
    5.  换行符号不限制 (win mac 不一致)
2.  vue组件名称多单词组成 (忽略index.vue)
3.  props解构 (关闭)

```js
rules: {
   // prettier专注于代码的美观度 (格式化工具)
   // 前置：
   // 1. 禁用格式化插件 prettier  format on save 关闭
   // 2. 安装Eslint插件, 并配置保存时自动修复
   'prettier/prettier': [
      'warn',
      {
         singleQuote: true, // 单引号
         semi: false, // 无分号
         printWidth: 80, // 每行宽度至多80字符
         trailingComma: 'none', // 不加对象|数组最后逗号
         endOfLine: 'auto' // 换行符号不限制（win mac 不一致）
      }
   ],
      // ESLint关注于规范, 如果不符合规范，报错
      'vue/multi-word-component-names': [
         'warn',
         {
            ignores: ['index'] // vue组件名称多单词组成（忽略index.vue）
         }
      ],
         'vue/no-setup-props-destructure': ['off'], // 关闭 props 解构的校验 (props解构丢失响应式)
            // 添加未定义变量错误提示，create-vue@3.6.3 关闭，这里加上是为了支持下一个章节演示。
            'no-undef': 'error'
}
```

![image-20230831163101352](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308311631742.png)

