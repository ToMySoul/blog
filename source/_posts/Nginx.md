---
title: Nginx
categories: 工作
tags: 
- nginx
- 反向代理
---
# Nginx 

docker run -itd   --name nginx \-v /home/nginx/conf.d:/etc/nginx/conf.d  -v /home/nginx/conf:/etc/nginx/conf   -v /home/nginx/html:/etc/nginx/html \-v  /home/nginx/log:/usr/log/nginx \-p 8080:80  nginx

