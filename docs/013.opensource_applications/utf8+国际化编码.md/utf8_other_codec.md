# utf8的支持

很多的库都支持utf8编码，比如：qt,boost,icu但是都是很重量级的库，所以这里只介绍一个轻量级的utf8库：utf8cpp

* (来自utf8pp作者)选择：对于替代方案和比较，我推荐以下文章：[C 和 C++ 编码 API 的奇妙可怕的世界](https://thephd.dev/the-c-c++-rust-string-text-encoding-api-landscape)（带有一些 Rust）， 作者：JeanHeyd Meneide。在本文中，将此库与以下内容进行了比较：
  * [simdutf](https://github.com/simdutf/simdutf)
  * [iconv](https://www.gnu.org/software/libiconv/)
  * [boost.text](https://github.com/tzlaine/text)
  * [ICU](https://unicode-org.github.io/icu/userguide/conversion/converters.html)
  * [encoding_rs](https://github.com/hsivonen/encoding_rs)
  * [Windows API functions for converting text between encodings](https://learn.microsoft.com/en-us/windows/win32/api/stringapiset/)
  * [ztd.text](https://github.com/soasis/text/)

* [utf8cpp](https://github.com/nemtrif/utfcpp)
  * 很多都是转换函数和一些简单的处理函数都在utf8::域名下[REF](https://github.com/nemtrif/utfcpp?tab=readme-ov-file#table-of-contents)
    * append
    * next
    * peek_next
    * prior
    * advance
    * distance
    * utf16to[u]8/utf8to16,utf32to8/utf8to32
    * find_invalid,is_valid,replace_invalid
    * starts_with_bom

## 关于如何在c++11中和之后中定义utf8字面量字符串和字符

### 所有字符类型

* 四种字符字面量类型：普通，L(宽字符),u(utf16),U(utf32);对于字符串*2，跟一个R表示原始字符串

### UNICODE支持

* c++11中支持定义UNICODE-8 UNICODE-16 UNICODE-32的**字符串**字面量 ;实际就是长度为32、16、8的char数组 

* 唯独utf8 **字符** 字面量c++17中才支持（但是要说明的是utf8字符字面量，utf16字符字面量无法表示超出长度的字符，）