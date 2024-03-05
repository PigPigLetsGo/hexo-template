---
title: 九、分布式搜索
categories:
    - [计算机学科,java,spring,springcloud,Elasticsearch]
tags:
    - 计算机学科
    - springcloud
    - Elasticsearch
---

## 九、分布式搜索:christmas_tree: 

elasticsearch基础

-  初始elasticsearch
-  索引库操作
-  文档操作
-  RestAPI

### 9.1、初始elasticsearch:deciduous_tree: 

-  了解ES
-  倒排索引
-  es的一些概念
-  安装es，kibana

#### 9.1.1、什么是elasticsearch:evergreen_tree: 

elasticsearch是一款非常强大的开源==搜索引擎==，可以帮助我们从==海量数据中快速找到需要的内容==。

elasticsearch结合kibana，Logstash，Beats，也就是elastic stack(ELK)。被广泛应用在日志数据分析，实时监控等领域。

elasticsearch是elastic stack的核心，负责存储，搜索，分析数据。

![image-20231011175401979](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111754105.png)

Elasticsearch的底层实现就是==Lucene==的技术

Lucene是一个Java语言的==搜索引擎类库==，是Apache公司的顶级项目，由Doug Cutting于1999年研发。

官网地址：https://lucene.apache.org/

Lucene的优势：

-  易扩展
-  高性能 (基于倒排索引)

2004年Shay Banon基于Lucene开发了Compass

2010年Shay Banon重写了Compass，取名为Elasticsearch

官网地址：https://www.elastic.co/cn/

目前最新的版本是：7.12.1

相比与lucene，elasticsearch具备下列优势：

-  支持分布式，可水平扩展
-  提供Restful接口，可被任何语言调用

**为什么学习elasticsearch?**。

搜索引擎技术排名：

1、Elasticsearch：开源的分布式搜索引擎

2、Splunk：商业项目

3、Solr：Apache的开源搜索引擎

![image-20231011192322393](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111923929.png)

>  **总结**：
>
>  什么是elasticsearch?
>
>  -  一个开源的==分布式搜索引擎==，可以用来实现搜索，日志统计，分析，系统监控等功能
>
>  什么是elastic stack (LEK)？
>
>  -  是以elasticsearch为核心的技术栈，包括beats，Logstash，kibana，elasticsearch
>
>  什么是Lucene?
>
>  -  是Apache的开源搜索引擎类库，提供了搜索引擎的核心API

### 9.2、正向索引和倒排索引:deciduous_tree: 

传统数据库 (如MySQL) 采用正向索引，例如给下表 (tb_goods) 中的id创建索引：

正向索引：做局部内部检索的时候性能比较差

![image-20231011193327307](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111933421.png)

elasticsearch采用倒排索引：

-  文档 (document)：每条数据就是一个文档
   -  代表的就是数据，每一条数据就是一个文档
-  词条 (term)：文档按照语义分成的词语
   -  如果是中文就按照中文的语义进行分词

倒排索引，会形成一个==新的表==。这张==表里面有两个字段==，一个字段是==词条==另一个是==文档id==。

倒排索引，它在存储时会先把文档中的==内容分成词条去存储==。比方说我拿到了第一条数据那我要对标题创建倒排索引，那么我就来吧标题内容做个分词得到两个词语小米和手机，然后把每个这两个词语存到词条里，然后后面记录它的文档id。==重复的词语不会存入词条而是在已存在的词条后面追加上这个重复的词语的id==.

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111945438.gif)

![image-20231011195333829](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111953084.png)

>  **总结**:
>
>  什么是文档和词条？
>
>  -  每一条数据就是一个文档
>  -  对文档中的内容分词，得到的词语就是词条
>
>  什么是正向索引？
>
>  -  基于文档id创建索引。查询词条时必须先找到文档，而后判断是否包含词条
>
>  什么是倒排索引？
>
>  -  对文档内容分词，对词条创建索引，并记录词条所在文档的信息。查询时先根据词条查询到文档id，而后获取到文档

### 9.3、文档:deciduous_tree: 

elasticsearch是面向文档存储的，可以是数据库中的一条商品数据，一个订单信息。

文档数据会被序列化为json格式后存储在elasticsearch中。

![image-20231011200117293](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112001435.png)

### 9.4、索引 (index):deciduous_tree: 

-  索引 (index)：相同类型的文档的集合
-  映射 (mapping)：索引中文档的字段约束信息，类似表的结构约束

![image-20231011200324370](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112003417.png)

### 9.5、概念对比:deciduous_tree: 

| MySQL  | Elasticsearch | 说明                                                         |
| ------ | ------------- | ------------------------------------------------------------ |
| Table  | Index         | 索引(index)，就是文档的集合，类似数据库的表(table)           |
| Row    | Document      | 文档(document)，就是一条条的数据，类似数据库中的行(Row)，文档都是JSON格式 |
| Column | Field         | 字段(Field)，就是JSON文档中的字段，类似数据库中的列(Column)  |
| Schema | Mapping       | Mapping(映射) 是索引中文档的约束，例如字段类型约束。类似数据库的表结构(Schema) |
| SQL    | DSL           | DSL是elasticsearch提供的JSON风格的请求语句，用来操作elasticsearch，实现CRUD |

### 9.5、架构:deciduous_tree: 

MySQL：擅长事务类型操作，可以确保数据的安全和一致性

Elasticsearch：擅长海量数据的搜索，分析，计算

![image-20231011201341793](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112013859.png)

>  **总结**：
>
>  文档：一条数据就是一个文档，es中是json格式
>
>  字段：json文档中的字段
>
>  索引：同类型文档的集合
>
>  映射：索引中文档的约束，比如字段名称，类型
>
>  elasticsearch与数据库的关系：
>
>  -  数据库负责事务类型操作
>  -  elasticsearch负责海量数据的搜索，分析，计算

### 9.6、安装和部署:deciduous_tree: 

[查看elaticsearch安装和部署](./Elasearch/安装elasticsearch.md)。

### 9.7、分词器:deciduous_tree: 

es在创建倒排索引时需要对文档进行分词；在搜索时，需要对用户输入内容分词。但默认的分词规则对中文处理并不友好。我们在kibana的DevTools中测试：

![image-20231011210532721](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112105308.png)

语法说明：

-  POST：请求方式
-  /_analyze：请求路径，这里省略了http://192.168.150.101:9200，有kibana帮我们补充
-  请求参数，json风格：
   -  analyzer：分词器类型，这里是默认的standard分词器
   -  text：要分词的内容

kibana中测试结果如下：

```
{
  "tokens" : [
    {
      "token" : "黑",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "<IDEOGRAPHIC>",
      "position" : 0
    },
    {
      "token" : "马",
      "start_offset" : 1,
      "end_offset" : 2,
      "type" : "<IDEOGRAPHIC>",
      "position" : 1
    },
    {
      "token" : "程",
      "start_offset" : 2,
      "end_offset" : 3,
      "type" : "<IDEOGRAPHIC>",
      "position" : 2
    },
    {
      "token" : "序",
      "start_offset" : 3,
      "end_offset" : 4,
      "type" : "<IDEOGRAPHIC>",
      "position" : 3
    },
    {
      "token" : "员",
      "start_offset" : 4,
      "end_offset" : 5,
      "type" : "<IDEOGRAPHIC>",
      "position" : 4
    },
    {
      "token" : "学",
      "start_offset" : 5,
      "end_offset" : 6,
      "type" : "<IDEOGRAPHIC>",
      "position" : 5
    },
    {
      "token" : "习",
      "start_offset" : 6,
      "end_offset" : 7,
      "type" : "<IDEOGRAPHIC>",
      "position" : 6
    },
    {
      "token" : "java",
      "start_offset" : 7,
      "end_offset" : 11,
      "type" : "<ALPHANUM>",
      "position" : 7
    },
    {
      "token" : "太",
      "start_offset" : 11,
      "end_offset" : 12,
      "type" : "<IDEOGRAPHIC>",
      "position" : 8
    },
    {
      "token" : "棒",
      "start_offset" : 12,
      "end_offset" : 13,
      "type" : "<IDEOGRAPHIC>",
      "position" : 9
    },
    {
      "token" : "了",
      "start_offset" : 13,
      "end_offset" : 14,
      "type" : "<IDEOGRAPHIC>",
      "position" : 10
    }
  ]
}
```

可以看到它对英文的分词挺好的但是对中文的分词却不好

解决办法：

处理中文分词，一般会使用IK分词器：https://github.com/medcl/elasticsearch-analysis-ik

安装和使用IK分词器，参考文章：[查看IK安装](./Elasearch/安装elasticsearch.md).

### 9.8、索引库操作:deciduous_tree: 

-  mapping映射属性
-  索引库的CRUD

#### 9.8.1、mapping属性:evergreen_tree: 

mapping是对索引库中文档的==约束==，常见的mapping属性包括：

-  type：字段数据类型，常见的简单类型有
   -  字符串：text (可分词的文本)，keyword (精确值，例如：品牌，国家，ip地址)
   -  数值：long，integer，short，byte，double，float
   -  布尔：boolean
   -  日期：date
   -  对象：object
-  在elasticsearch中是没有数组这种类型的，但是它允许某一个字段有多个值
-  index：是否创建倒排索引，默认为true (全创建倒排索引)
-  analyzer：使用哪种分词器 (结合text使用除了text以外其它都不需要用)
   -  值：分词器名称：ik_smart或者ik_max_word
-  properties：该字段的子字段
   -  name有两个子字段可以使用properties来指定子字段了

![image-20231012090710123](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310120907321.png)

>  **总结**：
>
>  mapping常见属性有哪些？
>
>  -  type：数据类型
>  -  index：是否索引
>  -  analyzer：分词器
>  -  properties：子字段
>
>  type常见的有哪些？
>
>  -  字符串：text，keyword
>  -  数字：long，integer，short，byte，double，float
>  -  布尔：boolean
>  -  日期：date
>  -  对象object

#### 9.8.2、创建索引库:evergreen_tree: 

ES中通过Restful请求操作索引库，文档。请求内容用DSL语句来表示。创建索引库和mapping的DSL语法如下：

PUT 添加索引

info 用户信息内容很长所以是可分词的text，分词器是ik_smart。email 邮箱 没有分词的必要，因为它作为一个整体才有意义所以使用keyword不进行分词，因此不进行分词也就不需要analyzer了，使用index=false表示这个字段不参与创建倒排索引。name 名称，带有嵌套，它有指定的properties，里面还有一个firstName。

![image-20231012091855829](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310120918101.png)

```json
PUT /dkx
{
  "mappings": {
    "properties": {
      "info": {
        "type": "text",
        "analyzer": "ik_smart"
      },
      "email": {
        "type": "keyword",
        "index": false
      },
      "name": {
        "type": "object",
        "properties": {
          "firstName": {
            "type": "keyword"
          },
          "lastName": {
            "type": "keyword"
          }
        }
      }
    }
  }
}
```

运行结果：

```json
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "dkx"
}
```

创建成功！

#### 9.8.3、查看，删除索引库:evergreen_tree: 

查看索引库语法：

```
GET /索引库名
```

示例：

```
GET /dkx
```

删除索引库的语法：

```
DELETE /索引库名
```

示例：

```
DELETE /dkx
```

#### 9.8.4、修改索引库:evergreen_tree: 

事实上在ES中索引库是不允许修改的

因为索引库创建完了以后它的数据结构也就是mapping映射都已经定义好了。ES会基于这些mapping去创建倒排索引，如果去修改某一个字段就会导致整体倒排索引彻底失效，这样带来的影响是非常大的。

所以在ES中是禁止修改索引库的！

索引库和mapping一旦创建无法修改，但是可以添加新的字段，语法如下：

```json
PUT /索引库名/_mapping
{
   "properties": {
      // 这里必须是新的字段名不能和存在的重复否则它以为你要修改就报错
      "新字段名": {
         "type": "integer"
      }
   }
}
```

示例：

```json
PUT /dkx/_mapping
{
   "properties": {
      "age": {
         type: "integer"
      }
   }
}
```

案例演示图：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121021571.gif)

>  **总结**：
>
>  索引库操作有哪些？
>
>  -  创建索引库：PUT /索引库名
>  -  查询索引库：GET/索引库名
>  -  删除索引库： DELETE /索引库名
>  -  添加字段：PUT /索引库名/_mapping

#### 9.8.5、文档操作:evergreen_tree: 

-  新增文档
-  查询文档
-  删除文档
-  修改文档

##### 9.8.5.1、添加文档:palm_tree: 

新增文档的DSL语法如下：

```json
POST /索引库名/_doc/文档id
{
   "字段1": "值1",
   "字段2": "值2",
   "字段3": {
      "子属性1": "值3",
      "子属性2": "值4"
   },
   // ...
}
```

![image-20231012102808294](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121028570.png)

##### 9.8.5.2、查看，删除文档:palm_tree: 

查看文档语法：

```
GET /索引库名/_doc/文档id
```

示例：

```
GET /dkx/_doc/1
```

删除文档的语法：

```
DELETE /索引库名/_doc/文档id
```

示例：

```
DELETE /dkx/_doc/1
```

##### 9.8.5.3、修改文档:palm_tree: 

方式一：全量修改，会删除旧文档，添加新文档，如果没有旧文档就是新增

```json
PUT /索引库名/_doc/文档id
{
   "字段1": "值1",
   "字段2": "值2",
   // ... 略
}
```

![image-20231012103906432](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121039009.png)

方式二：增量修改，修改指定字段值

```json
POST /索引库名/_update/文档id
{
   "doc": {
      "字段名": "新的值"
   }
}
```

![image-20231012104219010](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121042126.png)

>  **总结**：
>
>  文档操作有哪些？
>
>  -  创建文档：POST /索引库名/_cod/文档id {json文档}
>  -  查询文档：GET /索引库名/_doc/文档id
>  -  删除文档：DELETE/索引库名/_doc/文档id
>  -  修改文档：
>     -  全量修改：PUT/索引库名/_doc/文档id {json文档}
>     -  增量修改：POST/索引库名/_update/文档id {“doc”: {字段}}

#### 9.8.6、RestClient操作索引库:evergreen_tree: 

-  创建索引库
-  删除索引库
-  判断索引库是否存在

##### 9.8.6.1、什么是RestClient:palm_tree: 

ES官方提供了各种不同语言的客户端，用来操作ES。这些客户端的本质就是组装DSL语句，通过http请求发送给ES。官方文档地址：https://www.elastic.co/guide/en/elasticsearch/client/index.html

##### 9.8.6.2、案例，利用JavaRestClient实现创建，删除索引库，判断索引库是否存在:palm_tree: 

根据sql文件数据创建索引库[sql文件](./Elasearch/案例sql.md).索引库名为hotel，mapping属性根据数据库结构定义。

基本步骤如下：

1、idea中导入hotel-demo项目，项目地址：https://gitee.com/doukaixin/typora.git

![image-20231012112156928](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121121088.png)

2、分析数据结构，定义mapping属性

mapping要考虑的问题：

字段名，数据类型，是否参与搜索，是否分词，如果分词，分词器是什么？

