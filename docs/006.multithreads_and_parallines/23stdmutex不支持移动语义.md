# std::mutex不支持拷贝和移动语义

# 原因1：状态唯一性
* std::mutex 封装了操作系统底层的同步原语（如 pthread 的 mutex）。这些资源：
    * 绑定到特定内存地址
    * 不能被"转移"到新内存位置
    * 移动会导致原对象和新对象状态不一致

# 原因2：线程安全性保障
* 如果允许移动：
    * 锁定状态下移动：新/旧对象谁负责解锁？
    * 并发访问时移动：其他线程无法感知资源已转移

# 实际影响的场景
1. 需要在容器中存储互斥量
    ```
    std::vector<std::mutex> mutexes;  // ❌ 错误！需要可移动元素
    // 正确方案：存储指针
    std::vector<std::unique_ptr<std::mutex>> mutexes;
    // 初始化
    mutexes.push_back(std::make_unique<std::mutex>());
    ```
2. 需要转移锁的管理权
    * 说明了包含锁的数据对象的拷贝和移动函数的锁成员需要新建。
    ```
    class Resource {
        std::mutex mtx_;
        // ... 数据 ...
    };
    // 错误尝试：移动整个资源对象
    Resource res1;
    Resource res2 = std::move(res1);  // ❌ 编译失败
    // 正确方案：转移数据所有权，重建互斥量
    class Resource {
        std::mutex mtx_;  // 互斥量本身不可移动
        Data data_;       // 但数据可以移动
    public:
        Resource(Resource&& other) 
            : data_(std::move(other.data_)) 
        {
            // 注意：mtx_ 被默认构造（新互斥量）
        }
    };
    ```

# 代替方案
虽然 std::mutex 不可移动，但锁管理器可以移动：

1. std::unique_lock（可移动）
2. std::shared_lock（C++14，可移动）

# 总结
1. std::mutex 本身不可拷贝/移动 - 设计上禁止转移所有权
2. 需要转移锁控制权时，使用 std::unique_lock
3. 容器中存储互斥量需用 指针（如 std::unique_ptr<std::mutex>）
4. 移动包含互斥量的类时，需 显式处理互斥量状态（通常重建）