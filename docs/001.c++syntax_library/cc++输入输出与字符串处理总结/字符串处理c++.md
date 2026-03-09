# c++字符串处理
* 使用algorithm的算法
* 使用string头文件中的函数和成员函数

# basic_string成员函数

### 赋值
* assign 优先与 operator=

### 访问
* at
* operator[]
* front back
* data 返回指向字符串首字符的指针不同于c_str只能返回const char*,data可返回char*,可修改。c++11后保证同c_str空终止。const只可以重载成员函数。全局不可以。
* c_str 返回字符串的不可修改的标准 C 字符数组版本
* operator basic_string_view 返回到整个字符串的不可修改的 basic_string_view

### 容量
* empty size/length max_size 
* capacity 返回当前对象分配的存储空间能保存的字符数量
* reserve(new_cap) 如果 new_cap 大于当前 capacity()，那么分配新存储，并使 capacity() 大于或等于 new_cap。如果 new_cap 小于或等于当前 capacity()，那么没有效果。(C++20 起)所以不建议小于capacity()。如需要使用shrink_to_fit。
* shrink_to_fit() 配合clear()使用来默认化容量重用对象。这是减少 capacity() 到 size() 的非强制请求。是否满足请求取依赖于实现。保证capacitpy大于等于size()。

### 修改
* clear 如同通过执行 erase(begin(), end()) 从字符串中移除所有字符。此调用后 size() 返回零。C++ 标准未明确要求此函数不更改 capacity，但既存实现都不更改容量。这意味着它们不释放分配的内存。
* insert
* erase
* push/pop_back
* append/ 类似于assign 强于operator+=
* repalce
* copy 复制子串 [pos, pos + count) 到 dest 指向的字符串。如果请求的子串越过字符串结尾，或 count == npos，那么复制的子串是 [pos, size())。产生的字符串不是空终止的。
* resize(size_type[,charT]) 重设字符串大小，使其包含 count 个字符。大于添加默认的charT值或给出的值，小于则缩减size到指定值。
* swap(& other) 字符串交换

### 查找
* find rfind 子串第一次和最后一次出现
* find_first_[not]_of 首次出现、缺失 
* find_last_[not]_of 最后一次出现、缺失

### 其他
* compare([p1,count1,]const &/*str[,[p2],count2]) 比较两个字符序列【p1,p1+count1),[p2,p2+count2)
* substr(pos=0,count=npos) 含子串 [pos, pos + count) 或 [pos, size()) 的字符串。

# 非成员函数
* swap erase_[if]
* operator+ == <<>>(ios,str) getline(input,str,delim)

### string数值转换
* stoi stol stoll stoul stoull stof stod stold string到整数浮点数
* to_wstring to_string 整数浮点数到string wstring