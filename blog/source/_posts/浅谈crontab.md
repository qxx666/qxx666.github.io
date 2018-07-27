---
title: 浅谈crontab
date: 2018-07-27 09:17:41
tags:
---

#### 摘要：
我们在linux系统除了部署一些服务外，经常会做一些定时任务，以减少人工干预。
对于linux系统，有at工具和crontab工具以帮助我们定制我们的定时任务。对于at工具
是针对仅运行一次的计划任务，运行完后便退出系统。crontabs则提供周期性的执行定时任务，在
系统安装完成后这两个服务均是默认与系统一起安装了。下面就介绍crond服务的安装和使用。

<!---more--->

####  <font color=#0099ff > 查看crond服务并安装

1、查看crontabs服务软件是否安装
```commandline
[root@www at]# rpm -qa crontabs
crontabs-1.11-6.20121102git.el7.noarch
```

2、若没有返回信息，则可通过以下命令安装
```commandline
yum  install crontabs   //安装crontabs
```

3、查看crontab服务安装信息
```commandline
[root@www at]# rpm -ql crontabs
/etc/cron.daily     
/etc/cron.hourly
/etc/cron.monthly
/etc/cron.weekly
/etc/crontab
/etc/sysconfig/run-parts
/usr/bin/run-parts
/usr/share/man/man4/crontabs.4.gz
/usr/share/man/man4/run-parts.4.gz
```

####  <font color=#0099ff > crontab 各文件说明
```text
1、/etc/cron.deny         //该文件中所列用户不允许使用crontab命令
2、/etc/cron.allow        //该文件中所列用户允许使用crontab命令
3、/var/spool/cron/      //所有用户crontab文件存放的目录,以用户名命名
4、/etc/crontab            //用户所建立的crontab文件中
5、/etc/cron.daily/         //该目录下定义用户每天执行的任务脚本
6、/etc/cron.hourly/      //该目录下定义用户每小时要执行的任务脚本
7、/etc/cron.monthly/   //该目录下定义用户每月执行的任务脚本
8、/etc/cron.weekly/    //该目录下定义用户每周执行的任务脚本

```

#### <font color=#0099ff > 详解/etc/crontab文件
用户所建立的crontab文件中，每一行都代表一项任务，每行的每个字段代表一项设置，
它的格式共分为六个字段，前五段是时间设定段，第六段是要执行的命令段，

1、查看一下该文件内容
```commandline
[root@www at]# cat /etc/crontab
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
```
2、说明
```text
minute： 表示分钟，可以是从0到59之间的任何整数。
hour：表示小时，可以是从0到23之间的任何整数。
day：表示日期，可以是从1到31之间的任何整数。
month：表示月份，可以是从1到12之间的任何整数。
week：表示星期几，可以是从0到7之间的任何整数，这里的0或7代表星期日。
command：要执行的命令，可以是系统命令，也可以是自己编写的脚本文件。
```
![](/assets/img/cronatb/crontab-list.png)

```text
在以上各个字段中，还可以使用以下特殊字符：
星号（*）：代表所有可能的值，例如month字段如果是星号，则表示在满足其它字段的制约条件后每月都执行该命令操作。
逗号（,）：可以用逗号隔开的值指定一个列表范围，例如，“1,2,5,7,8,9”
中杠（-）：可以用整数之间的中杠表示一个整数范围，例如“2-6”表示“2,3,4,5,6”
正斜线（/）：可以用正斜线指定时间的间隔频率，例如“0-23/2”表示每两小时执行一次。同时正斜线可以和星号一起使用，例如*/10，如果用在minute字段，表示每十分钟执行一次。
```

#### <font color=#0099ff > crontab命令详解
1、命令格式
```text
crontab [-u user] file
crontab [-u user] [ -e | -l | -r ]
-u user：用来设定某个用户的crontab服务，例如，“-u ixdba”表示设定ixdba用户的crontab服务，此参数一般有root用户来运行。
 file：file是命令文件的名字,表示将file做为crontab的任务列表文件并载入crontab。如果在命令行中没有指定这个文件，crontab命令将接受标准输入（键盘）上键入的命令，并将它们载入crontab。
-e：编辑某个用户的crontab文件内容。如果不指定用户，则表示编辑当前用户的crontab文件。
-l：显示某个用户的crontab文件内容，如果不指定用户，则表示显示当前用户的crontab文件内容。
-r：从/var/spool/cron目录中删除某个用户的crontab文件，如果不指定用户，则默认删除当前用户的crontab文件。
-i：在删除用户的crontab文件时给确认提示

```

#### <font color=#0099ff >一些例子

```command
crontab -uroot -e
3,15 * * * * command //在上午8点到11点的第3和第15分钟执行

01   *   *   *   *    root run-parts /etc/cron.hourly   //run-parts这个参数了，如果去掉这个参数的话，后面就可以写要运行的某个脚本名，而不是目录名了

```

#### 注意事项
有时我们创建了一个crontab，但是这个任务却无法自动执行，而手动执行这个任务却没有问题，这种情况一般是由于在crontab文件中没有配置环境变量引起的。
在crontab文件中定义多个调度任务时，需要特别注意的一个问题就是环境变量的设置，因为我们手动执行某个任务时，是在当前shell环境下进行的，程序当然能找到环境变量，而系统自动执行任务调度时，是不会加载任何环境变量的，因此，就需要在crontab文件中指定任务运行所需的所有环境变量，这样，系统执行任务调度时就没有问题了。
不要假定cron知道所需要的特殊环境，它其实并不知道。所以你要保证在shelll脚本中提供所有必要的路径和环境变量，除了一些自动设置的全局变量，所以注意如下3点：
```
1、脚本中涉及文件路径时写全局路径；
2、脚本执行要用到java或其他环境变量时，通过source命令引入环境变量，如：
cat start_cbp.sh
#!/bin/sh
source /etc/profile
export RUN_CONF=/home/d139/conf/platform/cbp/cbp_jboss.conf
/usr/local/jboss-4.0.5/bin/run.sh -c mev &
3、当手动执行脚本OK，但是crontab死活不执行时。这时必须大胆怀疑是环境变量惹的祸，并可以尝试在crontab中直接引入环境变量解决问题。如：
0 * * * * . /etc/profile;/bin/sh /var/www/java/audit_no_count/bin/restart_audit.sh
```
 注意清理系统用户的邮件日志

每条任务调度执行完毕，系统都会将任务输出信息通过电子邮件的形式发送给当前系统用户，这样日积月累，日志信息会非常大，可能会影响系统的正常运行，因此，将每条任务进行重定向处理非常重要。
例如，可以在crontab文件中设置如下形式，忽略日志输出：

0 */3 * * * /usr/local/apache2/apachectl restart >/dev/null 2>&1

“/dev/null 2>&1”表示先将标准输出重定向到/dev/null，然后将标准错误重定向到标准输出，由于标准输出已经重定向到了/dev/null，因此标准错误也会重定向到/dev/null，这样日志输出问题就解决了。

##### 其他注意事项
新创建的cron job，不会马上执行，至少要过2分钟才执行。如果重启cron则马上执行。

当crontab突然失效时，可以尝试/etc/init.d/crond restart解决问题。或者查看日志看某个job有没有执行/报错tail -f /var/log/cron。

千万别乱运行crontab -r。它从Crontab目录（/var/spool/cron）中删除用户的Crontab文件。删除了该用户的所有crontab都没了。

在crontab中%是有特殊含义的，表示换行的意思。如果要用的话必须进行转义\%，如经常用的date ‘+%Y%m%d’在crontab里是不会执行的，应该换成date ‘+\%Y\%m\%d’。
