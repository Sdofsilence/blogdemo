# 输入对于\n和\0的处理区别 
* c标准io:~~gets不能用~~ puts会添加换行

##### 以下四个函数都以\n为默认结束读取delim分隔符
* c:   fgets getline 两者不同之处在于getline返回一个int值 fgets返回指针而无法处理字符串包含\0，而以\n分隔的字符串，fgets fputs只适合处理字符串不含\0,且以\0分隔的字符串，若需要处理以\n分隔字符串
* c++: get getline
* c函数会保留\n到字符串中，c++get不提取\n，getline提取\n后舍弃。
* 所有的字符串读取函数c和c++的都会自动在字符串尾部添加\0

| 函数     |       默认结束分隔符   | 换行符处理       |
|  :---     | :---      |  :---     |
|  C 的 fgets                    |  换行符\n,count-1,EOF     |  保留换行符     |
|  C 的 getline                 |  换行符\n,EOF              |  保留换行符     |
|  C++ 的 istream::get          |  用户指定，默认\n           |  不提取换行符，换行符在流中     |
|  C++ 的 istream::getline     |  用户指定，默认\n            |  提取并丢弃换行符，不存储在字符串中     |