其中，字段名和数据类型可以基于表结构一目了然，是否参与搜索与是否分词就比较特殊了，它们与业务强相关的。比如说我们当前的酒店业务，酒店名称就一定是要参与搜索功能的它也要分词因为内容比较长，而分词器我们肯定会使用ik_max_word尽可能多的分词搜索范围大一些。

![image-20231012112427548](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121124067.png)

根据上述分析去创建对应的索引库

>  小提示：
>
>  ES中支持两种地理坐标数据类型：
>
>  -  geo_point：由纬度(latitude)和径度(longitude)确定的一个点。例如：
>
>     “32.82342343，120.3153263”
>
>  -  geo_shape：有多个geo_point组成的复杂几何图形。例如一条直线，
>
>     “LINESTRING(-77.12312312，-77.1235343 38.2323234)”

```json
PUT /dkx
{
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword"
      },
      "name": {
        "type": "text",
        "analyzer": "ik_max_word"
      },
      "address": {
        "type": "keyword",
        "index": false
      },
      "price": {
        "type": "integer"
      },
      "score": {
        "type": "integer"
      },
      "brand": {
        "type": "keyword"
      },
      "city": {
        "type": "keyword"
      },
      "starName": {
        "type": "keyword"
      },
      "business": {
        "type": "keyword"
      },
       # 酒店在地图上用一个点来表示所以使用geo_point，而上图的sql中酒店是两个字段一个经度一个纬度，而在ES中一个geo_point代表了经纬度，所以使用一个字段来包裹geo_point
      "location": {
        "type": "geo_point"
      },
      "pic": {
        "type": "keyword",
        "index": false
      }
    }
  }
}
```

虽然定义完了酒店的所有字段了但是有一个小的问题。那我们的name，brand，businne等字段这些都要参与搜索，那这些将来都要参与搜索也就意味着将来用户输入关键字我可能要根据多个字段搜索，也就是查询条件不是一个值而是多个值。那根据一个字段搜效率高，还是根据多个字段搜效率高呢？显然是一个字段搜。但是需求是希望用户输入名称能搜到，输入品牌也能搜到，输入商圈也能搜到，多个搜索条件而我又想性能好该怎么办？

ES提供了一个功能可以解决这个问题，如下小提示

>  **小提示**：
>
>  字段拷贝可以使用copy_to属性将当前字段拷贝到指定字段。示例：
>
>  ```json
>  "all": {
>     "type": "text",
>     "analyzer": "ik_max_word"
>  },
>  {
>     "brand": {
>        "type": "keyword",
>        "copy_to": "all"
>     }
>  }
>  ```

完整的索引库如下：

```json
PUT /dkx
{
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword"
      },
      "name": {
        "type": "text",
        "analyzer": "ik_max_word",
        "copy_to": "all"
      },
      "address": {
        "type": "keyword",
        "index": false
      },
      "price": {
        "type": "integer"
      },
      "score": {
        "type": "integer"
      },
      "brand": {
        "type": "keyword",
        "copy_to": "all"
      },
      "city": {
        "type": "keyword"
      },
      "starName": {
        "type": "keyword"
      },
      "business": {
        "type": "keyword",
        "copy_to": "all"
      },
      "location": {
        "type": "geo_point"
      },
      "pic": {
        "type": "keyword",
        "index": false
      },
      "all": {
        "type": "text",
        "analyzer": "ik_max_word"
      }
    }
  }
}
```

3、初始化JavaRestClient

3.1、引入es的RestHighLevelClient依赖：

```xml
<dependency>
   <groupId>org.elasticsearch.client</groupId>
   <artifactId>elasticsearch-rest-high-level-client</artifactId>
   <!--在properties中声明了版本在这里可以不加版本-->
</dependency>
```

3.2、因为SpringBoot默认的ES版本是7.6.2、所以我们需要覆盖默认的ES版本：

```xml
<properties>
   <java.version>1.8</java.version>
   <!--使用elasticsearch必须在properties中强制声明版本号-->
   <!--统一控制elasticsearch版本号，是因为依赖内部的版本不统一-->
   <elasticsearch.version>7.12.1</elasticsearch.version>
</properties>
```

3.3、初始化RestHighLevelClient：

```java
@BeforeEach
void setup()
{
    this.client = new RestHighLevelClient(RestClient.builder(
    HttpHost.create("http:/192.168.249.128:9200")
    /*
    微服务集群可以写多个
    HttpHost.create("http:/192.168.249.128:9200"),
    HttpHost.create("http:/192.168.249.128:9200"),
    HttpHost.create("http:/192.168.249.128:9200")
    */
    ));
}
```

4、利用JavaRestClient创建索引库

需要注意：dkx这个索引库已经创建过了再创建就会报错如下：

需要先将其删除后再进行测试

```
[dkx/CGJFBdUgTFCV46PbgurYiA] ElasticsearchStatusException[Elasticsearch exception [type=resource_already_exists_exception, reason=index [dkx/CGJFBdUgTFCV46PbgurYiA] already exists]
]
```

![image-20231012142539675](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121425282.png)

```java
@Test
void createHotelIndex()
{
   // 1. 创建Request对象
   CreateIndexRequest request = new CreateIndexRequest("dkx");
   // 2. 准备请求的参数：DSL语句
   String msg;
   try {
      // 通过类路径来获取hote.json配置文件的绝对路径
      String path = Thread.currentThread().getContextClassLoader().getResource("hote.json").getPath();
      // 如果获取绝对路径中含有中文可能会乱码所以需要进行编码处理
      path = URLDecoder.decode(path, "utf-8");
      // 通过字符输入流来读取指定位置的json文件内容
      FileReader reader = new FileReader(path);
      msg = "";
      int i = 0;
      while((i = reader.read()) != -1)
      {
         // 将读取到的内容拼接到一个字符串中
         msg += (char)i;
      }
      // msg 是读取的json文件里面是创建索引库的DSL语句
      request.source(msg, XContentType.JSON);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   // 3. 发送请求
   try {
      client.indices().create(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
}
```

hote.json

```json
{
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword"
      },
      "name": {
        "type": "text",
        "analyzer": "ik_max_word",
        "copy_to": "all"
      },
      "address": {
        "type": "keyword",
        "index": false
      },
      "price": {
        "type": "integer"
      },
      "score": {
        "type": "integer"
      },
      "brand": {
        "type": "keyword",
        "copy_to": "all"
      },
      "city": {
        "type": "keyword"
      },
      "starName": {
        "type": "keyword"
      },
      "business": {
        "type": "keyword",
        "copy_to": "all"
      },
      "location": {
        "type": "geo_point"
      },
      "pic": {
        "type": "keyword",
        "index": false
      },
      "all": {
        "type": "text",
        "analyzer": "ik_max_word"
      }
    }
  }
}
```

在kibana中执行GET请求，请求/dkx索引库查看是否存在

![image-20231012153324541](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121533685.png)

5、利用JavaRestClient删除索引库

```java
@Test
public void deleteHoteIndex()
{
   // 1. 创建Request对象
   DeleteIndexRequest request = new DeleteIndexRequest("dkx");
   try {
      // 2.发送请求
      client.indices().delete(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
}
```



6、利用JavaRestClient判断索引库是否存在

```java
@Test
public void exitHoteIndex()
{
   // 1. 创建Request请求
   GetIndexRequest request = new GetIndexRequest("dkx");
   try {
      // 2.发送请求
      boolean exists = client.indices().exists(request, RequestOptions.DEFAULT);
      System.out.println(exists ? "存在" : "不存在");
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
}
```

>  **总结**:
>
>  索引库操作的基本步骤：
>
>  -  初始化RestHighLevelClient
>  -  创建XxxIndexRequest。Xxx是CREATE，Get，Delete
>  -  准备DSL (CREATE时需要)
>  -  发送请求。调用RestHighLevelClient#indices().xxx()方法，xxx是create，exists，delete

#### 9.8.7、RestClient操作文档:evergreen_tree: 

-  新增文档
-  查询文档
-  删除文档
-  修改文档
-  批量导入文档

##### 9.8.7.1、案例，利用JavaRestClient实现文档的CRUD:palm_tree: 

去数据库查询酒店数据，导入到dkx索引库，实现酒店数据的CRUD

**基本步骤如下**：

1、初始化JavaRestClient

新建一个测试类，实现文档相关操作，并且完成JavaRestClient的初始化

```java
@SpringBootTest
public class HoteIndexTest1 {
    private RestHighLevelClient client;
   
 	 @BeforeEach
    void setup()
    {
        this.client = new RestHighLevelClient(RestClient.builder(
                HttpHost.create("http://192.168.249.128:9200")
        ));
    }
    @AfterEach
    void tearDown()
    {
        try {
            this.client.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```



2、利用JavaRestClient新增酒店数据

先通过mysql查询酒店数据，然后给这条数据创建倒排索引，即可完成添加：

```java
@Autowired
private IHotelService iHotelService;

@Test
public void testAddDocument()
{
   // 根据id查询酒店数据
   Hotel byId = iHotelService.getById(36934L);
   // 转换为文档类型
   HotelDoc hotelDoc = new HotelDoc(byId);
   // 1.准备Request对象
   IndexRequest request = new IndexRequest("dkx").id(hotelDoc.getId().toString());
   // 2.准备JSON文档
   request.source(JSON.toJSONString(hotelDoc), XContentType.JSON);
   // 3.发送请求
   try {
      client.index(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
}
```

![image-20231012155720876](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121557188.png)

到kibana中通过命令：GET /dkx/_doc/36934查看是否成功添加了

![image-20231012163857506](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121638859.png)

3、利用JavaRestClient根据id查询酒店数据

根据id查询到的文档数据是json，需要反序列化为java对象：

```java
@Test
public void testGetDocumentById()
{
   // 1.准备Request
   GetRequest request = new GetRequest("dkx", "36934");
   // 2.发送请求，得到响应
   GetResponse documentFields;
   try {
      documentFields = client.get(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   // 3.解析响应结果
   String json = documentFields.getSourceAsString();
   // 通过JSON.parseObject将json数据解析为指定的对象
   HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
   System.out.println(hotelDoc);
}
//---------------------打印结果---------------------
HotelDoc(id=36934, name=7天连锁酒店(上海宝山路地铁站店), address=静安交通路40号, price=336, score=37, brand=7天酒店, city=上海, starName=二钻, business=四川北路商业区, location=31.251433, 121.47522, pic=https://m.tuniucdn.com/fb2/t1/G1/M00/3E/40/Cii9EVkyLrKIXo1vAAHgrxo_pUcAALcKQLD688AAeDH564_w200_h200_c1_t0.jpg)
```

![image-20231012162936320](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121629666.png)

4、利用JavaRestClient删除酒店数据

```java
@Test
public void testDeleteDocument()
{
   // 1.准备Request
   DeleteRequest request = new DeleteRequest("dkx", "36934");
   // 2.发送请求
   try {
      client.delete(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
}
```

![image-20231012165946036](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121659241.png)

如果删除后再去通过java代码去查询返回结果就是null

5、利用JavaRestClient修改酒店数据

修改文档数据有两种方式：

方式一：全量更新。再次写入id一样的文档，就会删除旧文档，添加新文档，它的java代码和添加没有区别

方式二：局部更新。只更新部分字段，我们演示方式二

```java
@Test
public void testUpdateDocument()
{
   // 1.准备Request
   UpdateRequest request = new UpdateRequest("dkx", "36934");
   // 2.准备请求参数
   request.doc(
      // 使用 逗号隔开 两个一对
      "price", "952",
      "starName", "五钻"
   );
   // 3.发送请求
   try {
      client.update(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
}
```

![image-20231012164318114](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121643267.png)

修改前通过id获取数据

```
HotelDoc(id=36934, name=7天连锁酒店(上海宝山路地铁站店), address=静安交通路40号, price=336, score=37, brand=7天酒店, city=上海, starName=二钻, business=四川北路商业区, location=31.251433, 121.47522, pic=https://m.tuniucdn.com/fb2/t1/G1/M00/3E/40/Cii9EVkyLrKIXo1vAAHgrxo_pUcAALcKQLD688AAeDH564_w200_h200_c1_t0.jpg)
```

修改后通过id获取数据

```
HotelDoc(id=36934, name=7天连锁酒店(上海宝山路地铁站店), address=静安交通路40号, price=952, score=37, brand=7天酒店, city=上海, starName=五钻, business=四川北路商业区, location=31.251433, 121.47522, pic=https://m.tuniucdn.com/fb2/t1/G1/M00/3E/40/Cii9EVkyLrKIXo1vAAHgrxo_pUcAALcKQLD688AAeDH564_w200_h200_c1_t0.jpg)
```

>  **总结**：
>
>  文档操作的基本步骤：
>
>  -  初始化RestHighLevelClient
>  -  创建XxxRequest。XXX是Index，Get，Update，Delete
>  -  准备参数 (Index和Update时需要)
>  -  发送请求。调用RestHighLevelClient#.xxx()方法，xxx是Index，Get，Update，Delete
>  -  解析结果 (Get时需要)

#### 9.8.8、案例，利用JavaRestClient批量导入酒店数据到ES:evergreen_tree: 

需求：批量查询酒店数据，然后批量导入索引库中

思路：

1、利用mybatis-plus查询酒店数据

2、将查询到的酒店数据 (Hotel) 转换为 文档类型数据 (HotelDoc)

3、利用JavaRestClient中的Bulk批处理，实现批量新增文档，示例代码如下 

