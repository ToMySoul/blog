---
title: Linux常用命令
date: 2023-04-06 11:08:26
categories: 工作
tags:
- Linux 
- 常用命令
---

## 文件
### 文件查找
> find -name [文件名]
> find -name  * [文件名] *
> find [路径] -name [文件名] 
>例： find / -name nginx 


### 文件删除
> rm -rf  [文件夹路径]
> rm -f [文件路径+文件名]

### 文件生成
> echo "[内容]" > [文件名+后缀]
> echo "hello world" > helloworld.txt

### 文件编辑vim
> 保存所有文件退出
>:wq 
> 强制退出
>:q!
>调到最后一行
>:$ 
>调到第一行
>:1 

### 文件压缩解压
> 文件解压
> tar -zxvf 【压缩包.tar.gz】


## 系统
### 服务
> 查看服务列表
> systemctl list-unit-files 
> 启动 | 停止 | 重启
> systemctl start | stop | restart [服务名称]
> 设置开机服务自启
> systemctl enable [服务名.service]
> 取消开机服务自启
> systemctl disable [服务名.service]
> 查看服务开启是否自启
> systemctl is-enabled [服务名.service]

### 进程  
  > 根据端口查找占用的进程  
  > netstat -nlp | grep [端口号]
  > lsof -i:[端口号]
  
