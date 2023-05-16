---
title: windows
categories: 工作
tags: 
- windows
---



cmd 切换系统盘 ： G： enter键

打开注册表：regedit

查看mysql 服务：sc query mysql

删除mysql服务：sc delete mysql



1. 找到需要删除的目录文件 cmd  输入命令

   for /r %i in (*.lastUpdated) do del %i 

UserArchetypes.xml

编辑器网址

https://notepad-plus-plus.org/

## 端口
>查询端口 netstat -ano |findstr "62001"
>taskkill /f /t /im "15936" 
>taskkill /pid 26308

## 服务 
> 开启服务net start 【serviceName】
> 删除服务 sc delete 【serviceName】

