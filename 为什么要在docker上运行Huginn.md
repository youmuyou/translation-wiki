## 为什么要在docker上运行Huginn

你可以在这个网站上面开始部署 [docker](http://www.docker.io/).

如果你安装了docker就可以开始快速部署和使用huginn. docker容器非常适合进行测试或正式部署. Huginn使用环境变量进行安装配置, 而不只是用 .env文件就可以的,  Docker容器可以把需要的环境变量使用命令自动部署完毕.

## 启动容器

### 快速启动Huginn

#### OSX下面使用Kitematic

1. 下载以及安装 [Kitematic](https://www.docker.com/docker-kitematic)
* 启动Kitematic然后搜索 `cantino/huginn`
* 点击 `create` 然后等待容器下载完毕和启动
* 点击图标边上的链接 'WEB PREVIEW'
* 使用默认的帐号密码登录你的Huginn实例

#### OSX/Windows/Linux平台下面如何使用 docker machine

1. 根据你的系统下载 [docker machine](https://docs.docker.com/machine/#installation) 
* 根据安装向导安装直到可以运行`docker ps`
* 使用这个命令获取正在运行的VM虚拟机的ip `docker-machine ls`
* 使用这个命令启动你的Huginn容器 `docker run -it -p 3000:3000 cantino/huginn`
* 在浏览器里面打开Huginn [http://docker-machine ip:3000](http://<docker-machine ip>:3000)
* 使用默认帐号密码登录Huginn

#### Linux

1. 根据这个步骤安装docker [install instructions](https://docs.docker.com/installation/)
* 使用这个命令启动你的Huginn容器 `docker run -it -p 3000:3000 cantino/huginn`
* 在浏览器里面打开Huginn [http://localhost:3000](http://localhost:3000)
* 使用默认帐号密码登录Huginn

## 配置和连接到数据库容器

根据这个链接上面的介绍 [instructions on the docker hub registry](https://registry.hub.docker.com/u/cantino/huginn/) 配置和连接到MySQL 或PostgreSQL数据库容器.

## 运行多个Huginn进程在独立的容器里面

使用这个上面的镜像`cantino/huginn-single-process` 你可以轻松地运行多个Huginn进程在独立的的容器里面在需要单独使用的时候. 你可以看看这篇文章 [Docker hub](https://hub.docker.com/r/cantino/huginn-single-process/) 还有这篇 [documentation for the container](https://github.com/cantino/huginn/tree/master/docker/single-process)

### 其他的设置:

其他的Docker设置:

* 如果你不像使用官方的repo, 也可以看看这个: https://registry.hub.docker.com/u/andrewcurioso/huginn/
* 如果你想在单独的容器上运行Huginn的web进程, 这里也有别的方法 https://github.com/hackedu/huginn-docker.这个也是使用编译好的Unicorn作为web服务器.
