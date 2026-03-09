# dockerfile参考

* 定义：Docker 可以通过读取 Dockerfile 中的指令自动构建镜像。Dockerfile 是一个文本文件，其中包含了用户在命令行上用来组装镜像的所有命令。

* dockerdocs reference  <https://docs.docker.com/reference/dockerfile/>

## dockerfile支持的指令（详细指令参见上面的链接）

|指令   | 描述 | 语法格式 |
|:---|:---|:---|
|ADD    | 添加本地或远程文件和目录。| ADD [OPTIONS] \<src\> ... \<dest\> <br> ADD [OPTIONS] ["\<src\>", ... "\<dest\>"] |
|ARG    | 使用构建时变量。| ARG \<name\>[=\<default value\>] [\<name\>[=\<default value\>]...] |
|CMD    | 指定默认命令。| CMD ["executable","param1","param2"] (exec form) <br> CMD ["param1","param2"] (exec form, as default parameters to ENTRYPOINT) <br> CMD command param1 param2 (shell form) |
|COPY   | 复制文件和目录。| COPY [OPTIONS] \<src\> ... \<dest\> <br> COPY [OPTIONS] ["\<src\>", ... "\<dest\>"] |
|ENTRYPOINT | 指定默认可执行文件。| ENTRYPOINT command param1 param2 <br> ENTRYPOINT ["executable", "param1", "param2"] |
|ENV    | 设置环境变量。| ENV \<key\>=\<value\> [\<key\>=\<value\>...] |
|EXPOSE | 描述你的应用程序正在监听哪些端口。| EXPOSE \<port\> [\<port\>/\<protocol\>...] |
|FROM   | 从一个基础镜像创建一个新的构建阶段。| FROM [--platform=\<platform\>] \<image\>[:\<tag\>/@digest] [AS \<name\>] |
|HEALTHCHECK| 在启动时检查容器的健康状态。| HEALTHCHECK [OPTIONS] CMD command <br> HEALTHCHECK NONE(disable check from base image) |
|LABEL  | 向镜像添加元数据。| LABEL \<key\>=\<value\> [\<key\>=\<value\>...] (used by `docker inspect`)|
|MAINTAINER| 指定镜像的作者。| MAINTAINER \<name\> (use LABEL instead)|
|ONBUILD| 指定镜像在构建过程中使用的指令。| ONBUILD INSTRUCTION |
|RUN    | 执行构建命令。| RUN [OPTIONS] \<command\> ...(可多行(换行转义或heredocs))\<br\>RUN [OPTIONS] [ "\<command\>", ... ] |
|SHELL  | 设置镜像的默认外壳。Linux 上的默认 shell 是 ["/bin/sh", "-c"]，Windows 上的默认 shell 是 ["cmd", "/S", "/C"]。| SHELL ["executable", "parameters"] |
|STOPSIGNAL | 指定用于退出容器的系统调用信号，是一个 SIG<NAME> 格式的信号名，例如 SIGKILL。如果没有定义，默认是 SIGTERM。| STOPSIGNAL signal |
|USER   | 设置用户和组 ID。| USER \<user\>[:\<group\>] <br> USER UID[:GID] |
|VOLUME | 创建卷挂载。| VOLUME ["/data"] |
|WORKDIR| 更改工作目录(不存在自动创建)。影响 RUN、CMD ENTRYPOINT、COPY 和 ADD 指令 | WORKDIR /path/to/workdir |

## 格式

这是dockerfile的格式

```dockerfile
# Comment
INSTRUCTION arguments
```

指令不区分大小写。但通常约定使用大写字母，以便更容易将它们与参数区分开来。

Docker 按顺序执行 Dockerfile 中的指令。一个 Dockerfile 必须以 FROM 指令开始 。这可能在 解析指令 、 注释 和全局作用域之后。 ARGs. FROM 指令指定了你正在构建的基础镜像 。FROM 只能由一个或多个 ARG 指令前置，这些指令声明了在 Dockerfile 的 FROM 行中使用的参数。

BuildKit 将以 # 开头的行视为注释，除非该行是有效的 解析指令 。行中其他位置的 # 标记被视为参数。

## dockerfile示例

* [构建最佳实践页面](https://docs.docker.com/build/building/best-practices/)
* [入门教程](https://docs.docker.com/get-started/)
* [语言特定的入门指南](https://docs.docker.com/guides/language/)
