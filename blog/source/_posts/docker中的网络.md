---
title: docker中的网络
date: 2018-05-24 17:51:05
tags:
---
docker中的中涉及到多种网络结构，做简单介绍

* 本地网络

| 网络类型      | 说明                |
|:-------------|:-----------------------        |
| bridge       | --net=bridge 默认模式,NAT转发    |
| host         | --net=host 使用宿主机网络        |
|  none        | --net=none 无网卡                |
| container    | --net=container:容器名或ID ,共用其它容器网络   |

如果要是容器与宿主机进行通讯则不可选择host和container这两种网络，如果你仅需要Docker容器与宿主机网段相同，容器与其他同网段节点相互通信，不与宿主机进行通信，可以使用host网络模型！

* 跨主机网络

| 网络类型   | 说明      |
| :------------- | :-------------   |
| overlay       |vxlan模式           |
| macvlan       |使用外部lan,需手动配置 |

macvlan的原理是在宿主机物理网卡上虚拟出多个子网卡，通过不同的MAC地址在数据链路层进行网络数据转发的，它是比较新的网络虚拟化技术，需要较新的内核支持，使用macVLAN模式的容器，无法ping通宿主机，宿主机也无法ping通该容器，对其他同网段的服务器和容器都可以联通。

<!--more-->
## 使用bridge网络

在网络方面，桥接网络是链路层设备，它在网络段之间转发流量。网桥可以是硬件设备或在主机内核中运行的软件设备。

就Docker而言，桥接网络使用软件桥接器，该软件桥接器允许连接到同一桥接网络的容器进行通信，同时提供与未连接到该桥接网络的容器的隔离。 Docker桥驱动程序会自动在主机中安装规则，以便不同网桥上的容器无法直接相互通信。

桥接网络适用于在同一个Docker守护程序主机上运行的容器。对于在不同Docker守护程序主机上运行的容器之间的通信，您可以在操作系统级别管理路由，也可以使用覆盖网络。

启动Docker时，会自动创建默认桥接网络（也称为网桥），并且除非另行指定，否则新启动的容器将连接到该网络。您还可以创建用户定义的自定义网桥。用户定义的网桥优于默认网桥。

##### 启用从Docker容器转发到外部世界
默认情况下，来自连接到默认网桥的容器的流量不会转发到外部世界。要启用转发，您需要更改两个设置。这些不是Docker命令，它们会影响Docker主机的内核。

1、配置Linux内核以允许IP转发。
```
$ sysctl net.ipv4.conf.all.forwarding=1
```
2、将iptables FORWARD策略的策略从DROP更改为ACCEPT
```
$ sudo iptables -P FORWARD ACCEPT
```
##### 配置默认网桥，修改/etc/docker/daemon.json的文件，添加类似如下以下字段
```
{
  "bip": "192.168.1.5/24",
  "fixed-cidr": "192.168.1.5/25",
  "fixed-cidr-v6": "2001:db8::/64",
  "mtu": 1500,
  "default-gateway": "10.20.1.1",
  "default-gateway-v6": "2001:db8:abcd::89",
  "dns": ["10.20.1.2","10.20.1.3"]
}
```

## 使用overlay网络
overlay网络驱动程序在多个Docker守护程序主机之间创建分布式网络。该网络位于特定于主机的网络之上，允许连接到它的容器（包括群集服务容器）安全地进行通信。 Docker透明地处理每个数据包与正确的Docker守护程序主机和正确的目标容器的路由。

初始化swarm或将Docker主机加入现有swarm时，会在该Docker主机上创建两个新网络

1、一个名为ingress的overlay网络，用于处理与群服务相关的控制和数据流量。创建群组服务时，如果不将其连接到用户定义的覆盖网络，则默认情况下会连接到入口网络

2、一个名为docker_gwbridge的bridge网络，它将各个Docker守护程序连接到参与swarm的其他守护进程。

注意：不同主机上的服务通过overlay网络中的名为ingress网络进行通信，当访问到达某主机后，该主机的容器则通过名为docker_gwbridge的bridge网络进行通信

#### 创建overlay网络
您可以使用docker network create创建用户定义的覆盖网络，方法与创建用户定义的网桥网络相同。服务或容器一次可以连接到多个网络。服务或容器只能通过它们各自连接的网络进行通信。

###### 准备工作

1、 使用overlay网络的Docker守护程序的防火墙规则，您需要以下端口打开来往于overlay网络上的每个Docker主机的流量：
- 用于集群管理通信的TCP端口2377
- TCP和UDP端口7946用于节点之间的通信
- UDP端口4789用于overlay网络流量

2、在创建覆盖网络之前，您需要使用docker swarm init将Docker守护程序初始化为swarm管理器，或者使用docker swarm join将其连接到现有的swarm。这些中的任何一个都会创建默认的入口overlay网络，默认情况下由群集服务使用。即使您从未计划使用群组服务，也需要执行此操作。之后，您可以创建其他用户自定义的overlay网络。


###### 网络创建

