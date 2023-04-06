---
title: Linux
date: 2023-04-06 11:08:26
categories: 工作
tags:
- Linux 
---
# Linux 

 查看centos 版本： cat /etc/redhat-release 


system   oracle 

sys   oracle



ps -ef|grep xxx 显示进程 p

> rm -rf  [文件夹路径]
>
> rm -f [文件路径+文件名]

## 部署docker 

1. 检查内核版本，大于3.10即可

   

   ```
   uname -r
   ```

2. 使用sudo或root权限的用户登陆终端

3. 安装Docker依赖的工具

   

   ```
   yum install -y yum-utils device-mapper-persistent-data lvm2
   ```

4. 添加阿里云的yum源

   

   ```
   yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
   ```

5. 更新yum源

   

   ```
   yum update -y
   ```

6. 安装Docker（CE社区版）

   

   ```
   yum -y install docker-ce
   ```

7. 查看docker是否安装成功

   ```
   docker version
   ```

8. 启动docker服务

```
systemctl start docker
```

9. 查看docker是否启动成功

```
docker info
```

systemctl enable docker.service

## docker 

### mysql 

下载镜像：

yum install mysql:latest 

挂载容器

docker run --name mysql -p 3306:3306 -v /home/mysql/conf/my.cnf:/etc/mysql/my.cnf -v /home/mysql/data:/var/lib/mysql -v /home/mysql/logs:/logs -e MYSQL_ROOT_PASSWORD=admin123456 --restart=always -d mysql

连接数据库，需要开放服务器数据库端口

参数解释

-- name 为你运行的镜像命名

-p 映射端口 虚拟机端口 : docker端口

-e 为mysql设置密码

-- privileged=true 为mysql获取root权限

-v 挂载目录/文件 虚拟机目录/文件:docker目录/文件

-d 守护进程后台运行

-it 启动并运行

--restart=always 在docker服务重启后,自动重启mysql服务,也可以吧docker 服务作为开机启动.

### java 

tar zxvf  【jdk压缩包】

配置环境变量

> vim /etc/profile

尾部添加信息

> export JAVA_HOME=/usr/local/jdk/jdk1.8.0_361
> export CLASSPATH=$:CLASSPATH:$JAVA_HOME/lib/
> export PATH=$PATH:$JAVA_HOME/bin

刷新环境配置

> source /etc/profile 

### 容器

进入容器：


docker  exec -it 【容器id】 /bin/bash

ctrl +d 退出容器

#### 容器日志

### oracle

> 

> docker run -d akaiot/oracle_11g -it -p 1521:1521 --name oracle11g --restart=always   -v /home/oracle/data:/home/oracle/app/oracle/oradata  --mount source=oracle_vol 



进入数据库

## 其他


find  -name 【文件夹】


## Nginx

下载压缩包 http://nginx.org/en/download.html

上传安装包

/usr/local/src

解压 

tar -xvf 【压缩包】

安装nginx 服务器

在nginx 目录下

>  yum -y install gcc pcre-devel zlib-devel openssl openssl-devel

./configure 

在nginx 压缩包根目录 执行命令

> make 
>
> make install 

找到nginx 工作目录 whereis nginx 

通常在 :usr/local/nginx 

来到工作目录下./

1.启动命令: ./nginx
2.重启命令: ./nginx -s reload
3.关闭命令: ./nginx -s stop

前端静态资源放在nginx 根目录下 dist 

ps : 代理需要域名解析

nginx 删除

ps -ef|grep nginx





 oracle依赖 打包问题

```
mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0.4 -Dpackaging=jar -Dfile=ojdbc6-11.2.0.4.jar -DgeneratePom=true
```



PL/SQL 工具类

product code: ke4tv8t5jtxz493kl8s2nn3t6xgngcmgf3

serial Number: 264452

password: xs374ca

 查看linux 服务列表 

systemctl list-unit-files 



三、systemctl参数说明
1、使用语法
用法：systemctl [OPTIONS…] {COMMAND} …

2 、参数说明
参数	参数说明
start	立刻启动后面接的unit
stop	立刻关闭后面接的unit
restart	立刻关闭后启动后面接的unit，亦即执行stop再start的意思
reload	不关闭后面接的unit的情况下，重载配置文件，让设定生效
enable	设定下次开机时，后面接的unit会被启动
disable	设定下次开机时，后面接的unit 不会被启动
status	目前后面接的这个unit 的状态，会列出是否正在执行、是否开机启动等信息。
is-active	目前有没有正在运行中
is-enable	开机时有没有预设要启用这个unit
kill	不要被kill这个名字吓着了,它其实是向运行unit的进程发送信号
show	列出unit的配置。
mask	注销unit,注销后你就无法启动这个unit了
unmask	取消对unit的注销
list-units	依据unit列出目前有启动的unit。若加上–all才会列出没启动的。（等价于无参数）
list-unit-files	列出所有以安装unit以及他们的开机启动状态（enabled、disabled、static、mask）。
–type=TYPE	就是unit type，主要有service，socket，target等
get-default	取得目前的 target
set-default	设定后面接的 target 成为默认的操作模式
isolate	切换到后面接的模式
————————————————
版权声明：本文为CSDN博主「恒悦sunsite」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/carefree2005/article/details/125886811
