# std::copy_exception() 和 std::current_exception() 在处理异常时的区别
# std::current_exception()
* 工作方式：
    * 捕获当前异常：只能在 catch 块内部调用
    * 保留原始异常：创建当前异常对象的副本
    * 异常类型保留：完整保留原始异常的类型信息
* 特点：
    * 必须在 catch 块内使用：如果在非异常上下文中调用，返回空的 exception_ptr
    * 保留调用栈：某些实现会保留部分调用栈信息
    * 资源消耗：需要实际抛出异常才能捕获
# std::copy_exception() (C++11 起)
* 工作方式：
    * 直接构造异常：不需要实际抛出异常
    * 模板函数：接受任意可复制类型
    * 无抛出保证：操作本身不会抛出异常
# 区别对比
|特性 | std::current_exception() | std::copy_exception() |
| :--- | :--- | :--- |
| 调用位置 | 只能在catch块内 | 任意位置 |
| 异常要求 | 需要实际抛出 | 无需抛出异常 |
| 性能开销 | 较高 | 较低(直接构造) |
| 异常类型保留 | 完整保留原始类型 | 保留指定类型 |
| 空指针风险 | 在非异常上下文中返回空指针 | 总是返回有效指针 |
| c++版本 | c++11 | c++11 |

# 实际应用场景
1. 直接创建已知异常
2. 跨线程异常传递
3. 自定义错误处理

# 注意事项
1. 类型擦除问题
    > p.set_exception(std::make_exception_ptr("Error"));// 错误：丢失具体异常类型 \
    >p.set_exception(std::copy_exception(std::runtime_error("Error")));// 正确：保留类型信息
2. 异常生命周期
    ```
    std::exception_ptr create_exception() {
    // 危险：局部对象将被销毁
    std::string msg = "Temporary error";
    return std::copy_exception(std::runtime_error(msg.c_str()));//std::runtime_error()是一个局部的临时对象在语句结束时就释放了。但是如果用copy_exception能够保证复制一个副本且线程安全。
    }
    ```
3. 最佳实践组合：
    ```
    try {
    risky_operation();
    } catch (...) {
    // 捕获未知异常时保留类型信息
    p.set_exception(std::current_exception());
    }
    ```

# 总结
1. std::current_exception()：
    * 用于捕获已抛出的异常
    * 保留完整异常上下文
    * 必须在 catch 块内使用
2. std::copy_exception()：
    * 直接构造异常不抛出
    * 性能更优（减少70%+开销）
    * 可在任意位置创建异常
    * 类型安全（避免字符串字面量问题）

# 建议
* 当处理已存在的异常 → 用 current_exception()
* 当创建新的异常对象 → 用 copy_exception()
* 性能关键路径 → 优先 copy_exception()