1、 创建用户自定义的overlay网络

```
$ docker network create -d overlay my-overlay
```

2、要创建可由群集服务或独立容器用于与在其他Docker守护程序上运行的其他独立容器通信的overlay网络，请添加--attachable标志：

```
$ docker network create -d overlay --attachable my-attachable-overlay
```
您可以指定IP地址范围，子网，网关和其他选项。有关详细信息，请参阅docker network create --help。
#### overlay加密
默认情况下，使用GCM模式下的AES算法加密所有群集服务管理流量。群中的管理器节点每隔12小时轮换用于加密会话数据的密钥。

要加密应用程序数据，请在创建覆盖网络时添加--opt encrypted。这样可以在vxlan级别启用IPSEC加密。此加密会产生不可忽视的性能损失，因此您应该在生产中使用此选项之前对其进行测试。

启用覆盖加密后，Docker会在所有节点之间创建IPSEC隧道，在这些节点上为连接到覆盖网络的服务安排任务。这些隧道还在GCM模式下使用AES算法，管理器节点每12小时自动旋转密钥。

您可以将覆盖网络功能与--opt encrypted --attachable一起使用，并将非托管容器附加到该网络

```
$ docker network create --opt encrypted --driver overlay --attachable my-attachable-multi-host-network
```

#### 自定义默认的ingress网络

大多数用户永远不需要配置ingress网络，但Docker 17.05及更高版本允许您这样做。如果自动选择的子网与网络上已存在的子网冲突，或者您需要自定义其他低级网络设置（如MTU），则此功能非常有用。

自定义ingress网络涉及删除和重新创建它。这通常在您在swarm中创建任何服务之前完成。如果您具有发布端口的现有服务，则需要先删除这些服务，然后才能删除入口网络。

在没有ingress网络存在的时间内，不发布端口的现有服务继续运行但不是负载平衡的。这会影响发布端口的服务，例如发布端口80的WordPress服务。

1、使用docker network inspect ingress检查ingress网络，并删除容器连接到它的所有服务。这些是发布端口的服务，例如发布端口80的WordPress服务。如果未停止所有此类服务，则下一步失败。

2、删除现有的ingress网络：

```
$ docker network rm ingress

WARNING! Before removing the routing-mesh network, make sure all the nodes
in your swarm run the same docker engine version. Otherwise, removal may not
be effective and functionality of newly created ingress networks will be
impaired.
Are you sure you want to continue? [y/N]
```
3、使用--ingress标志创建新的overlay网络，以及要设置的自定义选项。此示例将MTU设置为1200，将子网设置为10.11.0.0/16，并将网关设置为10.11.0.2。

```
$ docker network create \
  --driver overlay \
  --ingress \
  --subnet=10.11.0.0/16 \
  --gateway=10.11.0.2 \
  --opt com.docker.network.driver.mtu=1200 \
  my-ingress
```
4、重新启动您在第一步中停止的服务。

#### 自定义docker_gwbridge接口
docker_gwbridge是一个虚拟网桥，可将overlay网络（包括ingress网络）连接到单个Docker守护程序的物理网络。 Docker初始化swarm或将Docker主机加入swarm时会自动创建它，但它不是Docker设备。它存在于Docker主机的内核中。如果您需要自定义其设置，则必须在将Docker主机加入群组之前或从群集中临时删除主机之后执行此操作。

1、停止容器

2、删除现有的docker_gwbridge接口。

```
$ sudo ip link set docker_gwbridge down

$ sudo ip link del dev docker_gwbridge
```

3、启动Docker。不要加入或初始化群

