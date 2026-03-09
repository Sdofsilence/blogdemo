# c++11、14、17、20cppreference简介

## c++11
* static_assert
* long long
* auto
* 委托构造函数
* constexpr
* char16_t char32_t
* type_tarits类型特征
* 模版别名
* alignsas alignsof
* default delete默认函数
* 类型enum
* atomic
* nullptr
* 显示转换运算符static/const/dynmic/reinterpet_cast
* & &&
* u U unicode字符字面量
* 继承构造函数
* inline namespace
* thread_local
* 列表初始化
* 非静态成员初始化器
* 前置enum声明
* 用户定义字面量
* 右值引用
* lambda表达式
* 移动成员函数
* override final
* decltype
* 智能指针
### headers
* 5个数据结构 array tuple foward_list unordered_map unordered_set
* 5个多线程 atomic future thread mutex conditon_variavle
* 1一个 funcational 函数包装器 引用包装器ref cref
* 2个类型库 type_traits ratio
* 3个工具库 random regex chrono

## c++14
* 0b二进制字面量
* decltype(auto) 按decltype规则的auto
* lambda 初始化捕获
* 泛型lambda
* 变量模版
* constexpr拓展
* 默认初始化器的聚合体
* 单引号分隔数字
### headers无添加的头文件但是有修改
* std::make_unique
* std::shared_timed_mutex 与 std::shared_lock
* std::integer_sequence
* std::exchange
* std::quoted
* 其他小改进：某些算法的双范围重载 类型特征的类型别名版本 basic_string、 duration 与 complex 的用户定义字面量

## c++17
* 下列特性被合并入 C++17：
    * 来自文件系统 TS：文件系统库
    * 来自库基础 v1 TS：std::variant、std::any、std::optional、std::string_view、std::apply、多态分配器、搜索器等   等
    * 来自库基础 v2 TS：std::void_t、std::conjunction、std::disjunction、std::negation、std::not_fn、std::gcd、 std::lcm
    * 来自并行 v1 TS：执行策略、std::reduce、std::inclusive_scan、std::exclusive_scan 等等，但不包括exception_list

### 语言特性
* u8字面量
* lambda捕获*this
* 变量： inline变量 结构化绑定 if switch初始化语句
* 模版： 折叠表达式 类模版实参推导 aoto非类型形参
* 命名空间：简化嵌套 。。。
* 三个属性 fallthrouth maybe_unused nodiscard
* _has_include
### 新的头文件
* 五个个工具类 variant optional any string_view byte
* 五个个头文件(并发、文件系统、内存管理、字符转换、数学特殊函数) execution filesystem memory_resource charconv
### 其他
* scoped_lock timespec_get chrono取整函数

## c++20
* 提供了一些重要功能特性（概念与约束、模块、协程和范围）新的库fomat 线程库 库特性测试宏 chrono日历和时区库 数学常数
### headers
* concepts coroutine ranges span format...(bit compare numbers source_localtion syncstream version)
* 四个线程库：barrier latch semaphore stop_token 