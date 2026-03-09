# random chrono （time.h）regex


## random

```cpp
#include <random>

void test_for_random() {
	std::random_device rd;
	std::mt19937 rd_engine(rd());
	std::uniform_int_distribution<> distri(0, 9);
	std::array<int, 10> arrs{};
	for (int i = 0; i < 10; ++i) {
		std::cout << i << ":" << arrs[i] << std::endl;
	}


	for (int i = 0; i < 1000000; ++i) {
		++(arrs[distri(rd_engine)]);
	}

	for (int i = 0; i < 10; ++i) {
		std::cout << i << ":" << arrs[i] << std::endl;
	}
}
```


## chrono (time.h)

* [C++ std::chrono库使用指南 (实现C++ 获取日期,时间戳,计时等功能)](https://zhuanlan.zhihu.com/p/679451085)
* [C标准库-time.h](https://developer.aliyun.com/article/1604761)

```cpp
#include <chrono>
#include<windows.h>
#include<time.h>


void test_for_chrono() {

	auto time_begin = std::chrono::steady_clock::now();
	Sleep(10);
	auto time_end = std::chrono::steady_clock::now();
	auto time_travel = std::chrono::duration_cast<std::chrono::milliseconds>(time_end - time_begin);
	std::cout << time_travel.count() << "ms" << std::endl;
	//--------------------------------------
	auto start = std::chrono::high_resolution_clock::now();
	Sleep(10);
	auto stop = std::chrono::high_resolution_clock::now();

	auto duration = std::chrono::duration_cast<std::chrono::milliseconds>(stop - start);

	std::cout << "Time taken by function: " << duration.count() << " microseconds" << std::endl;
	//--------------------------------------
	auto time1 = std::time(nullptr);
	Sleep(10);
	auto time2 = std::time(nullptr);

	time_t difftime = time2 - time1;

	auto tm = std::localtime(&difftime);  //这里需要区分localtime（自动转换时区）和gmtime(转换为gmt/utc时间)
	time_t timeback = mktime(tm);
	std::cout << asctime(tm) << std::endl;
	std::cout << ctime(&difftime) << std::endl;
}
```

* 任意两种 duration 类型之间可以通过**构造函数**转换，只要：
  * 能够隐式地将 Rep1 转换为 Rep2（这里 int → double）
  * Period1 是 Period2 的整数倍或分数关系（nano → ratio<1>）
  * 需要注意的是double类型的cout输出和一般的输出一样，受到浮点数精度和宽度限制使用`cout<<fixed << setprecision(9) ;`可以输出ns的精度

```cpp
  cout<<fixed << setprecision(9) ;//浮点数精度到达ns
  auto begin = chrono::system_clock::now();
  auto n = match_str.find(search_str);
  auto end = chrono::system_clock::now();
  auto time = end - begin;
  auto dtime = chrono::duration<double>(time);
  cout<<"string::find() used "<<dtime.count()<<" s"<<endl;
```

## regex

* 在 其他regex中已经存在"workslog_classify_by_category\014.__WorkingTools__\2其他\regex\regex.md"中
* 需要注意的重点有三个
  * 如何分割字符串string:有两种方法，1 使用成员函数find(ch,pos)和substr(pos,len) 2 使用stringstream配合std::getline(istream&,string&,char);
  * 如何使用regex_match(str,m,re);匹配简单分割的独立字符串
  * 如何使用regex_search(str,m,re);重复匹配字符串：1. 使用regex_search(std::string::const_iterator begin,std::string::const_iterator end,m,re);手动迭代起始位置，每次复制为m.suffix().first;更新begin 2 使用regex_iterator(begin,end,re))构造 起始迭代器，空参数 regex_iterator()构造结束迭代器，解引用每次的迭代器就是std::match_results<std::string::const_iterator>/特化的std::smatch m; m.str()获取匹配的字符串 [REF](https://cppreference.cn/w/cpp/regex/regex_iterator)
* 容易忘记的类型和对象是std:::match_results的成员函数，empty(),size(),ready(), length(pos),position(pos),prefix()和suffix()返回std::sub_match（这是for(auto& : )迭代match_results的对象类型）表示匹配的前后没有匹配的部分
