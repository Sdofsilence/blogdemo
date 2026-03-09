# CMake 学习资源

## 记录

* cmake官方入门指南<https://cmake.com.cn/getting-started/> 官方introduction<https://cmake.org/cmake/help/latest/index.html#> 官方指南<https://cmake.org/cmake/help/latest/guide/tutorial/index.html#guide:CMake%20Tutorial> 官方cmake语法文档<https://cmake.org/cmake/help/latest/manual/cmake-language.7.html> 过于丰富和复杂适合查询
* 现代cmake教程<https://crossroadw.github.io/ModernCMake/> 有点粗糙但是适合快速浏览
* Modern CMake 简体中文版<https://modern-cmake-cn.github.io/Modern-CMake-zh_CN/> 推荐"现代CMake"(介绍有推荐书籍)
* 书栈网cmake Cookbook<https://www.bookstack.cn/read/CMake-Cookbook/README.md> 内容丰富适合系统学习(耗时深入)
* b站“只喝白开水”<https://b23.tv/ZwNlEbm> 一个现代cmake（各种推荐例如vcpkg）的教学合集
* b站“爱编程的大丙”cmake保姆级教程<https://subingwen.cn/cmake/CMake-primer/> 适合快速学习或者立刻上手
* 现代 CMake 入门<[An Introduction to Modern CMake &#8212; Modern CMake](https://cliutils.gitlab.io/modern-cmake/README.html)> 来自 一些实践知乎.md
* 在vs studio中使用cmake 项目 microsoft learn<https://learn.microsoft.com/zh-cn/cpp/build/cmake-projects-in-visual-studio?view=msvc-170> (不同于此,vs studio中的(打开文件夹)可以基于.json文件(CppProperties.json,task.vs.json,launch.vs.json三个json文件)和文件夹的项目而无需生成项目或解决方案)

## 跨平台的问题

* 虽然cmake可以跨平台或者编辑器编译器，但是有些东西却是依赖于平台和编译器的(例如编译环境、编译工具链、ide特有的配置文件)，需要明确哪些无法共享的配置信息需要不同平台不同实现，明确使用项目的使用这需要保证哪些配置在哪些环境下需要特别配置。例如在vs studio中存在cmakesetting.txt文件(不是cmake标准支持的，在vs2022中可以强制使用cmake的presets.json文件代替)；由此，在使用vs studio创建的使用cmake时，需要利用`vcvarsall.bat [平台]`命令配置环境变量，并从命令行中启动`devenv`来打开vs studio，借此保证cmake可以正确使用windows环境下的配置(可能在vs code中也是如此)【并不是这样的，使用vs只要配置好CMakeLists.txt和CMakePresets.json后运行vs中的生成cmake缓存就可以配置成功无需其他环境配置，可能在vs code中是需要的(但是微软的cmake tools插件可能会配置环境)】。

## cmake快速上手

* b站“爱编程的大丙”个人博客 cmake保姆级教程<https://subingwen.cn/cmake/CMake-primer/> 适合快速学习或者立刻上手

## cmake基于target的一些函数

* 在现代 CMake 中，围绕“目标”（target）的概念设计了许多命令来帮助开发者更精确地控制构建过程。这些命令允许你定义、配置和管理各种类型的目标，如可执行文件、库等，并设置它们的属性、依赖关系等。以下是一些常用的与 target 相关的命令及其简要说明：

### 定义目标

* add_executable(\<name\> [source1] [source2 ...])
  * 创建一个可执行文件目标。
* add_library(\<name\> [STATIC | SHARED | MODULE] [source1] [source2 ...])
  * 创建一个库目标。可以指定是静态库（STATIC）、共享库（SHARED）还是模块库（MODULE）。
* add_custom_target(\<name\> ...) 
  * 添加一个自定义目标，通常用于执行一些特定的任务或脚本。
* add_custom_command(...)
  * 添加一条自定义命令，可用于生成源文件或其他输出。

### 配置目标属性

* target_include_directories(\<target\> [SYSTEM] [AFTER|BEFORE] \<interface|PUBLIC|PRIVATE\> [items1...] )
  * 指定目标的头文件搜索路径。INTERFACE, PUBLIC, 和 PRIVATE 控制了哪些依赖于该目标的其他目标可以访问这些包含目录。
* target_compile_definitions(\<target\> [INTERFACE|PUBLIC|PRIVATE] [definitions1...])
  * 设置目标的预处理器宏定义。
* target_compile_options(\<target\> [INTERFACE|PUBLIC|PRIVATE] [options1...])
  * 为指定的目标添加编译器选项。
* target_compile_features(\<target\> [INTERFACE|PUBLIC|PRIVATE] [features1...])
  * 指定目标需要的语言特性版本（如 C++11, C++14 等）。
* target_link_libraries(\<target\> [item1 [item2 [...]]] [[debug|optimized|general] \<item\>] ...) 
  * 指定目标需要链接的库，并且可以传递依赖信息给下层目标。
* set_target_properties(\<target1\> [\<target2\> ...] PROPERTIES prop1 value1 prop2 value2 ...)
  * 设置目标的任意属性值。

### 管理依赖关系

* add_dependencies(\<target\> [\<target-dependency\>...])
  * 显式地声明目标之间的依赖关系，确保一个目标在其依赖项之前构建。
* target_link_options(\<target\> [INTERFACE|PUBLIC|PRIVATE] [options1...])
  * 向目标添加链接器选项。

### 安装与导出

* install(TARGETS \<targets\> [...])
  * 指定如何安装目标及其相关文件。
* export(EXPORT \<export-name\> [...])
  * 导出目标以供其他项目使用。

### 其他

* get_target_property(VAR target property)
  * 获取指定目标的某个属性的当前值。
* get_property(VAR TARGET \<target\> PROPERTY \<prop-name\>)
  * 另一种获取目标属性的方法。
