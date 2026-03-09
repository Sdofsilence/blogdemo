# gcc命令选项

 [https://www.man7.org/linux/man-pages/man1/gcc.1.html](https://www.man7.org/linux/man-pages/man1/gcc.1.html)

[Master GCC Command Line Options: A Beginner-Friendly Guide (2025)](https://embeddedprep.com/gcc-command-line-options/)

GCC 是 GNU 的 C 和 C++ 编译器，支持多种语言如 C、C++ 和 Objective-C。通过 GCC，可以完成从预处理到生成可执行文件的整个编译过程。以下是 GCC 常用编译选项的详细说明。

基本选项

_-E_：仅执行预处理，不编译、汇编或链接。 

_-S_：将代码编译为汇编代码，生成 _.s_ 文件。 

_-c_：只编译生成目标文件，不进行链接。

_-o <文件>_：指定输出文件名，默认生成 _a.out_。 

_-x <语言>_：指定输入文件的语言类型，例如 _c_ 或 _c++_。

优化选项

_-O0_：不进行优化，默认选项。 _-O1_ 或 _-O_：基本优化，减少代码大小和执行时间。 _-O2_：进一步优化，启用更多优化标志。 _-O3_：最高级别优化，包括内联函数和循环展开。 _-Os_：优化代码大小，适合嵌入式开发。 _-Ofast_：忽略标准合规性，启用所有可能的优化。 _-Og_：为调试优化，兼顾调试和性能。

调试选项

_-g_：生成调试信息，供调试器使用。 

_-ggdb_：生成尽可能多的 GDB 调试信息。 

_-gstabs_：以 stabs 格式生成调试信息。

链接选项

_-static_：禁止使用动态库，生成静态链接的可执行文件。 _-shared_：生成共享目标文件，用于创建动态库。 _-L<目录>_：指定库文件的搜索路径。 _-l<库名>_：链接指定的库，例如 _-lm_ 链接数学库。

警告选项

_-Wall_：启用所有常见的警告信息。 _-Wextra_：启用额外的警告信息。 _-Werror_：将所有警告视为错误。 _-w_：禁止生成任何警告信息。

预处理选项

_-D<宏>_：定义宏，等价于代码中的 _#define_。 _-U<宏>_：取消对宏的定义。 _-I<目录>_：指定头文件的搜索路径。 _-include <文件>_：强制包含指定的头文件。

特殊选项

_-ansi_：启用 ANSI C 标准，禁用 GNU C 的扩展特性。 _-funsigned-char_：将 _char_ 类型视为无符号类型。 _-fno-asm_：禁止使用 _asm_、_inline_ 和 _typeof_ 关键字。
