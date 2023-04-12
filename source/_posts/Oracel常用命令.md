---
title: Oracel常用命令
date: 2023-04-10 09:19:24
categories: Oracel
tags:
- oracle 
- 常用命令
---

##  常用指令

### 查看表空间
> select t1.name,t2.name from v$tablespace t1,v$datafile t2 where t1.ts#=t2.ts#;

### 常见表空间
> 创建名为HSP的表空间存储路径及文件名为/../../HSP.dbf 初始大小为100MB 自动扩容 扩容大小为10mb
> create tablespace HSP datafile '/u01/app/oracle/oradata/XE/HSP.dbf' size 100m autoextend on next 10m;

### 创建用户并分配数据库
> create user HSP identified by 123456 default tablespace HSP;

### 授予用户dba权限
> grant connect,resource,dba to HSP

### 连接数据库
> 进入到sqlplus 环境不登录到数据库，如果关闭了数据库就需要进入到sqlplus 环境启动数据库
> sqlplus /nolog; 
> 
> 
> 关闭数据库
> 
