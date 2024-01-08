---
title: 搜索服务器使用idea
categories:
   - [tools,idea]
tagls:
   - idea
---

如果上方的服务器激活地址都失效了，还有个办法可以搜索网络上可用的激活服务器，一般人我不告诉他，请务必低调使用。

1. 首先打开这个网站：https://search.censys.io/

2. 然后在搜索框中输入：`services.http.response.headers.location: account.jetbrains.com/fls-auth`，点击`搜索`，网站会检索出很多 IP 地址

   ![search.censys.io](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131411486.png)

3. 任意点一个 IP 地址查看详情页，确保`80/HTTP`地址下的`Status Code`状态码为`302`，这时候复制出`Detail`这里的 IP 地址，作为我们的 License Server 地址

   ![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131411450.png)

4. 和上面一样，输入地址后点击激活

   ![输入激活服务器IP地址](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131412970.png)

5. 如果无法激活再回到搜索结果页换一个 IP 地址再次试验，小编试验了 3 次就成功了，IDEA 成功永久 激活，亲测有效

   ![IEAD正版激活成功](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131412586.png)



