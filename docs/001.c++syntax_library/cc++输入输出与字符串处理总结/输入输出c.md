# c 输入输出 分标准库和glic POSIX平台
* 参考：<http://www.cnitblog.com/guopingleee/archive/2007/08/13/31701.html>

# iso c标准
## 文件
* fopen 1.(c_str*filename,c_str*mode) mode：rwa+,+表示可读写，b二进制 wx w+x文件存在函数失败。文件存在：r从头读，w销毁内容，a从结尾写; 文件不存在；r失败，wa创建文件。filename 的格式是实现定义的，而且不需要表示一个文件（譬如可以是控制台或另一能通过文件系统 API 访问的设备）。在支持的平台上， filename 可以包含绝对或相对路径。
* fclose 1.(FILE*) 关闭给定的文件流。冲入任何未写入的缓冲数据到 OS 。舍弃任何未读取的缓冲数据。
* fflush 1.(FILE*) 对于输出流（及最后操作为输出的更新流），从 stream 的缓冲区写入未写的数据到关联的输出设备。对于输入流（及最后操作为输入的更新流），行为未定义。
* void setbuf(FILE*,char*buff) 为文件流设置缓冲区 若提供空指针，则关闭缓冲。设置用于流操作的内部缓冲区。其长度至少应该为 BUFSIZ 个字符。
* int setvbuf(FILE*,char*buff,mode,size) 以 mode 所指示值更改给定文件流 stream 的缓冲模式。mode:_IOFBF _IOLBF _IONBF全缓冲 行缓冲 无缓冲

## 面向字节输入输出
* fread(*buff,size,count,FILE*stream) fread 返回成功读取的对象数，若出现错误或文件尾条件，则可能小于 count 。不区别文件尾和错误，而调用者必须用 feof 和 ferror 鉴别出现者为何。
* fwrite(*buff,size,count,FILE*stream) 返回成功写入的对象数，若错误发生则可能小于 count。

## 无格式化输入输出
* fgetc/getc (FILE*) fputc/putc (*ch,FILE*)
* char* fgets (*str,count,FILE*) int fputs(*str,FILE*) fgets读取换行符且添加额外的\0 fpus什么都不加 **都不能处理字符串中包含\0的数据只能处理空结尾不含\0的数据**
* ungetc (ch,FILE*)
* 标准io:getchar putchar gets(移除) puts puts写字符串到stdout丢弃\0,附加\n
* 标准没有getline/getdelim函数拓展(自定义或拓展) int getline(**str,*count,[delim],FILE*) 返回读取的字符数,如果*str为空或不够容纳，free(*str)后malloc足够大的内存。读取的字符（包括任何分隔符）应存储在对象中，并在遇到分隔符或文件结尾时添加终止NULL。

## 格式化输入输出
* [sf]scanf [sf]printf ...v前缀可变参数版本 snprintf指示长度

## 文件定位
* ftell(FILE*) fseek(FILE*,offset,origin) rewind(FILE*)
* fgetpos(FILE*,*pos) fsetpos(FILE*,*pos) 成功时为 ​0​ ，否则为非零值。

## 错误处理
* int feof int ferror void clearerr 参数都是(FILE*)
* perror(ch*s) 输出当前错误字符串到stderr,后随errno

## 文件操作
* int remove(*filename)
* int rename(*old,*new)
* FILE* tmpfile()  创建并打开一个临时文件。该文件作为二进制文件、更新模式（如同为 fopen 以 "wb+" 模式）打开。该文件的文件名保证在文件系统中唯一。至少可以在程序的生存期内能打开 TMP_MAX 个文件（此极限可能与 tmpnam 共享，并可能为 FOPEN_MAX 所进一步限制）。
* char* tmpnam(*filename) 创建独有的合法文件名（长度不长于 L_tmpnam）并将它存储于 filename 所指向的字符串。此函数足以生成至多 TMP_MAX 个独有文件名，但它们中的一部分甚至全部可能正在文件系统中使用，从而不适合作为返回值。 若filename不是空指针则为filename。否则为指向内部静态缓冲区的指针。若不能生成适合的文件名，则返回空指针。

## 宏
* EOF FOPEN_MAX FILENAME_MAX BUFSIZ _IOFBF _IOLBF _IONBF SEEK_SET SEEK_CUR SEEK_END TMP_MAX L_tmpname


# glic
* 去看glic 11、12、13(二进制) 