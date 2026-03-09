# c字符串处理

### 字符分类和大小写转换 cctype/ctype.h
* isalnum isalpha isdigit isspace isblank... tolower toupper

### 数值转换 cstdlib/stdlib.h
* atoi atof atol atoll 
* strtol strtoll strtoul strtoull
* strtof strtod strtold

### 字符串 cstring/string.h
* strcpy strcmp strcat strlen str[r]chr strstr 
* strspn size_t strspn( const char* dest, const char* src );返回dest由src中找到的字符组成的最大起始段的长度。
* strcspn 返回dest由src中找不到的字符组成的最大起始段的长度
* strpbrk [const] char* strpbrk([const] char* dest, const char* breakset );指向dest中首个在breakset中的字符的指针，不存在为空指针。如果第一个参入为const则返回const char*指针。
* strtok char* strtok( char* str, const char* delim );如果找到了这种字符，那么会用空字符覆盖它，并结束当前记号。序列中的下次调用的搜索目标从下一字符开始。返回值：指向下个记号起始的指针，或若无更多记号则为空指针。

### 字符数组cstring/string.h
* memset memcpy memcmp memchr memmove

### 其他
* char* strerror(int) 返回给定错误码的文本。
* errno_t strerror_s(char*,rsize_t,errno_t)
* size_t strerrorlen_s(errrno_t) 计算当以 errnum 调用 strerror_s 则本会写入的，本地环境限定错误消息的不截断长度。长度不包含空终止符。辅助上一函数计算长度。