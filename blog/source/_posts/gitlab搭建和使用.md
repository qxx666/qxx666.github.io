---
title: gitlab搭建和使用
date: 2018-02-07 17:02:59
tags:
---
摘要：本篇主要讲述利用docker搭建企业个人的git服务器作为代码的存储和版本控制仓库，本例利用gitlab作为服务提供系统。以及介绍gitlab的使用方法。

<!---more--->

#### gitlab介绍：
大家对github都很熟悉，可能没听说过gitlab，github和gitlab都是基于web的Git repositories(仓库)，拥有流水线型的web开发流程，它们为开发团队存储、分享、发布、测试和合作web开发项目提供了中心化的、云存储的场所。Git版本控制——对于多人共同开发一个project来说非常重要——它提供了分享开源项目的平台。GitLab拥有更多的特性，它可以让开发团队拥有更多的安全性和灵活性的选择
#### gitlab优势
GitLab拥有GitHub拥有的一切，但他拥有更多——让团队对它们的repositories拥有更多的控制，它的特色：
* 非常便捷的用户界面，在同一界面上获取到：projects，最近的projects，用户，最近的用户，群组和状态；

* 允许设置仓库权限是公用的还是私有的；

* “Snippet support”让用户分享一个project的部分代码，而不是整个project。

* 受保护的分支是一种提升代码安全性的新方法，它们允许用户设置project的获取权限，所以一个团队中只有特定的人可以push，force push或者删除一个分支的代码。

* Authentication levels更进一步的提升安全性，允许用户给人读写以外的权限。举例来说，你可以给一个组员跟踪变动的权限却不给他获取代码的权限。

* 你可以设置获取到团队的整体的改进进度，而不是你个人的进度。

* 开发者通过打上“仍在进行中”状态标签让其他成员知道代码没有完成，从而阻止未完成的代码合并到其他的代码中

* “innersourcing”公司的资源如果员工不再权限范围内，将不知道这个资源的存在。

#### 利用docker中搭建gitlab

* 环境准备

  1、linux主机，本例系统是linux发行版centos7.3平台

  2、先要安装docker环境，docker环境安装和使用我就不在这重复了，可以参考我博客的另一篇文章

 [docker介绍和安装使用](/_posts/docker入门教程.md)


 * gitlab部署

 ```
 sudo docker run --detach \
     --publish 443:443 --publish 80:80 --publish 22:22 \
     --name gitlab \
     --restart always \
     --volume /home/gitlab/config:/etc/gitlab \
     --volume /home/gitlab/logs:/var/log/gitlab \
     --volume /home/gitlab/data:/var/opt/gitlab \
     gitlab/gitlab-ce:latest
 ```
端口分别映射到80，22，443 如过没有开启https访问方式443端口可不做处理，各文件则分别隐射到本地/home/gitlab目录下，具体字段的意思我就不一一说明可以参考docker入门教程，

* 查看服务是否已经启动

  ![](/assets/img/docker.png)
  首次启动可能比较慢，需要等待一分钟左右的时间。我们可以使用sudo docker ps命令查看当前所有Docker容器的状态。当它的状态由starting变为运行时间时，说明成功启动了。我们直接使用上面配置的IP地址,在浏览器中访问即可。

* 浏览器访问
在浏览器输入你部署主机的ip地址和映射端口，第一次登入回让你设置管理员密码，设置好后，用root用户名，你设置的密码登入就可进入管理员的界面，如图：
![](/assets/img/root.png)

其他用户则需要用邮箱进行注册后登入，登入后就可以创建自己的项目库

 * 更多内容参考官方文档
 [GitLab官方文档](https://docs.gitlab.com/omnibus/docker/README.html#run-the-image)

#### gitlab使用教程

##### 环境准备：
1、git客户端

##### 具体步骤
1、先登入gitlab创建一个库，如下图，创建一个test库，该库可以选择是私有还是公开，还是企业内部共享
![](/assets/img/库.png)

2、库创建好后可以添加一个Readme.md,在此添加该文件作为下面代码修改的演示
![](/assets/img/read.png)

3、在本地安装git客户端

  * linux用户可以直接
```
yum install git
```

* windows用户可以去官网下载git客户端进行安装，然后加入到系统的环境变量中

  [git客户端下载](https://git-scm.com/)

4、代码库的下载和上传

* linux用户

  ```
  git clone http://ip:port/user/yourproject
  ```
  例如我以root身份创建的了一个test项目库
  ![](/assets/img/git1.png)
  进入该test文件就可以看到该项目所有文件了。
  ![](/assets/img/git2.png)

  * 修改代码并上传
  ```
  vim README.md   //打开文件并写入一些文字，保存退出文本编辑
  git add test  //git保存修改
  git commit   //提交修改
  git push     //上传到gitlab服务器
```

  登入gitlab服务器则可以看到你提交的修改文档文档
  ![](/assets/img/t1.png)

* windows用户

1、确保已经安装了git客户端，打开powershell。从gitlab上拷贝代码库
![](/assets/img/w.png)
2、进入test文件夹，修改README.md文件，添加一些文字，然后上传
![](/assets/img/w1.png)

至于怎么在powershell中打开vim编辑器可参考我另一篇博客
[powershell中打开vim](/_posts/powershell的一些操作)

然后保存修改，提交修改，并上传到gitlab，第一次上传时需要你输入用户名和密码，根据提示输入你注册时的账号和密码即可
![](/assets/img/w3.png)

* 浏览器登入gitlab产看修改结果
![](/assets/img/w2.png)


###### 这里简单的说了一下gitlab部署和使用，至于怎么利用docker swarm将gitlab做成服务以集群的方式工作，以保证高可用，以及gitlab版本控制，权限控制等更多操作，等下一次有时间整理出来供大家参考。
