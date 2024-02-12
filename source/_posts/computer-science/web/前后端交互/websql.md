---
title: 初始websql
categories:
   - [计算机学科,web,前后端交互]
tags:
   - 计算机学科
   - web
   - 前后端交互
---

创建数据库

```sql
var db = openDatabase (`person`,1,`person`,0)
```

![image-20230529154635137](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040755693.png)

创建表

```sql
db.transaction(tx=>{
    tx.executeSql('create table if not exists student (id unique,name)')
})
```

![image-20230529154933720](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040755846.png)

向表中添加数据

```sql
db.transaction(tx=>{
    tx.executeSql('insert into student (id,name) values (?,?)' , [1,'张三'])
    tx.executeSql('insert into student (id,name) values (?,?)' , [2,'李四'])
})
```

![image-20230529155239715](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040755867.png)

查询

```sql
db.transaction(tx=>{
    tx.executeSql('select * from student',[],(tx,res)=>{
        let rows = res.rows
        let end = rows.length
        for(let i = 0;i < end;i++){
            console.log(rows.item(i))
        }
    })
})
```

​	![image-20230529155622959](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040755870.png)