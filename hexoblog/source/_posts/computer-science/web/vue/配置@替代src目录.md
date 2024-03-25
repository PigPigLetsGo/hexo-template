---
title: 配置@代替src目录
categories: 
    - [计算机学科,web,vue]
tags:
    - vue3
    - 计算机学科
    - 代码经验
---

## 配置

##### 1 安装path模块

```js
npm i path --save-dev
```

##### 2 配置jsconfig.js

一般项目自动生成的配置如下：

```js
{
  "compilerOptions": {
    "target": "es5",
    "module": "esnext",
    "baseUrl": "./",
    "moduleResolution": "node",
    "paths": {
      "@/*": [
        "src/*"
      ]
    },
    "lib": [
      "esnext",
      "dom",
      "dom.iterable",
      "scripthost"
    ]
  }
}

```



```js
{
   "compilerOptions": {
      "baseUrl": "./",
      "paths": {
         "@/*": ["src/*"]
      }
   },
   "exclude": ["node_modules", "dist"]
}
```

##### 3 配置vue.config.js

```js
// 配置configureWebpack
'use strict'
const path = require('path')
const resolve = dir => path.join(__dirname, dir)
 
module.exports = {
  configureWebpack: {
    name: 'vue Element Admin',
    resolve: {
      alias: {
        '@': resolve('src')
      }
    }
  }
}
```