4、使用docker network create命令，使用自定义设置手动创建或重新创建docker_gwbridge桥。此示例使用子网10.11.0.0/16。有关可自定义选项的完整列表.
![请参阅Bridge驱动程序选项](https://docs.docker.com/engine/reference/commandline/network_create/#bridge-driver-options)
```
$ docker network create \
--subnet 10.11.0.0/16 \
--opt com.docker.network.bridge.name=docker_gwbridge \
--opt com.docker.network.bridge.enable_icc=false \
--opt com.docker.network.bridge.enable_ip_masquerade=true \
docker_gwbridge
```
5、初始化或加入群。由于桥已经存在，Docker不会使用自动设置创建它

## Operations for swarm services
1、在overlay网络上发布端口

连接到同一overlay网络的swarm服务有效地将所有端口相互暴露。对于可在服务外部访问的端口，必须使用docker service create或docker service update上的-p或--publish标志发布该端口。支持遗留冒号分隔语法和较新的逗号分隔值语法。较长的语法是首选，因为它有点自我记录。

2、绕过swarm服务的路由网格

默认情况下，发布端口的swarm服务使用路由网格来实现。当您连接到任何swarm节点上的已发布端口（无论它是否正在运行给定服务）时，您将被透明地重定向到正在运行该服务的worker。实际上，Docker充当您的群服务的负载均衡器。使用路由网格的服务以虚拟IP（VIP）模式运行。甚至在每个节点上运行的服务（通过--global标志）也使用路由网格。使用路由网格时，无法保证哪个Docker节点服务客户端请求。

要绕过路由网格，可以使用DNS循环（DNSRR）模式启动服务，方法是将--endpoint-mode标志设置为dnsrr。您必须在服务前运行自己的负载均衡器。 Docker主机上的服务名称的DNS查询返回运行该服务的节点的IP地址列表。配置负载均衡器以使用此列表并平衡节点之间的流量。

3、单独的控制和数据流量

默认情况下，与群组管理相关的控制流量以及进出应用程序的流量都在同一网络上运行，尽管群集控制流量已加密。您可以将Docker配置为使用单独的网络接口来处理两种不同类型的流量。初始化或加入swarm时，请分别指定--advertise-addr和--datapath-addr。您必须为加入群集的每个节点执行此操作。

## Operations for standalone containers on overlay networks

1、将独立容器连接到overlay网络

ingress网络是在没有--attachable标志的情况下创建的，这意味着只有swarm服务可以使用它，而不是独立的容器。您可以将独立容器连接到使用--attachable标志创建的用户定义的overlay网络。这使得在不同Docker守护程序上运行的独立容器能够进行通信，而无需在各个Docker守护程序主机上设置路由。

2、发布端口

|Flag value    | Description     |
| :------------- | :------------- |
|-p 8080:80	  | Map TCP port 80 in the container to port 8080 on the overlay network.      |
| -p 8080:80/udp|Map UDP port 80 in the container to port 8080 on the overlay network.|
|-p 8080:80/sctp|Map SCTP port 80 in the container to port 8080 on the overlay network.|
|-p 8080:80/tcp -p 8080:80/udp|Map TCP port 80 in the container to TCP port 8080 on the overlay network, and map UDP port 80 in the container to UDP port 8080 on the overlay network.|

3、容器发现

在大多数情况下，您应该连接到服务名称，该名称是负载平衡的，并由支持该服务的所有容器（“任务”）处理。要获取支持该服务的所有任务的列表，对tasks.<service-name>.执行DNS查找。



## 使用Macvlan网络

某些应用程序，尤其是遗留应用程序或监视网络流量的应用程序，希望直接连接到物理网络。在这种情况下，您可以使用macvlan网络驱动程序为每个容器的虚拟网络接口分配MAC地址，使其看起来像是直接连接到物理网络的物理网络接口。在这种情况下，您需要在Docker主机上指定一个物理接口，用于Macvlan，以及Macvlan的子网和网关。您甚至可以使用不同的物理网络接口隔离Macvlan网络。请记住以下事项：

1、由于IP地址耗尽或“VLAN传播”，很容易无意中损坏您的网络，在这种情况下，您的网络中有大量不同的唯一MAC地址。

2、您的网络设备需要能够处理“混杂模式”，其中一个物理接口可以分配多个MAC地址。
如果您的应用程序可以使用bridge（在单个Docker主机上）或overlay（跨多个Docker主机进行通信），那么从长远来看，这些解决方案可能会更好。

## 创建Macvlan网络
创建Macvlan网络时，它可以处于桥接模式或802.1q干线桥接模式。

- 在桥接模式下，Macvlan流量通过主机上的物理设备。

- 在802.1q中继桥接模式下，流量通过Docker在运行中创建的802.1q子接口。这使您可以在更细粒度的级别上控制路由和筛选。

#### Bridge mode

要创建与给定物理网络接口桥接的Macvlan网络，请使用--driver macvlan和docker network create命令。您还需要指定父级，即流量将在Docker主机上实际通过的接口。

```
$ docker network create -d macvlan \
  --subnet=172.16.86.0/24 \
  --gateway=172.16.86.1  \
  -o parent=eth0 pub_net
```
如果您需要排除在Macvlan网络中使用的IP地址，例如当已使用给定的IP地址时，请使用--aux-addresses：

```
$ docker network create -d macvlan  \
  --subnet=192.168.32.0/24  \
  --ip-range=192.168.32.128/25 \
  --gateway=192.168.32.254  \
  --aux-address="my-router=192.168.32.129" \
  -o parent=eth0 macnet32
```

#### 802.1q trunk bridge mode
如果指定包含点的父接口名称（例如eth0.50），则Docker会将其解释为eth0的子接口并自动创建子接口。
```
$ docker network  create  -d macvlan \
    --subnet=192.168.50.0/24 \
    --gateway=192.168.50.1 \
    -o parent=eth0.50 macvlan50
```

## 使用ipvlan而不是macvla

在上面的示例中，您仍在使用L3网桥。您可以使用ipvlan代替，并获得L2网桥。指定-o ipvlan_mode = l2。
```
$ docker network create -d ipvlan \
    --subnet=192.168.210.0/24 \
    --subnet=192.168.212.0/24 \
    --gateway=192.168.210.254  \
    --gateway=192.168.212.254  \
     -o ipvlan_mode=l2 ipvlan210
```
