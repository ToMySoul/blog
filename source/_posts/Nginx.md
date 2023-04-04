---
title: Nginx
tags: 
- nginx
---
# Nginx 

docker run -itd   --name nginx \-v /home/nginx/conf.d:/etc/nginx/conf.d  -v /home/nginx/conf:/etc/nginx/conf   -v /home/nginx/html:/etc/nginx/html \-v  /home/nginx/log:/usr/log/nginx \-p 8080:80  nginx

