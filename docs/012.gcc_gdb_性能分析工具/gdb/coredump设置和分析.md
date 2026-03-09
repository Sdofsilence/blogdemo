# coredup设置、生成和分析

**coredump基础**

* Aarchlinux 核心转储：<https://wiki.archlinuxcn.org/wiki/%E6%A0%B8%E5%BF%83%E8%BD%AC%E5%82%A8>

* Linux 中启用核心转储：全面指南:<https://linuxvox.com/blog/linux-enable-core-dump/>

* github 核心转储&Valgrind：<https://github.com/ANSANJAY/LinuxCoredumpValgrind>

* Linux 中收集核心转储文件的新方法：<https://www.howtouselinux.com/post/how-to-collect-core-dump-file-in-linux>

* opensource.com 创建和调试 Linux 转储文件：<https://opensource.com/article/20/8/linux-dump>

* die.net core dump 文件：<https://linux.die.net/man/5/core>

**分离符号表**

* **分离符号表**：
  * 可执行文件符号表导出与使用<https://asiocity.github.io/2021/01/02/binary-symbols-with-debug/>
  * Linux命令strip详解：<https://developer.aliyun.com/article/1566964> linux下strip命令<https://worktile.com/kb/ask/332331.html>
  * 生成可调试的Release版本二进制文件--调试符号信息提取和附加调试链接section<https://unpluggedcoder.me/2014/04/24/%E7%94%9F%E6%88%90%E5%8F%AF%E8%B0%83%E8%AF%95%E7%9A%84Release%E7%89%88%E6%9C%AC%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%96%87%E4%BB%B6--%E8%B0%83%E8%AF%95%E7%AC%A6%E5%8F%B7%E4%BF%A1%E6%81%AF%E6%8F%90%E5%8F%96%E5%92%8C%E9%99%84%E5%8A%A0%E8%B0%83%E8%AF%95%E9%93%BE%E6%8E%A5section/index.html> 转载前面这个的分析<https://www.cnblogs.com/8335IT/p/18079295>
  * Linux下编译生成符号表的方法 (linux 编译符号表)<https://www.idc.net/help/126963/>
  * strip后发布的程序如何gdb调试<https://blog.csdn.net/sydyh43/article/details/123131122>
  * c++ 符号表分离———objcopy(调试信息挂载)<https://blog.csdn.net/cyteven/article/details/13015511>
  * release程序分离符号表：<https://www.cnblogs.com/cobbliu/articles/11985061.html>
  * GDB 调试 - 正确地加载调试符号文件<https://www.cnblogs.com/codingMozart/p/17117698.html>

## 后续添加的关于符号表文件单独加载的方法(可能和上面的重复)

* [可执行文件符号表导出与使用](https://asiocity.github.io/2021/01/02/binary-symbols-with-debug/) 介绍了符号表生成和绑定的方法

* [GDB 调试 - 正确地加载调试符号文件](https://www.cnblogs.com/codingMozart/p/17117698.html) 详细介绍了，如果符号表位置不是默认位置，如何指定符号表位置，如何加载符号表
