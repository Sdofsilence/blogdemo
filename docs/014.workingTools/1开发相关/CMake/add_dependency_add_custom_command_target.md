# 依赖管理，或者说让一个目标的构建或者执行在某个目标的前后

* 默认情况下不需要你手动管理依赖，cmake语法能够自己管理，但是你也可以显示依赖

## add_dependencies

```cmake
add_dependencies(<target> <target-dependency>)
```

* <target>：你想要指定依赖项的目标（例如，你的可执行文件）。

* <target-dependency>：你的目标所依赖的那个目标（例如，一个必须先构建的库）。
  这个命令的作用是确保<target-dependency>会在<target>之前被构建。这里的“目标”特指由 add_executable、add_library 或 add_custom_target 命令创建的顶层目标。

主要应用场景：add_dependencies 命令在以下情况尤其有用：

* 当你的目标依赖于使用 add_custom_target 创建的自定义目标时。
* 处理那些不直接通过链接建立关系的目标间的构建顺序。

## 替代方案：add_custom_target 或 add_custom_command DEPENDS 关键字

  对于由 add_custom_target 或 add_custom_command 创建的自定义规则，你也可以使用这些命令的 DEPENDS 选项来指定文件级别或目标级别的依赖关系



### `add_custom_target/command` 主要参数



* `COMMAND`：要执行的命令。

* `DEPENDS`：指定此目标所依赖的其他目标或文件。这有助于确保命令以正确的顺序执行。

* `WORKING_DIRECTORY`：命令执行时的工作目录。

* `COMMENT`：构建时显示的注释信息。

* `ALL`：若包含此选项，则该目标会被添加到默认的构建目标中，意味着每次运行构建命令（如 `make` 或 `ninja`）时，此自定义目标都会被执行

`add_custom_command` 独有的两种模式：

* **生成文件模式（`OUTPUT`）**：用于指定生成一个或多个文件的命令。当其他目标（如通过 `add_executable` 或 `add_library` 创建的目标）依赖于这些输出文件时，该命令会在构建时自动执行。

* **构建事件模式（`TARGET`）**：为已有的目标（如可执行文件或库）在构建的特定阶段（如 `PRE_BUILD`, `PRE_LINK`, `POST_BUILD`: 分别指定在target的前后运行）附加自定义命令。
  
  

将 `add_custom_command` 和 `add_custom_target` 结合使用

**典型模式**：使用 `add_custom_command` 生成文件，然后创建一个 `add_custom_target` 依赖于这些生成的文件。这样，当你构建这个自定义目标时，CMake 会确保先执行生成文件的命令。



```cmake
# 1. 定义一个命令来生成源文件
add_custom_command(
  OUTPUT generated_source.cpp
  COMMAND your_generator_tool -o generated_source.cpp input_file.xml
  DEPENDS input_file.xml
  COMMENT "Generating source file"
)

# 2. 创建一个自定义目标，它依赖于上面生成的源文件
add_custom_target(MyGeneratedSource DEPENDS generated_source.cpp)

# 3. 创建一个库或可执行文件，它使用生成的源文件，并且可以依赖于自定义目标以确保生成步骤先发生
add_library(MyLib generated_source.cpp other_source.cpp)
add_dependencies(MyLib MyGeneratedSource) # 显式声明依赖关系
```



```cmake
add_executable(my_app main.cpp)

# 使用 add_custom_command 后处理
add_custom_command(
  TARGET my_app POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:my_app> ${CMAKE_BINARY_DIR}/bin/
)
```



**`VERBATIM` 的重要性**：建议使用 `VERBATIM` 参数，它能让 CMake 正确转义命令中的特殊字符，确保命令跨平台执行的一致性



## 不同之处

| 特性       | `add_custom_command` | `add_custom_target`  |
| -------- | -------------------- | -------------------- |
| **可调用性** | ❌ 不能直接调用             | ✅ 可以通过 `--target` 调用 |
| **执行时机** | 自动（依赖触发）             | 手动或通过 `ALL` 自动       |
| **需要输出** | ✅ 必须关联文件             | ❌ 独立存在               |
| **典型用途** | 生成文件、构建钩子            | 独立任务、工作流             |



**决策指南**
---------

**使用 `add_custom_command` 当：**

* 操作与具体文件生成相关

* 需要自动集成到构建流程中

* 操作是构建的必要步骤

**使用 `add_custom_target` 当：**

* 需要手动调用特定任务

* 操作是独立的（如代码格式化、清理）

* 想要组合多个步骤到一个可调用目标



### 如何选择

简单来说，选择取决于你的后处理任务范围：

* 希望有一个**可以手动调用的独立任务**，请选用 **`add_custom_target`**。

* 如果处理逻辑**专门针对某个特定的可执行文件或库**，请选用 **`add_custom_command`** 并将其绑定到该目标上。



所有的构建前后的操作因该交给脚本或者用add_custom_target手动调用，但是像生成compile_command.json这种必须让它每次运行又没有顺序要求的可以用指定ALL的add_custom_target。


