---
layout: post
title: 基于LXD容器在Ubuntu 18.04系统上安装ROS2 Humble
tags: [linux]
---

因为ROS2 Humble官方只支持Utunbu 22.04，因而在Ubuntu 18.04的系统上如果想要使用ROS2 Humble，需要配置容器环境。

# 1. 安装和配置LXD虚拟容器

1. 使用`sudo apt install lxd`安装lxd，我是一次成功的
2. 初始化lxd：<br>
在[Ubuntu提供的教程](https://ubuntu.com/blog/install-ros-2-humble-in-ubuntu-20-04-or-18-04-using-lxd-containers)上，建议指令是`sudo lxd init --minimal`，但是我没有成功。我使用`sudo lxd init`，一路使用默认配置到底，成功初始化。
3. 至此，如果要运行lxd的相关指令，需要有root权限。为了系统的安全，我们并不想每次运行的时候都需要root权限。这里通过将当前用户添加到lxd组里来解决这个问题：<br>
执行`sudo usermod -a -G lxd $USER`并重启系统。
4. 为了加快访问速度，改用清华的镜像源来实现加速$^{【2】}$：<br>
`lxc remote add tuna-images https://mirrors.tuna.tsinghua.edu.cn/lxc-images/ --protocol=simplestreams --public`<br>
执行完之后会在$HOME/.config/lxd/config.yml文件中加入一个清华的镜像源。
5. 基于清华的镜像源创建容器：<br>
`lxc launch tuna-images:ubuntu/22.04 ubuntu-container`
6. 至此，虚拟容器已经创建成功。可以使用`lxc list`来查看创建好的容器。

# 参考资料

【1】 [Install ROS 2 Humble in Ubuntu 20.04 or 18.04 using LXD containers](https://ubuntu.com/blog/install-ros-2-humble-in-ubuntu-20-04-or-18-04-using-lxd-containers)
【2】 [虚拟容器LXD命令详解](https://blog.csdn.net/weixin_45266856/article/details/129091058)
