---
title: Idea-问题集
date: 2023-04-06 14:56:53
categories: 工作
tags: 
- idea
- 问题集
---
## Idea问题集

###  插件库搜索不到调整代理
>Settings
>Plugins
>设置
>HttpProxySettings
>CheckConnection
>https://plugins.jetbrains.com/
### idea 依赖变灰色引用问题
在setting 找到 ignorefile

### 本地依赖导入
```
mvn install:install-file -DgroupId=[项目包名groupId] -DartifactId=[工程名artifactId] -Dversion=[jar 包版本号] -Dpackaging=jar -Dfile=[jar包本地路径path] -DgeneratePom=true
例：mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0.4 -Dpackaging=jar -Dfile=ojdbc6-11.2.0.4.jar -DgeneratePom=true
<dependency>
  <groupId>com.oracle</groupId>
  <artifactId>ojdbc6</artifactId>
  <version>11.2.0.4</version>
</dependency>
  
```

依赖传递问题
```
 pomA ,pomB   pomB 依赖于 pomA  不想引用pomA 某个jar 
 则设置属性 <optional>true</optional> <!-- 表示依赖不会传递 -->
 
 注：如果A为父依赖，则该optional 不生效。 
 该依赖为C依赖必须则将该依赖添加到C中即可，去除掉父pom该依赖。
```


