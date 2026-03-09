# operator new 和 delete 重载的形式

## operator new , operator []

### 全局操new作符分类

1. 分为非数组版本可数组版本

2. 简单分为常用形式size_t参数调用malloc分配内存 和 自c++17起的带alignment参数的,调用std::aligned_alloc分配内存的形式(所以new简单来说有四个版本,如果是c++17前，两个版本即1.)
    > void* operator new  ( std::size_t count ); \
    > void* operator new[]( std::size_t count ); \
    > void* operator new  ( std::size_t count, std::align_val_t al );  (自 C++17 起)\
    > void* operator new[]( std::size_t count, std::align_val_t al );  (自 C++17 起)

3. 分为带`const std::nothrow_t& tag`的 noexcept 非抛出异常版本 和 一般的可抛出异常版本(仅仅是全局的，用户定义的由用户自己决定和提供是否抛出异常)

4. (总结)：分为全局的操作符 和 类限定的操作符 + 非分配的new 和 new[],默认参数类型为(size_t,void*)的两个操作符只可以是全局的，且不可以替换(所以一般来说全局的有4种可替换(不算是否抛出异常)+2种new操作符不可以替换和重载，有用户参数args的重载也是四种)(c++17前则只有2(不算是否抛出异常)+2种，可以替换的只有两种即最简单的两个new和new[])
    * 用户可替换的一般new类型
        > 5.中提到的8种类型可以替换
    * 用户定义就地分配函数的类型
        > void* operator new  ( std::size_t count , /* args... */); \
        > void* operator new[]( std::size_t count , /* args... */); \
        > void* operator new  ( std::size_t count, std::align_val_t al , /* args... */);  (自 C++17 起)\
        > void* operator new[]( std::size_t count, std::align_val_t al , /* args... */);  (自 C++17 起)
    * 用户不可定义(替换)的类型(第二个参数为void*类型就地分配函数)
    > void* operator new  ( std::size_t count, void* ptr );
    > void* operator new[]( std::size_t count, void* ptr );

5. 由1.2.3.定义的 2(没有额为参数)\*2(带对齐值参数)\*2(带非抛出异常的tag参数) = 8 种操作符可以替换( 如果提供了替换函数，则将使用它来代替实现提供的默认版本。 这种替换发生在程序启动之前。) 最基本的是2(没有额为参数)\*2(带对齐值参数)=4种。

### 类特定的分配函数

1. 类特定的分配函数，最简单的两种定义在类内的static重载(算上是否有std::align_val_t参数有四种)。

2. 类特定的就地分配函数，形式同1.种的四种，但是多出可变的参数args...。

3. 如果类级别的 operator new 是模板函数，则它必须具有 void* 的返回类型，第一个参数 std::size_t，并且必须具有两个或多个参数。换句话说，只有就地形式可以是模板。

## operator delete 和 operator delete[]

### 全局operator delete

1. 区分为可替换的一般(根据是否带std::size_t 参数和 std::align_val_t参数分为8种) 和 可替换的放置释放函数(对应四种普通的operator new参数但是多一个std::nothrow_t&参数) 非分配的放置释放函数void(void*,void*)形式不可替换 用户定义的放置释放函数void(void*,/\*args...\*/)形式(分别有8,4,2,2种，共16种(前12种可以换有默认实现))前8种由delete 或 delete[]调用，后4种和2种当初始化失败或者放置式new表达式抛出异常时自动调用，同样的后两种对应operator new(void*,void*)不可替换用于非分配式放置释放函数,最后的两种是全局用户重载的带args...参数的相同参数的放置式new表达式调用。

2.除了用户定义的放置释放函数和内特定的operator delete不为noexcept,其他可替换的全局operator delete都是noexcept修饰的

### 类特有的释放函数operator delete

1. 同理同全局(根据是否带std::size_t 参数和 std::align_val_t参数分为8种)

2. 类特有的放置释放函数(带args...参数的2种)

3. 类特有的c++20开始有的销毁式释放函数(忽略)

## 总结

* 基本的重载使用可以分为六种(基于c++11的)：
    1. 最简单的可替换的两种(一般new表达式) void* new(std::size_t) void delete(void*) noexcept;
    2. (可以不关注，只需记住这种形式不可以自定义)不可以替换的两种(全局不可替换不可重载放置式new表达式) void* new(std::size_t,void*) void delete(void*,void*) noexcept;
    3. 用户定义的带args参数的两种(放置式new表达式)。void* new(std::size_t,/args.../) void delete(void*,/args.../)

* 对于类特定的则只需要关注1.和3.:即一般的两种 和 (放置new表达式)带args...额外参数的两种。

## 一些需要注意的小问题

* 即使未包含 \<new\> 头文件，重载operator new(1-4) 也会在每个翻译单元中隐式声明。即使未包含 \<new\> 头文件，重载operator delete (1-8(和new对应的4种，加上4种带size_t参数的delete)) 也会在每个翻译单元中隐式声明。

* (标准关于delete的是否带size_t参数的选择(只有类特定的是明确的))如果找到的释放函数是类特定的，则大小未知类特定的释放函数（不带 std::size_t 类型的参数）优先于大小感知类特定的释放函数（带有 std::size_t 类型的参数）。否则，查找达到全局作用域，并且如果类型是完整的，并且仅对于数组形式，操作数是指向具有非平凡析构函数或其（可能是多维）数组的类类型的指针，则选择全局大小感知全局函数（带有 std::size_t 类型的参数）。否则，未指定是选择全局大小感知释放函数（带有 std::size_t 类型的参数）还是全局大小未知释放函数（不带std::size_t 类型的参数）。

* 不要在重载的operator new中使用可能会导致内存分配的函数例如std::cout,会导致递归调用operator new导致栈溢出。
* 重载 operator new 时，应该始终处理 size == 0 的情况，一般处理: 当 size = 0时，size = 1 分配一个大小为1的内存。
* 当分配失败时需要处理，返回nullptr,或者抛出std::bad_alloc异常。以下是一种可能的nothrow版本的实现(对throw版本的封装)
    > void* operator new(std::size_t size, const std::nothrow_t&) noexcept { \
    > try { return operator new(size); } \
    > catch (...) { return nullptr; }  // 捕获所有异常 \
    > }

* 在性能关键场景优先使用内存池而非全局重载，不要依赖于全局重载，而是直接使用内存池/对象池。

* 如何引入全局的operator new/delete?
  * using 在类中只能引入基类中的成员函数或类型；所以using ::operator new;是错的。
  * 如果你想在类中使用全局的 operator new：应该使用代理的函数/适配器函数,直接调用它:`return ::operator new(size)`.
  * 不要在类中使用using ::operator new;这是语法错误。
