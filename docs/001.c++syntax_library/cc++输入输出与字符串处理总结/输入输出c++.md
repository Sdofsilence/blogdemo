# c++输入输出
### 继承关系 
* ios_base管理格式化和错误
* basic_ios<charT,char_traits<charT>> 继承自ios_base 管理流缓冲(状态、异常、本地化、格式信息、缓冲区和流、字符)
* basic_istream basic_ostream 继承自 basic_ios 组合basic_steambuf basic_iosteam继承自前两者
* basic_ifstream basic_ofstream basic_istringsteam basic_ostringsteam basic_fsteam basic_stringsteam 分别继承自以上输出 输出 输出输出 流。分别组合了继承自basic_streambuf的 basic_filebu 和 basic_stringbuf成员。
* c++23数组输入/输出<spansteam>

### 头文件及typederf/using别名
* 头文件：ios(ios_base basic_ios) streambuf istream(istream iostream) ostream (全部被包含在iostream 中且定义了全局的 8个标准流对象cin cout cerr clog及w开头的宽字节版本) cerr和clog区别是cerr是无缓冲的
* 头文件：fstream sstream spanstream

### 格式化控制
* ios头文件中：控制bool 基数前缀和进制 对齐 浮点格式 大写模式
* istream头文件：ws 消耗空白符
* ostream头文件：ends endl flush
* iomanip头文件：setiosflags resetiosflags setbase setfill setprecision setw get/put_money/time quoted

### 辅助类型
* streamoff streamsize fpos{streampos...更多参考cppreference} * 前两个定义为有符号整数(msvc:longlong),类模板 std::fpos 的特化标识流或文件中的绝对位置，在iosream的成员函数tellg,seekg,tellp,seekp时使用
* fpos的特化：streampos wstreampos (char,wchar_t) (u8,u16,u32)(char8_t,char16_t,char32_t)所有的都是同一类型可以从streamoff隐式转换，也可以显示转换为streamoff。

# 具体使用iostream
## istream
### 格式化输入(面向类型)
* operator>>
### 无格式输入(面向字符或字节)
* get 1.() 2.(&ch) 3.(*s,count) 4.(*s,count,ch_delim) 5.(&streambuf) 6.(&streambuf,delim) 12读取字符 3456读取字符串一行\n或delim结尾，不会提取此字符（与 getline() 不同）。
* peek 1.() 返回下个字符或eof。
* unget 1.() 设置 gcount() 计数器为零。
* putback 1.(ch) 设置 gcount() 计数器为零。
* getline (*s,count) (*s,count,ch_delim) 提取该分隔符（与 basic_istream::get() 不同）并计入 gcount()，但不存储它。
* ignore 1.(ssize_t count =1,delim= eof); 从输入流提取并舍弃字符，直至并包含 delim。
* read readsome 1.(*s,ssize_t count) 前者返回*this,后者返回ssize_t。readsome的行为是高度实现限定的。
* gcount 1.() 返回最近的无格式输入操作所提取的字符数，或若该数不可表示则返回 std::streamsize 的最大可表示值。

#### 寻位
* tellg 1.() 返回size_t 失败size_t(-1) 
* seekg 1.(size_t pos) 2.(ssize_t off,ios_base::seekdir[beg,cur,end]); 设置当前关联 streambuf 对象的输入位置指示器。在进行其他任何操作前，seekg 都会清除 eofbit。不影响 gcount()。

#### 同步
* sync 1.() 成功时返回 ​0​，失败时或若流不支持此操作（无缓冲）时返回 -1。将输入缓冲区与关联数据源同步。对于 std::filebuf （即文件流缓冲区），它应当尝试清空输入缓冲区，并丢弃已读取但未处理的数据。对于非文件流（例如 std::istringstream 或 std::cin），sync 的行为是不确定的。

## ostream
### 格式化输出(面向类型)
* operator<<
### 无格式输出(面向字符或字节)
* put 1.(ch) 若输出因任何原因失败，则设置 badbit。不同于有格式输出函数，若输出失败，则此函数不设置 failbit。
* write 1.(*s,ssize_t count) 恰好插入了count个字符/插入到输出序列失败该情况下调用 setstate(badbit)。
#### 寻位
* tellp 返回size_t 失败size_t(-1) 
* seekp 1.(size_t pos) 2.(ssize_t off,ios_base::seekdir[beg,cur,end]); 失败设置ios_base::failbit
#### 同步
* flush 向底层输出序列写入尚未提交的更改。失败设置badbit。

## ios_base成员常量
* openmode app,binary,bianry,in,out,trunc,ate
* fmtflags 基，对齐，浮点，show[base,point,pos],skipws,unitbuf,uppercase
* iostate goodbit badbit failbit eofbit
* seekdir beg cur end
