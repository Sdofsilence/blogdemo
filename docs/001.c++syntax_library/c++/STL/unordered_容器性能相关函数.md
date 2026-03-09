# unordered_容器性能相关函数

* [揭秘 C++ 无序映射：std::unordered_map::bucket_count 完全指南](https://runebook.dev/zh/docs/cpp/container/unordered_map/bucket_count)

## 桶接口

* begin(size_t n) end(size_t n) : 和一般的begin() end()不一样这是带参数的，返回的是第n个桶的迭代器
* bucket_count()
* bucket_size(size_t n) 返回第n个桶的元素个数
* bucket(const Key& key) ：返回key的桶编号
* max_bucket_count() 返回容器由于系统或库实现限制而能够容纳的最大桶数。

## 哈希策略接口

* load_factor() 返回每个桶的平均元素数量，即 size() 除以 bucket_count()。
* max_load_factor()/(设置为ml)max_load_factor(float ml) 管理最大加载因子（每个桶中的元素数量）。如果加载因子超过此阈值，容器会自动增加桶的数量。
* rehash(size_t count) 改变桶的数量，至少为count
* reserve(size_t count) 调用 rehash(std::ceil(count / max_load_factor()))

## 观察器

* hash_function() 返回用于哈希键的函数
* key_eq() 返回用于比较键是否相等的函数
