---
title: docker常用命令
date: 2023-04-06 13:21:41
categories: 工作
tags: 
- docker
- 常用命令
---


## 镜像
### 镜像
> 查看镜像列表
> docker  images 
> 删除镜像
> docker rmi [镜像名称]
>
## 容器
> 容器列表
> docker ps -a
> docker ps
> 删除容器
> docker rm [容器名/容器id]
> 进入容器
> docker exec -it [容器名/容器id] bash 
> 退出容器
> ctrl+d
>
###  查看容器日志
> 查看最近10条日志, 并持续打印
> docker logs -f --tail 10 【容器名称】
> -f : 跟踪日志输出
> -t : 显示时间戳
> --tail :仅列出最新N条容器日志
> -- since :显示某个日期至今的所有日志 
 

## 挂载
> docker run --name mysql -p 3306:3306 -v /home/mysql/conf/my.cnf:/etc/mysql/my.cnf -v /home/mysql/data:/var/lib/mysql -v /home/mysql/logs:/logs -e MYSQL_ROOT_PASSWORD=admin123456 --restart=always -d mysql
>
|  指令   | 目的     |
| ---- | ---- |
| -- name   | 为你运行的镜像命名     |
|  -p  |  映射端口 虚拟机端口 : docker端口   |
|  -v  |  挂载目录/文件 虚拟机目录/文件:docker目录/文件    |
|  -d  |  守护进程后台运行    |
|  -it |  启动并运行    |
| --restart=always   |  在docker服务重启后,自动重启mysql服务,也可以吧docker 服务作为开机启动.    |


