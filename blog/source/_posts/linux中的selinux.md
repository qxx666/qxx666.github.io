---
title: linux中的selinux
date: 2018-01-19 21:28:20
tags:
---
###### 摘要
### SELinux慨述和背景
selinux是整合到linux内核的一个模块，
security enhanced linux 安全强化的linux
- 当初设计的目标：避免资源的误用

  &nbsp;&nbsp;当初很多企业发现，通常系统出现问题的原因大部分都在于“内部员工资源的误用”所导致的，实际由外部攻击反而没那么严重，比如讲www目录权限设置为777
，在红帽RHEL7系统中启用SELinux技术的目的是为了让各个服务进程都受到约束，仅能获取到服务本应获取到的资源。例如您在自己的电脑上下载了一个美图软件，正在全神贯注得把自己P瘦点的时候，这款软件却在后台默默监听着浏览器中输入的密码信息，这显然不应该是这款美图软件本应做的事情（即便是访问电脑中的图片资源都情有可原），SELinux安全系统就是为了杜绝此类情况而设计的，它能够从多方面进行违法行为监控，首先是对服务进程进行功能限制，SELinux域限制技术让服务程序做不了出格的事情，其次还能够对文件进行资源限制，SELinux安全上下文让文件只能被所属于的服务程序所获取到。
- 传统的文件权限与账号关系：自主访问控制
- 以策略规则制定特定程序读取特定文件：委托访问控制，MAC

<!---more--->

### selinux的运行模式
SELinux是通过MAC的方式来控管进程，它控制的主题是进程，而目标则是进程能否读取的“文件资源”

- 主体（subject）
 - selinux主要想要管理的也就是进程，因此可以将“主体”跟进程画上等号
- 目标（object）
 - 主体进程能否访问的“目标资源”一般就是文件系统。因此可以将目标与文件系统画上等号
- 策略（policy）
 - 由于进程与文件数量庞大，因此selinuc会依据某些服务来制定基本的访问安全性策略。这些策略内还会有详细的规则（rule）来指定不同服务开放某些资源的访问与否。
 在目前的centos系统中主要提供两个策略
    - targeted：针对网络服务限制较多，针对本机限制较少，是默认策略
    - strict：完整的selinux限制，限制方面较为严格
    - 建议使用默认的targeted
- 安全上下文（security context）
 - 主体能不能访问目标除了策略指定之外，主体与目标的安全上下文必须一致才能够顺利访问。这个安全上下文有点类似文件系统的rwx，安全上下文设置很重要，一旦设置错误，某些服务可能就不能用。
- 运行过程
  - 主体程序在通过selinux策略内的规则放行后，就可以与目标资源进行安全上下文比较，若比较失败则无法访问目标，若比较成功则可以开始访问目标，最终能否访问目标还是要看文件的rex权限

### selinux的启动，关闭，查看

- selinux三种模式
 - enforcing 强制模式，代表selinux正在运行，确正确限制domain/type了
 - permissive：宽容模式，代表selinux正在运行，不过仅会警告信息并不会实际限制domain/type的访问。这种模式可以用来作为selinux的调试只用
 - disabled：关闭，selinux并没有实际运行

### selinux的策略与规则管理

- 策略查阅
 - 默认使用targeted策略，可以通过seinfo来查询

 ```
>seinfo [-Atrub]
  - -A:列出selinux的状态，规则布尔值，身份识别，角色，类型等所有信息
  - -t：列出selinux的所有类型种类
  - -r：列出selinux的所有角色种类
  - -u：列出selinux的所有的身份标识
  - -b：列出所有规则的种类
 - 在查到相关的类型或者布尔值后，想要知道更详细的信息可以用sesearch这个命令
 ```
 ```
>sesearch [-a] [-s 主体类型] [-t 目标类型] [-b 布尔值]
 - -a：列出该类型或布尔值的所有相关信息
 - -t：后面还要接类型，例如：-t httpd_t
 - -b：后面还要接布尔值的规则，例如：-b httpd_enable_ftp_server
```

 ```
 >[root@linuxprobe ~]# ls -Zd /var/www/html

 >drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 /var/www/html
 ```
 ```
 >[root@linuxprobe ~]# ls -Zd /home/wwwroot

 >drwxrwxrwx. root root unconfined_u:object_r:home_root_t:s0 /home/wwwroot
```
- 默认目录的安全上下文查询与修改

- semanage命令用于查询与修改SELinux的安全上下文，格式为：“semanage [选项] [文件]”。

- SELinux服务极大的增强了Linux系统的安全性，将用户权限牢牢地锁在笼子里，semanage命令不仅能够像传统chcon命令一样对文件、目录进行策略设置，而且还能够对网络端口、消息接口等等进行管理，这些新特性咱们会在本章中慢慢感受。在使用semanage命令的时候有几个比较常用的参数，-l参数用于查询、-a参数用于添加、-m参数用于修改、-d参数用于删除等等，例如可以向新的网站数据目录中新增加一条SELinux安全上下文，让这个目录以及里面的所有文件能够被httpd服务程序所获取到：
```
 >[root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot

 >[root@linuxprobe ~]# semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/*
```
- 不过仅仅是这样设置完还不能让网站立即恢复访问，还需要使用restorecon命令来让刚刚设置的SELinux安全上下文立即生效，可以加上-Rv参数指定进行对目录的递归操作以及显示SELinux安全上下文的修改过程，最后再来刷新一下页面就能正常看到网页内容了，如图10-8所示。
```
 >[root@linuxprobe ~]# restorecon -Rv /home/wwwroot/

 >restorecon reset /home/wwwroot context unconfined_u:object_r:home_root_t:s0->unconfined_u:object_r:httpd_sys_content_t:s0

 >restorecon reset /home/wwwroot/index.html context unconfined_u:object_r:home_root_t:s0->unconfined_u:object_r:httpd_sys_content_t:s0
```
- 布尔值得查询与修改

- 有的服务访问的文件可能在域外，在selinux默认的域外，导致服务不能用，例如，httpd服务的网页数据目录被设置在了家目录，而selinux默认httpd可访问的域不包过该目录，在可以用getsebool命令来查询并过滤出所有服务相关的安全策略，off为禁止状态，on为允许状态：

  ```
  >[root@linuxprobe ~]# getsebool -a | grep http
  httpd_anon_write --> off

  httpd_builtin_scripting --> on

  httpd_can_check_spam --> off

  httpd_can_connect_ftp --> off

  httpd_can_connect_ldap --> off

  httpd_can_connect_mythtv --> off

  httpd_can_connect_zabbix --> off

  httpd_can_network_connect --> off

  httpd_can_network_connect_cobbler --> off
  ```

- 可以用setsebool命令来修改SELinux策略中各项规则的布尔值了，一定要加上-P参数让修改过后的SELinux布尔值策略项目永久生效这样操作后也会是立即生效的
```
>[root@linuxprobe ~]# setsebool -P httpd_enable_homedirs=on
```

### selinux所需的服务
- setroubleshoot：将错误信息写入/var/log/messages，最好开机时就将该服务启动
- auditd:将详细数据写入/var/log/audit/audit.log，auditd会将selinux发生的错误信息写入/var/log/auditd/audit.log中，最好开机时就将该服务启动