```java
@SpringBootTest
public class HoteIndexTest1 {
    private RestHighLevelClient client;

    @Autowired
    private IHotelService iHotelService;  
	 @Test
    public void testBulkRequest()
    {
        // 批量查询酒店数据
        List<Hotel> hotels = iHotelService.list();
        // 1.创建Request
        BulkRequest request = new BulkRequest();
        // 2.准备参数，添加多个新增的Request
        // 2.1 对查询出的酒店数据进行遍历
        for(Hotel hotel: hotels)
        {
            // 转换为文档类型HotelDoc
            HotelDoc hotelDoc = new HotelDoc(hotel);
            // 创建新增文档的Request对象
            request.add(new IndexRequest("dkx")
                    .id(hotel.getId().toString())
                    .source(JSON.toJSONString(hotelDoc), XContentType.JSON));
        }
        // 3.发送请求
        try {
            client.bulk(request, RequestOptions.DEFAULT);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }   
	@BeforeEach
    void setup()
    {
        this.client = new RestHighLevelClient(RestClient.builder(
                HttpHost.create("http://192.168.249.128:9200")
        ));
    }
    @AfterEach
    void tearDown()
    {
        try {
            this.client.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

到kibana页面通过命令：GET /dkx/_search 查看结果

可以看到查询出了201条数据

![image-20231012172343307](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121723744.png)

### 9.9、DSL查询文档:deciduous_tree: 

-  DSL查询分类
-  全文检索查询
-  精准查询
-  地理坐标查询
-  组合查询

#### 9.9.1、DSL Query的分类:evergreen_tree: 

Elasticsearch提供了基于JSON的DSL (Domain Specific Language) 来定义查询。常见的查询类型包括：

-  查询所有：查询出所有数据，一般测试用。例如：match_all
-  全文检索 (full text) 查询：利用分词器对用户输入内容分词，然后去倒排索引库中匹配。例如：
   -  match_query
   -  multi_match_query
-  精确查询：根据精确词条值查找数据，一般是查找keyword，数值，日期，boolean等类型字段。例如：
   -  ids
   -  range
   -  term
-  地理 (geo) 查询：根据经纬度查询。例如：
   -  geo_distance
   -  geo_bounding_box
-  复合 (compound) 查询：复合查询可以将上述各种查询条件组合起来，合并查询条件。例如：
   -  bool
   -  function_score

##### 9.9.1.1、DSL Query基本语法:palm_tree: 

查询的基本语法如下：

```json
GET /indexName/_search
{
   "query": {
      "查询类型": {
         "查询条件": "条件值"
      }
   }
}
```



![image-20231012173803886](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121738538.png)

>  总结：
>
>  查询DSL的基本语法是什么？
>
>  -  GET /索引库名/_search
>  -  {“query”: {“查询类型”:{“FIELD”:”TEXT”}}}

#### 9.9.2、全文检索查询:evergreen_tree: 

match查询：全文检索查询的一种，会对用户输入内容分词，然后去倒排索引检索，语法：

```json
GET /indexName/_search
{
   "query": {
      "match": {
         "FIELD": "TEXT"
      }
   }
}
```

![image-20231012180230588](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121802836.png)

multi_match：与match查询类似，只不过允许同时查询多个字段，语法：

```json
GET /indexName/_search
{
   "query": {
      "multi_match": {
         "query": "TEXT",
         "fields": ["FIELD1", "FIELD12"]
      }
   }
}
```

![image-20231012193303357](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121933446.png)

>  **总结**：
>
>  match和muliti_match的区别是什么？
>
>  -  match：根据一个字段查询
>  -  multi_match：根据多个字段查询，参与查询字段越多，查询性能越差

#### 9.9.3、精确查询:evergreen_tree: 

精确查询一般是查找keyword，数值，日期，boolean等类型字段。所以==不会==对搜索条件分词。常见的有：

-  term：根据词条精确值查询
-  range：根据值的范围查询

##### 9.9.3.1、精确查询-语法:palm_tree: 

精确查询一般是根据id，数值，keyword类型，或布尔字段来查询。语法如下：

term查询：     

```json    
// term查询
GET /indexName/_search
{
   "query": {
      "term": {
         "FIELD": {
            "value": "VLAUE"
         }
      }
   }
}
```

![image-20231012195904980](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310121959434.png)

range查询：

```json
// range查询
GET /indexName/_search
{
   "query": {
      "range": {
         "FIELD": {
           "gte": 10,
           "lte": 20
         }
      }
   }
}
```

![image-20231012200031386](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310122000353.png)

>  **总结**：
>
>  精确查询常见的有哪些？
>
>  -  term查询：根据词条精确匹配，一般搜索keyword类型，数值类型，布尔类型，日期类型字段
>  -  range查询：根据数值范围查询，可以是数值，日期的范围

#### 9.9.4、地理查询:evergreen_tree: 

根据经纬度查询。常见的使用场景包括：

-  携程：搜索我附近的酒店
-  滴滴：搜索我附近的出租车
-  微信：搜索我附近的人

根据经纬度查询，例如：

-  geo_bounding_box：查询geo_point值落在某个矩形范围的所有文档

```json
// geo_bounding_box查询
GET /indexName/_search
{
   "query": {
      "geo_bounding_box": {
         "FIELD": {
            "top_left": {
               "lat": 31.1,
           		"lon": 121.5
            },
            "bottom_right": {
               "lat": 30.9,
               "lon": 121.7
            }
         }
      }
   }
}
```

在搜索的时候根据“top_left” 和 “bottom_right” 两个点分别画一个横线和竖线 的 经纬度进行相交形成的一个矩形范围

![image-20231012202007403](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310122020535.png)

根据经纬度查询，例如：

-  geo_distance：查询到指定中心点小于某个距离值的所有文档

```json
// geo_distance 查询
GET /indexName/_search
{
   "query": {
      "geo_distance": {
         "distance": "15km",
         "FIELD": "31.21,121.5"
      }
   }
}
```

![image-20231013102411621](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310131024931.png)

![image-20231012202733050](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310122027316.png)

#### 9.9.5、复合查询:evergreen_tree: 

复合 (compound) 查询：复合查询可以将其它简单查询组合起来，实现更复杂的搜索逻辑，例如：

-  fuction score：算分函数查询，可以控制文档相关性算分，控制文档排名。例如百度竞价

![image-20231013080014517](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310130800939.png)

例如，我们搜索  “虹桥如家” ，结果如下：

这个词在第一条文档中出现了一次，而这个文档中词条的总数是：5

所以词条频率就是 1 / 5 = 0.2 分。词条出现的频率越多得分越高

证明你跟这个词相关性也越高

但是有个问题：

如家这个词在这几篇文档中都有出现，那再去吧这个 “如家” 进行累加还有意义吗？。没有意义

![image-20231013080054602](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310130801156.png)

所以我们就引入一个新的算法：TFIDF 算法

![image-20231013080723131](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310130807792.png)

我们以如家为例，那么包含如家的文档有三个，而文档总数也是三那 3 /3 = 1 那 Log/1 = 0 。那代表如家这个权重就是 0 。那相反虹桥这个词支出现在一个文档中，所以分母就是 1 ，而文档总数是 3 所以结果就是 3

![image-20231013081248428](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310130812773.png)

这个算法也是去做累加只不过它在做tf和idf的时候算法上面会更加复杂一点，为什么要演变一种新的算法呢？

是因为这种算法它不会受词频影响较大

在传统的tf算法中词频越高，将来得分会无限增加。会越来越高，但是BM25算法呢，最终的分会趋于一个水平，也就是它不会无限增长。所以相对来讲这种算法会更好一点得分会更平和一点。因此在ES高版本中都会采用默认都会采用BM25算法

![image-20231013081755494](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310130817842.png)

>  **总结**：
>
>  elasticsearch中的相关性打分算法是什么？
>
>  -  TF-IDF：在elasticsearch5.0之前，会随着词频增加而越来越大
>  -  BM25：在elasticsearch5.0之后，会随着词频增加而增大，但增长曲线会趋于水平

##### 9.9.6.1、Function Score Query:palm_tree: 

使用fucntion score query ，可以修改文档的相关性算分 (query score) ，根据新得到的算分排序。

>  我们知道ES在搜索文档的时候会对文档做相关性的一个打分，文档与搜索关键字的相关度越高那么打分就自然就越高。排名也会越靠前。不过有些时候我们希望人为的去控制文档的排名，比如说有部分文档人家掏钱了我就让它排名靠前一点，算分高一点那么这个时候就要用到Function Score Query了。这个查询可以在原始的相关性算法的基础上加以修改得到一个想要的算分从而去影响文档的排名

![image-20231013093343922](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310130933092.png)

###### 9.9.6.1.1、案例，给 “如家” 这个品牌的酒店排名靠前一些:tanabata_tree: 

把这个问题翻译一下，function score需要的三要素：

1、哪些文档需要算分加权？

-  品牌为如家的酒店

2、算分函数是什么？

-  weight就可以

3、加权模式是什么？

-  求和

先正常的进行查询查看结果

```json
GET /dkx/_search
{
  "query": {
    "function_score": {
      "query": {
        "match": {
          "all": "外滩"
        }
      }
    }
  }
}
```

结果

```json
{
  "took" : 61,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 3,
      "relation" : "eq"
    },
    "max_score" : 6.024319,
    "hits" : [
      {
        "_index" : "dkx",
        "_type" : "_doc",
        "_id" : "60487",
        "_score" : 6.024319,
        "_source" : {
          "address" : "黄浦路199号",
          "brand" : "君悦",
          "business" : "外滩地区",
          "city" : "上海",
          "id" : 60487,
          "latitude" : "31.245409",
          "longitude" : "121.492969",
          "name" : "上海外滩茂悦大酒店",
          "pic" : "https://m.tuniucdn.com/fb3/s1/2n9c/2Swp2h1fdj9zCUKsk63BQvVgKLTo_w200_h200_c1_t0.jpg",
          "price" : 689,
          "score" : 44,
          "starName" : "五星级"
        }
      },
      {
        "_index" : "dkx",
        "_type" : "_doc",
        "_id" : "432335",
        "_score" : 4.849986,
        "_source" : {
          "address" : "唐山路145号",
          "brand" : "7天酒店",
          "business" : "北外滩地区",
          "city" : "上海",
          "id" : 432335,
          "latitude" : "31.252585",
          "longitude" : "121.498753",
          "name" : "7天连锁酒店(上海北外滩国际客运中心地铁站店)",
          "pic" : "https://m2.tuniucdn.com/filebroker/cdn/res/c1/ba/c1baf64418437c56617f89840c6411ef_w200_h200_c1_t0.jpg",
          "price" : 249,
          "score" : 35,
          "starName" : "二钻"
        }
      },
      {
        "_index" : "dkx",
        "_type" : "_doc",
        "_id" : "434082",
        "_score" : 3.8909411,
        "_source" : {
          "address" : "复兴东路260号",
          "brand" : "如家",
          "business" : "豫园地区",
          "city" : "上海",
          "id" : 434082,
          "latitude" : "31.220706",
          "longitude" : "121.498769",
          "name" : "如家酒店·neo(上海外滩城隍庙小南门地铁站店)",
          "pic" : "https://m.tuniucdn.com/fb2/t1/G6/M00/52/B6/Cii-U13eXLGIdHFzAAIG-5cEwDEAAGRfQNNIV0AAgcT627_w200_h200_c1_t0.jpg",
          "price" : 392,
          "score" : 44,
          "starName" : "二钻"
        }
      }
    ]
  }
}
```

可以看到 “如家酒店” 排在最后一个我们通过下面的代码让其排名为第一

```json
GET /dkx/_search
{
  "query": {
    "function_score": {
      "query": {
        "match": {
          "all": "外滩"
        }
      },
      "functions": [ // 算分函数
        {
          "filter": { // 满足的条件，品牌必须是如家
            "term": {
              "brand": "如家"
            }
          },
          "weight": 10 // 算分权重为10
        }
      ],
      "boost_mode": "sum" // 算分模式使用fucntion_score与query_score相除的方式算分
    }
  }
}
```

结果：

```json
{
  "took" : 72,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 3,
      "relation" : "eq"
    },
    "max_score" : 13.890942,
    "hits" : [
      {
        "_index" : "dkx",
        "_type" : "_doc",
        "_id" : "434082",
        "_score" : 13.890942,
        "_source" : {
          "address" : "复兴东路260号",
          "brand" : "如家",
          "business" : "豫园地区",
          "city" : "上海",
          "id" : 434082,
          "latitude" : "31.220706",
          "longitude" : "121.498769",
          "name" : "如家酒店·neo(上海外滩城隍庙小南门地铁站店)",
          "pic" : "https://m.tuniucdn.com/fb2/t1/G6/M00/52/B6/Cii-U13eXLGIdHFzAAIG-5cEwDEAAGRfQNNIV0AAgcT627_w200_h200_c1_t0.jpg",
          "price" : 392,
          "score" : 44,
          "starName" : "二钻"
        }
      },
      {
        "_index" : "dkx",
        "_type" : "_doc",
        "_id" : "60487",
        "_score" : 7.024319,
        "_source" : {
          "address" : "黄浦路199号",
          "brand" : "君悦",
          "business" : "外滩地区",
          "city" : "上海",
          "id" : 60487,
          "latitude" : "31.245409",
          "longitude" : "121.492969",
          "name" : "上海外滩茂悦大酒店",
          "pic" : "https://m.tuniucdn.com/fb3/s1/2n9c/2Swp2h1fdj9zCUKsk63BQvVgKLTo_w200_h200_c1_t0.jpg",
          "price" : 689,
          "score" : 44,
          "starName" : "五星级"
        }
      },
      {
        "_index" : "dkx",
        "_type" : "_doc",
        "_id" : "432335",
        "_score" : 5.849986,
        "_source" : {
          "address" : "唐山路145号",
          "brand" : "7天酒店",
          "business" : "北外滩地区",
          "city" : "上海",
          "id" : 432335,
          "latitude" : "31.252585",
          "longitude" : "121.498753",
          "name" : "7天连锁酒店(上海北外滩国际客运中心地铁站店)",
          "pic" : "https://m2.tuniucdn.com/filebroker/cdn/res/c1/ba/c1baf64418437c56617f89840c6411ef_w200_h200_c1_t0.jpg",
          "price" : 249,
          "score" : 35,
          "starName" : "二钻"
        }
      }
    ]
  }
}
```

>  **总结**:
>
>  function score query定义的三要素是什么？
>
>  -  过滤条件：哪些文档要加分
>  -  算分函数：如何计算 function score
>  -  加权方式：function score 与 query score如何运算

##### 9.9.6.2、Boolean Query:palm_tree: 

布尔查询是一个或多个查询子句的组合。子查询的组合方式有：

-  must：必须匹配每个子查询，类似 “与”
-  should：选择性匹配子查询，类似 “或”
-  must_not：必须不匹配，不参与算分，类似 “非”
-  filter：必须匹配，不参与算分

```json
GET /indexName/_seach
{
   "query": {
      "bool": {
         // must里面是一个数组也就是说可以定义多个查询条件
         "must": [ // 要求查询的城市必须 是 "上海"
            {"term": {"city": "上海"}}
         ],
         "should": [ // 两个品牌中的任意一个都可以因为是 "或" 的关系 一个有表示为true
            {"term": {"brand": "皇冠假日"}},
            {"term": {"brand": "华美达"}}
         ],
         "must_not": [ // 取反，lte 是小于等于，由于取反操作 所以是查询价格 大于等于 500 的
            {"range": {"price": {"lte": 500}}}
         ],
         "filter": [ // 过滤 , 查询用户评价分大于45分的
            {"range": {"score": {"gte": 45}}}
         ]
      }
   }
}
```

###### 9.9.6.2.1、利用bool查询实现功能:tanabata_tree: 

**需求**：搜索名字包含 “如家” ， 价格不高于400，在坐标 31.21，121.5周围10km范围内的酒店。

```json
GET /dkx/_search // GET请求/索引库名/查询全部文档
{
  "query": { // 
    "bool": {
      "must": [
        {
          "match": {
            "name": "如家"
          }
        }
      ],
      "must_not": [
        {
          "range": {
            "price": {
              "gt": 400
            }
          }
        }
      ],
      "filter": [
        {
          "geo_distance": {
            "distance": "10km",
            "location": {
              "lat": 31.21,
              "lon": 121.5
            }
          }
        }
      ]
    }
  }
}
```

结果：

```json
{
  "took" : 173,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 3,
      "relation" : "eq"
    },
    "max_score" : 1.716119,
    "hits" : [
      {
        "_index" : "dkx",
        "_type" : "_doc",
        "_id" : "433576",
        "_score" : 1.716119,
        "_source" : {
          "address" : "南京东路480号保安坊内",
          "brand" : "如家",
          "business" : "人民广场地区",
          "city" : "上海",
          "id" : 433576,
          "location" : "31.236454, 121.480948",
          "name" : "如家酒店(上海南京路步行街店)",
          "pic" : "https://m.tuniucdn.com/fb2/t1/G6/M00/52/BA/Cii-U13eXVaIQmdaAAWxgzdXXxEAAGRrgNIOkoABbGb143_w200_h200_c1_t0.jpg",
          "price" : 379,
          "score" : 44,
          "starName" : "二钻"
        }
      },
      {
        "_index" : "dkx",
        "_type" : "_doc",
        "_id" : "434082",
        "_score" : 1.5234909,
        "_source" : {
          "address" : "复兴东路260号",
          "brand" : "如家",
          "business" : "豫园地区",
          "city" : "上海",
          "id" : 434082,
          "location" : "31.220706, 121.498769",
          "name" : "如家酒店·neo(上海外滩城隍庙小南门地铁站店)",
          "pic" : "https://m.tuniucdn.com/fb2/t1/G6/M00/52/B6/Cii-U13eXLGIdHFzAAIG-5cEwDEAAGRfQNNIV0AAgcT627_w200_h200_c1_t0.jpg",
          "price" : 392,
          "score" : 44,
          "starName" : "二钻"
        }
      },
      {
        "_index" : "dkx",
        "_type" : "_doc",
        "_id" : "1584362548",
        "_score" : 1.4174237,
        "_source" : {
          "address" : "御青路315-317号",
          "brand" : "如家",
          "business" : "周浦康桥地区",
          "city" : "上海",
          "id" : 1584362548,
          "location" : "31.15719, 121.572392",
          "name" : "如家酒店(上海浦东国际旅游度假区御桥地铁站店)",
          "pic" : "https://m.tuniucdn.com/fb3/s1/2n9c/2ybd3wqdoBtBeKcPxmyso9y1hNXa_w200_h200_c1_t0.jpg",
          "price" : 339,
          "score" : 44,
          "starName" : "二钻"
        }
      }
    ]
  }
}
```



>  **总结**：
>
>  bool查询有几种逻辑关系？
>
>  -  must：必须匹配的条件，可以理解为 “与”
>  -  should：选择性匹配的条件，可以理解为 “或”
>  -  must_not：必须不匹配的条件，不参与打分
>  -  filter：必须匹配的条件，不参与打分

#### 9.9.6、搜索结果处理:evergreen_tree: 

-  排序
-  分页
-  高亮

##### 9.9.6.1、排序:palm_tree: 

elasticsearch支持对搜索结果排序，默认是根据相关度算分 (_score) 来排序。可以排序字段类型有：keyword类型，数值类型，地理坐标类型，日期类型等。

**<font color='red'>注意</font>.**：一旦进行排序score就会放弃打分，因为打分就没有意义了

字段

```json
GET /indexName/_search
{
   "query": {
      "match_all": {}
   },
   "sort": [
      {
         "FIELD": "desc" // 排序字段和排序方式ASC , DESC
      }
   ]
}
```

地理坐标

```json
GET /indexName/_search
{
   "query": {
      "match_all": {}
   },
   "sort": [
      {
         "_geo_distance": {
            "FIELD": "纬度，经度",
            "order": "asc",
            "unit": "km"
         }
      }
   ]
}
```

###### 9.9.6.1.1、案例，对酒店数据按照用户评价降序排序，评价相同的按照价格升序排序:tanabata_tree: 

评价是score字段，价格是price字段，按照顺序添加两个排序规则即可。

```json
GET /dkx/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "score": "desc"
    },
    {
      "price": "asc"
    }
  ]
}
```

###### 9.9.6.1.2、案例，实现对酒店数据按照到你的位置坐标距离升序排序:tanabata_tree: 

```json
# 找到 31.034661, 121.612282 周围的酒店，距离升序排序
GET /dkx/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "_geo_distance": {
        # location的两种写法都可以
        # "location": "31.034661, 121.612282", 
        "location": {
          "lat": 31.034661,
          "lon": 121.612282
        }, 
        "order": "desc",
        "unit": "km"
      }
    }
  ]
}
```

##### 9.9.6.2、分页:palm_tree: 

elasticsearch默认情况下只返回top 10的数据。而如果要查询更多数据就需要修改分页参数了。

elasticsearch中通过修改from，size参数来控制要返回的分页结果：

```json
GET /indexName/_search
{
   "query": {
      "match_all": {}
   },
   "from": 990, // 分页开始的位置，默认为0
   "size": 10, // 期望获取的文档总数
   "sort": [
      {
         "price": "dsc"
      }
   ]
}
```

==计算公式==：from - 1 * size = 当前页数

虽然ES的分页查询与mysql很像但是其底层原理有很大差别。ES底层采用的是倒排索引，它的结构其实是不利于做分页的，所以ES采用的是逻辑上的一种分页，比方说要查`990~1000`这是条数据。对于ES来说它只能查出从`0~1000`个所有的数据，然后去截取`990~1000`的一部分数据

![image-20231013113208351](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310131132651.png)

这种情况在单点查询的时候没什么问题，但是如果是集群ES就会把数据进行拆分，放到不同的机器上。拆分的每一份称为分片，也就是每一片上的数据都不一样

###### 9.9.6.2.1、深度分页问题:tanabata_tree: 

ES是分布式的，所以会面临深度分页问题。例如按price排序后，获取from=990，size=10的数据

![image-20231013113540176](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310131135634.png)

1、首先在每个数据分片上都排序并查询到1000条文档

2、然后将所有节点的结果聚合，在内存中重新排序选出前1000条文档

3、最后从这1000条中，选取从990开始的10条文档

如果搜索页数过深，或者结果集 (from + size) 越大，对内存和CPU的消耗也越高。因此ES设定结果集查询的上限是10000

如下演示超出10000条会怎么样

```json
GET /dkx/_search
{
  "query": {
    "match_all": {}
  },
  "from": 9990, 
  "size": 10, 
  "sort": [
    {
      "price": "asc"
    }
  ]
}
//-----------------------查询结果-----------------------
{
  "took" : 33,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 201,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  }
}
```

9990 - 1 * 10 也就是刚好10000条数据，但是如果加1

```json
GET /dkx/_search
{
  "query": {
    "match_all": {}
  },
  "from": 9991,
  "size": 10, 
  "sort": [
    {
      "price": "asc"
    }
  ]
}
```

![image-20231013114259949](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310131143199.png)

查询报错了

###### 9.9.6.2.2、深度分页解决方案:tanabata_tree: 

针对深度分页，ES提供了两种解决方案

-  search after：分页时需要排序，原理是从上一次的排序值开始，查询下一页数据。官方推荐使用的方式。
-  scroll：原理将排序数据形成快照，保存在内存。官方已经不推荐使用。

>  **总结**：
>
>  from + size
>
>  -  优点：支持随机翻页
>  -  缺点：深度分页问题，默认查询上限 (from + size) 是10000
>  -  场景：百度，京东，谷歌，淘宝这样的随机翻页搜索
>
>  after search：
>
>  -  优点：没有查询上限 (单次查询的size不超过10000)
>  -  缺点：只能向后逐页查询，不支持随机翻页
>  -  场景：没有随机翻页需求的搜索，例如手机向下滚动翻页
>
>  scroll：
>
>  -  优点：没有查询上限 (单次查询的size不超过10000)
>  -  缺点：会有额外内存消耗，并且搜索结果是非实时的
>  -  场景：海量数据的获取和迁移。从ES7.1开始不推荐，建议使用after search方案。

##### 9.9.6.3、高亮:palm_tree: 

**高亮**：就是在搜索结果中把搜索关键字突出显示。

![image-20231013115559310](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310131156938.png)

原理是这样的：

-  将搜索结果中的关键字用标签标记出来
-  在页面中给标签添加css样式

语法：

```json
GET /indexName/_search
{
   "query": {
      "match": {
         "FIELD": "TEXT"
      }
   },
   "highlight": {
      "fields": { // 指定要高亮的字段
         "FIELD": {
            "pre_tags": "<em>", // 用来标记高亮字段的前置标签
            "post_tags": "</em>" // 用来标记高亮字段的后置标签
         }
      }
   }
}
```

代码演示：

```json
# 高亮查询，默认情况下，ES搜索字段必须与高亮字段一致，否则不会高亮
GET /dkx/_search
{
  "query": {
    "match": {
      "all": "如家"
    }
  },
  "highlight": {
    "fields": {
      "name": {
        
      }
    }
  }
}
//-------------------结果-------------------
{
        "_index" : "dkx",
        "_type" : "_doc",
        "_id" : "339952837",
        "_score" : 2.7569861,
        "_source" : {
          "address" : "良乡西路7号",
          "brand" : "如家",
          "business" : "房山风景区",
          "city" : "北京",
          "id" : 339952837,
          "location" : "39.73167, 116.132482",
          "name" : "如家酒店(北京良乡西路店)",
          "pic" : "https://m.tuniucdn.com/fb3/s1/2n9c/3Dpgf5RTTzrxpeN5y3RLnRVtxMEA_w200_h200_c1_t0.jpg",
          "price" : 159,
          "score" : 46,
          "starName" : "二钻"
        }
```

<font color='red'>注意</font>：高亮查询，默认情况下，ES搜索字段必须与高亮字段一致，否则不会高亮

演示代码：

```json
# 高亮查询，默认情况下，ES搜索字段必须与高亮字段一致，否则不会高亮
GET /dkx/_search
{
  "query": {
    "match": {
      "name": "如家"
    }
  },
  "highlight": {
    "fields": {
      "name": {
        
      }
    }
  }
}
//-----------------------------结果-----------------------------
"name" : [
   "<em>如家</em>酒店(北京良乡西路店)"
]
```

可以看到name字段加上了高亮标签了，但是 。我不希望在查询中指定name字段因为all搜索到的更多

解决办法如下：

"require_field_match": "true" ：是否需要字段高亮匹配，默认是true需要高亮字段与查询字段匹配

```json
# 高亮查询，默认情况下，ES搜索字段必须与高亮字段一致，否则不会高亮
GET /dkx/_search
{
  "query": {
    "match": {
      "all": "如家"
    }
  },
  "highlight": {
    "fields": {
      "name": {
         // 修改为false让其不与查询字段匹配就可以高亮了	
        "require_field_match": "false"
      }
    }
  }
}
```

>  **总结**：
>
>  搜索结果处理整体语法：
>
>  ```json
>  GET /dkx/_search
>  {
>   "query": {
>      "match": {
>         "all": "如家"
>      }
>   },
>     "from": 0, // 分页开始的位置
>     "size": 20, // 期望获取的文档总数
>     "sort": [
>        {
>           "price": "asc" // 普通排序
>        },
>        {
>           "_geo_distance": { // 距离排序
>              "location": "31.040699,121.618075",
>              "order": "asc",
>              "unit": "km"
>           }
>        }
>     ],
>     "highlight": {
>        "fields": { // 高亮字段
>           "name": {
>              "pre_tags": "<em>", // 用来标记高亮字段的前置标签
>              "post_tags": "</em>", // 用来标记高亮字段的后置标签
>              "require_field_match": "false"
>           }
>        }
>     }
>  }
>  ```

#### 9.9.7、RestClient查询文档:evergreen_tree: 

-  快速入门
-  match查询
-  精确查询
-  复合查询
-  排序，分页，高亮

##### 9.9.7.1、快速入门:palm_tree: 

我们通过match_all来演示下基本的API，先看请求DSL的组织：

![image-20231013122353640](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310131223853.png)

核心的API：

request.source()：

里面封装了：排序，分页，高亮等所有功能。

QueryBuilders：

里面有各种各样的查询matchQuery，matchAllQuery，boolQuery等等。

![image-20231013194730970](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310131947096.png)

代码演示：

```java
@Test
public void testMatchAll()
{
   // 1.准备Request
   SearchRequest request = new SearchRequest("dkx");
   // 2.准备DSL
   request.source().query(QueryBuilders.matchAllQuery());
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   // 4.解析响应
   SearchHits hits = search.getHits();
   // 4.1.获取总数
   long value = hits.getTotalHits().value;
   System.out.println("共搜索到" + value + "条数据");
   // 4.2.文档数组
   SearchHit[] hits1 = hits.getHits();
   // 4.3.遍历文档数组
   for(SearchHit hit : hits1)
   {
      // 4.4.获取文档source
      String json = hit.getSourceAsString();
      // 4.5.反序列化为HotelDoc对象
      HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
      System.out.println("HotelDoc: " + hotelDoc);
   }
}
//-----------------------打印结果-----------------------
共搜索到201条数据
HotelDoc: HotelDoc(id=36934, name=7天连锁酒店(上海宝山路地铁站店), address=静安交通路40号, price=336, score=37, brand=7天酒店, city=上海, starName=二钻, business=四川北路商业区, location=31.251433, 121.47522,
                   ...
```



RestAPI中其中构建DSL是通过HighLevelRestClient中的resource()来实现的，其中包含了查询，排序，分页，高亮等所有功能：

![image-20231013194937001](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310131954682.png)

RestAPI中其中构建查询条件的核心部分是由一名为QueryBuilders的工具类提供的，其中包含了各种查询方法：

![image-20231013195054455](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310131954893.png)

>  **总结**：
>
>  查询的基本步骤是：
>
>  1.  创建SearchRequest对象
>  2.  准备Request.source()，也就是DSL。
>      1.  QueryBuilders来构建查询条件
>      2.  传入Request.source()的query()方法
>  3.  发送请求，得到结果
>  4.  解析结果 (参考JSON结果，从外到内，逐层解析)

##### 9.9.7.2、全文检索查询:palm_tree: 

全文检索的match和multi_match查询与match_all的API基本一致。差别是查询条件，也就是query的部分。

![image-20231013225059138](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310132252478.png)

同样是利用QueryBuilders提供的方法：

```java
@Test
public void testMatch()
{
   // 1.准备request
   SearchRequest request = new SearchRequest("dkx");
   // 2.准备DSL
   request.source().query(QueryBuilders.matchQuery("all", "如家"));
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   // 4.解析响应
   SearchHits hits = search.getHits();
   long value = hits.getTotalHits().value;
   System.out.println("共搜索到" + value + "条数据");
   SearchHit[] hits1 = hits.getHits();
   for(SearchHit hit : hits1)
   {
      String json = hit.getSourceAsString();
      HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
      System.out.println("hotelDoc: " + hotelDoc);
   }
}
//--------------------打印结果--------------------
共搜索到30条数据
hotelDoc: HotelDoc(id=339952837, name=如家酒店(北京良乡西路店), address=良乡西路7号, price=159, score=46, brand=如家, city=北京, starName=二钻, business=房山风景区, location=39.73167, 116.132482,
                   ...
```

方法的抽取

```java
@Test
public void testMatchAll()
{
   // 1.准备Request
   SearchRequest request = new SearchRequest("dkx");
   // 2.准备DSL
   request.source().query(QueryBuilders.matchAllQuery());
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   // 4.调用解析响应函数
   handle(search);
}

@Test
public void testMatch()
{
   // 1.准备request
   SearchRequest request = new SearchRequest("dkx");
   // 2.准备DSL
   request.source().query(QueryBuilders.matchQuery("all", "如家"));
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   // 4.调用解析响应函数
   handle(search);
}

public void handle(SearchResponse search)
{
   // 4.解析响应
   SearchHits hits = search.getHits();
   long value = hits.getTotalHits().value;
   System.out.println("共搜索到" + value + "条数据");
   SearchHit[] hits1 = hits.getHits();
   for(SearchHit hit : hits1)
   {
      String json = hit.getSourceAsString();
      HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
      System.out.println("hotelDoc: " + hotelDoc);
   }
}
```

##### 9.9.7.3、精确查询:palm_tree: 

精确查询常见的有term查询和range查询，同样利用QueryBuilder实现：

![image-20231013210337545](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310132252500.png)

```java
@Test
public void testJingque() {
   // 1.准备Request
   SearchRequest request = new SearchRequest("dkx");
   // 2.准备DSL
   // 2.1词条查询
   request.source().query(QueryBuilders.termQuery("city", "深圳"));
   // 2.2范围查询
   request.source().query(QueryBuilders.rangeQuery("price").gte(100).lte(150));
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   handle(search);
}

public void handle(SearchResponse search) {
   // 4.解析响应
   SearchHits hits = search.getHits();
   long value = hits.getTotalHits().value;
   System.out.println("共搜索到" + value + "条数据");
   SearchHit[] hits1 = hits.getHits();
   for (SearchHit hit : hits1) {
      String json = hit.getSourceAsString();
      HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
      System.out.println("hotelDoc: " + hotelDoc);
   }
}
//-----------------打印结果-----------------
共搜索到5条数据
hotelDoc: HotelDoc(id=541619, name=如家酒店(上海莘庄地铁站龙之梦商业广场店), address=莘庄镇莘浜路172号, price=149, score=44, brand=如家, city=上海, starName=二钻, business=莘庄工业区, location=31.105797, 121.37755,
                   ...
```

##### 9.9.7.4、复合查询-boolean query:palm_tree: 

精确查询常见的有term查询和range查询，同样利用QueryBuilders实现：

![image-20231014105919882](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310141059562.png)

```java
@Test
public void testBooleanQuery()
{
   // 1.准备Request
   SearchRequest request = new SearchRequest("dkx");
   // 2.准备DSL
   // 2.1.准备BoolQuery
   BoolQueryBuilder boolQuery = QueryBuilders.boolQuery();
   // 2.2.添加term
   boolQuery.must(QueryBuilders.termQuery("city", "深圳"));
   // 2.3.添加range
   boolQuery.filter(QueryBuilders.rangeQuery("price").lte(250));
   // 2.4.将条件添加到 发送请求中
   request.source().query(boolQuery);
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   handle(search);
}

public void handle(SearchResponse search) {
   // 4.解析响应
   SearchHits hits = search.getHits();
   long value = hits.getTotalHits().value;
   System.out.println("共搜索到" + value + "条数据");
   SearchHit[] hits1 = hits.getHits();
   for (SearchHit hit : hits1) {
      String json = hit.getSourceAsString();
      HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
      System.out.println("hotelDoc: " + hotelDoc);
   }
}
//--------------------打印结果--------------------
共搜索到18条数据
hotelDoc: HotelDoc(id=517915, name=如家酒店·neo(深圳草埔地铁站店), address=布吉路1036号, price=159, score=44, brand=如家, city=深圳, starName=二钻, business=田贝/水贝珠宝城, location=22.583191, 114.118499,
                   ...
```

>  **总结**：
>
>  要构建查询条件，只要记住一个类：QueryBuilders

##### 9.9.7.5、搜索结果处理:palm_tree: 

###### 9.9.7.5.1、排序和分页:tanabata_tree: 

搜索结果的排序和分页是与query同级的参数，对应的API如下：

![image-20231014110315292](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310141103708.png)

```java
@Test
public void testPageAndSort()
{
   // 页码，每页大小
   int page = 1,size = 5;
   // 1.准备Request
   SearchRequest request = new SearchRequest("dkx");
   // 2.准备DSL
   request.source().query(QueryBuilders.matchAllQuery());
   // 2.1.排序 sort
   request.source().sort("price", SortOrder.ASC);
   // 2.2.分页 from size
   request.source().from((page - 1) * size).size(size);
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   handle(search);
}

public void handle(SearchResponse search) {
   // 4.解析响应
   SearchHits hits = search.getHits();
   long value = hits.getTotalHits().value;
   System.out.println("共搜索到" + value + "条数据");
   SearchHit[] hits1 = hits.getHits();
   for (SearchHit hit : hits1) {
      String json = hit.getSourceAsString();
      HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
      System.out.println("hotelDoc: " + hotelDoc);
   }
}
//-----------------------打印结果-----------------------
共搜索到201条数据
hotelDoc: HotelDoc(id=197837109, name=如家酒店·neo(深圳龙岗大道布吉地铁站店), address=布吉镇深惠路龙珠商城, price=127, score=43, brand=如家, city=深圳, starName=二钻, business=布吉/深圳东站, location=22.602482, 114.123284, pic=https://m.tuniucdn.com/fb2/t1/G6/M00/25/58/Cii-TF3PFZOIA7jwAAKInGFN4xgAAEVbAGeP4AAAoi0485_w200_h200_c1_t0.jpg)
hotelDoc: HotelDoc(id=2316304, name=如家酒店(深圳双龙地铁站店), address=龙岗街道龙岗墟社区龙平东路62号, price=135, score=45, brand=如家, city=深圳, starName=二钻, business=龙岗中心区/大运新城, location=22.730828, 114.278337, pic=https://m.tuniucdn.com/fb3/s1/2n9c/4AzEoQ44awd1D2g95a6XDtJf3dkw_w200_h200_c1_t0.jpg)
hotelDoc: HotelDoc(id=1630005459, name=7天连锁酒店（深圳地王大厦红桂路店）（原红桂路店）, address=罗湖区宝安南路2078号深港豪苑（与红桂路交汇处）, price=143, score=39, brand=7天酒店, city=深圳, starName=二钻, business=, location=22.550341, 114.10965, pic=https://m.tuniucdn.com/fb2/t1/G2/M00/EA/18/Cii-T1k1KaGIIkQVAAD4fD_T3FcAALTtABiCJ8AAPiU164_w200_h200_c1_t0.jpg)
hotelDoc: HotelDoc(id=541619, name=如家酒店(上海莘庄地铁站龙之梦商业广场店), address=莘庄镇莘浜路172号, price=149, score=44, brand=如家, city=上海, starName=二钻, business=莘庄工业区, location=31.105797, 121.37755, pic=https://m.tuniucdn.com/fb3/s1/2n9c/3mKs3jETvJDj3dDdkRB9UyLLvPna_w200_h200_c1_t0.jpg)
hotelDoc: HotelDoc(id=1400304687, name=如家酒店(深圳横岗地铁站新马商贸城店), address=龙岗大道横岗段4004号, price=149, score=43, brand=如家, city=深圳, starName=二钻, business=龙岗中心区/大运新城, location=22.642629, 114.202799, pic=https://m.tuniucdn.com/fb2/t1/G6/M00/25/5A/Cii-TF3PFkiIb27dAAEqdDcKl3YAAEViQGVWY0AASqM960_w200_h200_c1_t0.jpg)
```

###### 9.9.7.5.2、高亮:tanabata_tree: 

高亮API包括请求DSL构建和结果解析两部分。我们先看请求的DSL构建：

![image-20231014125137358](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310141251993.png)

```java
@Test
public void testHighlight()
{
   // 1.准备Request
   SearchRequest request = new SearchRequest("dkx");
   // 2.准备DSL
   // 2.1.query查询
   request.source().query(QueryBuilders.matchQuery("all", "如家"));
   // 2.2.高亮
   request.source().highlighter(new HighlightBuilder()
                                .field("name")
                                .requireFieldMatch(false)
                               );
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   handle(search);
}

public void handle(SearchResponse search) {
   // 4.解析响应
   SearchHits hits = search.getHits();
   long value = hits.getTotalHits().value;
   System.out.println("共搜索到" + value + "条数据");
   SearchHit[] hits1 = hits.getHits();
   for (SearchHit hit : hits1) {
      String json = hit.getSourceAsString();
      HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
      System.out.println("hotelDoc: " + hotelDoc);
   }
}
//---------------------打印结果---------------------
共搜索到30条数据
hotelDoc: HotelDoc(id=339952837, name=如家酒店(北京良乡西路店), address=良乡西路7号, price=159, score=46, brand=如家, city=北京, starName=二钻, business=房山风景区, location=39.73167, 116.132482, pic=https://m.tuniucdn.com/fb3/s1/2n9c/3Dpgf5RTTzrxpeN5y3RLnRVtxMEA_w200_h200_c1_t0.jpg)
hotelDoc: HotelDoc(id=1455383931, name=如家酒店(深圳宝安客运中心站店), address=西乡河西金雅新苑34栋, price=169, score=45, brand=如家, city=深圳, starName=二钻, business=宝安商业区, location=22.590272, 113.881933,
                   ...
```

从上面的打印结果中可以看到并没有找到高亮的部分啊。这是因为打印的是source的部分

高亮的部分在另一个部分中如下图所示：

![image-20231014130537007](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310141305411.png)

###### 9.9.7.5.2.1、高亮结果解析：

```java
@Test
public void testHighlight()
{
   // 1.准备Request
   SearchRequest request = new SearchRequest("dkx");
   // 2.准备DSL
   // 2.1.query查询
   request.source().query(QueryBuilders.matchQuery("all", "如家"));
   // 2.2.高亮
   request.source().highlighter(new HighlightBuilder()
                                .field("name")
                                .requireFieldMatch(false)
                               );
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   handle(search);
}

public void handle(SearchResponse search) {
   // 4.解析响应
   SearchHits hits = search.getHits();
   long value = hits.getTotalHits().value;
   System.out.println("共搜索到" + value + "条数据");
   SearchHit[] hits1 = hits.getHits();
   for (SearchHit hit : hits1) {
      String json = hit.getSourceAsString();
      HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
      // 获取高亮结果
      Map<String, HighlightField> highlightFields = hit.getHighlightFields();
      // CollectionUtils：Spring提供的工具类isEmpty判断Map集合是否为空
      if(! CollectionUtils.isEmpty(highlightFields))
      {
         // 根据字段名获取高亮结果
         HighlightField name = highlightFields.get("name");
         // 判断根据字段获取的内容是否为null
         if(name != null)
         {
            // 获取高亮值
            String content = name.getFragments()[0].string();
            // 覆盖非高亮结果
            hotelDoc.setName(content);
         }
      }
      System.out.println("hotelDoc: " + hotelDoc);
   }
}
//-----------------打印结果-----------------
共搜索到30条数据
hotelDoc: HotelDoc(id=339952837, name=<em>如家</em>酒店(北京良乡西路店), address=良乡西路7号, price=159, score=46, brand=如家, city=北京, starName=二钻, business=房山风景区, location=39.73167, 116.132482,
                   ...
```

>  **总结**：
>
>  -  所有搜索DSL的构建，记住一个API：
>
>     SearchRequest的source()方法
>
>  -  高亮结果解析是参考JSON结果，逐层解析

###### 9.9.7.5.3、距离排序:tanabata_tree: 

距离排序与普通字段排序有所差异，API如下：

![image-20231014165529717](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310141655675.png)

```java
@Test
public void testJvliSort()
{
   // 1.准备Request
   SearchRequest request = new SearchRequest("dkx");
   // 2.3.排序
   String location = "31.251433, 121.47522";
   if(location != null && (!"".equals(location)))
   {
      // 2.4.根据距离进行排序
      request.source().sort(SortBuilders.geoDistanceSort("location",
                                                         new GeoPoint(location)
                                                        )
                            .order(SortOrder.ASC)
                            .unit(DistanceUnit.KILOMETERS)
                           );
   }
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   // 4.解析响应结果
   handle(search);
}

public void handle(SearchResponse search) {
   // 4.解析响应
   SearchHits hits = search.getHits();
   long value = hits.getTotalHits().value;
   System.out.println("共搜索到" + value + "条数据");
   SearchHit[] hits1 = hits.getHits();
   for (SearchHit hit : hits1) {
      String json = hit.getSourceAsString();
      HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
      // 获取高亮结果
      Map<String, HighlightField> highlightFields = hit.getHighlightFields();
      // CollectionUtils：Spring提供的工具类isEmpty判断Map集合是否为空
      if(! CollectionUtils.isEmpty(highlightFields))
      {
         // 根据字段名获取高亮结果
         HighlightField name = highlightFields.get("name");
         // 判断根据字段获取的内容是否为null
         if(name != null)
         {
            // 获取高亮值
            String content = name.getFragments()[0].string();
            // 覆盖非高亮结果
            hotelDoc.setName(content);
         }
      }
      System.out.println("hotelDoc: " + hotelDoc);
   }
}
//--------------------打印结果--------------------
共搜索到201条数据
hotelDoc: HotelDoc(id=36934, name=7天连锁酒店(上海宝山路地铁站店), address=静安交通路40号, price=336, score=37, brand=7天酒店, city=上海, starName=二钻, business=四川北路商业区, location=31.251433, 121.47522, pic=https://m.tuniucdn.com/fb2/t1/G1/M00/3E/40/Cii9EVkyLrKIXo1vAAHgrxo_pUcAALcKQLD688AAeDH564_w200_h200_c1_t0.jpg, distance=null, isAD=null)
hotelDoc: HotelDoc(id=60935, name=上海虹口三至喜来登酒店, address=四平路59号, price=1899, score=46, brand=喜来登, city=上海, starName=五星级, business=四川北路商业区, location=31.2579, 121.487954,
                   ...
```

###### 9.9.7.5.4、组合查询-function score:tanabata_tree: 

Function Score查询可以控制文档的相关性算分，使用方式如下：

![image-20231014173905116](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310141739473.png)

```java
@Test
public void testFunctionScore()
{
   // 1.准备Request
   SearchRequest request = new SearchRequest("dkx");
   // 2.准备DSL
   // 算分控制
   FunctionScoreQueryBuilder functionScoreQueryBuilder =
      QueryBuilders.functionScoreQuery(
      // 查询name = 外滩
      QueryBuilders.matchQuery("name", "外滩"),
      new FunctionScoreQueryBuilder.FilterFunctionBuilder[]{
         new FunctionScoreQueryBuilder.FilterFunctionBuilder(
            // 过滤字段
            QueryBuilders.termQuery("brand", "如家"),
            // 加权
            ScoreFunctionBuilders.weightFactorFunction(5)
         )
      }
   );
   request.source().query(functionScoreQueryBuilder);
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   handle(search);
}

public void handle(SearchResponse search) {
   // 4.解析响应
   SearchHits hits = search.getHits();
   long value = hits.getTotalHits().value;
   System.out.println("共搜索到" + value + "条数据");
   SearchHit[] hits1 = hits.getHits();
   for (SearchHit hit : hits1) {
      String json = hit.getSourceAsString();
      HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
      // 获取高亮结果
      Map<String, HighlightField> highlightFields = hit.getHighlightFields();
      // CollectionUtils：Spring提供的工具类isEmpty判断Map集合是否为空
      if(! CollectionUtils.isEmpty(highlightFields))
      {
         // 根据字段名获取高亮结果
         HighlightField name = highlightFields.get("name");
         // 判断根据字段获取的内容是否为null
         if(name != null)
         {
            // 获取高亮值
            String content = name.getFragments()[0].string();
            // 覆盖非高亮结果
            hotelDoc.setName(content);
         }
      }
      System.out.println("hotelDoc: " + hotelDoc);
   }
}
//---------------------打印结果---------------------
共搜索到3条数据
hotelDoc: HotelDoc(id=434082, name=如家酒店·neo(上海外滩城隍庙小南门地铁站店), address=复兴东路260号, price=392, score=44, brand=如家, city=上海, starName=二钻, business=豫园地区, location=31.220706, 121.498769, pic=https://m.tuniucdn.com/fb2/t1/G6/M00/52/B6/Cii-U13eXLGIdHFzAAIG-5cEwDEAAGRfQNNIV0AAgcT627_w200_h200_c1_t0.jpg, distance=null, isAD=null)
hotelDoc: HotelDoc(id=60487, name=上海外滩茂悦大酒店, address=黄浦路199号, price=689, score=44, brand=君悦, city=上海, starName=五星级, business=外滩地区, location=31.245409, 121.492969, pic=https://m.tuniucdn.com/fb3/s1/2n9c/2Swp2h1fdj9zCUKsk63BQvVgKLTo_w200_h200_c1_t0.jpg, distance=null, isAD=null)
hotelDoc: HotelDoc(id=432335, name=7天连锁酒店(上海北外滩国际客运中心地铁站店), address=唐山路145号, price=249, score=35, brand=7天酒店, city=上海, starName=二钻, business=北外滩地区, location=31.252585, 121.498753, pic=https://m2.tuniucdn.com/filebroker/cdn/res/c1/ba/c1baf64418437c56617f89840c6411ef_w200_h200_c1_t0.jpg, distance=null, isAD=null)
```

###### ES-RestClient黑马旅游案例，项目地址：https://gitee.com/doukaixin/typora.git

### 9.10、深入elasticsearch:deciduous_tree: 

-  数据聚合
-  自动补全
-  数据同步
-  集群

#### 9.10.1、数据聚合:evergreen_tree: 

-  聚合的种类
-  DSL实现聚合
-  RestAPI实现聚合

##### 9.10.1.1、聚合的分类:palm_tree: 

聚合 (aggregations) 可以实现对文档数据的统计，分析，运算。聚合常见的有三类：

<font color='red'>注意</font>：聚合的字段一定是不分词的。可以是keyword，日期，数值，布尔，但绝对不可以是text类型的

-  桶 (Bucket) 聚合：用来对文档做分组
   -  TermAggregation：按照文档字段值分组 
   -  Date Histogram：按照日期阶梯分组，例如 一周为一组，或者 一月为一组
-  度量 (Metric) 聚合：用以计算一些值，比如：最大值，最小值，平均值等
   -  Avg：求平均值
   -  Max：求最大值
   -  Min：求最小值
   -  Stats：同时求max，min，avg，sum等
-  管道 (pipeline) 聚合：对其它聚合的结果为基础做聚合

>  **总结**：
>
>  什么是聚合？
>
>  -  聚合是对文档数据的统计，分析，计算
>
>  聚合的常见种类有哪些？
>
>  -  Bucket：对文档数据分组，并统计每组数量
>  -  Metric：对文档数据做计算，例如avg
>  -  Pipeline：基于其它聚合结果再做聚合
>
>  参与聚合的字段类型必须是：
>
>  -  keyword
>  -  数值
>  -  日期
>  -  布尔

###### 9.10.1.1.1、DSL实现Bucket聚合:tanabata_tree: 

现在，我们要统计所有数据中的酒店品牌有几种，此时可以根据酒店品牌的名称做聚合。

类型为term类型，DSL示例：

```json
GET /indexName/_search
{
   "size": 0, // 设置size为0,结果中不包含文档，只包含聚合结果
   "aggs": { // 定义聚合
      "brandAgg": { // 给聚合起个名字
         "terms": { // 聚合的类型，按照品牌值聚合，所以选择term
            "field": "brand", // 参与聚合的字段
            "size": 20 // 希望获取的聚合结果数量
         }
      }
   }
}
```

代码演示：

```json
# 聚合功能
GET /dkx/_search
{
  "size": 0,
  "aggs": {
    "brandAgg": {
      "terms": {
        "field": "brand",
        "size": 10
      }
    }
  }
}
//-----------------查询结果-----------------
"buckets" : [
        {
          "key" : "7天酒店",
          "doc_count" : 30
        },
        {
          "key" : "如家",
          "doc_count" : 30
        },
        {
          "key" : "皇冠假日",
          "doc_count" : 17
        },
        {
          "key" : "速8",
          "doc_count" : 15
        },
   ...
```

默认是倒序排序值越大的越靠前，排序的规则是可以进行修改的

###### 9.10.1.1.2、Bucket聚合-聚合结果排序:tanabata_tree: 

默认情况下，Bucket聚合会统计Bucket内的文档数量，记为`_count`，并按照`_count`降序排序。

我们可以修改结果排序方式：

```json
GET /indexName/_search
{
   "size": 0,
   "aggs": {
      "brandAgg": {
         "terms": {
            "field": "brand",
            "order": {
               "_count": "asc" // 按照_count升序排列
            },
            "size": 20
         }
      }
   }
}
```

代码演示：

```json
# 聚合功能
GET /dkx/_search
{
  "size": 0,
  "aggs": {
    "brandAgg": {
      "terms": {
        "field": "brand",
        "size": 10,
        "order": {
          "_count": "asc"
        }
      }
    }
  }
}
//--------------打印结果--------------
"brandAgg" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 130,
      "buckets" : [
        {
          "key" : "万丽",
          "doc_count" : 2
        },
        {
          "key" : "丽笙",
          "doc_count" : 2
        },
        {
          "key" : "君悦",
          "doc_count" : 4
        },
         ...
```

###### 9.10.1.1.3、Bucket聚合-限定聚合范围:tanabata_tree: 

默认情况下，Bucket聚合是对索引库的所有文档做聚合，我们可以限定要聚合的文档范围，只要添加query条件即可。

```json
GET /indexName/_search
{
   "query": {
      "range": {
         "price": {
            "lte": 200 // 只对200元以下的文档聚合
         }
      }
   },
   "size": 0,
   "aggs": {
      "brandAgg": {
         "terms": {
            "field": "brand",
            "size": 0
         }
      }
   }
}
```

代码演示：

```json
# 聚合功能，限定聚合范围
GET /dkx/_search
{
  "query": {
    "range": {
      "price": {
        "lte": 200
      }
    }
  },
  "size": 0, 
  "aggs": {
    "brandAgg": {
      "terms": {
        "field": "brand",
        "size": 20,
        "order": {
          "_count": "asc"
        }
      }
    }
  }
}
//----------------打印结果----------------
"buckets" : [
        {
          "key" : "7天酒店",
          "doc_count" : 1
        },
        {
          "key" : "汉庭",
          "doc_count" : 1
        },
        {
          "key" : "速8",
          "doc_count" : 2
        },
        {
          "key" : "如家",
          "doc_count" : 13
        }
      ]
```

>  **总结**：
>
>  aggs代表聚合，与query同级，此时query的作用是？
>
>  -  限定聚合的文档范围
>
>  聚合必须得三要素：
>
>  -  聚合名称
>  -  聚合类型
>  -  聚合字段
>
>  聚合可配置属性有：
>
>  -  size：指定聚合结果数量
>  -  order：指定聚合结果排序方式
>  -  field：指定聚合字段

###### 9.10.1.1.4、DSL实现Metrics聚合:tanabata_tree: 

例如，我们要求获取每个品牌的用户评分的min，max，avg等值。

我们可以利用stats聚合：

```json
GET /indexName/_search
{
   "size": 0,
   "aggs": {
      "brandAgg": {
         "terms": {
            "field": "brand",
            "size": 20
         },
         "aggs": { // 是brands聚合的子聚合，也就是分组后每组分别计算
            "score_stats": { // 聚合名称
               "stats": { // 聚合类型，这里stats可以计算min，max，avg等
                  "field": "score" // 聚合字段，这里是score
               }
            }
         }
      }
   }
}
```

代码演示：

```json
# 嵌套聚合metrics
GET /dkx/_search
{
  "size": 0,
  "aggs": {
    "brandAgg": {
      "terms": {
        "field": "brand",
        "size": 20
      },
      "aggs": {
        "score_stats": {
          "stats": {
            "field": "score"
          }
        }
      }
    }
  }
}
//-------------------------打印结果-------------------------
"buckets" : [
        {
          "key" : "7天酒店",
          "doc_count" : 30,
          "score_stats" : {
            "count" : 30,
            "min" : 35.0,
            "max" : 43.0,
            "avg" : 37.86666666666667,
            "sum" : 1136.0
          }
        },
        {
          "key" : "如家",
          "doc_count" : 30,
          "score_stats" : {
            "count" : 30,
            "min" : 43.0,
            "max" : 47.0,
            "avg" : 44.833333333333336,
            "sum" : 1345.0
          }
        },
   ...
```

对score_stats中的avg做一个排序

```json
# 嵌套聚合metrics
GET /dkx/_search
{
  "size": 0,
  "aggs": {
    "brandAgg": {
      "terms": {
        "field": "brand",
        "size": 20,
        "order": {
          "score_stats.avg": "desc" // 对score_stats中的avg进行降序排序
        }
      },
      "aggs": {
        "score_stats": {
          "stats": {
            "field": "score"
          }
        }
      }
    }
  }
}
//----------------------打印结果----------------------
"buckets" : [
        {
          "key" : "万丽",
          "doc_count" : 2,
          "score_stats" : {
            "count" : 2,
            "min" : 46.0,
            "max" : 47.0,
            "avg" : 46.5,
            "sum" : 93.0
          }
        },
        {
          "key" : "凯悦",
          "doc_count" : 8,
          "score_stats" : {
            "count" : 8,
            "min" : 45.0,
            "max" : 47.0,
            "avg" : 46.25,
            "sum" : 370.0
          }
        },
   ...
```

##### 9.10.1.2、RestAPI实现聚合:palm_tree: 

我们以品牌聚合为例，演示下Java的RestClient使用，先看请求组装：

![image-20231014211458877](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310142115151.png)

```json
@Test
public void testAggreation()
{
   // 1.准备Request
   SearchRequest request = new SearchRequest("dkx");
// 2.准备DSL
// 2.1.设置size去掉文档数据
request.source().size(0);
// 2.2.聚合
request.source().aggregation(AggregationBuilders
                             .terms("brandAgg")
                             .field("brand")
                             .size(10)
                            );
// 3.发送请求
SearchResponse search;
try {
   search = client.search(request, RequestOptions.DEFAULT);
} catch (IOException e) {
   throw new RuntimeException(e);
}
System.out.println(search);
}
//--------------------打印结果--------------------
{"took":83,"timed_out":false,"_shards":{"total":1,"successful":1,"skipped":0,"failed":0},"hits":{"total":{"value":201,"relation":"eq"},"max_score":null,"hits":[]},"aggregations":{"sterms#brandAgg":{"doc_count_error_upper_bound":0,"sum_other_doc_count":39,"buckets":[{"key":"7天酒店","doc_count":30},{"key":"如家","doc_count":30},{"key":"皇冠假日","doc_count":17},{"key":"速8","doc_count":15},{"key":"万怡","doc_count":13},{"key":"华美达","doc_count":13},{"key":"和颐","doc_count":12},{"key":"万豪","doc_count":11},{"key":"喜来登","doc_count":11},{"key":"希尔顿","doc_count":10}]}}}
```

###### 9.10.1.2.1、聚合结果解析:palm_tree: 

![image-20231014212710555](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310142127780.png)

```java
@Test
public void testAggreation()
{
   // 1.准备Request
   SearchRequest request = new SearchRequest("dkx");
   // 2.准备DSL
   // 2.1.设置size去掉文档数据
   request.source().size(0);
   // 2.2.聚合
   request.source().aggregation(AggregationBuilders
                                .terms("brandAgg")
                                .field("brand")
                                .size(10)
                               );
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   // 4.调用函数解析响应结果
   handleAgg(search);
}

private void handleAgg(SearchResponse search) 
{
   Aggregations aggregations = search.getAggregations();
   // 4.1.根据聚合名称获取聚合结果
   Terms brandTerms = aggregations.get("brandAgg");
   // 4.2.获取Buckets
   List<? extends Terms.Bucket> buckets = brandTerms.getBuckets();
   // 4.3.遍历桶
   for (Terms.Bucket bucket : buckets) {
      // 4.4.获取key
      String keyAsString = bucket.getKeyAsString();
      System.out.println(keyAsString);
   }
}
//-------------------打印结果-------------------
7天酒店
如家
皇冠假日
速8
万怡
华美达
和颐
万豪
喜来登
希尔顿
```

#### 9.10.2、自动补全:evergreen_tree: 

-  拼音分词器
-  自定义分词器
-  自动补全查询
-  实现酒店搜索框自动补全

##### 9.10.2.1、自动补全需求说明:palm_tree: 

当用户在搜索框输入字符时，我们应该提示出与该字符有关的搜索项，如图：

![image-20231014223100805](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310142231178.png)

##### 9.10.2.2、使用拼音分词:palm_tree: 

要实现根据字母做不全，就必须对文档按照拼音分词。在GitHub上恰好有elasticsearch的拼音分词插件。地址：https://github.com/medcl/elasticsearch-analysis-pinyin/releases

安装方式与IK分词器一样，分三步：

1、解压

2、上传到虚拟机中，elasticsearch的plugin目录

![image-20231014224223983](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310142242869.png)

3、重启elasticsearch

```sh
[root@192 _data]# docker restart es
es
[root@192 _data]#
```

4、测试

![image-20231014224855664](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310142248099.png)

##### 9.10.2.3、自定义分词器:palm_tree: 

elasticsearch中分词器 (analyzer) 的组成包含三部分：

-  character filters：在tokenizer之前对本文进行处理。例如删除字符，替换字符
-  tokenizer：将文本按照一定的规则切割成词条 (term)。例如 keyword，就是不分词；还有 ik_smart
-  tokenizer filter：将tokenizer输出的词条做进一步处理。例如大小写转换，同义词处理，拼音处理等

![image-20231015110900632](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310151109094.png)

我们可以在创建索引库时，通过settings来配置自定义的analyzer (分词器)：

![image-20231015111515104](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310151115651.png)

代码演示：

```json
# 自定义分词器
PUT /test
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_analzer": {
          "tokenizer": "ik_max_word",
          "filter": "py"
        }
      },
      "filter": {
        "py": {
          "type": "pinyin",
          "keep_full_pinyin": false,
          "keep_joined_full_pinyin": true,
          "keep_original": true,
          "limit_first_letter_length": 16,
          "remove_duplicated_term": true,
          "none_chinese_pinyin_tokenize": false
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "name": {
        "type": "text",
        "analyzer": "my_analzer"
      }
    }
  }
}
```

测试

```json
POST /test/_analyze // 使用my_analzer自定义分词器需要带上test索引库名
{
  "text": ["如家酒店还不错嘛"],
  "analyzer": "my_analzer"
}
//------------------查询结果------------------
"tokens" : [
    {
      "token" : "如家",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "rujia",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "rj",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "酒店",
      "start_offset" : 2,
      "end_offset" : 4,
      "type" : "CN_WORD",
      "position" : 1
    },
    {
      "token" : "jiudian",
      "start_offset" : 2,
      "end_offset" : 4,
      "type" : "CN_WORD",
      "position" : 1
    },
    {
      "token" : "jd",
      "start_offset" : 2,
      "end_offset" : 4,
      "type" : "CN_WORD",
      "position" : 1
    },
   ...
```



添加两个name字段然后再通过拼音测试是否能查询到

```json
POST /test/_doc/1
{
  "id": 1,
  "name": "狮子"
}

POST /test/_doc/2
{
  "id": 2,
  "name": "柿子"
}
```

查询测试：

```json
GET /test/_search
{
  "query": {
    "match": {
      "name": "shizi"
    }
  }
}
//----------------------查询结果----------------------
"hits" : [
      {
        "_index" : "test",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 0.25069216,
        "_source" : {
          "id" : 1,
          "name" : "狮子"
        }
      },
      {
        "_index" : "test",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 0.25069216,
        "_source" : {
          "id" : 2,
          "name" : "柿子"
        }
      }
    ]
```

但是有个问题，如果查询的是中文的 “狮子” 也就查询出同音的结果测试如下：

```json
GET /test/_search
{
  "query": {
    "match": {
      "name": "掉入狮子笼"
    }
  }
}
//--------------------查询结果--------------------
"hits" : [
      {
        "_index" : "test",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 0.33425623,
        "_source" : {
          "id" : 1,
          "name" : "狮子"
        }
      },
      {
        "_index" : "test",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 0.3085442,
        "_source" : {
          "id" : 2,
          "name" : "柿子"
        }
      }
    ]
```

拼音分词器合适在创建倒排索引的时候使用，但不能在搜索的时候使用。

创建倒排索引时：

![image-20231015114300456](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310151144762.png)

我们要分开，创建时和搜索时使用不同的分词器

因此字段在创建倒排索引时应该用 `my_analyer`分词器，字段在搜索时应该使用 `ik_smart`分词器；

```json
# 自定义分词器
PUT /test
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_analzer": {
          "tokenizer": "ik_max_word",
          "filter": "py"
        }
      },
      "filter": {
        "py": {
          ...
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "name": {
        "type": "text",
        "analyzer": "my_analzer", // 创建索引时使用的分词器
         "search_analyzer": "ik_smart" // 搜索时使用的分词器
      }
    }
  }
}
```

代码演示：

```json
# 自定义分词器
PUT /test
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_analzer": {
          "tokenizer": "ik_max_word",
          "filter": "py"
        }
      },
      "filter": {
        "py": {
          "type": "pinyin",
          "keep_full_pinyin": false,
          "keep_joined_full_pinyin": true,
          "keep_original": true,
          "limit_first_letter_length": 16,
          "remove_duplicated_term": true,
          "none_chinese_pinyin_tokenize": false
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "name": {
        "type": "text",
        "analyzer": "my_analzer",
        "search_analyzer": "ik_smart"
      }
    }
  }
}

POST /test/_doc/1
{
  "id": 1,
  "name": "狮子"
}

POST /test/_doc/2
{
  "id": 2,
  "name": "柿子"
}
```

测试查询结果：

```json
GET /test/_search
{
  "query": {
    "match": {
      "name": "掉入狮子笼"
    }
  }
}
//---------------------查询结果---------------------
"hits" : [
      {
        "_index" : "test",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 0.9530773,
        "_source" : {
          "id" : 1,
          "name" : "狮子"
        }
      }
    ]
```

>  总结：
>
>  如何使用拼音分词器？
>
>  1.  下载pinyin分词器
>  2.  解压并放到elasticsearch的plugin目录
>  3.  重启即可
>
>  如何自定义分词器？
>
>  1.  创建索引库时，在settings中配置，可以包含三部分
>  2.  character filter
>  3.  tokenizer
>  4.  filter
>
>  拼音分词器注意事项？
>
>  -  为了避免搜索到同音字，搜索时不要使用拼音分词器

##### 9.10.2.4、completion suggester查询:palm_tree: 

elasticsearch提供了Completion Suggester查询来实现自动补全功能。这个查询会匹配以用户输入内容开头的词条并返回。为了提高补全查询的效率，对于文档中字段的类型有一些约束

-  参与补全查询的字段必须是completion类型。
-  字段的内容一般是用来补全的多个词条形成的数组。

文档中title：[“Sony”, “WH-1000XM3”] 。字段值是一个数组，“品牌”, “产品”。它是形成了两个词条而不是一个词条为什么要分开呢？

原因：自动补全是根据词条做自动补全的，如果将两个信息何为一个字符串将来做自动补全时就只能通过S来补全了，而输入W时是不会补全产品名称的，分成数组就是两个词。用户输入S时能补全 “Sony”，用户输入W时能补全 “WH-1000XM3”，我们尽量吧词语分成一个一个词条放到数组中

![image-20231015121026947](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310151210270.png)

查询语法如下：

```json
// 自动补全查询
GET /test/_search
{
   "suggest": {
      "title_suggest": {
         "text": "s", // 关键字
         "completion": {
            "field": "title", // 补全查询的字段
            "skip_duplicates": true, // 跳过重复的
            "size": 10 // 获取前10条结果
         }
      }
   }
}
```

代码演示：

```json
# 自动补全的索引库
PUT /test1
{
  "mappings": {
    "properties": {
      "title": {
        "type": "completion"
      }
    }
  }
}

# 添加示例数据
POST /test1/_doc
{
  "title": ["Sony", "WH-1000XM3"]
}

POST /test1/_doc
{
  "title": ["SK-II", "PITERA"]
}

POST /test1/_doc
{
  "title": ["Nintendo", "switch"]
}
```

测试查询补全结果：

```json
# 自动补全查询
GET /test1/_search
{
  "suggest": {
    "titleSuggest": {
      "text": "so",
      "completion": {
        "field": "title",
        "skip_duplicates": true, 
        "size": 10 
      }
    }
  }
}
//-------------------查询结果-------------------
"options" : [
          {
            "text" : "Sony",
            "_index" : "test1",
            "_type" : "_doc",
            "_id" : "reCRMYsBNslGvgguxrYc",
            "_score" : 1.0,
            "_source" : {
              "title" : [
                "Sony",
                "WH-1000XM3"
              ]
            }
          }
        ]
```

>  **总结**：
>
>  自动补全对字段的要求：
>
>  -  类型是completion类型
>  -  字段值是多次词条的数组

##### 9.10.2.5、RestAPI实现自动补全:palm_tree: 

先看请求参数构造的API：

![image-20231015143537333](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310151435297.png)

代码演示：

```json
@Test
public void testSuggest()
{
    // 1.准备Request
    SearchRequest request = new SearchRequest("dkx");
    // 2.准备DSL
    request.source().suggest(new SuggestBuilder()
            .addSuggestion(
                    "mySuggestion",
                    SuggestBuilders
                            .completionSuggestion("suggestion")
                            .prefix("so")
                            .size(10)
            ));
    // 3.发送请求
    SearchResponse search;
    try {
         search = client.search(request, RequestOptions.DEFAULT);
     } catch (IOException e) {
         throw new RuntimeException(e);
     }
     System.out.println(search);
}
//----------------------打印结果----------------------
"suggest":{"mySuggestion":[{"text":"so","offset":0,"length":2,"options":[{"text":"松岗商业中心区","_index":"dkx","_type":"_doc","_id":"1393017952","_score":1.0,"_source":{"address":"松岗镇河滨北路12号盛华大厦","brand":"汉庭","business":"松岗商业中心区","city":"深圳","id":1393017952,"location":"22.768912, 113.83325","name":"汉庭酒店(深圳宝安松岗地铁站店)",
```

###### 9.10.2.5.1、结果解析:tanabata_tree: 

![image-20231015144817860](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310151448335.png)

```java
@Test
public void testSuggest()
{
   // 1.准备Request
   SearchRequest request = new SearchRequest("dkx");
   // 2.准备DSL
   request.source().suggest(new SuggestBuilder()
                            .addSuggestion(
                               "suggestions",
                               SuggestBuilders
                               .completionSuggestion("suggestion")
                               .prefix("so")
                               .size(10)
                            ));
   // 3.发送请求
   SearchResponse search;
   try {
      search = client.search(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
   // 4.解析结果
   handleSuggestion(search);
}

private void handleSuggestion(SearchResponse search) {
   Suggest suggest = search.getSuggest();
   // 4.1.根据补全查询名称，获取补全结果
   CompletionSuggestion suggestion = suggest.getSuggestion("suggestions");
   // 4.2.获取options
   List<CompletionSuggestion.Entry.Option> options = suggestion.getOptions();
   // 4.3.遍历结果
   for (CompletionSuggestion.Entry.Option option : options) {
      // 4.4.获取text的值
      String content = option.getText().toString();
      System.out.println(content);
   }
}
//--------------------打印结果--------------------
松岗商业中心区
松岗商业中心区
松江大学城
松江大学城
松江大学城
```

#### 9.10.3、数据同步:evergreen_tree: 

-  数据同步思路分析
-  实现elasticsearch与数据库数据同步

##### 9.10.3.1、数据同步问题分析:palm_tree: 

elasticsearch中的酒店数据来自于mysql数据库，因此mysql数据发生改变时，elasticsearch也必须跟着改变，这个就是elasticsearch与mysql之间的==数据同步==。

###### 9.10.3.1.1、方案一：同步调用:tanabata_tree: 

缺点：业务耦合

![image-20231015152527620](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310151525267.png)

###### 9.10.3.1.2、方案二：异步通知:tanabata_tree: 

![image-20231015152813104](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310151528971.png)

###### 9.10.3.1.3、方案三：监听binlog:tanabata_tree: 

在mysql中binlog默认是关闭的，但是开启了binlog在mysql里做CRUD时都会将相应的操作记录在binlog中。也就是说mysql变化了binlog就会变化，而我们可以利用Canal这样的中间件去监听binlog。一旦binlog发生变化就去通知微服务更新es数据

缺点：增加了mysql的压力

![image-20231015152942969](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310151529543.png)

>  **总结**:
>
>  方式一：同步调用
>
>  -  优点：实现简单，粗暴
>  -  缺点：业务耦合度太高
>
>  方式二：异步通知
>
>  -  优点：低耦合，实现难度一般
>  -  缺点：依赖mq的可靠性
>
>  方式三：监听binlog
>
>  -  优点：完全解除服务间耦合
>  -  缺点：开启binlog增加数据库负担，实现复杂度高

##### 9.10.3.2、利用MQ实现mysql与elasticsearch数据同步:palm_tree: 

利用课前资料提供的hotel-admin项目作为酒店管理的微服务。当数据库发生CRUD时，要求对elasticsearch中数据也要完成相同操作。

步骤：

-  导入hotel-admin项目(项目地址：https://gitee.com/doukaixin/typora.git)，启动并测试酒店数据的CRUD
-  声明exchange，queue，Routingkey
-  在hotel-admin中的增，删，改业务中完成消息发送
-  在hotel-demo (项目地址：https://gitee.com/doukaixin/typora.git) 中完成消息监听，并更新elasticsearch中数据
-  启动并测试数据同步功能

1、导入hotel-admin项目，启动并测试酒店数据的CRUD

![image-20231015194242526](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310151942076.png)

2、声明exchange，queue，Routingkey

![image-20231015194554807](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310151945245.png)

2.1、创建MqConstans类记录队列名称和Routingkey

```java
public class MqConstans {
    // 交换机
    public final static String HOTEL_EXCHANGE = "hotel.topic";
    // 监听新增和修改的队里
    public final static String HOTEL_INSERT_QUEUE = "hotel.insert.queue";
    // 监听删除的队列
    public final static String HOTEL_DELETE_QUEUE = "hotel.delete.queue";
    // 新增或修改的RoutingKey
    public final static String HOTEL_INSERT_KEY = "hotel.insert";
    // 删除的RoutingKey
    public final static String HOTEL_DELETE_KEY = "hotel.delete";
}
```

2.2、创建MqConfig配置类，配置队列与交换机的关系绑定

```java
@Configuration
public class MqConfig {
    // 定义交换机Bean
    @Bean
    public TopicExchange topicExchange()
    {
        // name：交换机名称 ，durable：是否持久化，autoDelete：是否自动解除绑定关系
        return new TopicExchange(MqConstans.HOTEL_EXCHANGE, true, false);
    }

    // 定义队列
    @Bean
    public Queue insertQueue()
    {
        // name：队列名称，durable：是否持久化
        return new Queue(MqConstans.HOTEL_INSERT_QUEUE, true);
    }

    // 定义队列
    @Bean
    public Queue deleteQueue()
    {
        // name：队列名称，durable：是否持久化
        return new Queue(MqConstans.HOTEL_DELETE_QUEUE, true);
    }

    @Bean
    // 绑定关系
    public Binding insertQueueBinding()
    {
        // 将insertQueue队列绑定到交换机上，并绑定Routingkey
        return BindingBuilder.bind(insertQueue())
                .to(topicExchange()).with(MqConstans.HOTEL_INSERT_KEY);
    }

    @Bean
    public Binding deleteQueueBinding()
    {
        // 将insertQueue队列绑定到交换机上，并绑定Routingkey
        return BindingBuilder.bind(insertQueue())
                .to(topicExchange()).with(MqConstans.HOTEL_DELETE_KEY);
    }
}
```

3、在hotel-admin中的增，删，改业务中完成消息发送

3.1、在hotel-admin项目中也添加constans/MqConstans类方便调用

```java
public class MqConstans {
    // 交换机
    public final static String HOTEL_EXCHANGE = "hotel.topic";
    // 监听新增和修改的队里
    public final static String HOTEL_INSERT_QUEUE = "hotel.insert.queue";
    // 监听删除的队列
    public final static String HOTEL_DELETE_QUEUE = "hotel.delete.queue";
    // 新增或修改的RoutingKey
    public final static String HOTEL_INSERT_KEY = "hotel.insert";
    // 删除的RoutingKey
    public final static String HOTEL_DELETE_KEY = "hotel.delete";
}
```

3.2、配置RabbitMQ的yml并导入依赖

3.3、在Controller中编写发生消息的代码

```java
@RestController
@RequestMapping("hotel")
public class HotelController {

    @Autowired
    private IHotelService hotelService;

    @Autowired
    private RabbitTemplate rabbitTemplate;

    @GetMapping("/{id}")
    public Hotel queryById(@PathVariable("id") Long id){
        return hotelService.getById(id);
    }

    @GetMapping("/list")
    public PageResult hotelList(
            @RequestParam(value = "page", defaultValue = "1") Integer page,
            @RequestParam(value = "size", defaultValue = "1") Integer size
    ){
        Page<Hotel> result = hotelService.page(new Page<>(page, size));
        return new PageResult(result.getTotal(), result.getRecords());
    }

    @PostMapping
    public void saveHotel(@RequestBody Hotel hotel){
        hotelService.save(hotel);
        rabbitTemplate.convertAndSend(MqConstans.HOTEL_EXCHANGE, MqConstans.HOTEL_INSERT_KEY, hotel.getId());
    }

    @PutMapping()
    public void updateById(@RequestBody Hotel hotel){
        if (hotel.getId() == null) {
            throw new InvalidParameterException("id不能为空");
        }
        hotelService.updateById(hotel);
        rabbitTemplate.convertAndSend(MqConstans.HOTEL_EXCHANGE, MqConstans.HOTEL_INSERT_KEY, hotel.getId());
    }

    @DeleteMapping("/{id}")
    public void deleteById(@PathVariable("id") Long id) {
        hotelService.removeById(id);
        rabbitTemplate.convertAndSend(MqConstans.HOTEL_EXCHANGE, MqConstans.HOTEL_DELETE_KEY, id);
    }
}
```

4、在hotel-demo中完成消息监听，并更新elasticsearch中数据

```java
@Override
public void insertOrupdateByid(Long id) {
   Hotel hotel = getById(id);
   HotelDoc hotelDoc = new HotelDoc(hotel);
   // 1.准备Request
   IndexRequest request = new IndexRequest("dkx").id(hotel.getId().toString());
   // 2.准备DSL
   request.source(JSON.toJSONString(hotelDoc), XContentType.JSON);
   // 3.发送请求
   try {
      client.index(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
}

@Override
public void deleteById(Long id) {
   // 1.准备Request
   DeleteRequest request = new DeleteRequest("dkx", id.toString());
   // 2.准备DSL
   try {
      client.delete(request, RequestOptions.DEFAULT);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
}
```

5、启动并测试数据同步功能

<font color='red'>注意</font>：要启动RabbitMQ否则链接就会报错

启动两个项目后查看 队列 情况

![image-20231015213307513](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310152133072.png)

再看一下交换机

![image-20231015213344108](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310152133845.png)

证明 队列和交换机创建成功了。

查看它们的绑定关系

![image-20231015213451830](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310152134821.png)

两个BindingKey分别绑定了Routingkey

接下来进行修改测试，比如说先修改这个广告位的价格看下，找到它的id

![image-20231016074258159](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310160743271.png)

然后在酒店管理中找到这个对应的id信息，对其进行修改

![image-20231016074402893](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310160744989.png)

现在的价格是589，修改为1058

![image-20231016074503675](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310160745904.png)

然后去查看网页的价格是否更新了

可以看到价格被改变了，说明数据同步成功了。

但是这里有个问题就是 每次修改后广告就没了，自然权重也就下降看不到它是第一个了，这里需要做持久化否则每次更新isAD的状态就消息了

![image-20231016081236685](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310160812307.png)

测试删除，删除如下选中的酒店信息

找到对应的id，然后去管理页面进行删除操作

![image-20231016092546878](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310160925485.png)

在酒店管理页面找到对应信息进行删除操作。

![image-20231016092628220](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310160926330.png)

![image-20231016092649279](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310160926213.png)

删除后查看酒店信息网页，可以看到少了一条信息也就证明了那条信息被删除了

 ![image-20231016092713820](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310160930574.png)

#### 9.10.4、elasticsearch集群:evergreen_tree: 

-  搭建ES集群
-  集群脑裂问题
-  集群故障转移
-  集群分布式存储
-  集群分布式查询

##### 9.10.4.1、ES集群结构:palm_tree: 

单击的elasticsearch做数据存储，必然面临两个问题：海量数据存储问题，单点故障问题。

-  海量数据存储问题：将索引库从逻辑上拆分为N个分片 (shard) ，存储到多个节点
-  单点故障问题：将分片数据在不同节点备份 (replica)

![image-20231016102545479](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161025488.png)

##### 9.10.4.2、搭建ES集群:palm_tree: 

我们计划利用3个docker容器模拟3个es的节点。具体步骤参考elasticsearch文章：[查看](./Elasearch/安装elasticsearch.md).

##### 9.10.4.3、ES集群的节点角色:palm_tree: 

elasticsearch中集群节点有不同的职责划分：

| 节点类型         | 配置参数                                       | 默认值 | 节点职责                                                     |
| ---------------- | ---------------------------------------------- | ------ | ------------------------------------------------------------ |
| master  eligible | node.master                                    | true   | 备选主节点：主节点可以管理和记录集群状态，决定<br />分片在哪个节点，处理创建和删除索引库的请求 |
| data             | node.data                                      | true   | 数据节点：存储数据，搜索，聚合，CRUD                         |
| ingest           | node.ingest                                    | true   | 数据存储之前的预处理                                         |
| coordinating     | 上面3个参数都为false<br />则为coordinating节点 | 无     | 路由请求到其它节点<br />合并其它节点处理的结果，返回给用户   |

##### 9.10.4.4、ES集群的分布式查询:palm_tree: 

elasticsearch中的每个节点角色都有自己不用的职责，因此建议集群部署时，每个节点都有独立的角色。

下图中，有三个coordinating节点(协调节点) 还有 n 个数据节点 和 三个候选主节点。将来有一个节点挂了就可以从三个候选主节点中选取一个出来。LB为负载均衡器，对协调节点做负载均衡

![image-20231016133909366](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161339823.png)

缺点：

候选主节点结构会有一个脑裂问题

##### 9.10.4.5、ES集群的脑裂:palm_tree: 

默认情况下，每个节点都是mster gligible节点，因此一旦master节点宕机，其它候选节点会选举一个成为主节点。当主节点与其它节点网络故障时，可能发生脑裂问题。

什么是脑裂呢？

比方说，有三个候选主节点，第一个节点现在当选为主节点。剩下两个是候选节点

![image-20231016134608313](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161346747.png)

现在假设说发生了网络故障

不是宕机了，是因为网络的原因导致，node2和node3跟node1连接不上了，但是node1跟其它部分数据节点还是可以连同的。node2和node3和一些数据节点也是可以连同的。这个时候就等于集群被分开了。

当node2和node3连不上node1的时候它会认为node1挂了，就会在node2和node3中又选举出一个老大

![image-20231016134716821](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161347107.png)

比方说选举的老大为node3

这是集群里面就等于出现了两个老大

现在就出问题了，因为有两个老大，一部分数据节点跟node1结合将来做CRUD的时候由node1来控制

还有另一部分由node3来控制，它们各自去处理各自的集群。一旦网络恢复，当用户来访问时就会出现两边数据不一致的情况。这可就出大事儿了，这就是脑裂问题。

![image-20231016135040891](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161350808.png)

为了避免脑裂，需要要求选票超过 (eligible节点数量 + 1) / 2才能当选为主，因此eligible节点数量最好是奇数。对应配置项是discovery.zen.minimun_master_nodes，在es7.0以后，已经成为默认配置，因此一般不会发生脑裂问题。

>  **总结**：
>
>  master eligible节点的作用是什么？
>
>  -  参与集群选主
>  -  主节点可以管理集群状态，管理分片信息，处理创建和删除索引库的请求
>
>  data节点的作用是什么？
>
>  -  数据的CRUD
>
>  coordinator节点的作用是什么？
>
>  -  路由请求到其它节点
>  -  合并查询到的结果，返回给用户

##### 9.10.4.6、测试引出问题:palm_tree: 

当新增文档时，应该保存到不同分片，保证数据均衡，那么coordinating node如何确定数据该存储到哪个分片呢？

这里由于虚拟机中没有启动kibana所以使用postman，api接口测试工具来进行添加文档数据

![image-20231016140915224](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161409037.png)

进行查询文档的数据情况

三条数据就查询出来了

![image-20231016141318310](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161413380.png)

查询端口为9201为es02的数据情况

![image-20231016141452636](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161414897.png)

查询端口为9202为es03的数据情况

![image-20231016141537985](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161415226.png)

可以看到都是同样的结果，我是在9200插入的文档数据结果在每一个节点都可以查到。那问题来了这些数据到底是存到哪个分片上了呢？

我们可以通过 “explain” 命令，这个命令可以让我们看到查询的结果到底在哪里

```json
{
    "explain" : true,
    "query": {
        "match_all": {}
    }
}
```

查询的结果：

```json
"hits": [
            {
                "_shard": "[itcast][0]",
                "_node": "tzSKl2laTTKoDwMz4AfkAQ",
                "_index": "itcast",
                "_type": "_doc",
                "_id": "5",
                "_score": 1.0,
                "_source": {
                    "title": "试着插入一条 id = 1"
                },
                "_explanation": {
                    "value": 1.0,
                    "description": "*:*",
                    "details": []
                }
            },
            {
                "_shard": "[itcast][1]",
                "_node": "tzSKl2laTTKoDwMz4AfkAQ",
                "_index": "itcast",
                "_type": "_doc",
                "_id": "3",
                "_score": 1.0,
                "_source": {
                    "title": "试着插入一条 id = 1"
                },
                "_explanation": {
                    "value": 1.0,
                    "description": "*:*",
                    "details": []
                }
            },
            {
                "_shard": "[itcast][2]",
                "_node": "nrhAcc9RQX2XXJoxYWFYPg",
                "_index": "itcast",
                "_type": "_doc",
                "_id": "1",
                "_score": 1.0,
                "_source": {
                    "title": "试着插入一条 id = 1"
                },
                "_explanation": {
                    "value": 1.0,
                    "description": "*:*",
                    "details": []
                }
            }
        ]
```

可以看到id为5的在 0 号分片，id为3的在 1 号分片，id为1的在 2 号分片

刚好这三条数据在每个分片上都有一个，那么问题来了，我明明是在9200插入的数据怎么会出现在三台机器上呢，这说明 协调节点 确实是工作了。那问题是它是怎么工作的呢？

elasticsearch会通过hash算法来计算文档应该存储到哪个分片：

```
shard = hash(_routing) % number_of_shards
```

用hash计算的结果去对number_of_shards进行取余，只要_routing值变化那么计算出的结果也会随着变化

说明：

-  _routing默认是文档id
-  算法与分片数量有关，因此索引库一旦创建，分片数量不能修改(如果改了数据就找不到了)！

##### 9.10.4.7、通过案例进行分析:palm_tree: 

比方说现在来做新增

新增文档流程：

如下有集群三个节点，分别是node1，node2，node3。在这个集群的每个节点上都会有一些分片。其中深蓝色叫primarisha 主分片，p-0，p-1，p-2分别就是0号主分片，1号主分片，2号主分片。每个分片都要有副本，浅蓝色就是副本r-0就是 0号分片的副本分片

![image-20231016143113264](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161431150.png)

如果说我现在要插入一条文档，文档的id 为 1 。请求到达了node1。这是node1就充当了协调节点的角色

协调角色要用上述的哈希运算了，假设说文档id为1哈希运算结果为2，那就意味着它要存储到2号分片

![image-20231016143543105](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161435085.png)

于是就把请求路由到node3节点了 

![image-20231016143719016](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161437775.png)

然后就把数据写入到2号分片了，写入后发现自己是老大还是主分片，所以它就要把主分片数据同步给从分片。从分片的副本分片在 1号分片中 于是请求就同步过去了

这样主分片和副本分片就都保存了这个数据了

![image-20231016144022919](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161440114.png)

最终把相应结果返回给协调节点，然后再返回给用户

![image-20231016144216794](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161442717.png)

如果是id去查询也是相同的道理，删除，修改都是如此。所以凡是根据id操作都是这个流程

但是上面代码中进行查询的时候我们使用的是如下代码：

```json
{
    "explain" : true,
    "query": {
        "match_all": {}
    }
}
```

当发送请求时，并不知道要查询的数据在哪个分片上，没有id啊，那这种情况下协调节点又是怎么工作的呢？

那这就是分布式查询的过程了：

elasticsearch的查询分成两个阶段：

-  scatter phase：分散阶段，coordinating node会把请求发到每一个分片

   每个分片都查询了后进入聚集阶段

-  gather phase：聚集阶段，coordinating node汇总data node的搜索结果，并处理为最终结果集返回给用户

协调节点为虚线，表示这里的协调节点可以是三个节点中任意一个，因为默认情况下每个节点都是协调节点

![image-20231016144743512](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161447068.png)

>  **总结**：
>
>  分布式新增如何确定分片？
>
>  -  coordinating node根据id做hash运算，得到结果shard数量取余，余数就是对应的分片
>
>  分布式查询：
>
>  -  分散阶段：coordinating node将查询请求分发给不同分配
>  -  收集阶段：将查询结果汇总到coordinating node，整理并返回给用户

##### 9.10.4.8、ES集群的故障转移:evergreen_tree: 

集群的master节点会监控集群中的节点状态，如果发现有节点宕机，会立即将宕机节点的分片数据迁移到其它节点，确保数据安全，这个叫做故障转移。

比方说，有集群三个节点node1，node2，node3每个节点上都有分片目前主节点是node1。

![image-20231016145745390](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161457402.png)

现在假设说其中一个节点出现了故障，比如说node1主节点挂了。此时两个候选节点就会选出一个主节点

![image-20231016150004147](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161500046.png)

比方说选中了node2，这时集群就有主了，主节点就要发挥自己的工作了。它要查看下集群的健康状态了，分片状态。

它一看这不行啊，因为p-1有主分片，但是没有副本分片。p-2有主分片有副本分片。p-0只有副本分片，没有主分片。

那也就是说 1 号  分片 和 0 号分片的数据是不安全的，因为只有一份一旦挂了就完了。

这时集群的状态就不是健康的了，而是处于危险的边缘

这时主节点就会做个措施。它会检查挂掉的节点它上面有什么分片，然后把它迁移到健康的节点上。确保任何一个分片都至少有两份，也就是一个主分片，一个副本分片。

![image-20231016150711670](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161507621.png)

我们看下控制台

现在es01是主节点，集群状态是健康的，我们把es01给它停掉

![image-20231016151046860](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161510371.png)

通过命令：`docker-compose stop es01` 这样主节点就被停掉了

```sh
[root@localhost ~]# docker-compose stop es01
Stopping es01 ... done
[root@localhost ~]#
```

我们再去看控制台，发现主变成了es02，并且把数据进行了迁移

![image-20231016151445895](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161514231.png)

那么数据有没有丢失呢，我们搜索查看一下：

```json
{
    "query": {
        "match_all": {}
    }
}
```

查询结果：

```json
{
    "took": 27,
    "timed_out": false,
    "_shards": {
        "total": 3,
        "successful": 3,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 3,
            "relation": "eq"
        },
        "max_score": 1.0,
        "hits": [
            {
                "_index": "itcast",
                "_type": "_doc",
                "_id": "5",
                "_score": 1.0,
                "_source": {
                    "title": "试着插入一条 id = 1"
                }
            },
            {
                "_index": "itcast",
                "_type": "_doc",
                "_id": "3",
                "_score": 1.0,
                "_source": {
                    "title": "试着插入一条 id = 1"
                }
            },
            {
                "_index": "itcast",
                "_type": "_doc",
                "_id": "1",
                "_score": 1.0,
                "_source": {
                    "title": "试着插入一条 id = 1"
                }
            }
        ]
    }
}
```

数据没有丢失

如果我再重启es01节点，执行命令：`docker-compose start es01`

```sh
[root@localhost ~]# docker-compose start es01
Starting es01 ... done
[root@localhost ~]#
```

再切回到控制台看下情况

我们发现es01又回来了，但是不是主了。但是现在的主节点又把数据给迁移回去了

确保 每一个分片上都会有数据，确保数据是均衡的

![image-20231016151901158](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161519914.png)

>  **总结**:
>
>  故障转移：
>
>  -  master宕机后，EligibleMaster选举为新的主节点。
>  -  master节点监控分片，节点状态，将故障节点上的分片转移到正常节点，确保数据安全
