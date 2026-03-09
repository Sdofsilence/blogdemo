# dev container作为 vs code插件的使用

## 插件概述(简述)和教程

* vs code插件市场概述<https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers>

* vs code软件拓展概述<https://code.visualstudio.com/docs/devcontainers/containers>

* 官方网站<https://containers.dev/> 其中的overview,specification,supporting tools三个部分(是关于如何自己定义自己的环境的参考)

* vs code软件高级dev容器配置概述<https://code.visualstudio.com/remote/advancedcontainers/overview> (提高性能，配置单独的容器，在远程docker主机上开发等)

* 最**重要**的“创建开发容器(手动/自动)”<https://code.visualstudio.com/docs/devcontainers/create-dev-container>

## 两个重要配置文件devcontainer.json 和 dockerfile 的编写

### devcontainer.json文件

* dev container开发方案：
  * 1.启动独立的容器隔离工具链或加快设置速度。
  * 2.使用image,dockerfile,docker compose定义的容器部署应用。
  * 3.使用docker或kubernetes构建和部署应用。
  * 此外：你可以附加到已运行的容器，或者使用远程docker主机上开发。

* 自动创建开发容器
  * 命令面板中选择 Dev Containers： Add Dev Container Configuration Files... 命令，而不是手动创建 .devcontainer，选择预定义的容器配置将所需的文件自动添加到项目中作为起点，你可以根据需要进一步自定义。Dev Containers: Configure Container Features允许更新现有配置。
  * 你也可以重用dockerfile在，Dev Containers： Add Dev Container Configuration Files... 命令界面。

* 编辑配置文件的一般过程
  * 见原文<https://code.visualstudio.com/docs/devcontainers/create-dev-container#_full-configuration-edit-loop>.

### 使用docker compose的多容器配置(见原文)
