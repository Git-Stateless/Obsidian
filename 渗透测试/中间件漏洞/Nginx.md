# Nginx 中间件漏洞
Nginx （engine x）是一个高性能的HTTP 和反向代理Web 服务器，同时也提供了IMAP/POP3/SMTP 服务。
中国大陆使用Nginx 网站用户有：百度、京东、新浪、网易、腾讯等。
Nginx 可以在大多数Unix、Linux 编译运行，也有Windows 移植版。

## CRLF

CRLF：就是CR 和LF，分别表示回车和换行， CR 命令让打印头回到左边。LF 命令
让纸前进一行。

在HTTP 报文中，行与行之间使用CRLF 间隔。

攻击者一旦向请求行或首部中的字段注入恶意的CRLF，就能注入一些首部字段或报文主
体，并在响应中输出，所以又称为HTTP 响应拆分漏洞（HTTP Response Splitting）。

在Nginx 中配置文件中，有三个可以接受URL 的变量：
1.$URI
2.$DOCUMENT_URI
3.$REQUEST_URI

其中：
1.$URI- 获取解码后的请求路径
2.$DOCUMENT_URI- 获取解码后的请求路径
3.$REQUEST_URI- 没有解码的完整的URL

![](Pasted%20image%2020210607222526.png)

![](Pasted%20image%2020210607222542.png)

## HTTP.sys 漏洞

Http.sys是Microsoft Windows处理HTTP请求的内核驱动程序。HTTP.sys会错误解析某些特殊构造的HTTP请求，导致远程代码执行漏洞。成功利用此漏洞后，攻击者可在System帐户上下文中执行任意代码。由于此漏洞存在于内核驱动程序中，攻击者也可以远程导致操作系统蓝屏。

影响版本：
* IIS7.0及以上
* Windows7 SP1
* Windows8
* Windows8.1
* WindowsServer 2008 R2 SP1
* WindowsServer 2012
* WindowsServer 2012 R2
* WindowsServer 2012 R2 SP1

漏洞验证：

1. 利用curl工具

``curl -v IP -H "Host:irrelevant" -H "Range: bytes=0-18446744073709551615"``

如果服务器返回 HTTP/1.1 416 Requested Range Not Satisfiable 则说明漏洞存在；
如果服务器返回HTTP Error 400. The request has an invalid header name. 则已打补丁

2. 利用 Metasploit检测

``use auxiliary/scanner/http/ms15_034_http_sys_memory_dump``

漏洞修复：
1. 官方补丁：https://support.microsoft.com/zh-cn/kb/3042553
2. 临时解决办法，禁用IIS内核缓存。但可能会造成IIS性能下降
