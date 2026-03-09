# todo 2025.3.16

* 用一个.cpp文件的方式记住c++11 14 17 的新语法和功能，例如：

* c++11：
```
enum class/struct:type{red = 常量，...}
class Cbase [final] { //final override default delete关键字 noexcept
    //移动构造复制运算符，移动语义 const_cast.static_cast.dynamic_cast.reinterpret_cast
    initializer_list 列表初始化
    委托与继承的构造函数
    alignof alignas
}

```

template<typename... Args>
decltype(auto) func()[->int] {
    return 2.0;
}

template<typename T>
using my_vector = std::vector<T>;

template<>
constexpr using pi = 3.1415926535; 

int main(){
    nullptr,long long , 
    range-for
    funcation bind lambda
    thread mutex conditon_variable atomic future task_packge
    regex,tuple,pair,random,
    unordered_set/map,smart_ptr,ratio,type_traits,chrono,
}