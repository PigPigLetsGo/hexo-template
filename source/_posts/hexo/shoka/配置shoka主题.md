---
title: 配置shoka主题，以及解决依赖版本问题导致的代码块显示问题
categories:
    - [hexo,shoka]
tags:
    - hexo
    - shoka
---

# 配置shoka主题

**前提**：配置好hexo，会有版本问题下面再解决

1.  访问github拉取shoka主题代码

    ```http
    https://github.com/amehime/hexo-theme-shoka
    ```

2.  或者直接使用如下命令拉取项目

    ```http
    git clone https://github.com/amehime/hexo-theme-shoka.git ./themes/shoka
    ```

    需要知道的几点：

    1.  shoka拉取下来后里面有一个example目录，这个目录里面都是例子代码，并不会配置了对项目生效的 😪
    2.  其中example下的package.json是hexo目录中的package.json的参考文件，_config.yml是hexo目录中的`_config.yml`参考文件，`_config.shoka.yml`是shok中的`_config.yml`参考文件。注意不要混淆 !
    3.  建议将shoka中的example中的配置文件与hexo的配置文件内容放到hexo的配置文件中，避免重复的设置项 不然会报错的

3.  拉取下来shoka项目后确认这个项目放到的位置是在hexo的themes目录中

4.  修改hexo的_config.yml文件，将里面的主题项修改为shoka

    ```yaml
    # Extensions
    ## Plugins: http://hexo.io/plugins/
    ## Themes: http://hexo.io/themes/
    theme: shoka # edit for Theme.shoka
    ```

    配置完成后基本上shoka这个主题就应用上去了，我们就可以去配置shoka的_config.yml配置了

5.  安装依赖的插件：

    | 插件名称                        | npm 地址 | 功能                              | 依赖程度      |
    | ------------------------------- | -------- | --------------------------------- | ------------- |
    | hexo-renderer-multi-markdown-it | 链接     | md 文件渲染器，压缩 css/js/html   | 必需          |
    | hexo-autoprefixer               | 链接     | 给生成的 css 文件们添加浏览器前缀 | 必需          |
    | hexo-algoliasearch              | 链接     | 站内搜索功能                      | 搜索按钮失灵  |
    | hexo-symbols-count-time         | 链接     | 文章或站点字数及阅读时间统计      | 统计没有      |
    | hexo-feed                       | 链接     | 生成 Feed 文件                    | Feed 文件没有 |

    -  hexo-renderer-multi-markdown-it   ：https://www.npmjs.com/package/hexo-renderer-multi-markdown-it
    -  hexo-autoprefixer : https://www.npmjs.com/package/hexo-autoprefixer
    -  hexo-algoliasearch ：https://www.npmjs.com/package/hexo-algoliasearch
    -  hexo-symbols-count-time ：https://www.npmjs.com/package/hexo-symbols-count-time
    -  hexo-feed ：https://www.npmjs.com/package/hexo-feed

6.  如果直接下载上面的插件可能导致 页面的 代码块 显示有问题 这是 版本问题所导致的解决问题如下：修改package.json文件

7.  修改hexo的package.json的文件内容，将shoka的example目录中的package.json文件的内容拷贝到hexo的package.json中即可

8.  修改完package.json文件内容后执行下面的命令降级hexo

    -  重新全局安装hexo 5.4.2

       ```bash
       npm install -g hexo@5.4.2
       ```

    -  然后在hexo目录下更新依赖

       ```bash
       npm install
       ```

    -  然后重新生成就可以解决问题了

       ```bash
       hexo clean && hexo g && hexo s
       ```

       注意：浏览器可能有缓存，记得刷新缓存

9.  然后将 shoka中的 example\source 目录中的 `_data` ，assets， friends 这几个目录放到hexo的source目录中，注意你要配置友情链接可以看friends目录中的说明，这个目录就是友情链接的配置文件，其余的如下：

10.  什么是about，friends，links？就是如下图

     ![image-20240103190306102](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031903220.png)

     -  about：关于 的页面配置，在hexo的source目录中创建about目录里面创建index.md文件，在里面写helloworld然后重新生成hexo s 打开网页后点击关于查看 会显示hello world字样

     -  links：链接 的配置同样的在hexo的source目录中创建links目录然后里面创建index.md文件用于配置链接的页面

     -  `_post`：里面是配置 .md 笔记的，在该目录中创建 目录分类 比如 java目录里面存放java的笔记 易于管理 并且 创建java目录后我们需要在hexo的`_config.yml`文件中进行category_map的配置格式如下：

        ```yaml
        # Category & Tag
        default_category: uncategorized
        category_map:
          计算机学科: computer-science
          数据结构与算法: data-structures-and-algorithms
          编程题: programming-questions
          Java: java
          js: js
          web: web
          video: video
          movie: movie
          Tvdrama: Tvdrama
          animation: animation
          Reality: Reality
          问题总汇: bug
          css: css
          vue: vue
          vue3: vue3
          database: databse
          mongodb: mongodb
          mysql: mysql
        ```

     -  **注意**：下面还有一个tag_map的配置项，这里并不是配置标签的不要被误导了，标签是在笔记文件中的YAML Front Matter里面配置的tag 选项 具体格式如下：

        ```.md笔记文章
        ---
        title: 这是文章的标题
        categories:
        	- [分类,分类,分类]
        tags:
        	- 标签
            - 标签
        ---
        
        hello world,这里是我的笔记内容
        ```

11.  其它的配置就看个人喜好来配置就好了，至此基本的配置完毕！