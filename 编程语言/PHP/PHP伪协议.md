# PHP 伪协议

*PHP带有很多内置URL风格的封装协议，可用于fopen、copy、file_exsits 和 filesize等文件系统函数。除了这些内置封装协议，还能通过stream_wrapper_register注册自定义的封装协议。这些协议都被称为伪协议*

常见伪协议：
* file://	访问本地文件系统
* http://	访问http(s)网站
* ftp://	访问ftp(s)网站
* php://	访问各个输入输出流
* zlib://	处理压缩流
* data://	读取数据流
* glob://	查找匹配的文件路径模式
* phar://	PHP文档
* ssh2://	Secure Shell2
* rar://	RAR数据压缩
* ogg://	处理音频流
* expect://		处理交互的流
