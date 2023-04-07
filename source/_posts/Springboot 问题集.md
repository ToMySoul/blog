---
title: springboot 问题集
categories: 工作
tags: 
- springboot
- idea
- 问题集
---

# Springboot 问题集
## 多个module配置加载问题
多个子module config 加载问题，启动类的module 扫描不到其他module配置文件 将其他module 文件作为依赖包给 启动类module
 ---
##  spring-boot-maven-plugin  Unable to find main class 子项目打包问题
原因：父类spring-boot-maven-plugin 会扫描springboot 启动类，子类没有对应的启动类导致报错。
思路：
1. 父类该依赖限定作用域。
```
       <!-- jar运行配置 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>${springboot.version}</version>
            <scope>test</scope>
        </dependency>
```
2. 子类不加载该依赖。




---
## Maven
### pom文件名词解释
scop 参数值

| 依赖范围 | 对于编译classpath有效 | 对于测试classpath有效 | 对于运行classpath有效 | 参与打包 |             例子              |
| :------: | :-------------------: | :-------------------: | :-------------------: | :------: | :---------------------------: |
| compile  |           √           |           √           |           √           |    √     |          Spring-core          |
|   test   |           ×           |           √           |           ×           |    ×     |             Junit             |
| provided |           √           |           √           |           ×           |    ×     |       JDK、Servlet-api        |
| runtime  |           ×           |           √           |           √           |    √     |        Jdbc驱动实现类         |
|  system  |           √           |           √           |           ×           |    ×     | 本地的、Maven仓库外的类库文件 |







