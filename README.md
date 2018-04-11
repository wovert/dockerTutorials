# Docker 教程

## Why Docker 
1. 环境配置
软件开发最大的麻烦事之一，就是环境配置。用户计算机的环境都不相同，你怎么知道自家的软件，能在那些机器跑起来？

**操作系统的设置，各种库和组件的安装**。举例来说，安装一个 Python 应用，计算机必须有 Python 引擎，还必须有各种依赖，可能还要配置环境变量。

开发者常常会说："It works on my machine"。

2. 虚拟机
虚拟机（virtual machine）就是带环境安装的一种解决方案。它可以在一种操作系统里面运行另一种操作系统，比如在 Windows 系统里面运行 Linux 系统。应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。

虽然用户可以通过虚拟机还原软件的原始环境。但是，这个方案有几个缺点。

**（1）资源占用多**

虚拟机会独占一部分内存和硬盘空间。它运行的时候，其他程序就不能使用这些资源了。哪怕虚拟机里面的应用程序，真正使用的内存只有 1MB，虚拟机依然需要几百 MB 的内存才能运行。

**（2）冗余步骤多**

虚拟机是完整的操作系统，一些系统级别的操作步骤，往往无法跳过，比如用户登录。

**（3）启动慢**

启动操作系统需要多久，启动虚拟机就需要多久。可能要等几分钟，应用程序才能真正运行。

3. Linux 容器
Linux 发展出了一种虚拟化技术：Linux 容器（Linux Containers，缩写为 **LXC**）。

Linux 容器不是模拟一个完整的操作系统，而是对进程进行隔离。或者说，在**正常进程的外面套了一个保护层**。对于容器里面的进程来说，它接触到的各种资源都是虚拟的，从而实现与底层系统的隔离。

由于容器是进程级别的，相比虚拟机有很多优势。

**（1）启动快**

容器里面的应用，直接就是底层系统的一个进程，而不是虚拟机内部的进程。所以，启动容器相当于启动本机的一个进程，而不是启动一个操作系统，速度就快很多。

**（2）资源占用少**

容器只占用需要的资源，不占用那些没有用到的资源；虚拟机由于是完整的操作系统，不可避免要占用所有资源。另外，多个容器可以共享资源，虚拟机都是独享资源。

**（3）体积小**

容器只要包含用到的组件即可，而虚拟机是整个操作系统的打包，所以容器文件比虚拟机文件要小很多。

总之，容器有点像轻量级的虚拟机，能够提供虚拟化的环境，但是成本开销小得多。

## 什么是 Docker
- 2013年发布至今， [Docker](https://www.docker.com/)

Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。

Docker 将**应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样**。有了 Docker，就不用担心环境问题。

总体来说，Docker 的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行**版本管理、复制、分享、修改**，就像管理普通的代码一样。

## Docker 的安装
Docker 是一个开源的商业产品，有两个版本：社区版（Community Edition，缩写为 CE）和企业版（Enterprise Edition，缩写为 EE）。企业版包含了一些收费服务，个人开发者一般用不到。下面的介绍都针对社区版。

## Docker CE 的 安装请参考官方文档。
- Mac
- Windows
- Ubuntu
- Debian
- [CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)
- Fedora
- 其他 Linux 发行版

## docker 命令
- 查看版本信息
    + `docker --version`
    + `docker info`

- Docker 需要用户具有 sudo 权限，为了避免每次命令都输入sudo，可以把用户加入 Docker 用户组（官方文档）。
`$ sudo usermod -aG docker $USER`

- Docker 是服务器----客户端架构。命令行运行docker命令的时候，需要本机有 Docker 服务。如果这项服务没有启动，可以用下面的命令启动（官方文档）。
- # service 命令的用法
`$ sudo service docker start`

- # systemctl 命令的用法
`$ sudo systemctl start docker`

## image 文件
> Docker 把应用程序及其依赖，打包在 image 文件里面。只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。

image 是二进制文件。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。举例来说，你可以在 Ubuntu 的 image 基础上，往里面加入 Apache 服务器，形成你的 image。


- # 列出本机的所有 image 文件。
`$ docker image ls`

- # 删除 image 文件
`$ docker image rm [imageName]`
image 文件是通用的，一台机器的 image 文件拷贝到另一台机器，照样可以使用。一般来说，为了节省时间，我们应该尽量使用别人制作好的 image 文件，而不是自己制作。即使要定制，也应该基于别人的 image 文件进行加工，而不是从零开始制作。

为了方便共享，image 文件制作完成后，可以上传到网上的仓库。Docker 的官方仓库 Docker Hub 是最重要、最常用的 image 仓库。此外，出售自己制作的 image 文件也是可以的。

## hello world

- 国内连接 Docker 的官方仓库很慢，还会断线，需要将默认仓库改成国内的镜像网站
首先，运行下面的命令，将 image 文件从仓库抓取到本地。
`$ docker image pull library/hello-world`

上面代码中，docker image pull是抓取 image 文件的命令。library/hello-world是 image 文件在仓库里面的位置，其中library是 image 文件所在的组，hello-world是 image 文件的名字。

由于 Docker 官方提供的 image 文件，都放在library组里面，所以它的是默认组，可以省略。因此，上面的命令可以写成下面这样。`$ docker image pull hello-world`

- 查看 image 文件
`$ docker image ls`

- 运行这个 image 文件。
`$ docker container run hello-world`


- docker container run 命令会从 image 文件，生成一个正在运行的容器实例。
注意，docker container run命令具有自动抓取 image 文件的功能。如果发现本地没有指定的 image 文件，就会从仓库自动抓取。因此，前面的docker image pull命令并不是必需的步骤。

如果运行成功，你会在屏幕上读到下面的输出。

`$ docker container run hello-world`
Hello from Docker!
This message shows that your installation appears to be working correctly.

... ...
输出这段提示以后，hello world就会停止运行，容器自动终止。

有些容器不会自动终止，因为提供的是服务。比如，安装运行 Ubuntu 的 image，就可以在命令行体验 Ubuntu 系统。
`$ docker container run -it ubuntu bash`

对于那些不会自动终止的容器，必须使用docker container kill 命令手动终止。


`$ docker container kill [containID]`

## 容器文件
> image 文件生成的容器实例，本身也是一个文件，称为容器文件。也就是说，一旦容器生成，就会同时存在两个文件： image 文件和容器文件。而且关闭容器并不会删除容器文件，只是容器停止运行而已。


- 列出本机正在运行的容器
`$ docker container ls`

- 列出本机所有容器，包括终止运行的容器
`$ docker container ls --all`

上面命令的输出结果之中，包括容器的 ID。很多地方都需要提供这个 ID，比如上一节终止容器运行的docker container kill命令。

终止运行的容器文件，依然会占据硬盘空间，可以使用docker container rm命令删除。

`$ docker container rm [containerID]`
运行上面的命令之后，再使用 `docker container ls --all`命令，就会发现被删除的容器文件已经消失了。

