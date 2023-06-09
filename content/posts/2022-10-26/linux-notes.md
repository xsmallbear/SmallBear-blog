---
title: "Linux 避坑笔记"
date: 2022-10-26
type: "post"
tags: ["database"]
showTableOfContents: true
---

# 源
apt官方软件源位于`/etc/apt/sources.list`下\
`cat /etc/apt/sources.list | grep -v "#"` 查看过滤过注释的源列表

[USTC Mirror 网站](https://mirrors.ustc.edu.cn/help/debian.html#id5)

``` bash
sudo sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
```

更改完 sources.list 文件后运行 `sudo apt-get update` 更新索引以生效。

# 查看文件内容
`cat FILE` 输出文件的全部内容\
`cat FILE1 FILE2` 连接两个文件并输出\
`less FILE` 使用less翻页器显示文件内容

less快捷键
| 按键     | 效果                              |
| -------- | --------------------------------- |
| d/u      | 向下/上滚动半页（down/up）        |
| f/d      | 向下/上滚动一整页（forward/back） |
| g / G    | 跳转到文件开头/结尾               |
| j        | 向下移动一行                      |
| k        | 向上移动一行                      |
| /PATTERN | 在文件中搜索 PATTERN              |
| n / N    | 跳转到下一个/上一个找到的 PATTERN |
| q        | 退出                              |

# 进程标识符(PID)
- 进程标识符(PID, Process Identifier)是一个数字,要对进程做任意操作需要使用这个PID

# nice和PRI
- nice 操作系统调度线程的优先程度
- priority 操作系统内部使用的调度标志
```bash
$ nice -10 vim #use 10 nice run vim
$ renice -n 10 -p 12345 # 设置pid12345的线程nice为-10
```

# 进程状态
```bash
Status: 
R: running #正在跑
S: sleeping #可以被中断的睡眠
T: traced/stopped #被跟踪/挂起的进程
Z: zombie #僵尸进程
D: disk sleep #不能被中断的睡眠
```

# 前后台切换
**Ctrl+z** 把程序挂在后台暂停执行
``` bash
$ jobs #查看暂停在后台的任务
$ [1]    suspended  ping baidu.com
$ [2]  - suspended  vim
$ [3]  + suspended  htop
$ bg %2 #让2号任务后台继续运行
$ fg %3 #让3号任务在前台继续执行
```

# 进程信号
htop中选择进程摁下k可以选择信号
```bash
$ kill [signal-number] PID #如果没有参数会自动发送15 (SIGTERM)
``` 

# 服务管理
服务进程独立与用户的登入，不随用户的退出而被终止，这类工作与后台的进程称为守护进程(deamon)

`systemctl` 命令管理服务\
`systemctl status` 预览系统服务运行情况\
`systemctl list-units` 查看所有的服务内容

# 自定义服务
服务的配置文件们位于`/etc/systemd/system`下

``` conf
[Unit]
Description=Jupyter Notebook    # 该服务的简要描述

[Service]
PIDFile=/run/jupyter.pid        # 用来存放 PID 的文件
ExecStart=/usr/local/bin/jupyter-notebook --allow-root
                                # 使用绝对路径标明的命令及命令行参数
WorkingDirectory=/root          # 服务启动时的工作目录
Restart=always                  # 重启模式，这里是无论因何退出都重启
RestartSec=10                   # 退出后多少秒重启

[Install]
WantedBy=multi-user.target      # 依赖目标，这里指进入多用户模式后再启动该服务
```
配置文件名称就是服务的名字\
`systemctl start jupyter` 启动\
`systemctl stop jupyter` 停止\
`systemctl enable jupyter` 设置开机启动\
`systemctl disable jupter` 取消开机启动

# 列行性任务
`at`单次计划任务
```bash
$ at now + 1min
> echo "hello">
> <EOT> （按下 Ctrl + D）
```
at会将stdout和stderr的内容以邮件的方式发送给用户

`crontab `周期性计划任务
```bash
# 分 时 日 月 星期 命令
* * * * * echo"hello" >> ~/count #每分钟输出hello到count文件下
```

# 用户信息保存
系统中的用户配置信息保存在`/etc/passwd`文件下\
每一行都是一个用户，信息由 `:` 隔开
``` conf
root:x:0:0:root:/root:/bin/bash

# 用户名
# 可选的加密后的密码
# 数字用户ID
# 数字组ID
# 用户名和注释字段
# 用户主目录
# 可选的用户命令解释器
```
用户的密码的哈希信息保存在`/etc/shadow`里
```conf
ustc:$6$UN2k0DcnU7QtJ2jW$NKcO3Tbem.3wte5tp5o.P6TDFxx5jaYu0lHFy9qNp4XehBGP/69HwtOYaE/s7PQPKKVCJZCnVJQh2A6.rN4oJ.:18948:0:99999:7:::
```

# 用户分类
1. root用户
3. 系统用户
2. 普通用户 home目录位于/home/下

`su username`可以换用户，如果没有参数切换成root用户\
`sudo -u username command`以另一个用户的身份执行命令

Ubuntu等linux发行版默认禁止了root用户的登入\
可以使用`sudo su`得到一个root用户权限的shell

# 用户组
用户组可以为一批用户一次性设置权限\
使用`groups`命令查看自己所在的用户组\
用户组有自己的编号:GID (Group ID)

# 命令行的用户操作
修改密码`passwd username`如果没有参数则修改自己的密码




