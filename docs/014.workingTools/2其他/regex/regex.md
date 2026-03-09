# regex 语法或使用方法记录

## regex 语法或资源
* regexper.cn:<https://regexper.cn/>
* 开放组第九章正则表达式<https://pubs.opengroup.org/onlinepubs/9799919799/>

## c++ regex
* runoob:正则表达式教程<https://www.runoob.com/regexp/regexp-tutorial.html> 
* runoob:c++正则表达式库\<regex\><https://www.runoob.com/cplusplus/cpp-libs-regex.html> 对于unicode支持不好,语法也有限制
* c++boost.regex<https://boost.ac.cn/doc/libs/1_88_0/libs/regex/doc/html/index.html> 推荐说的性能有点问题？unicode 支持需要联合使用 icu 库
* c++更现代的库：1. PCRE2 2.RE2(google) 3.Hyperscan(Intel)

## python regex
* runoob: python3 re库  <https://www.runoob.com/python3/python3-reg-expressions.html> 
* python re库<https://docs.python.org/zh-cn/3/howto/regex.html>

## c++ 正则表达式库必知必会

**C++ <regex> 库的主要类和函数**

* std::regex：表示一个正则表达式对象。const char*(可以带size_t count或\0结尾的字符串)；string对象；两个迭代器范围；initializer_list<CharT>；重点是要`\`需要转意,或者使用`R"(原始字符串)"`。
* match_results 标识一个正则表达式匹配，包括所有子表达式匹配.两个特化：cmatch 和 smatch;分别是对const char*和std::string::const_iterator的特化.如果需要特化自己的类型指定指针类型，如 match_results<vector<char>::const_iterator>可以用来接收vector<char> 的匹配结果. 
* std::regex_match（<字符串>,match_results,regex,regex_constants::match_flag_type=match_default）
）：检查整个字符串是否与正则表达式匹配。三类字符串：const char*、string、两个迭代器范围。
* std::regex_search：在字符串中搜索与正则表达式匹配的部分。**每次只匹配前面一部分而不是全部结果，需要自己循环，但是迭代器是const_iterator不能修改，很难用，不如自己分割字符串然后用regex_match**
* std::regex_replace：替换字符串中与正则表达式匹配的部分。分为两类fmt替换字符串为const char*和string类。字符串也是三种，但是迭代器范围多一个参数oupputiterator作为第一个参数。
* std::sregex_iterator：迭代器，用于遍历所有匹配项。只适合regex不变的情况。**regex_search和sregex_iterator都有个问题无法处理前缀无效的情况如：";;;;;;abc;aaa;ewr;"匹配的时候需要“^[a-z]{3};”那么会跳过第一个匹配项导致失败，如果不加^这个会导致“adfadc;aaa;ewr;”这种第一个错误匹配adc。总之就是自己分割字符串并使用regex_match**
* std::sregex_token_iterator：用于分割字符串。**分割字符串的一个方便方式（默认是find+substr）**
  * 关键参数：（就是regex中的子表达式下标）
    * submatch = -1：返回不匹配的部分（分隔符之间的内容）**用于复杂分割字符串（多种且无规律）**
    * submatch = 0：返回匹配的部分（相当于 sregex_iterator）
    * submatch = n：返回第 n 个捕获组的内容

##  正则表达式记录

* [正则表达式的先行断言(lookahead)和后行断言(lookbehind)](https://www.runoob.com/w3cnote/reg-lookahead-lookbehind.html)
