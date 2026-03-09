# c++测试框架

* [jetbrain-cline单元测试指南](https://www.jetbrains.com/help/clion/unit-testing-tutorial.html) 介绍了gtest boost-test catch2 Doctest四个框架的集成

## ctest

* 在测试目录下的CMakeLists.txt中添加include(CTest)/enable_testing()
* 使用add_test()添加测试程序
* 在构建目录下运行cmake --build . --target test，使用 ctest 运行测试
  ```
  -R <regex>            Run tests matching regular expression
  -E <regex>            Exclude tests matching regular expression
  -L <regex>            Run tests with labels matching the regex
  -LE <regex>           Run tests with labels not matching regexp
  -C <config>           Choose the configuration to test
  -V,--verbose          Enable verbose output from tests.
  -N,--show-only        Disable actual execution of tests.
  -I [Start,End,Stride,test#,test#|Test file] Run specific tests by range and number.
  -H                           Display a help message
  ```
* 测试用例手动在主程序中硬编码
* 默认情况下，如果满足以下所有条件，则测试通过
  * 测试找到了测试**可执行文件**
  * 测试运行时**未出现异常**
  * 测试以**返回代码0**结束

### ctest 问题

* enable_testing()位置：建议只在顶层CMakeLists.txt中添加，子目录中添加会报错，而顶层CMakeLists.txt中不添加，则测试用例无法发现（vscode 插件的问题）。

## catch2

### catch2 cmake配置

```cmake
add_executable(testkmp testkmp.cpp)

include(FetchContent)

FetchContent_Declare(
Catch2
GIT_REPOSITORY https://github.com/catchorg/Catch2.git
GIT_TAG v3.8.1 # Use the latest stable release
)

FetchContent_MakeAvailable(Catch2)
message(STATUS "Catch2 source dir: ${catch2_SOURCE_DIR}")
message(STATUS "Catch2 binary dir: ${catch2_BINARY_DIR}")

target_link_libraries(testkmp PRIVATE Catch2::Catch2WithMain)

list(APPEND CMAKE_MODULE_PATH ${catch2_SOURCE_DIR}/extras)
include(CTest)
include(Catch)
catch_discover_tests(testkmp)
```

### 测试用例组织方式

| 情况 | 推荐方式 | 原因 |
| ---------------------------- | ---------------------------- | ------------ |
| **简单函数，少量测试** | 一个 TEST_CASE 中多个断言 | 简单直接 |
| **多个相关场景** | TEST_CASE 中包含多个 SECTION | 保持独立性 |
| **完全不相关的测试** | 多个独立的 TEST_CASE | 完全隔离 |
| **行为驱动开发** | SCENARIO/GIVEN/WHEN/THEN | 描述行为 |
| **大量输入输出对** | 参数化测试 | 减少重复代码 |

### 使用例子：

简单的测试用例:一个断言失败则用例失败

```c++
#include <catch2/catch_test_macros.hpp>

TEST_CASE( "Factorials are computed", "[factorial]" ) {
    REQUIRE( Factorial(1) == 1 );
    REQUIRE( Factorial(2) == 2 );
    REQUIRE( Factorial(3) == 6 );
    REQUIRE( Factorial(10) == 3628800 );
}
```

测试用例中包含多个section,前面的断言共享，但是section之间相互独立互不影响

```c++
TEST_CASE( "vectors can be sized and resized", "[vector]" ) {

    std::vector<int> v( 5 );

    REQUIRE( v.size() == 5 );
    REQUIRE( v.capacity() >= 5 );

    SECTION( "resizing bigger changes size and capacity" ) {
        v.resize( 10 );

        REQUIRE( v.size() == 10 );
        REQUIRE( v.capacity() >= 10 );
    }
    SECTION( "resizing smaller changes size but not capacity" ) {
        v.resize( 0 );

        REQUIRE( v.size() == 0 );
        REQUIRE( v.capacity() >= 5 );
    }
    SECTION( "reserving bigger changes capacity but not size" ) {
        v.reserve( 10 );

        REQUIRE( v.size() == 5 );
        REQUIRE( v.capacity() >= 10 );
    }
    SECTION( "reserving smaller does not change size or capacity" ) {
        v.reserve( 0 );

        REQUIRE( v.size() == 5 );
        REQUIRE( v.capacity() >= 5 );
    }
}
```

当然，你可以把测试用例组织成多个测试用例，一个用例一个test_case，一个test_case应对一个场景

此外，你还可以使用BDD测试风格的宏定义：SCENARIO/GIVEN/WHEN/THEN。

## googletest(对于单元测试来说过于复杂)

* [GoogleTest User’s Guide](https://google.github.io/googletest/)
* [gTest和gMock指引](https://www.yuyoung32.com/post/gtest%E5%92%8Cgmock%E6%8C%87%E5%BC%95/)

## 测试框架和mock工具的搭配选择

社区中常见的 Catch2 + mock 组合：综合官方 issue、IDE 文档和博客，社区常见的几种组合可以大致概括为：

* 优先选择（现代 C++ 项目）：
    * Catch2 + Trompeloeil
    * 使用文档： <https://accu.org/conf-docs/PDFs_2017/Bj%C3%B6rn_Fahller_Trompeloeil.pdf>
    * cook book: <https://github.com/rollbear/trompeloeil/blob/main/docs/CookBook.md>
    * CLion 官方文档、Trompeloeil 文档都明确提及适配与集成方式，是社区目前最主流的组合之一。
    * jetbrains.com/help/clion/catch-tests-support.html

* 简洁优先 / 中小型项目：
    * Catch2 + FakeIt
    * 在 GitHub issue 中被提到，API 比较漂亮，文档里也强调可与各种测试框架搭配使用。
    * https://github.com/catchorg/Catch2/issues/1659

* 较老项目或已有 HippoMocks 使用习惯：
    * Catch2 + HippoMocks
    * CLion 文档直接点名，说明这个组合在实践中是可行且被工具支持的。
    * jetbrains.com/help/clion/catch-tests-support.html
    * 遗留代码 / C 代码 / 重构成本高：
* Catch2 + FSeam
    * FSeam 的文章和示例中就以 Catch2 作为测试框架演示其用法，尤其适合“不改接口就能 mock”的场景。
* 对于“已经深度使用 Google Test/gMock”的团队：
    * 一般就停留在 gTest + gMock，没必要强行把 Catch2 混进来；
    * 如果只是想在少量模块里用 Catch2 的语法，又想复用既有 gMock 期望，可以考虑让这两个框架在不同测试可执行文件里共存，但工程复杂度较高，不推荐作为常规方案。