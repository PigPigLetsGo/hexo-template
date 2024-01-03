---
title: vue3-pnpm 包管理器 - 创建项目
categories: 
    - [计算机学科,web,vue3]
tags:
    - web
    - vue3
    - 计算机学科
---

## pnpm 包管理器 - 创建项目

一些优势：比同类工具快2倍左右，节省磁盘空间 … 

安装方式：`npm install -g pnpm` 

创建项目：`pnpm create vue` 

-D = `--save-dev` ，-g = `--global` 

dev启动项目，具体的启动方式看package.json中怎么配置vue3一般使用vite来启动

| npm                     | yarn                | pnpm                |
| ----------------------- | ------------------- | ------------------- |
| npm install             | yarn                | pnpm install        |
| npm install axios       | yarn axios          | pnpm add axios      |
| npm install axios -D/-g | yarn add axios -D/g | pnpm add axios -D/g |
| npm uninstall axios     | yarn remove axios   | pnpm remove axios   |
| npm run dev             | yarn dev            | pnpm dev            |

-  <span alt='solid'>注意</span>：使用 pnpm 创建项目的时候不要直接在根目录创建，而是在根目录中创建一个子目录，用这个子目录作为项目的根目录。
-  **原因**：防止报 权限不足

```sh
 npm install -g pnpm

added 1 package in 14s

1 package is looking for funding
  run `npm fund` for details
 pnpm create vue
../../../.pnpm-store/v3/tmp/dlx-12928    |   +1 +
../../../.pnpm-store/v3/tmp/dlx-12928    | Progress: resolved 1, reused 0, downloaded 1, added 1, done

Vue.js - The Progressive JavaScript Framework

√ Project name: ... vue3-big-event-admin
√ Add TypeScript? ... No / Yes
√ Add JSX Support? ... No / Yes
√ Add Vue Router for Single Page Application development? ... No / Yes
√ Add Pinia for state management? ... No / Yes
√ Add Vitest for Unit Testing? ... No / Yes
√ Add an End-to-End Testing Solution? » No
√ Add ESLint for code quality? ... No / Yes
√ Add Prettier for code formatting? ... No / Yes

Scaffolding project in E:\Vue\Demo7\Demo\vue3-big-event-admin...

Done. Now run:

  cd vue3-big-event-admin
  pnpm install
  pnpm format
  pnpm dev

 cd .\vue3-big-event-admin\
 ls


    目录: E:\Vue\Demo7\Demo\vue3-big-event-admin


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2023/8/31     15:48                .vscode
d-----         2023/8/31     15:48                public
d-----         2023/8/31     15:48                src
-a----         2023/8/31     15:48            296 .eslintrc.cjs
-a----         2023/8/31     15:46            302 .gitignore
-a----         2023/8/31     15:48            163 .prettierrc.json
-a----         2023/8/31     15:46            331 index.html
-a----         2023/8/31     15:48            652 package.json
-a----         2023/8/31     15:48            705 README.md
-a----         2023/8/31     15:48            309 vite.config.js


 pnpm install
Packages: +194
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Progress: resolved 216, reused 0, downloaded 194, added 194, done
node_modules/.pnpm/vue-demi@0.14.6_vue@3.3.4/node_modules/vue-demi: Running postinstall script, done in 281ms
node_modules/.pnpm/esbuild@0.18.20/node_modules/esbuild: Running postinstall script, done in 230ms

dependencies:
+ pinia 2.1.6
+ vue 3.3.4
+ vue-router 4.2.4

devDependencies:
+ @rushstack/eslint-patch 1.3.3
+ @vitejs/plugin-vue 4.3.4
+ @vue/eslint-config-prettier 8.0.0
+ eslint 8.48.0
+ eslint-plugin-vue 9.17.0
+ prettier 3.0.3
+ vite 4.4.9

Done in 36.7s
 pnpm dev

> vue3-big-event-admin@0.0.0 dev E:\Vue\Demo7\Demo\vue3-big-event-admin
> vite

Port 5173 is in use, trying another one...

  VITE v4.4.9  ready in 480 ms

  ➜  Local:   http://localhost:5174/
  ➜  Network: use --host to expose
  ➜  press h to show help
```

