# c++中使用utf-8
* c++字符串字面量:
  * "" 使用文件的编码
  * L"" 宽字节编码(大小和编码依赖于实现windows:UTF16;linux:UTF32)
  * u8"" UTF-8
  * u""  UTF-16
  * U""  UTF-32
  * R前缀 可以和上面的五种一起使用跟在后面表示非转义的字符串

## big ending / little endian(字节序)理解的补充

* 整体看作 from big/little to end 格列佛游记中的从哪端开始，big endian就是从大的那一端开始，little就是从小的那一端开始。
* 这里的 big/little 指的是权重的大小，就如同数字从左往右左边大右边小。
* 而写入顺序默认是从低地址到高地址。
* 简单理解就是是从权重大的一头开始还是从权重小的一头开始按从低到高的地址写入。

## 所谓的编码支持
* 声明时候的字节如何存储，`string str = u8"你好"`按照utf-8编码是六个字节`\xe4\xbd\xa0\xe5\xa5\xbd`(大端),按unicode16编码`string str = u"你好"`是`\x4f\x60\x59\x7d`(大端)，占用四个字节。
* 字符串处理函数的支持，如编码转换和遍历。
* 推荐的方式，
  * 可以使用默认的c++自己的string/u8string(c++20)存储，没有问题，只是不支持处理。
  * 头文件<codecvt> wstring_convert (c++17被弃用但是仍然广泛使用)转换为utf16(也是一个变长的编码(2基本平面/4辅助平面一个字符))/utf32(定长编码)但是也不好用
  * 使用 utf8cpp/boost.locale/
  * 使用 icu4c<https://unicode-org.github.io/icu/userguide/icu4c/>
