# docker

* dockerdocs网站<https://docs.docker.com/>主要参考get started和reference两部分

## overview 将软件打包成标准化单元，用于开发、运输和部署

* 容器是软件的标准单元，它打包代码及其所有依赖项，以便应用程序从一个计算环境快速可靠地运行到另一个计算环境。Docker 容器镜像是一个轻量级、独立、可执行的软件包，其中包括运行应用程序所需的一切：代码、运行时、系统工具、系统库和设置。
容器镜像在运行时成为容器，对于 Docker 容器，镜像在 Docker 引擎上 运行时成为容器。容器化软件可用于基于 Linux 和 Windows 的应用程序，无论基础设施如何，都将始终以相同的方式运行。容器将软件与其环境隔离开来，并确保它能够统一工作，尽管开发和暂存之间存在差异。
* 在 Docker 引擎上运行的 Docker 容器：
  * 标准： Docker 为容器创建了行业标准，因此它们可以移植到任何地方
  * 轻量级： 容器共享机器的作系统系统内核，因此不需要每个应用程序的作系统，从而提高服务器效率并降低服务器和许可成本
  * 安全： 容器中的应用程序更安全，Docker 提供业界最强的默认隔离能力

* 比较容器和虚拟机
  * 容器
    * 容器是应用层的抽象，将代码和依赖项打包在一起。多个容器可以在同一台机器上运行，并与其他容器共享作系统内核，每个容器在用户空间中作为隔离进程运行。容器比 VM 占用更少的空间（容器映像的大小通常为数十 MB），可以处理更多的应用程序，并且需要更少的 VM 和作系统。
  * 虚拟机
    * 虚拟机 （VM） 是物理硬件的抽象，可将一台服务器转换为多台服务器。虚拟机管理程序允许多个虚拟机在一台计算机上运行。每个 VM 都包含作系统、应用程序、必要的二进制文件和库的完整副本——占用数十 GB。VM 的启动速度也可能很慢。
  * 容器和 VM 一起使用在部署和管理应用方面提供了极大的灵活性

## docker cli 子命令基本命令参考

* 详细命令参数和解释见原文连接<https://docs.docker.com/reference/cli/docker/>

| 命令 | 描述 |
|:---:|:---:|
|docker build (legacy builder)| 从一个 Dockerfile 构建镜像|
|docker builder | 管理构建|
|docker buildx  | Docker Buildx|
|docker checkpoint| 管理检查点|
|docker compose | Docker Compose|
|docker config | 管理 Swarm 配置|
|docker container| 管理容器|
|docker context | 管理上下文|
|docker debug   | 进入任何容器或镜像的 shell。`docker exec`调试的替代方案。|
|docker desktop | (Beta)Docker 桌面版|
|docker image   | 管理镜像|
|docker init    | 为您的项目创建 Docker 相关启动文件|
|docker inspect | 返回Docker 对象的低级信息|
|docker login   | 认证到注册中心|
|docker logout  | 从注册中心登出|
|docker manifest| 管理 Docker 镜像清单和清单列表|
|docker model   | Docker 模型运行器|
|docker network | 管理网络|
|docker node    | 管理 Swarm 节点|
|docker offload | 从 CLI 控制 Docker 卸载|
|docker plugin  | 管理插件|
|docker scout   | Docker Scout 的命令行工具|
|docker search  | 在 Docker Hub 中搜索镜像|
|docker secret  | 管理 Swarm 密钥|
|docker service | 管理 Swarm 服务|
|docker stack   | 管理 Swarm 堆栈|
|docker swarm   | 管理 Swarm|
|docker system  | 管理 Docker|
|docker trust   | 管理 Docker 镜像的信任|
|docker version | 显示 Docker 版本信息|
|docker volume | 管理卷|

## compose 文件参考

* compose文件参考<https://docs.docker.com/reference/compose-file/>

* Docker Compose 新手？了解更多关于 [Docker Compose 的关键特性和应用场景](https://docs.docker.com/compose/intro/features-uses/) 或 [尝试快速入门指南](https://docs.docker.com/compose/gettingstarted/)

## dockerfile参考

* dockerfile参考<https://docs.docker.com/reference/dockerfile/>
