---
title: Cmd命令大全
date: 2016-01-02
comments: true
categories: note
description: 这里是我平常用的终端命令，怕忘记就在这里留下个印记
---

### 【mac】

|命令|功能描述|语句|
|:-----|:-------|:-------|
|目录操作|  |  |
| mkdir | 创建一个目录 | mkdir dirname |
| rmdir | 删除一个目录 | rmdir dirname |
| mvdir | 移动或重命名一个目录 | mvdir dir1 dir2 |
| chmod | 给目录赋予权限 | chmod dir1 777 |
| pwd | 显示当前目录的路径名 | pwd |
| ls | 显示当前目录的内容 | ls -la |
|文件操作|  |  |
| cat | 显示或连接文件 | cat filename |
| od | 显示非文本文件的内容 | od -c filename |
| cp | 复制文件或目录 | cp file1 file2 |
| rm | 删除文件或目录 | rm -rf filename |
| mv | 改变文件名或所在目录 | mv file1 file2 |
| find | 使用匹配表达式查找文件 | find /或. -name &quot;*.c&quot; -print |
| file | 显示文件类型 | file filename |
|选择操作|  |  |
| head | 显示文件的最初几行 | head -20 filename |
| tail | 显示文件的最后几行 | tail -15 filename |
| diff | 比较并显示两个文件的差异 | diff file1 file2 |
| sort | 排序或归并文件 | sort -d -f -u file1 |
| uniq | 去掉文件中的重复行 | uniq file1 file2 |
| comm | 显示两有序文件的公共和非公共行 | comm file1 file2 |
| wc | 统计文件的字符数、词数和行数 | wc filename |
| nl | 给文件加上行号 | nl file1 >file2 |
| 进程操作 |  |  |
| lsof | 显示该端口号下进程 | lsof -i:9000 |
| ps | 显示进程当前状态 | ps u |
| kill | 终止进程 | kill -9 9000 |
| 时间操作 |  |  |
| date | 显示系统的当前日期和时间 | date |
| cal | 显示日历 | cal 8 1996 |
| time | 统计程序的执行时间 | time a.out |
| 网络与通信操作 |  |  |
| telnet | 远程登录 | telnet hpc.sp.net.edu.cn |
| rlogin | 远程登录 | rlogin hostname -l username |
| rsh | 在远程主机执行指定命令 | rsh f01n03 date |
| ftp | 在本地主机与远程主机之间传输文件 | ftp ftp.sp.net.edu.cn |
| rcp | 在本地主机与远程主机之间复制文件 | rcp file1 host1:file2 |
| ping | 给一个网络主机发送 回应请求 | ping hpc.sp.net.edu.cn |
| mail | 阅读和发送电子邮件 | mail |
| write | 给另一用户发送报文 | write username pts/1 |
| Korn Shell 命令 |  |  |
| history | 列出最近执行过的 几条命令及编号 | history |
| r | 重复执行最近执行过的 某条命令 | r -2 |
| alias | 给某个命令定义别名 | alias del=rm -i |
| unalias | 取消对某个别名的定义  | unalias del |
| 其它命令 |  |  |
| uname | 显示操作系统的有关信息 | uname -a |
| clear | 清除屏幕或窗口内容 | clear |
| env | 显示当前所有设置过的环境变量 | env |
| who | 列出当前登录的所有用户 | who |
| whoami | 显示当前正进行操作的用户名 | whoami |
| df /tmp | 显示文件系统的总空间和可用空间 |  |


### 【windows】

命令-功能描述
netstat -ano 【查看所有的端口占用情况】
netstat -aon|findstr "3000" 【查看指定端口的占用情况】
tasklist|findstr "2017" 【查看PID对应的进程】
taskkill /f /t /im xxx.exe 【结束该进程】
