---
title: oracle
tags: 
- oracle
---


 oracle依赖 打包问题

```
mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0.4 -Dpackaging=jar -Dfile=ojdbc6-11.2.0.4.jar -DgeneratePom=true
```



## 常用命令





数据类型修改不能由小改大，同等类型长度可以扩容

字符串设置默认值需要英文引号包起来

```
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

Oracle数据库，用mybatis-gen.xml 自动生成Java对象的时候，会根据number类型的长度不同生成不同的数据类型

| number长度 | Java类型   |
| ---------- | ---------- |
| 1~4        | Short      |
| 5~9        | Integer    |
| 10~18      | Long       |
| 18+        | BigDecimal |



#### 主键自增问题

mybaits-plus  

1.配置config 

```
@Bean
public OracleKeyGenerator oracleKeyGenerator(){
    return  new OracleKeyGenerator();
}
```

实体类增加注解；value 值 为oracle 增加的序列  sequence

``` 
@KeySequence(value = "PAT_MASTER_INDEX")
```

alter table  【表明】 add constraint 【主键名】 primary key (【字段名】);
