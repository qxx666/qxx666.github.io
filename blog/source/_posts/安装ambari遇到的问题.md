---
title: 安装ambari遇到的问题
date: 2018-01-22 16:47:28
tags:
---

### Ambari介绍
Ambari 跟 Hadoop 等开源软件一样，也是 Apache Software Foundation 中的一个项目，并且是顶级项目。目前最新的发布版本是 2.0.1，未来不久将发布 2.1 版本。就 Ambari 的作用来说，就是创建、管理、监视 Hadoop 的集群，但是这里的 Hadoop 是广义，指的是 Hadoop 整个生态圈（例如 Hive，Hbase，Sqoop，Zookeeper 等），而并不仅是特指 Hadoop。用一句话来说，Ambari 就是为了让 Hadoop 以及相关的大数据软件更容易使用的一个工具。

说到这里，大家就应该明白什么人最需要 Ambari 了。那些苦苦花费好几天去安装、调试 Hadoop 的初学者是最能体会到 Ambari 的方便之处的。而且，Ambari 现在所支持的平台组件也越来越多，例如流行的 Spark，Storm 等计算框架，以及资源调度平台 YARN 等，我们都能轻松地通过 Ambari 来进行部署。

Ambari 自身也是一个分布式架构的软件，主要由两部分组成：Ambari Server 和 Ambari Agent。简单来说，用户通过 Ambari Server 通知 Ambari Agent 安装对应的软件；Agent 会定时地发送各个机器每个软件模块的状态给 Ambari Server，最终这些状态信息会呈现在 Ambari 的 GUI，方便用户了解到集群的各种状态，并进行相应的维护。详细的操作和介绍会在后续章节介绍。

<!---more--->
### Ambari安装
安装 Ambari 最方便的方式就是使用公共的库源（public repository）。有兴趣的朋友可以自己研究一下搭建一个本地库（local repository）进行安装。这个不是重点，所以不在此赘述。在进行具体的安装之前，需要做几个准备工作。

1、SSH 的无密码登录；

Ambari 的 Server 会 SSH 到 Agent 的机器，拷贝并执行一些命令。因此我们需要配置 Ambari Server 到 Agent 的 SSH 无密码登录。

2、确保 Yum 可以正常工作；

通过公共库（public repository），安装 Hadoop 这些软件，背后其实就是应用 Yum 在安装公共库里面的 rpm 包。所以这里需要您的机器都能访问 Internet。

3、确保 home 目录的写权限。

Ambari 会创建一些 OS 用户。

4、确保机器的 Python 版本大于或等于 2.6.

5、确保各主机能正确解析其他主机的主机名

可以设置本地的主机名和ip地址的映射，即将各主机的主机名和ip地址对应关系加到各主机的/etc/hosts文件中

以上的准备工作完成后，便可以真正的开始安装 Ambari 了

##### Ambari 的公共库文件（public repository）
centos6系统版本可下载：wget  http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.1.1/ambari.reposud

centos7系统版本可下载：wget http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.2.0.0/ambari.repo

将下载好的ambari.repo文件放到/etc/yum.repo.d/下

```
mv ambari.repo /etc/yum.repos.d/
yum clean all
yum list|grep ambari
```
看到以下内容，说明ambari库设置正确
![](/assets/img/ambari-repo.png)

然后便可以执行一下命令进行安装
```
yum install ambari-server
```
安装完成后执行一下命令进行初始化ambari-server
~~~
amari-server setup
~~~
在这个交互式的设置中，采用默认配置即可。Ambari 会使用 Postgres 数据库，默认会安装并使用 Oracle 的 JDK。默认设置了 Ambari GUI 的登录用户为 admin/admin。并且指定 Ambari Server 的运行用户为 root。

一般选择默认就可以了，如果安装途中遇到
