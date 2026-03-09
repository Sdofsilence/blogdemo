# c++\<random\>头文件的使用
* 参考cppreference: <https://zh.cppreference.com/w/cpp/numeric/random>

## 内容
* 均匀随机位生成器 ：1.生成均匀分布的整数序列的 伪随机数生成器 2.非确定的 随机数生成器(如果可用)std::random_device
* 随机数分布：1.均匀分布 2.正太分布 3. 伯努利分布 4. 泊松分布
* 均匀随机位生成器和分布被设计为一起使用以生成随机值。所有随机数引擎都可以进行明确播种、序列化和反序列化，以用于可重复的模拟器。

### 随机数引擎
* 是以种子数据为熵源生成伪随机数的**均匀随机位生成器**。
* 主要有：线性同于算法、梅森缠绕器算法、延迟斐波那契算法、基于计数器的可并行算法

### 随机数引擎适配器
* discard_block_engine<engine,size p,size r> 舍弃部分输出(块大小p：表示每次从基础引擎抽取多少个随机数。每个块使保留的数量 0< r <=p：表示在这 p 个随机数中保留多少个，剩余的将被丢弃。) 每当 discard_block_engine 需要生成一个新的随机数时，它会从基础引擎中获取 p 个随机数，并只保留其中的前 p-r 个，剩下的 r 个则被丢弃。每当 std::discard_block_engine 需要生成一个新的随机数时，它会执行以下步骤：1.从基础引擎获取 p 个随机数。2.只保留这些随机数中的前 p-r 个，剩下的 r 个被丢弃。3.返回第一个保留的随机数作为结果。4.对于接下来的请求，继续返回剩余保留的随机数，直到当前批次的保留随机数用完，然后重复上述过程。无论调用 gen() 多少次，只要还有保留的随机数未被使用，就不会触发新一轮的基础引擎抽样。只有当所有保留的随机数都已被使用后，才会从基础引擎获取新的随机数集合。

* independet_bits_engine<engine,size W,UIntType> 产生位数与被包装引擎所产生者不同的随机数。W 必须大于零，且不大于 std::numeric_limits<UIntType>::digits。

* shuffle_order_engin<engine,size K> 它打乱基础引擎生成的随机数。K:内部表大小,必须大于0 。它维护一个大小为 K 的表，并在请求时派送从该表随机选择的数，并将它替换为基础引擎生成的数。

### 预定义的随机数生成器
* default_random_engine 依赖于实现在msvc中是mt19937
* 最小标准(线性同余)：minstd_rand0 minstd_rand
* 梅森缠绕器：mt19937 mt19937_64
* 延迟斐波那契 ranlux24_base ranlux48_base
* discard_block_engin延迟斐波那契 ranlux24 ranlux48
* shuffle_order_engine<minstd_rand0,256> knuth_b

### 随机数分布
* 均匀分布 1. uniform_int_distribution范围[a,b] 2. uniform_real_distribution范围[a,b)
* 正太分布(实数) 1. nomal_distribution(mean(均值),stddev(标准差))(标准/高斯分布) 其他：对数、平方、柯西、费舍尔f、学生t分布。
* 泊松分布 1.poisson_distribution 其他：指数、伽马、威布尔、极值分布。
* 伯努利分布 1.bernoulli_distribution(产生bool值) 其他：二项分布、负二项分布、几何分布。
* 采样分布: 1.discrete_distribution(离散分布) 其他：分段均匀分布、分段线性分布

### 工具
generate_canonical 预设的[0,1)的均匀随机浮点数
seed_seq 种子序列生成器 1.(it_begin,it_end) 2.(std::initializer_list<T> il) 成员函数.generate(out_it_begin,out_it_end)用于产生无偏差的种子序列。

### 实际使用的两个注意点
1. 引擎种子值的设置
    * 对象初始化时设置 1.(result_type value) 2.(std::seed_seq seq)
    * seed()成员函数 1.seed(result_type value) 2.(std::seed_seq seq) 
    * 获取种子类型例如：normal_distribution<float>::result_type
2. 随机数分布对象的参数设置
    * 对象初始化时设置 1.(分布参数...) 2.(const param_type& params)
    * param(const param_type& params)成员函数 
    * params类型为分布类型的成员类型:例，uniform_int_distribution<int>::param_type 它是一个结构体，可以直接赋值分布参数，也有一个公开成员函数_Init(分布参数)设置参数 如果参数为空为查询返回当前的params。

### 具体使用场景
* 常用的随机数引擎有 default_random_engine、mt19937、mt19937_64、minstd_rand0、minstd_rand 。

1. 获取确定性随机数
    * 使用固定的value 或 固定的seq 初始化或seed() 设置随机数引擎。调用 operator()即可。

2. 获取非确定性随机数
    * 使用std::random_device 播种随机数引擎 或 它产生的序列初始化 seq 再用seq 初始化或seed() 设置随机数引擎。

3. 获取满足具体随机分布的随机数(确定性依赖于参数的engine是1.2.中的哪一种)
    * 把随机数引擎对象作为 随机分布对象的第一个参数
    * 随机分布对象的两种调用方式 1.operator(engine) 2.(engine, const param_type& params)(不修改参数params，只生效一次);
    * 确定性依赖于参数中的随机数引擎的确定性。

### 例子
~~~
std::random_device r;
std::default_random_engine e1(r());
std::uniform_int_distribution<int> uniform_dist(1, 6);

int rand_int = uniform_dist(e1);
std::cout << "随机选择的值：" << rand_int << '\n';
~~~