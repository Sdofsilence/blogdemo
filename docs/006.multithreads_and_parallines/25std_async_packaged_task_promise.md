# std::async std::packaged_task std::promise
# std::async
* 在 C++ 中，std::async 的行为取决于其启动策略（std::launch 策略）
    * 显示指定std::launch::async: 强制异步执行，创建新线程
    * 显示指定std::launch::deferred: 延迟到get()/wait()时在调用线程中执行，不创建新线程
    * 默认策略:实现选择不强制要求不确定，可能是以上的其中一个，不要依赖默认策略，除非接受不确定的行为。

# std::packaged_task
* 使用场景：
    * 需要将任务函数放入任务队列，由线程池异步执行。
    * 需要分离任务提交和结果获取(典型 生产者-消费者 模型)

# std::promise
* 使用场景：
    * 需要在函数中或复杂逻辑中手动设置结果。
    * 实现自定义的异步模式(如基于事件的I/O)
    * 需要跨线程传递非返回值的结果(如通过引用修改的变量)

# 选择建议
1. 简单的异步获取函数结果：std::async
2. 需要在任意位置手动设置结果：std::promise
3. 需要异步执行函数并获取结果：std::packaged_task
4. 2和3可以组合但是不建议。