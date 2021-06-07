# IIS 中间件漏洞

## IIS6.0 PUT漏洞

IIS6.0 server在web服务扩展中开启了WebDAV（Web-based Distributed Authoring and Versioning）。WebDAV是一种HTTP1.1的扩展协议。它扩展了HTTP 1.1，在GET、POST、HEAD等几个HTTP标准方法以外添加了一些新的方法，如PUT，使应用程序可对Web Server直接读写，并支持写文件锁定(Locking)及解锁(Unlock)，还可以支持文件的版本控制。可以像在操作本地文件夹一样操作服务器上的文件夹，该扩展也存在缺陷，利用PUT方法可直接向服务器上传恶意文件，控制服务器。

利用条件：
1. 开起来WebDAV
2. IIS配置了可以写入的权限

利用方法：
PUT / MOVE 的往服务器中传后门文件

## IIS 短文件名枚举漏洞
漏洞产生的原因是为了兼容MS-DOS程序，windows为文件名较长的文件生成了对应的windows 8.3的短文件名，攻击者可通过短文件名猜解对应的文件，访问下载敏感资源，绕过某些限制

dir /x一下可以看到短文件名长什么样

![](Pasted%20image%2020210607223514.png)

倒数第二列就是短文件名

短文件名是有一定规则的：

1.只有前六位字符直接显示，后续字符用~1指代。其中数字1还可以递增，如果存在多个文件名类似的文件（名称前6位必须相同，且后缀名前3位必须相同）；

2.后缀名最长只有3位，多余的被截断，超过3位的长文件会生成短文件名；

3.所有小写字母均转换成大写字母；

4.长文件名中含有多个“.”，以文件名最后一个“.”作为短文件名后缀；

5.长文件名前缀/文件夹名字符长度符合0-9和Aa-Zz范围且需要大于等于9位才会生成短文件名，如果包含空格或者其他部分特殊字符，不论长度均会生成短文件；

IIS8.0以下版本需要开启asp.net支持，IIS8.0及以上可不需要asp.net，转用OPTIONS与TRACE方法猜解

![](Pasted%20image%2020210607223716.png)

漏洞利用工具：
[lijiejie/IIS_shortname_Scanner: an IIS shortname Scanner](https://github.com/lijiejie/IIS_shortname_Scanner)

漏洞修复：
1. 关闭对NTFS对8.3文件名格式的支持
2. 禁用ASP.NET功能
3. 禁止在URL中使用 “~” 及其 Unicode 编码