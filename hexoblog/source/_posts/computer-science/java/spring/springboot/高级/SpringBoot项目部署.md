---
title: SpringBoot项目部署
categories:
   - [计算机学科,java,spring,springboot,高级]
tags:
   - 计算机学科
   - springboot高级
---

SpringBoot项目开发完毕后，支持两种方式部署到服务器

1.  jar包 (官方推荐)
2.  war包

## jar

创建一个SpringBoot工程

![image-20230914111656588](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141116673.png)

创建UserController类返回一个访问成功的提示信息

![image-20230914111738022](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141117227.png)

测试访问是否有问题：

![image-20230914111756919](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141117069.png)

没有问题后 进行 打包

![image-20230914111821221](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141118506.png)

查看打包后的目录，复制路径然后打开资源管理器地址栏中输入复制的路径访问

![image-20230914111923603](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141119558.png)

就可以看到打包成功后的jar包了

![image-20230914111940320](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141119466.png)

在当前的目录中打开powerShell然后输入命令：`java -jar .\springboot-deploy-0.0.1-SNAPSHOT.jar`

![image-20230914112256831](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141122720.png)

然后访问路径

![image-20230914112318734](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141123411.png)

## war

将项目的打包方式改为war包的方式

![image-20230914112549411](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141125752.png)

编写SpringBoot的启动类

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

@SpringBootApplication
public class SpringbootDeployApplication extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootDeployApplication.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(SpringbootDeployApplication.class);
    }
}
```

点击maven的打包按钮进行打包为war包

![image-20230914113123574](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141131231.png)

打包完成后查看目录的打包情况

![image-20230914113223976](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141132438.png)

![image-20230914113337947](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141133425.png)

将springboot的war包放入到tomcat的webapps目录下

![image-20230914115152845](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141151982.png)

启动tomcat 

bin\startup.bat 双击运行

![image-20230914115310123](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141153758.png)

当我们再次访问localhost:8080/user/findAll时就会报404的，因为此时的虚拟目标发生了变化了。

![image-20230914115525516](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141155554.png)

虚拟目录为springboot-deploy-0.0.1-SNAPSHOT

![image-20230914115541682](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141155514.png)

访问localhost:8080/springboot-deploy-0.0.1-SNAPSHOT/user/findAll

![image-20230914115638227](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141156392.png)

还有第二个问题就是，更改端口号在项目中的resource中修改不起作用，因为这里对SpringBoot内置的tomcat的端口号的修改，而我们启动SpringBoot项目使用的是我们自己下载的外部的tomcat，需要在外部的tomcat中进行配置才行。