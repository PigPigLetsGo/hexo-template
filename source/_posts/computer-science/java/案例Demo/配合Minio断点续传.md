---
title: 配合Minio断点续传
categories: 
    - [计算机学科,java,案例Demo]
tags:
    - java
    - 计算机学科
    - 案例Demo
    - 项目
---

# 配合Minio断点续传

FTP (文件传输协议的简称) (File Transfer Protocol，FTP) 客户端软件断点续传指的是在下载或上传时，将下载或上传任务 (一个文件或一个压缩包) 认为的划分为几个部分，每一个部分采用一个线程进行上传或下载，如果碰到网络故障，可以从已经上传或下载的部分开始继续上传下载未完成的部分，而没有必要从头开始上传下载。用户可以节省时间，提高速度。—— 百度百科

**原理图解如下**：

![image-20240303164420177](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303164420177.png)

**大致流程如下**：

1、前端上传前先把文件分成块。

2、一块一块的上传，上传中断后重新上传，已上传的分块则不用再上传。

3、各分块上传完成最后在服务端合并文件。

**分块与合并测试代码**：

文件分块的流程如下：

1、获取源文件长度

2、根据设定的分块的大小计算出块数

3、从源文件读数据依次向每一个块文件写数据

```java
package com.xuecheng.media;

import org.apache.commons.codec.digest.DigestUtils;
import org.apache.commons.io.IOUtils;
import org.junit.jupiter.api.Test;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.RandomAccessFile;
import java.util.*;

/**
 * @author Mr.M
 * @version 1.0
 * @description 大文件处理测试
 * @date 2022/9/13 9:21
 */
public class BigFileTest {
    //测试文件分块方法
    @Test
    public void testChunk() throws IOException {
        File sourceFile = new File("d:/develop/bigfile_test/nacos.mp4");
        String chunkPath = "d:/develop/bigfile_test/chunk/";
        File chunkFolder = new File(chunkPath);
        if (!chunkFolder.exists()) {
            chunkFolder.mkdirs();
        }
        //分块大小
        long chunkSize = 1024 * 1024 * 1;
        //分块数量
        long chunkNum = (long) Math.ceil(sourceFile.length() * 1.0 / chunkSize);
        System.out.println("分块总数："+chunkNum);
        //缓冲区大小
        byte[] b = new byte[1024];
        //使用RandomAccessFile访问文件
        RandomAccessFile raf_read = new RandomAccessFile(sourceFile, "r");
        //分块
        for (int i = 0; i < chunkNum; i++) {
            //创建分块文件
            File file = new File(chunkPath + i);
            if(file.exists()){
                file.delete();
            }
            boolean newFile = file.createNewFile();
            if (newFile) {
                //向分块文件中写数据
                RandomAccessFile raf_write = new RandomAccessFile(file, "rw");
                int len = -1;
                while ((len = raf_read.read(b)) != -1) {
                    raf_write.write(b, 0, len);
                    if (file.length() >= chunkSize) {
                        break;
                    }
                }
                raf_write.close();
                System.out.println("完成分块"+i);
            }

        }
        raf_read.close();
    }
}
```

**文件合并流程**：

1、找到要合并的文件并按文件合并的先后进行排序

2、创建合并文件

3、依次从合并的文件中读取数据向合并文件写入数据

合并文件测试代码：

```java
  	//测试文件合并方法
    @Test
    public void testMerge() throws IOException {
        //块文件目录
        File chunkFolder = new File("d:/develop/bigfile_test/chunk/");
        //原始文件
        File originalFile = new File("d:/develop/bigfile_test/nacos.mp4");
        //合并文件
        File mergeFile = new File("d:/develop/bigfile_test/nacos01.mp4");
        if (mergeFile.exists()) {
            mergeFile.delete();
        }
        //创建新的合并文件
        mergeFile.createNewFile();
        //用于写文件
        RandomAccessFile raf_write = new RandomAccessFile(mergeFile, "rw");
        //指针指向文件顶端
        raf_write.seek(0);
        //缓冲区
        byte[] b = new byte[1024];
        //分块列表
        File[] fileArray = chunkFolder.listFiles();
        // 转成集合，便于排序
        List<File> fileList = Arrays.asList(fileArray);
        // 从小到大排序
        Collections.sort(fileList, new Comparator<File>() {
            @Override
            public int compare(File o1, File o2) {
                return Integer.parseInt(o1.getName()) - Integer.parseInt(o2.getName());
            }
        });
        //合并文件
        for (File chunkFile : fileList) {
            RandomAccessFile raf_read = new RandomAccessFile(chunkFile, "rw");
            int len = -1;
            while ((len = raf_read.read(b)) != -1) {
                raf_write.write(b, 0, len);

            }
            raf_read.close();
        }
        raf_write.close();

        //校验文件
        try (

                FileInputStream fileInputStream = new FileInputStream(originalFile);
                FileInputStream mergeFileStream = new FileInputStream(mergeFile);
        ) {
            //取出原始文件的md5
            String originalMd5 = DigestUtils.md5Hex(fileInputStream);
            //取出合并文件的md5进行比较
            String mergeFileMd5 = DigestUtils.md5Hex(mergeFileStream);
            if (originalMd5.equals(mergeFileMd5)) {
                System.out.println("合并文件成功");
            } else {
                System.out.println("合并文件失败");
            }
        }
    }
```

