# 如何构建deepwiki-open工具

* 从0到1精通DeepWiki-Open：开源AI文档工具用户完全手册[REF](https://blog.csdn.net/gitblog_00516/article/details/153443073)

* 内网服务器离线安装部署 Ollama[REF](https://www.cnblogs.com/LazyXiao/p/18879912)


## [服务器部署 Ollama](https://blog.csdn.net/qq_60985893/article/details/138334594)

[1](https://blog.csdn.net/qq_60985893/article/details/138334594)    [2](https://blog.csdn.net/qq_40999403/article/details/139320266)    [3](https://blog.csdn.net/weixin_63782093/article/details/140523420)

Ollama 是一个可以在本地快速搭建大模型的工具，支持多种操作系统，包括 macOS、Windows、Linux 以及 Docker 环境。以下是如何在服务器上部署 Ollama 的详细步骤。

**安装 Ollama**

**在 Linux 上安装 Ollama**

1. **更新包列表** ： *sudo apt-get update sudo apt-get upgrade*
2. **安装 Ollama** ： *curl -fsSL https://ollama.com/install.sh | sh*
3. **验证安装** ： *ollama --version*

**使用 Docker 安装 Ollama**

1. **安装 Docker** ： *sudo apt-get install ca-certificates curl gnupg lsb-release curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" sudo apt-get update sudo apt-get install docker-ce docker-ce-cli containerd.io*
2. **拉取 Ollama 镜像** ： *docker pull ollama/ollama:latest*
3. **运行 Ollama 容器** ： *docker run -d --gpus=all --restart=always -v /root/project/docker/ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama*

**部署 Llama3 模型**

* **下载模型** ： 在 Ollama 官网选择适合自己服务器 GPU 大小的模型，点击下载后会有对应的代码，直接在服务器上下载模型即可。模型的默认安装位置为  */usr/share/ollama/.ollama/models* 。

**部署 Open WebUI**

1. **拉取并运行 Open WebUI** ： *docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.nju.edu.cn/open-webui/open-webui:main*
2. **访问 UI 界面** ： 访问服务器的 3000 端口，即可看到 UI 界面。

**结语**

通过以上步骤，您可以在服务器上成功部署 Ollama 及其相关模型和 UI 界面。第一个注册的用户默认是管理员，其他用户注册时需要管理员同意。如果不想这么麻烦，可以在 UI 界面设置默认用户角色为用户 ^[2](https://blog.csdn.net/qq_40999403/article/details/139320266)^ 。

通过这些步骤，您可以有效地运行、访问、自定义和导入模型，充分利用 Ollama 的功能来满足各种需求。
