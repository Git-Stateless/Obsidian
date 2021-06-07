# Apache 中间件漏洞

## 配置文件错误
多后缀解析:
AddHandler application/x-httpd-php .php	

AddHandler 为相应的文件扩展名指定处理程序

将扩展名为.php 的文件交给x-httpd-php 程序处理

Apache 识别文件扩展名是从前往后的，如果遇到了无法识别的扩展名会接着往后识别，遇到的第一个可以识别的扩展名作为该文件的扩展名

Apache 从前往后获取第一个可以识别的扩展名，Web 应用从后往前获取第一个扩展名，将文件的最后一个扩展名设置为Web 应用程序允许上传的扩展名，将文件的第一个扩展名设置为Apache 可以识别的脚本类型

## CVE-2017-15715 换行解析漏洞

![](Pasted%20image%2020210607222226.png)

## Apache解析漏洞

1）一个文件名为test.x1.x2.x3的文件，apache会从x3的位置开始尝试解析，如果x3不属于apache能够解析的扩展名，那么apache会尝试去解析x2，直到能够解析到能够解析的为止，否则就会报错。
2）CVE-2017-15715，这个漏洞利用方式就是上传一个文件名最后带有换行符(只能是\x0A，如上传a.php，然后在burp中修改文件名为a.php\x0A)，以此来绕过一些黑名单过滤。

另外有一点需要注意，很多人会使用集成环境来部署Web服务器。集成环境部署简易、使用方便，因此应用广泛。但需要注意的是，集成环境的软件版本并不是中间件的版本。因此从安全角度考虑，需要严格检查集成环境中的Apache及PHP版本，避免出现解析漏洞或截断漏洞等。下面是一些存在解析漏洞的集成环境版本及中间件对应版本信息，用以核对当前服务器安全状态或进行漏洞测试：

● WampServer2.0 All Version (WampServer2.0i / Apache 2.2.11)
● WampServer2.1 All Version (WampServer2.1e-x32 / Apache 2.2.17)
● Wamp5 All Version (Wamp5_1.7.4 / Apache 2.2.6)
● AppServ 2.4 All Version (AppServ - 2.4.9 / Apache 2.0.59)
● AppServ 2.5 All Version (AppServ - 2.5.10 / Apache 2.2.8)
● AppServ 2.6 All Version (AppServ - 2.6.0 / Apache 2.2.8)

修复建议：
修改 Apache 的配置文件，防止文件名中存在“.php”的文件被执行。
```
<FailesMatch ".(php\.|php3\.|php5\.|php7\.">
	Order Deny,Allow
	Deny from all
```
