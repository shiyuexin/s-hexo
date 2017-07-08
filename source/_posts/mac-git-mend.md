---
title: Mac下Git命令自动补全
date: 2016-11-18
comments: true
categories: mac
description: 在Mac上git默认不支持命令自动补全,对于习惯了git bash补全的程序猿来说的确痛苦，毕竟可以少打那么那么多字母，接下来跟我走
---


## Quick Start

### 一 、安装 bash-completion

``` bash
$ brew install bash-completion
# 完成后-查看
$ brew info bash-completion
# 会输出类似以下内容
==> Caveats
Add the following lines to your ~/.bash_profile:
  if [ -f $(brew --prefix)/etc/bash_completion ]; then
    . $(brew --prefix)/etc/bash_completion
  fi

```
将if…then…那一句添加到~/.bash_profile（如果没有该文件，新建一个）
重启终端

### 二、拷贝文件、设置路径

访问网站 [https://github.com/git/git.git](https://github.com/git/git.git)
找到 ”contrib/completion/” 目录下的git-completion.bash ，
然后点击编辑，拷贝其内容，复制到文本文件，保存为 git-completion.bash 文件 
然后将文件用命令拷贝到 ～/ 目录下
``` bash
$ cp xxx/git-completion.bash ~/.git-completion.bash
```
xxx 为文件所在目录，注意拷贝后的文件名称为 .git-completion.bash

在~/.bashrc文件（该目录下如果没有，新建一个）中添加下边的内容
``` bash
$ source ~/.git-completion.bash
```

### 三、 启动: 终端输入

``` bash
$ source ~/.git-completion.bash
```

将下面这句话 添加到~/.bash_profile

``` bash
if [ -f ~/.git-completion.bash ]; then
   . ~/.git-completion.bash
fi
```

不添加想要补全起作用，每次都需要执行 
source ~/.git-completion.bash 
补全才能生效

