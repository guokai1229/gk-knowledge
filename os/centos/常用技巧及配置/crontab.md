# crontab

## 简介

crontab命令常见于Unix和类Unix的操作系统之中，用于设置周期性被执行的指令。该命令从标准输入设备读取指令，并将其存放于“crontab”文件中，以供之后读取和执行。该词来源于希腊语 chronos(χρνο)，原意是时间。常，crontab储存的指令被守护进程激活， crond常常在后台运行，每一分钟检查是否有预定的作业需要执行。这类作业一般称为cron jobs。

## 使用说明

### 使用权限

root用户和crontab文件的所有者

### 语法

```
crontab [-e [UserName]|-l [UserName]|-r [UserName]|-v [UserName]|File]

```

### 日期格式

```
*　　*　　*　　*　　*　　command
分　 时　 日　 月　 周　 命令 
```

- 第1列表示分钟1～59 每分钟用*或者 */1表示
- 第2列表示小时1～23（0表示0点）
- 第3列表示日期1～31 
- 第4列表示月份1～12 
- 第5列标识号星期0～6（0表示星期天） 
- 第6列要运行的命令 

### 参数

- -e [UserName]: 执行文字编辑器来设定时程表，内定的文字编辑器是 VI，如果你想用别的文字编辑器，则请先设定 VISUAL 环境变数来指定使用那个文字编辑器(比如说 setenv VISUAL joe) 
- -r [UserName]: 删除目前的时程表 
- -l [UserName]: 列出目前的时程表 
- -v [UserName]: 列出用户cron作业的状态

### 例子

```
30 21 * * * /usr/local/etc/rc.d/lighttpd restart 
上面的例子表示每晚的21:30重启apache。 
45 4 1,10,22 * * /usr/local/etc/rc.d/lighttpd restart 
上面的例子表示每月1、10、22日的4 : 45重启apache。 
10 1 * * 6,0 /usr/local/etc/rc.d/lighttpd restart 
上面的例子表示每周六、周日的1 : 10重启apache。 
0,30 18-23 * * * /usr/local/etc/rc.d/lighttpd restart 
上面的例子表示在每天18 : 00至23 : 00之间每隔30分钟重启apache。 
0 23 * * 6 /usr/local/etc/rc.d/lighttpd restart 
上面的例子表示每星期六的11 : 00 pm重启apache。 
* */1 * * * /usr/local/etc/rc.d/lighttpd restart 
每一小时重启apache 
* 23-7/1 * * * /usr/local/etc/rc.d/lighttpd restart 
晚上11点到早上7点之间，每隔一小时重启apache 
0 11 4 * mon-wed /usr/local/etc/rc.d/lighttpd restart 
每月的4号与每周一到周三的11点重启apache 
0 4 1 jan * /usr/local/etc/rc.d/lighttpd restart 
一月一号的4点重启apache 
```
