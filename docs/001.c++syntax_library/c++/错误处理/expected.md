# expected 基于 类型系统 的错误处理
```
// 1. 如果需要最接近C++23标准的行为
#include <tl/expected.hpp>  // 推荐

// 2. 如果已经在使用Abseil库
#include "absl/status/statusor.h"

// 3. 如果需要最丰富的功能集
#include <boost/outcome.hpp>

// 4. 等待C++23标准（如果项目可以使用最新标准）
#include <expected>  // C++23
```

Monadic 编程风格 在 C++ 中的用法，类似于函数式编程中的链式操作。支持的库：std::expected（c++23）,tl::expected,Boost.Outcome;这三个都支持链式错误处理。

```
auto result = step1()
    .and_then(step2)
    .and_then(step3)
    .transform(process_result)
    .or_else(handle_error);
```