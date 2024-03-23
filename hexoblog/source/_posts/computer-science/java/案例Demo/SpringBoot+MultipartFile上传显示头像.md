---
title: SpringBoot上传头像回显头像到页面
categories:
   - [计算机学科,java,案例Demo]
tags:
   - 计算机学科
   - java
   - 案例Demo
   - springboot
---

工具类

```java
import java.util.Random;
import java.util.UUID;

public class UploadUtils {
    /**
     * 获取文件真实名称
     * 由于浏览器的不同获取的名称可能为:c:/upload/1.jpg或者1.jpg
     * 最终获取的为  1.jpg
     * @param name 上传上来的文件名称
     * @return 真实名称
     */
    public static String getRealName(String name) {
        //获取最后一个"/"
        int index = name.lastIndexOf("\\");
        return name.substring(index + 1);
    }

    /**
     * 获取随机名称
     * @param realName 真实名称
     * @return uuid 随机名称
     */
    public static String getUUIDName(String realName) {
        //realname  可能是  1sfasdf.jpg   也可能是 1sfasdf 1
        //获取后缀名
        int index = realName.lastIndexOf(".");
       //        uuId中取到的随即名字中带有-将所有的-去除掉
        if (index == -1) {
            return UUID.randomUUID().toString().replace("-", "").toUpperCase();
        } else {
            return UUID.randomUUID().toString().replace("-", "").toUpperCase() + realName.substring(index);
        }
    }

    /**
     * 获取文件目录,可以获取256个随机目录
     * @return 随机目录
     */
    public static String getDir() {
        String s = "0123456789ABCDEF";
        Random r = new Random();
        // /随机到的字符/随机到的字符
        return "/" + s.charAt(r.nextInt(16)) + "/" + s.charAt(r.nextInt(16));
    }
}
```

Mapper

```java
@SuppressWarnings("all")
@Mapper
public interface UserMapper {
    int updateAvatar(User u);
}
```

Mapper.xml

```xml
<!--通过用户id更新头像-->
<update id="updateAvatar" parameterType="com.dkx.domain.User">
   update taobao.user set avatar = #{avatar} where id = #{id}
</update>
```

对应user表结构

![image-20230511091602576](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230511091602576.png)

Service

```java
public interface UserService {
    boolean updateAvatar(User u);
}
```

ServiceImpl

```java
@SuppressWarnings("all")
@Service
public class UserServiceImpl implements UserService {
    @Autowired
    private UserMapper mapper;

    @Override
    public boolean updateAvatar(User u){
        int flag = mapper.updateAvatar(u);
        return flag > 0 ? true : false;
    }
}
```

Controller

```java
@SuppressWarnings("all")
@RequestMapping("/user")
@Controller
public class UserController {
    @Autowired
    private UserService service;

	 @RequestMapping(value = "/upload" , method = RequestMethod.POST)
    public String addAvatar(MultipartFile file,HttpSession session) throws IOException {
//        获取文件的内容
        InputStream is = file.getInputStream();
//        获取原始文件名
        String original = file.getOriginalFilename();
        String fileName = original.substring(original.lastIndexOf("."),original.length());
        //判断用户传入的文件后缀名是否是图片
        if(fileName.equals(".png") || fileName.equals(".gif") || fileName.equals(".jpg")){
//        生成一个uuid名称
            String uuidFileName = UploadUtils.getUUIDName(fileName);
//        生成随机目录
            String randomDir = UploadUtils.getDir();
            File fileDir = new File("D:/updatefiles"+randomDir);
//        判断文件夹不存在则创建
            if(!fileDir.exists()){
                fileDir.mkdirs();
            }
//        创建新的文件夹
            File newFile = new File("D:/updatefiles"+randomDir,uuidFileName);
           // 该方法是SpringMVC中封装的函数，用于将MultipartFile对象内的文件输出到对应的路径中
            file.transferTo(newFile);
            String savePath = randomDir + "/" + uuidFileName;
//        获取当前的user
            User user = (User) session.getAttribute("user");
//        设置头像路径
            user.setAvatar(savePath);
//        调用业务更新
            service.updateAvatar(user);
            return "redirect:/user/userInfo";
        }
        return "redirect:/user/userInfo";
    }

    @Autowired
    ResourceLoader resourceLoader;

    @GetMapping("/get/{dir1}/{dir2}/{filename:.+}")
    public ResponseEntity get(@PathVariable String dir1,
                              @PathVariable String dir2,
                              @PathVariable String filename) {
        //1.根据用户名去获取相应的图片
        Path path = Paths.get("D:/updatefiles" + "/" + dir1 + "/" + dir2, filename);
        //2.将文件加载进来
        Resource resource = resourceLoader.getResource("file:" + path.toString());
        //返回响应实体
        return ResponseEntity.ok(resource);
    }

    @RequestMapping("/userInfo")
    public String userInfo(HttpSession session){
        User u = (User) session.getAttribute("user");
        if(u != null){
            return "taobao";
        }else{
            return "";
        }
    }
}
```

html

通过当前的input标签this对象调用files[0]获取数组第0索引的数据来判断是否可用上传按钮

```html
<span>
   <form action="/user/upload" method="post" enctype="multipart/form-data">
      <div class="row">
         <input type="file" onchange="if(this.files[0] != null && this.files[0] !== 'undefined'){en()}else{di()}" name="file"/><br>
         &nbsp;&nbsp;&nbsp;&nbsp;👆&nbsp;&nbsp;<button title="请选择图片后才能使用上传按钮" type="submit" value="头像" id="an_tou" disabled="true" class="btn btn-primary">头像</button><br>
      </div>
   </form>
</span>
```

显示img

```html
<span class="touxiang">
   <img th:src="|@{/user/get}${session.user.avatar}|"
        class="img-circle img-thumbnail" style="width:100px;height:100px"/>
</span>
```

可能遇到问题-上传文件大小受限制-解决方式如下：

```yaml
spring:
	servlet:
		multipart:
			max-file-size=100MB
			max-request-size=1000MB
```

如果想不让文件大小受限制，将spring.servlet.multipart.max-file-size这个参数设置为 -1