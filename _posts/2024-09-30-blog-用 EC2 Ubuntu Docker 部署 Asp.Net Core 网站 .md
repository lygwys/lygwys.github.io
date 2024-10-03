---
title: 用 EC2 Ubuntu Docker 部署 Asp.Net Core 网站'
excerpt: "明天就国庆节了。<br/><img src='https://s21.ax1x.com/2024/09/30/pA3ShZR.md.png'>"
date: 2024-09-30
permalink: /posts/2024/09/blog-post-mianfei xueshu blog EC2 Ubuntu Docker/
tags:
  - EC2
  - Ubuntu
  - Docker
  - Asp.Net Core
---


前阵子看到网上有一篇文章介绍过，互联网的数据正以惊人的速度湮灭。当时就想到要把曾经的劳动果实找回来，至少能有个纪念。

亚马逊云科技给力，免费提供40余种产品试用，看好套餐中的 EC2 主机，立马提出申请，很快就通过了。不能不说，亚马逊的办事效率真的很高，工作日，填写的信息与提供材料相符，当天就通过。

下边就是实现的过程，遇到的坑放文章最后。



EC2 实例
======
使用最新免费版本 Ubuntu 24.04
实例提供了一个公网 IP
默认安全组开放端口 22 以提供 SSH 连结，还默认开放了 80、443 端口，供已备案的网站使用。

因为不使用域名，所以在安全组入站规则中添加端口 8001/TCP 以 8001 端口公开第一个网站。

实例可生成一个 KEY 文件，需要保存在本地。SSH 客户端连接时需要这个文件。


连接工具
======
使用 MobaXterm，官网下载最新家庭便携版，免费使用，功能强大，既可图形化操作，也可命令行输入。使用时输入 IP 地址，设置保存的 KEY 文件路径，双击相应图标就可连接，不用输入用户名和密码。
将程序拷入 U 盘，可随时随地远程操作。


安装 docker
======
图方便，使用一条命令安装 Docker：

``````
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
````````````

使用镜像
======
这可是重头戏，要费些功夫。

                         
1.制作镜像
在要发布的网站文件根目录下新建 Dockerfile 文件，内容如下：

``````
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
ARG source
WORKDIR /app
EXPOSE 80
COPY . ./
ENTRYPOINT ["dotnet", "ZKEACMS.WebHost.dll"]
``````

将文件打包成镜像：

``````
docker build -t gyxjsfzzx .
``````

2.将镜像保存到镜像仓库 https://hub.docker.com

首先在 https://hub.docker.com/ 上新建镜像仓库 gyxjsfzzx

给本地的镜像文件打上标签：

``````
docker tag gyxjsfzzx:latest lygwys/gyxjsfzzx:v3.1
````````

最后上传到镜像仓库：

``````
docker push lygwys/gyxjsfzzx:v3.1
``````


3.拉取镜像：

``````
docker pull lygwys/gyxjsfzzx:v3.1
``````

常规情况，此时可以在互联网上随时随地拉取镜像到本地运行，但有时我们就不知道哪了个环节出现问题，反正我无论如何也不能在这台 EC2 上拉取任何一个镜像下来。

终级方法使用了镜像的导入导出，用到的命令如下：

查看本地所有镜像，可以看到镜像名称、标签和 IMAGE ID：

``````
docker imgas
``````

Windows11 本地导出镜像：【默认保存到 C:\Users\lygwys】

``````
docker save -o gyxjsfzzx.tar 7a8c8ffd35dc
``````

上传镜像到 EC2:
只需将文件拖拽到 MobaXterm 中，就可看到文件上传进度了。

远程 EC2 Ubuntu 载入镜像：

``````
docker load -i gyxjsfzzx.tar
``````

镜像打标签，不知道为什么 Ubuntu 载入镜像后，镜像的名称和 Tag 都是空白，给它重新打上：

``````
docker tag 7a8c8ffd35dc lygwys/gyxjsfzzx:v3.1
``````

在 ec2 ubuntu 24.04 的 docker 中运行镜像:

``````
docker run -d -p 8001:80   --restart always  --name jsfzzx  lygwys/gyxjsfzzx:v3.1
``````


[访问地址](http://161.189.160.180:8001)

### 终于看到了消失整整3年的老朋友了！

[![pA3ShZR.md.png](https://s21.ax1x.com/2024/09/30/pA3ShZR.md.png)](https://imgse.com/i/pA3ShZR)


坑
======
* EC2 无法拉取镜像，文中说了解决办法；
* 早期程序使用 .net 3.0,Ubuntu 24.04不支持，这就是为什么用 Docker 部署的原因，当然也可使用 UBuntu 的低版本来兼容；
* 我想将 SQLite 数据库挂载到宿主机上，以实现数据存档，但当新建一个容器重新运行时仍然不加载外部数据，网上查了资料，需要在镜像生成之前在程序文件中添加条件判断，没有试验。