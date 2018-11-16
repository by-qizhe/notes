# 简述
crond的概念和crontab是不可分割的。crontab是一个命令，常见于Unix和类Unix的操作系统之中，用于设置周期性被执行的指令。该命令从标准输入设备读取指令，并将其存放于“crontab”文件中，以供之后读取和执行。该词来源于希腊语chronos(χρόνος)，原意是时间。而crond正是它的守护进程

# 使用教程
## 安装
yum install vixie-cron
yum install crontabs

## 启动命令
service crond status    // 查看状态
service crond start
service crond stop
service crond restart
service crond reload
chkconfig –level 35 crond on    // 设置开机自启

## 设置执行命令
#### 文件式配置
vim /etc/crontab
默认文件格式：
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

注释中已经很解释得很好，看下几个例子：
30 5 * * * ls   # 每天的5:30执行ls命令
* * 2 * * ls    # 每月2号执行ls命令

> 修改文件完毕后，需重新载入配置才会生效

#### 命令式配置
执行crontab -e后，写入命令，最后保存
其保存路径在/var/spool/cron/当前用户名
