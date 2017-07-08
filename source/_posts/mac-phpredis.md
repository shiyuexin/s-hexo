---
title: Mac下安装php-redis扩展
date: 2016-11-11
comments: true
categories: mac
description: 开始在 PHP 中使用 Redis 前， 我们需要确保已经安装了 redis 服务及 PHP redis 驱动，且你的机器上能正常使用 PHP。
---

## Quick Start
### 一 、下载redis

Down [http://pecl.php.net/package/redis](http://pecl.php.net/package/redis)

### 二、解压安装并进入redis目录

``` bash
$ tar xzf redis-2.2.7.tgz
$ cd redis-2.2.7
```

### 三、在redis文件夹下，生成configure配置文件
#### 注意:(如果存在多个版本PHP,需查找phpize所需路径)
``` bash
$ /usr/local/php/bin/phpize
```
#### 成功出现:
``` bash
Configuring for:
PHP Api Version: 20090626
Zend Module Api No: 20090626
Zend Extension Api No: 220090626
```
### 四、安装(要用root用户)
``` bash
$ ./configure –with-php-config=/usr/local/php/bin/php-config 或(./configure)
$ make
$ make install
```
### 五、在php配置文件php.ini里加载redis扩展
#### 注意:(resis.so安装路径是否在默认路径,否"="后面用绝对路径)
``` bash
extension=redis.so
```

#### 重启php-fpm
#### 查看扩展安装情况
``` bash
$ php -m |grep redis
```
#### 或者在PHP工程目录的index.php顶端
``` bash
<?php phpinfo(); die;?>
```

#### 至此redis扩展应该安装成功,相关命令
##### 查找文件(path路径,'/'为根目录)
``` bash
$ find path -name "xxx"
```