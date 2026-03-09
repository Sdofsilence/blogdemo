# c++类型支持（额外类型、数值极限、RTTI）

## 额外类型
* 头文件\<cstddef\>
* size_t , ptrdiff_t , nullptr_t , NULL , max_align_t , offsetof , byte(c++17)
* 头文件\<cstdint\>
* 定宽整数类型：int8_t , int_fast8_t , int_least8_t等及无符号版本。
* 定义类型相关的 数值极限 和 含least的整数常量函数宏

## 数值极限
* c++ 数值极限：std::numeric_limits 头文件\<limits\>
    * 成员常量：查询算术类型的属性
        * 常见的常量
        * is_specialized 对所有存在 std::numeric_limits 特化的 T 是 true.
        * is_signed 鉴别有符号类型
        * is_intager 鉴别整数类型
        * is_iec559 鉴别 IEC 559/IEEE 754 浮点数类型
    * 成员函数：返回最值、无穷值、
        * min() 主要用于获取数值类型的最小正数值（特别是对于浮点类型），而对于整数类型，则是获取最小的负数值。
        * lowest() 用于获取数值类型的最小可能值，无论其是否为正数或负数。对于浮点类型，它通常指向最大负数值；对于整数类型，它与 min() 相同，指向最小的负数值。
        * max() 返回数值类型 T 所能表示的最大有限值。对所有有界类型都有意义。
        * epsilon 返回 1.0 与给定浮点类型的下个可表示值的差 = 1ULP(浮点数最小增量) 用于比较浮点数是否相等
        * rand_error() 返回给定浮点数类型的最大舍入误差 【0.5，1】ULP 不常用
        * infinity() 返回给定浮点类型的正无穷大值
        * quiet_NaN() 返回给定浮点数类型的安静 NaN 值 不会触发浮点异常。参与运算时，结果仍然是 NaN，程序继续执行。
        * signaling_NaN() 返回给定浮点数类型的发信 NaN 首次参与运算时会触发浮点异常（需启用浮点异常捕获）。
        * denorm_min() 返回给定浮点数类型的最小正非正规值(极小远远小于最小正规值)
* c数值极限接口：
* 整数类型极限： 头文件\<climits\>
* 库类型别名极限： 头文件\<cstdint\>
* 浮点数类型极限：头文件\<cfloat\>

## RTTI
* \<typeinfo\> 
    * type_info 包含某个类型的信息，typeid 运算符所返回的类
    * bad_typeid 当typeid 表达式中的实参为空值时抛出的异常
    * bad_cast 由非法的 dynamic_cast 表达式（即引用类型转型失败）抛出的异常
* \<typeindex\>
    * type_index 对type_info 对象的包装，用作关联容器和无序关联容器的索引` std::unordered_map<std::type_index, std::string> type_names;   type_names[std::type_index(typeid(int))] = "int";`