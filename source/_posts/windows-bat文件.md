---
title: windows-bat文件
date: 2023-04-13 14:30:22
categories: windows
tags:
- windows
- bat
---

## bat 打开 cmd 跳转某个目录并执行某些命令

>start cmd /k "命令1 & 命令2 & 命令3"    (无论前面命令是否成功, 后面都会执行)
>start cmd /k "命令1 && 命令2 && 命令3 " (仅当前面命令成功时, 才执行后面)
>start cmd /k "命令1 || 命令2 || 命令3"  (仅当前面命令失败时. 才执行后面)

