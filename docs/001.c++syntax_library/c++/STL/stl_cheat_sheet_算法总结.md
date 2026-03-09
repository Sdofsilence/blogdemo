# c++stl cheat sheet
* geeksforgeeks:<https://www.geeksforgeeks.org/cpp-stl-cheat-sheet/>
* tutorialspoint:<https://www.tutorialspoint.com/cpp_standard_library/cpp_stl_cheat_sheet.htm>
* 关于函数对象的详细：<https://zh.cppreference.com/w/cpp/utility/functional#.E9.80.8F.E6.98.8E.E5.87.BD.E6.95.B0.E5.AF.B9.E8.B1.A1>
* geeksforgeeks_stl_algorithm: <https://www.geeksforgeeks.org/algorithms-library-c-stl/>
* cppreference_stl_algorithm:<https://zh.cppreference.com/w/cpp/algorithm>
* c++stl_quizzes:<https://www.geeksforgeeks.org/cpp-stl-quizzes/>
* A Complete Guide to Standard C++ Algorithms:<https://github.com/HappyCerberus/book-cpp-algorithms> <"F:\tmp\workslog_classify_by_category\101.资源\a_complete_guide_to_standard_cpp_algorithms_v1_0_1.pdf">

## cppreference_stl_算法总结
* count equal mismatch copy move
* 增0删2改7查10
* 删：remove unique
* 改：生成|变换|顺序 ：生成: fill generate 变换: transform replace 顺序：reverse rotate shuffle
* 查：[all/any/none]_of find_[if]_[not] adjacent_find find_first_of [search find_end 子序列]
* 五加四
* 五：划分 二分 排序 归并 集合
* 五：最值 堆 排列 数值运算

## cppreference_stl_算法分类

### 搜索
* all_of any_of none_of
* find find_if find_if_not 在范围 [first, last) 中搜索元素第一次出现的位置。
* find_first_of 在范围 [first, last) 中搜索范围 [s_first, s_last) 中的任何元素。
* adjacent_find(相邻两个)
* count count_if
* equal
* mismatch

******
* search search_n(连续) 搜索范围 [first, last) 中首次出现元素序列 [s_first, s_last) 的位置。
* find_end 在范围 [first, last) 中搜索序列 [s_first, s_last) 最后一次出现的位置。

### 复制
* copy copy_if copy_n copy_backwark
* replace_copy replace_copy_if
* remove_copy remove_copy_if
* unique_copy
* reverse_copy
* rotate_copy
* partition_copy
* partial_sort_copy
* move move_backwark

### 交换
* swap swap_ranges iter_swap(交换迭代器所指元素)

### 变换
* transform replace replace_if

### 生成
* 复制方式：fill fill_n
* generate generate_n 以给定函数对象 g 所生成的值对范围 [first, last) 中的每(n)个元素赋值。

### 移除
* remove remove_if unique

### 顺序变更
* reverse rotate random_shuffle/shuffle

### 采样
* sample

### 划分
* is_partitioned partition stable_partition partition_point

### 排序
* sort stable_sort partial_sort(topk) is_sorted is_sorted_until nth_element(快速选择算法)

### 二分(在已划分范围上)
* lower_bound(最常用返回iter) upper_bound equal_range(前两个的组合) binary_search(只有判断)

### 集合(在已排序范围上)
* includes set_union set_intersection set_difference set_symmetric_difference

### 归并
* merge inplace_merge

### 堆
* make_heap push_head pop__head sort_head is_head is_head_until

### 最大最小
* max/min minmax min/max_element minmax_element clamp(c++17夹逼)

### 字典序比较
* lexicographical_compare

### 排列
* is_permutation prev_permutation next_permutation

### 数值计算
* iota accumalate inner_product adjacent_defference partial_sum [reduce c++17]

## cppreference_stl_算法需要注意的事项全局和成员的不同

* 全局find是查找一个元素，string find是查找一个子串，find_first_of是查找一个子串中的任意一个元素也可以是一个字符
* 全局replace是替换一个元素，string replace是替换一个子串范围成为另一个范围(完完全全的不一样)
