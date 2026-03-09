# clangd_和compile_commands.json文件.md

* clangd插件<https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-clangd>
* compile_commands.json文件<https://www.cnblogs.com/cong-wang/p/15026530.html>
* clangd 出现找不到系统头文件错误的办法：设置clangd的arguments参数：“--query-driver=/usr/bin/gcc”为使用的编译器路径即可**这种情况在windows vs code上面很常见**。（默认的clang++不用），至于fallback flags也是参数设置，但是是备用的，没有compile_commands.json文件，且clangd找不到头文件，就会使用这个参数。

## 一个大坑

* `"--query-driver=d:/mingw/bin/g++.exe"`windows下中的盘符不能是大写，否则能识别，但是报找不到头文件的问题。更简单的方法是路径通配符**，例如推荐写法：`"--query-driver=**/mingw64/**/g++.exe"`.

* 一个说的非常详细的评论<https://github.com/clangd/clangd/issues/1394#issuecomment-1328676884> 关于找不到头文件的问题。且提到了为什么盘符不能是大写的原因：使用 --query-driver 指示 clangd 提取你正在构建编译器的内置包含路径，而不是用启发式方法去查找它们。注意，这要求你的项目有compile_commands.json，并且--query-driver的参数与compile_commands.json中命令的argvcompile_commands.json相匹配。（也就是说要一模一样：）

* 但是即使这样，还是在跳转头文件的时候有一些头文件提示无法找到。评论提到了第四种方法：手动提供编译器内置的包含路径，使用 clangd config file，或者（对于不使用 compile_commands.json）的项目使用 compile_flags.txt。.clangd使用的yaml格式需要两个空格缩进。下面是mingw的配置要求，使用的是`g++ -E -v - 输出 编译器真实的 include 搜索顺序`,但是除了c++ 和 c++/backward 有顺序其他的都无所谓，但是最好按照命令查出来的顺序。**重要**：盘符必须是小写（cmake tool基本都是这样，应为在linux下面是区分大小写的，所以需要精确匹配路径，而windows下的路径不区分大小写导致了这种问题）。
  ```yaml
  CompileFlags: 
    Add:
      - -isystem
      - d:/ProgramFiles/mingw64/lib/gcc/x86_64-w64-mingw32/15.2.0/include/c++
      - -isystem
      - d:/ProgramFiles/mingw64/lib/gcc/x86_64-w64-mingw32/15.2.0/include/c++/x86_64-w64-mingw32
      - -isystem
      - d:/ProgramFiles/mingw64/lib/gcc/x86_64-w64-mingw32/15.2.0/include/c++/backward
      - -isystem
      - d:/ProgramFiles/mingw64/lib/gcc/x86_64-w64-mingw32/15.2.0/include
      - -isystem
      - d:/ProgramFiles/mingw64/lib/gcc/x86_64-w64-mingw32/15.2.0/include-fixed
      - -isystem
      - d:/ProgramFiles/mingw64/x86_64-w64-mingw32/include
  ```

* 总结：以后--query-driver=的参数直接从compile_commands.json中复制过去一定没有问题。但是clangd--query-driver只涵盖源文件直接包含的文件，对于间接包含的文件还是报错，这就需要.clangd配置文件。（上一条）还有一个问题就是必须将g++.exe所在的路径加入到环境变量中。（且g++环境最好使用mys2安装，mingw64有很多问题（比如不支持utf-8导致gdb无法显示数据））


