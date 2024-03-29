---
title: Minio
categories:
   - [计算机学科,java,第三方库]
tags:
   - 计算机学科
   - java
   - 第三方库
   - Minio
---

# 一、Minio

## 1.1、介绍

本项目采用Minio构建分布式文件系统，MiniO是一个非常轻量的服务，可以很简单的和其它应用的结合使用，它兼容亚马孙S3云存储服务接口，非常适合于存储大容量非结构化的数据，例如图片，视频，日志文件，备份数据和容器/虚拟机镜像等。

它一大特点就是轻量，使用简单，功能强大，支持各种平台，单个文件最大5TB，兼容Amazon S3接口，提供了Java，Python，Go等多版本SDK支持。

官网：https://min.io

中文：https://www.minio.org.cn/，http://docs.minio.org.cn/docs/

Minio集群采用去中心化共享架构，每个节点是对等关系，通过Nginx可对Minio进行负载均衡访问。

**去中心化有什么好处？**。

在大数据领域，通常的设计理念都是无中心和分布式。Minio分布式模式可以帮助你搭建一个高可用的对象存储服务，你可以使用这些存储设备，而不用考虑其真实物理位置。

它将分布在不用服务器上的多块硬盘组成一个对象存储服务。由于硬盘分布在不同的节点，分布式Minio避免了单点故障。如下图：

![image-20231030214114635](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310302141371.png)

Minio使用纠删码技术来保护数据，它是一种恢复丢失和损坏数据的数学算法，它将数据分块冗余的分散存储在各个节点的磁盘上，所有的可用磁盘组成一个集合，上图由8块硬盘组成一个集合，当上传一个文件时会通过纠删码算法计算对文件进行分块存储，除了将文件本身分成4个数据块，还会生成4个校验块，数据块和校验块会分散的存储在这8块硬盘上。

