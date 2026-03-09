# std::lock+lock_gard处理多个互斥锁

# 介绍
1. std::lock：
* 一个可变参数模板函数：template <class Lockable1, class Lockable2, class... LockableN> void lock(Lockable1&, Lockable2&, LockablesN&...);
* 原子性地锁定所有传入的互斥量，使用死锁避免算法（通常类似银行家算法）
* 要么锁定所有互斥量，要么一个都不锁定（异常安全）
* 不负责解锁，需要配合 RAII 包装器使用

2. std::lock_guard：
* RAII 风格的锁管理，构造时锁定，析构时解锁
* 可以配合 std::adopt_lock 标签，表示"接管已锁定的互斥量"

# 配合方式
```
std::mutex mtx1, mtx2, mtx3;
void func(){
    // 1. 使用 std::lock 原子性锁定所有互斥量
    std::lock(mtx1, mtx2, mtx3);
    // 2. 使用 lock_guard + adopt_lock 管理锁的生命周期
    std::lock_guard<std::mutex> lk1(mtx1, std::adopt_lock);
    std::lock_guard<std::mutex> lk2(mtx2, std::adopt_lock);
    std::lock_guard<std::mutex> lk3(mtx3, std::adopt_lock);
    // 3. 临界区操作
    // 4. 离开作用域时自动解锁（逆序析构）
}
```

# 优势/作用/目的
1. 提供原子性的多互斥量锁定
2. 死锁预防
3. 异常安全
4. 通过 RAII 确保资源释放

# c++17scoped_lock改进和最佳实践
1. 固定全局锁定顺序
2. 避免嵌套
3. 锁粒度控制