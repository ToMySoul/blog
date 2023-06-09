---
title: oracle
categories: 工作
tags: 
- oracle
- 数据库
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


### Linux java 部署
安装包下载地址
>https://download.oracle.com/otn/java/jdk/8u361-b09/0ae14417abb444ebb02b9815e2103550/jdk-8u361-linux-x64.tar.gz?AuthParam=1681278056_b4c2ae20f5d3113832792b9893466b61
解压
tar -zxvf [安装包]

配置环境变量
vim /etc/profile

```
export JAVA_HOME=/home/java/jdk1.8.0_361  #jdk安装目录
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
export PATH=$PATH:${JAVA_PATH}
```

通过命令source /etc/profile让profile文件立即生效
java  -version

