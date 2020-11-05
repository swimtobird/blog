---
title: 借用docker轻量快速配置php全家桶
date: 2017-08-05 16:41:03
tags:
- docker
- php
categories:
- 技术
comments: true
---
## 闲聊扯淡话

小弟我是p（pai）h（huang）p（pian）程序猿,期间在windows下开发一直感觉相当痛苦，特别是没有一套顺手的php开发环境，这样很难让我飙车到120。其实入门时候使用过[wamp](http://www.wampserver.com/en/) 、[phpstudy](http://www.phpstudy.net/) 这些一键集成的软件，可是如果你只在windows下跑过你的代码，你会发现部署在linux的测试和生产环境，总会出现你在windows下不会出现的奇葩问题(当年血与泪的教训)。基于此，我慢慢转向了虚拟机，奔向了linux环境。

刚刚开始是直接使用centos环境配置，期间学习了不少东西，不过也因为我专业是嵌入式开发的，所以linux下使用的困难其实并没有可怕，缺点就是所有一切东西都需要直接去安装配置，十分繁琐，让新手可能很容易就沮丧和放弃。

直到后来，我发现[homestead](https://docs.golaravel.com/docs/4.2/homestead/) 这个好东西，再次感谢开源这个伟大发明。homestead也是一个虚拟机，基于ubuntu集成好了php基本一切环境，php、mysql、redis、memcached、hhvm、Beanstalkd、php-fpm、php-cli、nginx等等。安装前提，其实主要解决翻墙问题，其他看文档，基本都十分流程可以安装好，一次搞定，终身幸福，十分十分友好。

虽然homestead真的是很棒的东西，直到现在我依然在使用中，但是它也有它的不足。首先，启动十分十分的慢，我电脑是固态，也要花点等待启动，其次是升级比较麻烦。所以基于以上，我一直在寻找如果是纯粹简单快速开发环境，直到看到了docker，眼前一亮。

## 介绍

### Docker

[Docker](https://www.docker.com/) 是一个开源项目,自动化部署应用程序软件的容器,在Linux, Mac OS and Windows提供一个额外的抽象层和自动化的[操作系统级的虚拟化](https://en.wikipedia.org/wiki/Operating-system-level_virtualization)。这部分内容大家去度娘、谷歌，这里复制官方简介。

## 开始

### 安装

#### 1 - 一键式安装[docker for windows](https://download.docker.com/win/stable/InstallDocker.msi)

傻瓜式点击next就安装好，不补充说明了。

#### 2 - 构建

安装好了docker后，接下来就是我们利用开源的[laraDock](https://github.com/LaraDock/laradock) 构建开发环境了。

A）先克隆laraDock仓库代码

```
git clone https://github.com/LaraDock/laradock.git
```

B)进入laradock目录和复制和重命名.env-example为.env

```
cp .env-example .env
```

C)启动容器,如果使用的vpn好的话，估计等待时间不长，就下载构建完成，以后都是直接启动

```
docker-compose up -d nginx mysql redis
```

D)打开浏览器输入http://localhost

```
That's it! enjoy :)
```

### 配置和操作

#### 1 - 配置说明

A)目录说明

项目代码需要和laradock平级，也就是如下所示

```
+ laradock
+ project-1
+ project-2
```

#### 2 - 操作说明

A）停用所有容器

```
docker-compose stop
```

停用指定容器

```
docker-compose stop {container-name}
```

B)展示所有运行中容器

```
docker-compose ps
```

C)删除所有构建容器

```
docker-compose down
```

D)重建所有容器

```
docker-compose build
```

重建指定容器

```
docker-compose build {container-name}
```

E)在容器运行命令

```
docker-compose exec {container-name} bash
```



#### 3 - 编辑默认容器配置

打开`docker-compose.yml`并更改任何你想要的东西

例如

要更改mysql数据库默认配置账号为user，密码为123456

```
### MYSQL ####
MYSQL_USER=user
MYSQL_PASSWORD=123456
```

修改完后记得执行重建容器命令

```
docker-composer build mysql
```

#### 4 - 疑惑

A)通过ssh访问`workspace`

刚刚从linux虚拟机转换过来，总是习惯想通过ssh去访问`workspace`，其实并需要这样，因此其实你通过这个`docker-compose exec workspace bash` 去执行，转变过程我也花了点时间。如果有兴趣也可以看看这篇文章[If you run SSHD in your Docker containers, you're doing it wrong!](https://jpetazzo.github.io/2014/06/23/docker-ssh-considered-evil/)，很明白说清楚为什么没有必要ssh去操作docker容器。

对了，友情提醒，如果你真不习惯这种方式，还是想要ssh去访问的话，要做两个事。先在`.env`配置里将`INSTALL_WORKSPACE_SSH`修改为`true`,然后执行`docker-compose exec workspace bash`，在执行之后和linux一样执行命令重置密码

## 感谢

开这个博客，写了第一篇技术文章，感谢我女朋友bobo的大力支持。对的，我虽然是程序猿，可我是有女朋友的，不是左右手哟。

## 参考资料

https://docs.docker.com/

http://laradock.io/

https://jpetazzo.github.io/2014/06/23/docker-ssh-considered-evil/