使用纠删码的好处是即便丢失一半数量(N//2)的硬盘，仍然可以恢复数据。比如上边集合中有4个以内的硬盘损坏仍可保证数据恢复，不影响上传和下载，如果多余一半的硬盘坏了则无法恢复

## 1.2、数据恢复演示

下边在本机演示Minio恢复数据的过程。在本地创建4个目录表示4个硬盘。

![image-20231030214820074](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310302148756.png)

下载minio，下载地址在 https://dl.minio.io/server/minio/release/，可从课程资料找到minio的安装文件minio.zip解压即可使用，CMD进入有minio.exe的目录，运行下边的命令：

```sh
minio.exe server D:\develop\minio_data\data1  D:\develop\minio_data\data2  D:\develop\minio_data\data3  D:\develop\minio_data\data4
```

启动完成后效果如下所示：

![image-20231031085828981](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310310858167.png)

说明如下：

```
WARNING: MINIO_ACCESS_KEY and MINIO_SECRET_KEY are deprecated.
         Please use MINIO_ROOT_USER and MINIO_ROOT_PASSWORD
Formatting 1st pool, 1 set(s), 4 drives per set.
WARNING: Host local has more than 2 drives of set. A host failure will result in data becoming unavailable.
WARNING: Detected default credentials 'minioadmin:minioadmin', we recommend that you change these values with 'MINIO_ROOT_USER' and 'MINIO_ROOT_PASSWORD' environment variables
```

1）老版本使用的MINIO_ACCESS_KEY 和 MINIO_SECRET_KEY不推荐使用，推荐使用MINIO_ROOT_USER 和MINIO_ROOT_PASSWORD设置账号和密码。

2）pool即minio节点组成的池子，当前有一个pool和4个硬盘组成的set集合

3）因为集合是4个硬盘，大于2的硬盘损坏数据将无法恢复。

4）账号和密码默认为minioadmin、minioadmin，可以在环境变量中设置通过'MINIO_ROOT_USER' and 'MINIO_ROOT_PASSWORD' 进行设置。

下边输入http://localhost:9000进行登录，账号和密码为：minioadmin/minioadmin

登录成功后，下一步创建bucket，桶，它相当于存储文件的目录，可以创建若干的桶。

![image-20231031090017162](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310310900241.png)

输入bucket的名称，点击“CreateBucket”，创建成功

![image-20231031090033336](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310310900332.png)

点击“upload”上传文件。

下边上传几个文件

![image-20231031090045752](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310310900295.png)

下边去四个目录观察文件的存储情况

![image-20231031090107880](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310310901020.png)

我们发现上传的1.mp4文件存储在了四个目录，即四个硬盘上。

下边测试minio的数据恢复过程：

1、首先删除一个目录。

删除目录后仍然可以在web控制台上传文件和下载文件。

稍等片刻删除的目录自动恢复。

2、删除两个目录。

删除两个目录也会自动恢复。

3、删除三个目录 。

由于 集合中共有4块硬盘，有大于一半的硬盘损坏数据无法恢复。

此时报错：We encountered an internal error, please try again.  (Read failed.  Insufficient number of drives online)在线驱动器数量不足。

# 二、SDK

## 2.1、上传文件

MinIO提供多个语言版本SDK的支持，下边找到java版本的文档：

地址：https://docs.min.io/docs/java-client-quickstart-guide.html

最低需求Java 1.8或更高版本:

maven依赖如下：

```xml
<dependency>
    <groupId>io.minio</groupId>
    <artifactId>minio</artifactId>
    <version>8.4.3</version>
</dependency>
<dependency>
    <groupId>com.squareup.okhttp3</groupId>
    <artifactId>okhttp</artifactId>
    <version>4.8.1</version>
</dependency>
```

在media-service工程添加此依赖。

参数说明：

需要三个参数才能连接到minio服务。

| 参数       | 说明                                         |
| ---------- | -------------------------------------------- |
| Endpoint   | 对象存储服务的URL                            |
| Access Key | Access key就像用户ID，可以唯一标识你的账户。 |
| Secret Key | Secret key是你账户的密码。                   |

## 2.2、添加

编写上传代码进行测试：

```java
import io.minio.MinioClient;
import io.minio.UploadObjectArgs;
import org.junit.jupiter.api.Test;

/**
 * @author Dkx
 * @version 1.0
 * @2023/10/319:34
 * @function
 * @comment 测试Minio的SDK
 */
public class MinioTest {
    MinioClient minioClient =
            MinioClient.builder()
                    .endpoint("http://192.168.249.128:9000")
                    .credentials("minioadmin",
                            "minioadmin")
                    .build();
    @Test
    public void test_update() throws Exception {
        UploadObjectArgs build = UploadObjectArgs.builder()
                .bucket("testbucket") // 桶名称
                .filename("D:\\Backup\\Documents\\My Pictures\\Camera Roll\\瑞克.png")  // 指定上传文件路径
                .object("dkx") // 指定上传文件的对象名(随便起名)
              //.object("/root/dkx/dkx1.png") // 可以创建多级目录来进行存储
                .build();
        // 调用函数上传文件
        minioClient.uploadObject(build);
    }
}
```

执行完后，查看上传的结果：

可以看到上传成功了！

![image-20231031100946983](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310311009033.png)

## 2.3、删除

测试删除指定的桶对象

```java
import io.minio.MinioClient;
import io.minio.RemoveObjectArgs;
import org.junit.jupiter.api.Test;

/**
 * @author Dkx
 * @version 1.0
 * @2023/10/319:34
 * @function
 * @comment 测试Minio的SDK
 */
public class MinioTest {
    MinioClient minioClient =
            MinioClient.builder()
                    .endpoint("http://192.168.249.128:9000")
                    .credentials("minioadmin",
                            "minioadmin")
                    .build();
    @Test
    public void test_update() throws Exception {
        // 删除指定桶对象
        RemoveObjectArgs removeObjectArgs = RemoveObjectArgs.builder()
                .bucket("testbucket")
                        .object("dkx")
                                .build();
        // 调用函数上传文件
        minioClient.removeObject(removeObjectArgs);
    }
}
```

查看删除结果：

![image-20231031101414466](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310311014474.png)

设置contentType可以通过com.j256.simplemagic.ContentType枚举类查看常用的mimeType（媒体类型）

通过扩展名得到mimeType，代码如下：

```java
//根据扩展名取出mimeType
ContentInfo extensionMatch = ContentInfoUtil.findExtensionMatch(".mp4");
String mimeType = MediaType.APPLICATION_OCTET_STREAM_VALUE;//通用mimeType，字节流
```

完善上边的代码 如下：

```java
   @Test
   public  void upload() {
   //根据扩展名取出mimeType
   ContentInfo extensionMatch = ContentInfoUtil.findExtensionMatch(".mp4");
   String mimeType = MediaType.APPLICATION_OCTET_STREAM_VALUE;//通用mimeType，字节流
   if(extensionMatch!=null){
      mimeType = extensionMatch.getMimeType();
   }
   try {
      UploadObjectArgs testbucket = UploadObjectArgs.builder()
         .bucket("testbucket")
         //                    .object("test001.mp4")
         .object("001/test001.mp4")//添加子目录
         .filename("D:\\develop\\upload\\1mp4.temp")
         .contentType(mimeType)//默认根据扩展名确定文件内容类型，也可以指定
         .build();
      minioClient.uploadObject(testbucket);
      System.out.println("上传成功");
   } catch (Exception e) {
      e.printStackTrace();
      System.out.println("上传失败");
   }

}
```

## 分块上传文件

```java
public R uploadImage(MultipartFile file) throws IOException, ServerException, InsufficientDataException, ErrorResponseException, NoSuchAlgorithmException, InvalidKeyException, InvalidResponseException, XmlParserException, InternalException
{
   File fileData = convertMultiPartToFile(file);
   FileInputStream input = new FileInputStream(fileData);
   String fileName = getUUID();
   PutObjectArgs build = PutObjectArgs.builder()
            .object(fileName + ".jpeg")
            .contentType("image/jpeg")
            .bucket(bucket)
            .stream(input, input.available(), -1).build();
   client.putObject(build);
   return R.ok(path + "/" + bucket + "/" + fileName + ".jpeg");
}
```

## 2.4、查询

通过查询文件查看文件是否存在minio中。

```java
MinioClient minioClient =
   MinioClient.builder()
   .endpoint("http://192.168.249.128:9000")
   .credentials("minioadmin",
                "minioadmin")
   .build();

//查询文件
@Test
public void test_search()
{
   GetObjectArgs getObjectArgs = GetObjectArgs.builder()
      .bucket("testbucket")
      .object("test/dkx.png")
      .build();
   GetObjectResponse object = null;
   FileOutputStream outputStream = null;
   try {
      object = minioClient.getObject(getObjectArgs);
      outputStream = new FileOutputStream("E:\\dkx.png");
      IOUtils.copy(object, outputStream);
   } catch (Exception e) {
      throw new RuntimeException(e);
   }finally {
      try {
         if(object != null)
         {
            object.close();
         }
         if(outputStream != null)
         {
            outputStream.close();
         }
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
   }

}
```

校验文件的完整性，对文件计算出md5值，比较原始文件的md5和目标文件的md5，一致则说明完整

```java
//校验文件的完整性对文件的内容进行md5
FileInputStream fileInputStream1 = new FileInputStream(new File("D:\\develop\\upload\\1.mp4"));
// 获取minio中文件的md5值
String source_md5 = DigestUtils.md5Hex(fileInputStream1);
FileInputStream fileInputStream = new FileInputStream(new File("D:\\develop\\upload\\1a.mp4"));
// 获取本地下载的文件的md5值
String local_md5 = DigestUtils.md5Hex(fileInputStream);
if(source_md5.equals(local_md5)){
    System.out.println("下载成功");
}
```

# 三、SpringBoot整合minio

## 3.1、配置yml

```yml
minio:
  endpoint: http://192.168.249.128:9000 #Minio服务所在地址
  bucketName: testbucket #存储桶名称
  accessKey: minioadmin #访问的key
  secretKey: minioadmin #访问的秘钥
```

编写配置类来加载配置

```java
@Configuration
public class MinioConfig {

    @Value("${minio.endpoint}")
    private String endpoint;
    @Value("${minio.accessKey}")
    private String accessKey;
    @Value("${minio.secretKey}")
    private String secretKey;

    @Bean
    public MinioClient minioClient() {
        MinioClient minioClient =
                MinioClient.builder()
                        .endpoint(endpoint)
                        .credentials(accessKey, secretKey)
                        .build();
        return minioClient;
    }
}
```

编写接口：

```java
public interface MediaFileService {
   /**
     * 上传文件
     * @param companyId 机构id
     * @param uploadFileParamsDto 文件信息
     * @param localFilePath 文件本地路径
     * @return 返回文件信息
     */
    UploadFileResultDto uploadFile(Long companyId, UploadFileParamsDto uploadFileParamsDto, String localFilePath);
}
```

编写接口的实现类：

```java
@Slf4j
@Service
public class MediaFileServiceImpl implements MediaFileService {
   @Autowired
   private MediaFilesMapper mediaFilesMapper;

   @Autowired
   private MinioClient minioClient;

   @Autowired
   private MediaFileService currentProxy;

   @Autowired
   private MediaProcessMapper mediaProcessMapper;

   //存储普通文件
   @Value("${minio.bucket.files}")
   private String bucket_mediafiles;

   //存储视频
   @Value("${minio.bucket.videofiles}")
   private String bucket_video;
   
   @Override
   public UploadFileResultDto uploadFile(Long companyId, UploadFileParamsDto uploadFileParamsDto,
                                         String localFilePath) {
      //文件名
      String filename = uploadFileParamsDto.getFilename();
      //先得到扩展名
      String extension = filename.substring(filename.lastIndexOf("."));

      //得到mimeType
      String mimeType = getMimeType(extension);

      //子目录
      String defaultFolderPath = getDefaultFolderPath();
      //文件的md5值
      String fileMd5 = getFileMd5(new File(localFilePath));
      String objectName = defaultFolderPath+fileMd5+extension;
      //上传文件到minio
      boolean result = addMediaFilesToMinIO(localFilePath, mimeType, bucket_mediafiles, objectName);
      if(!result){
         XueChengPlusException.cast("上传文件失败");
      }
      //将文件信息存入数据库
      MediaFiles mediaFiles = currentProxy.addMediaFilesToDb(companyId, fileMd5, uploadFileParamsDto, bucket_mediafiles, objectName);
      if(mediaFiles==null){
         XueChengPlusException.cast("文件上传后保存信息失败");
      }
      //准备返回的对象
      UploadFileResultDto uploadFileResultDto = new UploadFileResultDto();
      BeanUtils.copyProperties(mediaFiles,uploadFileResultDto);

      return uploadFileResultDto;
   }
}
```

编写controller层：

```java
/**
 * @author Mr.M
 * @version 1.0
 * @description 媒资文件管理接口
 * @date 2022/9/6 11:29
 */
@RestController
public class MediaFilesController {
   @Autowired
   MediaFileService mediaFileService;

   @RequestMapping(value = "upload/coursefile", consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
   public UploadFileResultDto upload(@RequestPart("filedata") MultipartFile file)
   {
      UploadFileParamsDto uploadFileParamsDto = new UploadFileParamsDto();
      // 原始文件名称
      uploadFileParamsDto.setFilename(file.getOriginalFilename());
      // 文件大小
      uploadFileParamsDto.setFileSize(file.getSize());
      // 文件类型
      uploadFileParamsDto.setFileType("001001");
      Long companyId = 1232141425L;
      // 创建一个临时文件
      File file1;
      try {
         file1 = File.createTempFile("minio", "temp");
         file.transferTo(file1);
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
      // 获取文件路径
      String absoluteFile = file1.getAbsolutePath();
      // Long companyId, UploadFileParamsDto uploadFileParamsDto, String localFilePath
      // 上传图片
      return mediaFileService.uploadFile(companyId, uploadFileParamsDto, absoluteFile);
   }
}
```

## 后端实现分片上传文件

### 5.2 断点续传技术

#### 5.2.1 什么是断点续传

通常视频文件都比较大，所以对于媒资系统上传文件的需求要满足大文件的上传要求。http协议本身对上传文件大小没有限制，但是客户的网络环境质量、电脑硬件环境等参差不齐，如果一个大文件快上传完了网断了没有上传完成，需要客户重新上传，用户体验非常差，所以对于大文件上传的要求最基本的是断点续传。

#### 什么是断点续传：

​    引用百度百科：断点续传指的是在下载或上传时，将下载或上传任务（一个文件或一个压缩包）人为的划分为几个部分，每一个部分采用一个线程进行上传或下载，如果碰到网络故障，可以从已经上传或下载的部分开始继续上传下载未完成的部分，而没有必要从头开始上传下载，断点续传可以提高节省操作时间，提高用户体验性。

断点续传流程如下图：

![image-20231102100719107](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021007223.png)

流程如下：

1、前端上传前先把文件分成块

2、一块一块的上传，上传中断后重新上传，已上传的分块则不用再上传

3、各分块上传完成最后在服务端合并文件

编写 文件上传前校验文件的业务逻辑代码：

编写 接口：

```java
/**
 * @author Mr.M
 * @version 1.0
 * @description 媒资文件管理业务类
 * @date 2022/9/10 8:55
 */
public interface MediaFileService {
	/**
     * 检查文件是否存在
     * @param fileMd5 文件md5值
     * @return
     */
    RestResponse<Boolean> checkFile(String fileMd5);
}
```

编写 接口 实现类：

```java
/**
 * @author Mr.M
 * @version 1.0
 * @description TODO
 * @date 2022/9/10 8:58
 */
@Slf4j
@Service
public class MediaFileServiceImpl implements MediaFileService {

   @Autowired
   private MediaFilesMapper mediaFilesMapper;

   @Autowired
   private MinioClient minioClient;

   @Autowired
   private MediaFileService currentProxy;

   @Autowired
   private MediaProcessMapper mediaProcessMapper;

   //存储普通文件
   @Value("${minio.bucket.files}")
   private String bucket_mediafiles;

   //存储视频
   @Value("${minio.bucket.videofiles}")
   private String bucket_video;

   @Override
   public RestResponse<Boolean> checkFile(String fileMd5) {
      //先查询数据库
      MediaFiles mediaFiles = mediaFilesMapper.selectById(fileMd5);
      if(mediaFiles!=null){
         //桶
         String bucket = mediaFiles.getBucket();
         //objectname
         String filePath = mediaFiles.getFilePath();
         //如果数据库存在再查询 minio
         GetObjectArgs getObjectArgs = GetObjectArgs.builder()
            .bucket(bucket)
            .object(filePath)
            .build();
         //查询远程服务获取到一个流对象
         try {
            FilterInputStream inputStream = minioClient.getObject(getObjectArgs);
            if(inputStream!=null){
               //文件已存在
               return RestResponse.success(true);
            }
         } catch (Exception e) {
            e.printStackTrace();
         }

      }

      //文件不存在
      return RestResponse.success(false);
   }
}
```

编写contrller：

```java
/**
 * @author Dkx
 * @version 1.0
 * @2023/10/3120:29
 * @function
 * @comment
 */
@RestController
public class BigFileController {
   @Autowired
   private MediaFileService mediaFileService;

   // 文件上传前检查文件
   @PostMapping("upload/checkfile")
   public RestResponse<Boolean> checkfile(@RequestParam("fileMd5") String fileMd5)
   {
      return mediaFileService.checkFile(fileMd5);
   }
}
```



编写 分块文件上传前的检查 业务逻辑代码：

编写 接口：

```java
/**
 * @author Mr.M
 * @version 1.0
 * @description 媒资文件管理业务类
 * @date 2022/9/10 8:55
 */
public interface MediaFileService {

   /**
     * 检查分块是否存在
     * @param fileMd5 文件md5值
     * @param chunkIndex 分块序号
     * @return
     */
   RestResponse<Boolean> checkChunk(String fileMd5, int chunkIndex);
}
```

编写 接口的 实现类：

```java
/**
 * @author Mr.M
 * @version 1.0
 * @description TODO
 * @date 2022/9/10 8:58
 */
@Slf4j
@Service
public class MediaFileServiceImpl implements MediaFileService {

   @Autowired
   private MediaFilesMapper mediaFilesMapper;

   @Autowired
   private MinioClient minioClient;

   @Autowired
   private MediaFileService currentProxy;

   @Autowired
   private MediaProcessMapper mediaProcessMapper;

   //存储普通文件
   @Value("${minio.bucket.files}")
   private String bucket_mediafiles;

   //存储视频
   @Value("${minio.bucket.videofiles}")
   private String bucket_video;

   @Override
   public RestResponse<Boolean> checkChunk(String fileMd5, int chunkIndex) {

      //根据md5得到分块文件所在目录的路径
      String chunkFileFolderPath = getChunkFileFolderPath(fileMd5);

      //如果数据库存在再查询 minio
      GetObjectArgs getObjectArgs = GetObjectArgs.builder()
         .bucket(bucket_video)
         .object(chunkFileFolderPath+chunkIndex)
         .build();
      //查询远程服务获取到一个流对象
      try {
         FilterInputStream inputStream = minioClient.getObject(getObjectArgs);
         if(inputStream!=null){
            //文件已存在
            return RestResponse.success(true);
         }
      } catch (Exception e) {
         e.printStackTrace();
      }

      //文件不存在
      return RestResponse.success(false);
   }
}
```

编写controller：

```java
/**
 * @author Dkx
 * @version 1.0
 * @2023/10/3120:29
 * @function
 * @comment
 */
@RestController
public class BigFileController {
   @Autowired
   private MediaFileService mediaFileService;

   // 分块文件上传前的检查
   @PostMapping("upload/checkchunk")
   public RestResponse<Boolean> checkchunk(@RequestParam("fileMd5") String fileMd5,
                                           @RequestParam("chunk") int chunk)
   {
      return mediaFileService.checkChunk(fileMd5, chunk);
   }
}
```



编写 文件 上传 业务逻辑代码：

编写 接口：

```java
/**
 * @author Mr.M
 * @version 1.0
 * @description 媒资文件管理业务类
 * @date 2022/9/10 8:55
 */
public interface MediaFileService {

   /**
     * 上传分块
     * @param fileMd5 文件md5值
     * @param chunk 分块序号
     * @param localChunkFilePath 分块文件本地路径
     * @return
     */
   RestResponse uploadChunk(String fileMd5, int chunk,
                            String localChunkFilePath);
}
```

编写 接口  实现类 ：

```java
/**
 * @author Mr.M
 * @version 1.0
 * @description TODO
 * @date 2022/9/10 8:58
 */
@Slf4j
@Service
public class MediaFileServiceImpl implements MediaFileService {

   @Autowired
   private MediaFilesMapper mediaFilesMapper;

   @Autowired
   private MinioClient minioClient;

   @Autowired
   private MediaFileService currentProxy;

   @Autowired
   private MediaProcessMapper mediaProcessMapper;

   //存储普通文件
   @Value("${minio.bucket.files}")
   private String bucket_mediafiles;

   //存储视频
   @Value("${minio.bucket.videofiles}")
   private String bucket_video;

   @Override
   public RestResponse uploadChunk(String fileMd5, int chunk, String localChunkFilePath) {
      //分块文件的路径
      String chunkFilePath = getChunkFileFolderPath(fileMd5) + chunk;
      //获取mimeType
      String mimeType = getMimeType(null);
      //将分块文件上传到minio
      boolean b = addMediaFilesToMinIO(localChunkFilePath, mimeType, bucket_video, chunkFilePath);
      if(!b){
         return RestResponse.validfail(false,"上传分块文件失败");
      }
      //上传成功
      return RestResponse.success(true);
   }
}
```

编写controller：

```java
/**
 * @author Dkx
 * @version 1.0
 * @2023/10/3120:29
 * @function
 * @comment
 */
@RestController
public class BigFileController {
   @Autowired
   private MediaFileService mediaFileService;

   // 上传文件
   @PostMapping("upload/uploadchunk")
   public RestResponse uploadchunk(@RequestParam("file")MultipartFile file,
                                   @RequestParam("fileMd5")String fileMd5,
                                   @RequestParam("chunk")int chunk)
   {
      File localFile = null;
      try {
         localFile = File.createTempFile("minio", "temp");
         file.transferTo(localFile);
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
      String absolutePath = localFile.getAbsolutePath();
      return mediaFileService.uploadChunk(fileMd5, chunk, absolutePath);
   }
}
```

编写 合并文件 业务逻辑代码：

编写 接口：

```java
/**
 * @author Mr.M
 * @version 1.0
 * @description 媒资文件管理业务类
 * @date 2022/9/10 8:55
 */
public interface MediaFileService {
   
   /**
     *  因为要解决事务失效的问题所以对外暴漏接口 来实现代理调用
     * @param companyId
     * @param fileMd5
     * @param uploadFileParamsDto
     * @param bucket
     * @param objectName
     * @return
     */
    MediaFiles addMediaFilesToDb(Long companyId, String fileMd5,
                                 UploadFileParamsDto uploadFileParamsDto,
                                 String bucket, String objectName);

   RestResponse mergechunks(Long companyId, String fileMd5,
                            int chunkTotal,
                            UploadFileParamsDto uploadFileParamsDto);
}
```

编写 接口 实现类：

```java
/**
 * @author Mr.M
 * @version 1.0
 * @description TODO
 * @date 2022/9/10 8:58
 */
@Slf4j
@Service
public class MediaFileServiceImpl implements MediaFileService {

   @Autowired
   private MediaFilesMapper mediaFilesMapper;

   @Autowired
   private MinioClient minioClient;

   @Autowired
   private MediaFileService currentProxy;

   @Autowired
   private MediaProcessMapper mediaProcessMapper;

   //存储普通文件
   @Value("${minio.bucket.files}")
   private String bucket_mediafiles;

   //存储视频
   @Value("${minio.bucket.videofiles}")
   private String bucket_video;

   @Override
   public RestResponse mergechunks(Long companyId, String fileMd5, int chunkTotal, UploadFileParamsDto uploadFileParamsDto) {
      //分块文件所在目录
      String chunkFileFolderPath = getChunkFileFolderPath(fileMd5);
      //找到所有的分块文件
      List<ComposeSource> sources = Stream.iterate(0, i -> ++i).limit(chunkTotal).map(i -> ComposeSource.builder().bucket(bucket_video).object(chunkFileFolderPath + i).build()).collect(Collectors.toList());
      //源文件名称
      String filename = uploadFileParamsDto.getFilename();
      //扩展名
      String extension = filename.substring(filename.lastIndexOf("."));
      //合并后文件的objectname
      String objectName = getFilePathByMd5(fileMd5, extension);
      //指定合并后的objectName等信息
      ComposeObjectArgs composeObjectArgs = ComposeObjectArgs.builder()
         .bucket(bucket_video)
         .object(objectName)//合并后的文件的objectname
         .sources(sources)//指定源文件
         .build();
      //===========合并文件============
      //报错size 1048576 must be greater than 5242880，minio默认的分块文件大小为5M
      try {
         minioClient.composeObject(composeObjectArgs);
      } catch (Exception e) {
         e.printStackTrace();
         log.error("合并文件出错,bucket:{},objectName:{},错误信息:{}",bucket_video,objectName,e.getMessage());
         return RestResponse.validfail(false,"合并文件异常");
      }

      //===========校验合并后的和源文件是否一致，视频上传才成功===========
      //先下载合并后的文件
      File file = downloadFileFromMinIO(bucket_video, objectName);
      try(FileInputStream fileInputStream = new FileInputStream(file)){
         //计算合并后文件的md5
         String mergeFile_md5 = DigestUtils.md5Hex(fileInputStream);
         //比较原始md5和合并后文件的md5
         if(!fileMd5.equals(mergeFile_md5)){
            log.error("校验合并文件md5值不一致,原始文件:{},合并文件:{}",fileMd5,mergeFile_md5);
            return RestResponse.validfail(false,"文件校验失败");
         }
         //文件大小
         uploadFileParamsDto.setFileSize(file.length());
      }catch (Exception e) {
         return RestResponse.validfail(false,"文件校验失败");
      }

      //==============将文件信息入库============
      MediaFiles mediaFiles = currentProxy.addMediaFilesToDb(companyId, fileMd5, uploadFileParamsDto, bucket_video, objectName);
      if(mediaFiles == null){
         return RestResponse.validfail(false,"文件入库失败");
      }
      //==========清理分块文件=========
      clearChunkFiles(chunkFileFolderPath,chunkTotal);

      return RestResponse.success(true);
   }

   /**
     * 清除分块文件
     * @param chunkFileFolderPath 分块文件路径
     * @param chunkTotal 分块文件总数
     */
   private void clearChunkFiles(String chunkFileFolderPath,int chunkTotal){
      Iterable<DeleteObject> objects =  Stream.iterate(0, i -> ++i).limit(chunkTotal).map(i -> new DeleteObject(chunkFileFolderPath+ i)).collect(Collectors.toList());;
      RemoveObjectsArgs removeObjectsArgs = RemoveObjectsArgs.builder().bucket(bucket_video).objects(objects).build();
      Iterable<Result<DeleteError>> results = minioClient.removeObjects(removeObjectsArgs);
      //要想真正删除
      results.forEach(f->{
         try {
            DeleteError deleteError = f.get();
         } catch (Exception e) {
            e.printStackTrace();
         }
      });

   }
   /**
     * 从minio下载文件
     * @param bucket 桶
     * @param objectName 对象名称
     * @return 下载后的文件
     */
   public File downloadFileFromMinIO(String bucket,String objectName){
      //临时文件
      File minioFile = null;
      FileOutputStream outputStream = null;
      try{
         InputStream stream = minioClient.getObject(GetObjectArgs.builder()
                                                    .bucket(bucket)
                                                    .object(objectName)
                                                    .build());
         //创建临时文件
         minioFile=File.createTempFile("minio", ".merge");
         outputStream = new FileOutputStream(minioFile);
         IOUtils.copy(stream,outputStream);
         return minioFile;
      } catch (Exception e) {
         e.printStackTrace();
      }finally {
         if(outputStream!=null){
            try {
               outputStream.close();
            } catch (IOException e) {
               e.printStackTrace();
            }
         }
      }
      return null;
   }

   /**
     * 得到合并后的文件的地址
     * @param fileMd5 文件id即md5值
     * @param fileExt 文件扩展名
     * @return
     */
   private String getFilePathByMd5(String fileMd5,String fileExt){
      return   fileMd5.substring(0,1) + "/" + fileMd5.substring(1,2) + "/" + fileMd5 + "/" +fileMd5 +fileExt;
   }

   //得到分块文件的目录
   private String getChunkFileFolderPath(String fileMd5) {
      return fileMd5.substring(0, 1) + "/" + fileMd5.substring(1, 2) + "/" + fileMd5 + "/" + "chunk" + "/";
   }
}
```

