# c++11以后的单例模式

### 以往的实现方式(双重检查锁+内存屏障)
* blibili网址：BV12QATewEww

### 懒汉模式和饿汉模式
* 两个模式的区别就是一个把static T instance；放在get_instance()里作为局部静态变量，一个把static T instance；放在.cpp源文件中作为全局静态变量并初始化为一个具体的值，(如果初始化为空值，且配合call_once也是懒汉模式)，而导致初始化时机不同(一个在main函数之前初始化，一个在第一次调用时初始化)
* static局部变量和 call_once都可以实现懒汉模式
* 如果在全局初始化instance 在getinstace中直接返回就是饿汉模式

### 在 C++11 中，可以使用以下两种方式实现线程安全的单例模式：

1. 静态局部变量

这种方法利用 C++11 提供的静态局部变量初始化的线程安全特性。静态局部变量只会在第一次调用时初始化，并且该初始化过程是线程安全的。

```
class Singleton {
public:
    static Singleton& getInstance() {
        static Singleton instance; // 静态局部变量
        return instance;
    }
private:
    Singleton() = default;
    ~Singleton() = default;
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete
```

2. std::call_once 和 std::once_flag

```
#include <iostream>
#include <mutex>
#include <memory>

class Singleton {
public:
    static std::shared_ptr<Singleton> getInstance() {
        std::call_once(initFlag, &Singleton::initSingleton);
        return instance;
    }

    void doSomething() {
        std::cout << "Singleton instance address: " << this << std::endl;
    }

private:
    Singleton() = default;
    ~Singleton() = default;
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

    static void initSingleton() {
        instance.reset(new Singleton());
    }

    static std::shared_ptr<Singleton> instance;
    static std::once_flag initFlag;
    int value;//数据成员可以不是静态的(static)
};

std::shared_ptr<Singleton> Singleton::instance;
std::once_flag Singleton::initFlag;
```

3. 饿汉模式
```
class EagerSingleton {
public:
    // 获取唯一实例的方法
    static std::shared_ptr<EagerSingleton> getInstance() {
        return instance; // 直接返回静态成员变量
    }

    void doSomething() const {
        std::cout << "Doing something in Eager Singleton." << std::endl;
    }

private:
    // 私有构造函数，防止外部构造
    EagerSingleton() {
        std::cout << "EagerSingleton instance created." << std::endl;
    }
    
    // 禁止拷贝构造和赋值操作符
    EagerSingleton(const EagerSingleton&) = delete;
    EagerSingleton& operator=(const EagerSingleton&) = delete;

    // 静态成员变量，保存唯一的实例，使用 shared_ptr 包装
    static std::shared_ptr<EagerSingleton> instance;
};

// 定义并初始化静态成员变量
std::shared_ptr<EagerSingleton> EagerSingleton::instance = std::make_shared<EagerSingleton>();
```

##### 选择哪种方式？

* 静态局部变量方法实现简单且高效，适用于大多数情况。

* std::call_once 和 std::once_flag 方法更加灵活，适合需要复杂初始化逻辑的场景。

* 饿汉模式的使用场景：
    * 饿汉式单例模式是一种创建型设计模式，它确保一个类只有一个实例，并提供一个全局访问点来访问这个唯一的实例。与懒汉式单例不同，饿汉式单例在类加载时就立即创建实例，而不是在第一次使用时才创建。这种特性决定了它的适用场景和局限性。
    * 饿汉式单例的适用场景
        * 资源总是会被使用：如果确定该单例对象在整个应用程序生命周期内都会被使用，那么饿汉式单例是一个不错的选择。因为在这种情况下，提前初始化并不会带来额外的开销。
        * 初始化成本较低：如果单例对象的初始化成本不高（例如，不需要复杂的计算或外部资源加载），那么可以在程序启动时立即初始化，而不会对性能产生显著影响。
        * 线程安全需求：饿汉式单例由于其静态初始化特性，在C++中是天然线程安全的，无需额外同步机制即可保证在多线程环境下正常工作。
        * 简单性和可靠性优先：对于一些简单的应用或组件，可能更倾向于选择实现简单、易于理解和维护的设计方案。饿汉式单例实现起来相对简单，减少了复杂度和出错的可能性。
        * 需要快速响应：在某些应用场景下，要求首次调用时能够快速响应，不希望有延迟。由于饿汉式单例在程序启动时就已经完成初始化，所以在首次调用 getInstance() 方法时可以立即返回实例，没有延迟。
    * 不适合饿汉式单例的情况
        * 高初始化成本：如果单例对象的初始化过程非常耗时或者占用大量系统资源（如加载大型文件、数据库连接等），那么在程序启动时就进行初始化可能会导致启动时间过长，甚至浪费资源（如果该单例对象最终并未被使用）。
        * 不确定是否需要使用的对象：如果不能确定某个单例对象在整个应用程序生命周期内一定会被使用，那么采用懒汉式单例可能是更好的选择，因为它只会在真正需要的时候才进行初始化。
        * 依赖顺序问题：如果单例对象的构造函数依赖于其他静态对象的状态，并且这些静态对象的初始化顺序可能导致问题（即所谓的“静态初始化顺序唐突”），则需要特别小心。虽然饿汉式单例本身是线程安全的，但静态初始化顺序的问题仍然可能存在。
    * 示例场景
        * 日志记录器：一个常见的例子是日志记录器（Logger）。通常情况下，应用程序从启动到结束都需要记录日志信息，因此可以使用饿汉式单例来确保日志记录器始终可用。
        * 配置管理器：另一个例子是配置管理器，负责读取并缓存应用程序配置。如果配置文件较小且加载速度快，可以在程序启动时就加载所有必要的配置数据，以便后续随时访问。
        * 数据库连接池：尽管数据库连接池的初始化可能较为耗时，但在某些高性能要求的应用中，可能会选择在启动时预先建立一定数量的数据库连接，以减少用户请求时的等待时间。