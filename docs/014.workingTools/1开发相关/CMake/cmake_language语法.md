# 语法

* 语法概念中有三点，变量（三种变量（一般的，cache,env）），commands(就是内置函数)和关键字（其中if条件语句和条件语法适用于 if、elseif 和 while() 子句的 condition 参数。为关键），和 生成表达式（类似于宏替换）


## 详细见

* [cmake-commands命令](https://cmake.com.cn/cmake/help/latest/manual/cmake-commands.7.html)

* [条件语法](https://cmake.com.cn/cmake/help/latest/command/if.html#condition-syntax)

* [生成表达式](https://cmake.com.cn/cmake/help/latest/manual/cmake-generator-expressions.7.html)
  * 生成器表达式在生成构建系统时进行求值，以产生特定于每个构建配置的信息。它们的格式为 $<...>。例如：
    ```
    target_include_directories(tgt PRIVATE /opt/include/$<CXX_COMPILER_ID>)
    ```