**视频上传流程**：

![image-20240303164513102](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303164513102.png)

1、前端对文件进行分块。 

2、前端上传分块文件前请求媒资服务检查文件是否存在，如果已经存在则不再上传。 

3、如果分块文件不存在则前端开始上传

 4、前端请求媒资服务上传分块。 

5、媒资服务将分块上传至MinIO。 

6、前端将分块上传完毕请求媒资服务合并分块。

 7、媒资服务判断分块上传完成则请求MinIO合并文件。 

8、合并完成校验合并后的文件是否完整，如果完整则上传完成，否则删除文件。

**minio合并文件测试**：

```java
//将分块文件上传至minio
@Test
public void uploadChunk(){
    String chunkFolderPath = "D:\\develop\\upload\\chunk\\";
    File chunkFolder = new File(chunkFolderPath);
    //分块文件
    File[] files = chunkFolder.listFiles();
    //将分块文件上传至minio
    for (int i = 0; i < files.length; i++) {
        try {
           UploadObjectArgs uploadObjectArgs = UploadObjectArgs.builder().bucket("testbucket").object("chunk/" + i).filename(files[i].getAbsolutePath()).build();
            minioClient.uploadObject(uploadObjectArgs);
            System.out.println("上传分块成功"+i);
        } catch (Exception e) {
          e.printStackTrace();
        }
    }
}
```

**实际项目中编写分块上传视频代码如下**：

上传文件前检查文件是否存在

```java
@PostMapping("upload/checkfile")
public RestResponse<Boolean> checkfile(@RequestParam("fileMd5") String fileMd5)
{
    return mediaFileService.checkFile(fileMd5);
}

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
```

检查分块是否存在

```java
// 分块文件上传前的检查
@PostMapping("upload/checkchunk")
public RestResponse<Boolean> checkchunk(@RequestParam("fileMd5") String fileMd5,
                                    @RequestParam("chunk") int chunk)
{
   return mediaFileService.checkChunk(fileMd5, chunk);
}

@Override
public RestResponse<Boolean> checkChunk(String fileMd5, int chunkIndex) {
    MediaFiles mediaFiles = mediaFilesMapper.selectById(fileMd5);
    if(mediaFiles != null)
    {
        GetObjectArgs getObjectArgs1 = GetObjectArgs.builder()
                .bucket(bucket_video)
                .object(mediaFiles.getFilePath())
                .build();
        try {
            GetObjectResponse object = minioClient.getObject(getObjectArgs1);
            if(object != null)
            {
                log.info("文件已经存在了");
                return RestResponse.validfail(false, "您上传的视频已经存在了");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
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
```

上传文件

```java
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

@Override
public RestResponse uploadChunk(String fileMd5, int chunk, String localChunkFilePath) {
    MediaFiles mediaFiles = mediaFilesMapper.selectById(fileMd5);
    if(mediaFiles != null)
    {
        GetObjectArgs getObjectArgs = GetObjectArgs.builder()
                .bucket(bucket_video)
                .object(mediaFiles.getFilePath())
                .build();
        try {
            GetObjectResponse object = minioClient.getObject(getObjectArgs);
            if(object != null)
            {
                XueChengPlusException.cast("该文件已经存在了");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

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
```

合并文件

```java
// 合并文件
@PostMapping("upload/mergechunks")
public RestResponse mergechunks(@RequestParam("fileMd5")String fileMd5,
                                @RequestParam("fileName")String fileName,
                                @RequestParam("chunkTotal")int chunkTotal)
{
    UploadFileParamsDto uploadFileParamsDto = new UploadFileParamsDto();
    uploadFileParamsDto.setFilename(fileName);
    uploadFileParamsDto.setTags("视频文件");
    uploadFileParamsDto.setFileType("001002");
    return mediaFileService.mergechunks(1232141425L, fileMd5, chunkTotal, uploadFileParamsDto);
}

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
```

