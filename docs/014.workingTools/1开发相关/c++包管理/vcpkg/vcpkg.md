# vcpkg 的使用

* 主要参考为msdn learn
  * 开始 [https://learn.microsoft.com/zh-cn/vcpkg/get_started/overview](https://learn.microsoft.com/zh-cn/vcpkg/get_started/overview)
    * 教程：在 Visual Studio Code 中通过 CMake 安装和使用包[https://learn.microsoft.com/zh-cn/vcpkg/get_started/get-started-vscode?pivots=shell-powershell](https://learn.microsoft.com/zh-cn/vcpkg/get_started/get-started-vscode?pivots=shell-powershell)
    * 
  * 使用包[https://learn.microsoft.com/zh-cn/vcpkg/consume/manifest-mode?tabs=cmake%2Cbuild-cmake](https://learn.microsoft.com/zh-cn/vcpkg/consume/manifest-mode?tabs=cmake%2Cbuild-cmake)
  * json文件参考：[https://learn.microsoft.com/zh-cn/vcpkg/reference/vcpkg-json](https://learn.microsoft.com/zh-cn/vcpkg/reference/vcpkg-json)
  * 版本控制参考：[https://learn.microsoft.com/zh-cn/vcpkg/users/versioning#baselines](https://learn.microsoft.com/zh-cn/vcpkg/users/versioning#baselines)

## 清单文件

* 教程：从清单文件安装依赖项[REF](https://learn.microsoft.com/zh-cn/vcpkg/consume/manifest-mode?tabs=msbuild%2Cbuild-MSBuild)
  * vcpkg integrate (两种：构建系统集成(可以让构建系统自己包含依赖)和shell集成)
  * vcpkg search `vcpkg search [options] [query]`
  * vcpkg add `vcpkg add port [options] <port-name>...`
  * vcpkg new `vcpkg new --application` | `vcpkg new --name hello --version 1.0 [--version-relaxed | --version-date | --version-string]`
  * vcpkg install 清单模式 `vcpkg install [options]`  |  经典模式 `vcpkg install [options] <package>...`
* 版本控制参考[REF](https://learn.microsoft.com/zh-cn/vcpkg/users/versioning)

### 清单文件和 vcpkg integrate集成的关系


下面这个表格能帮你更清楚地理解它们的分工与合作：

| 特性方面               | `vcpkg integrate`    | 清单模式                                                             | 如何配合                                   |
| ---------------------- | ---------------------- | -------------------------------------------------------------------- | ------------------------------------------ |
| **主要职责**     | **环境集成**     |                                                                      | 先通过集成配置环境，再为项目启用清单模式。 |
| **作用范围**     | 用户系统或全局开发环境 | 单个项目目录                                                         | 全局集成服务于各个项目的清单。             |
| **依赖管理方式** | 不直接管理依赖         | 通过项目根目录下的 `vcpkg.json` 文件声明依赖                       | 集成确保构建系统能识别 `vcpkg.json`。    |
| **依赖隔离**     | 无此功能               | **每个项目有独立的 `vcpkg_installed` 目录** ，避免依赖冲突。 | 集成工具链帮助实现这种隔离安装。           |
| **版本控制**     | 无此功能               | 支持版本基线、覆盖等                                                 | 集成工具链确保版本解析在构建时生效。       |

#### 它们如何协同工作

在实际项目中，两者协同工作的典型流程如下：

1. **安装并集成vcpkg** ：你首先需要克隆vcpkg，运行引导脚本，然后执行 `vcpkg integrate install` 命令进行全局集成[](https://archive.softwareheritage.org/browse/content/sha1_git:ab53d718010de101de8cbeb61bf22386bb4371ad/?path=README_zh_CN.md&release=2020.04&snapshot_id=9b452ef9d390f2b21bb02e326e1fa7f26bcb96fa)[](http://home.ustc.edu.cn/~linzhehui626/index.php/archives/24/)。对于CMake项目，更重要的是通过CMake工具链文件（通常在CMake配置时通过 `-DCMAKE_TOOLCHAIN_FILE=...` 指定[](https://archive.softwareheritage.org/browse/content/sha1_git:ab53d718010de101de8cbeb61bf22386bb4371ad/?path=README_zh_CN.md&release=2020.04&snapshot_id=9b452ef9d390f2b21bb02e326e1fa7f26bcb96fa)[](https://www.jetbrains.com/zh-cn/help/clion/package-management.html)）来集成。
2. **为项目创建并配置清单** ：在项目根目录创建一个 `vcpkg.json` 文件[](https://doc.qt.io/qtcreator/zh/creator-how-to-create-vcpkg-manifest-files.html)[](https://blog.csdn.net/m0_58648890/article/details/143183526)。这个文件里，你需要定义项目的名称、版本，并列出所需的依赖项[](https://blog.csdn.net/xiyangmo/article/details/119650687?spm=1001.2014.3001.5501)。你的CMakeLists.txt则需要通过 `find_package` 和 `target_link_libraries` 来使用这些依赖[](https://www.cnblogs.com/BDFFZI/p/18732764)。
3. **安装依赖与构建** ：配置好工具链和清单后，当你构建项目时，vcpkg会自动根据 `vcpkg.json`文件安装依赖到项目的 `vcpkg_installed`目录[](https://blog.csdn.net/m0_58648890/article/details/143183526)，然后完成编译。

#### 注意事项

* **清单模式下的安装命令** ：启用清单模式后，在项目目录中直接运行 `vcpkg install` 即可安装 `vcpkg.json`里定义的依赖[](https://blog.csdn.net/m0_58648890/article/details/143183526)。如果你试图像经典模式那样指定包名（例如 `vcpkg install sqlite3`），则会收到错误提示[](https://blog.csdn.net/m0_58648890/article/details/143183526)。
* **模式是互斥的，但集成不是** ：对于一个具体的vcpkg实例，经典模式和清单模式是互斥的[](https://www.jetbrains.com/zh-cn/help/clion/package-management.html)。但 `vcpkg integrate` 所做的全局集成，为这两种模式都提供了基础支持。当IDE在项目目录下检测到 `vcpkg.json`文件，通常会**自动切换到清单模式**[](https://www.jetbrains.com/zh-cn/help/clion/package-management.html)。

#### 总结

`vcpkg integrate` 和清单模式在vcpkg工作流中扮演着不同但互补的角色。**`vcpkg integrate` 负责搭建vcpkg与开发环境之间的桥梁，而清单模式则负责在项目层面进行精细化的依赖管理。** 要顺利使用清单模式，通常需要先做好环境集成。
