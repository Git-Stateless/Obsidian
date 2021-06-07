# PHP命令执行函数和运算符
## system 函数
system 函数用于执行外部程序，并显示输出。其用法如下：
`string system(string $command[,int &return_var])`

例如index.php文件内容为`<?php system('whoami');?>`。执行后输出whoami的结果：
```
# php index.php
# root
```

## exec 函数
exec 函数用于执行外部程序，其用法如下：
`string exec(string $command[,array &output[,int $return_var]])`

例如index.php文件内容为`<?php exec('whoami');?>`。执行后不会输出结果，需要接受echo函数，即`<?php echo exec('whoami');?>`
```
# php index.php
# root
```

## shell_exec 函数
shell_exec 函数通过shell环境执行命令，并且将完整的输出以字符串的方式返回。其用法如下：
`string shell_exec(string $cmd)`

例如index.php文件内容为`<?php shell_exec('whoami');?>`。执行后不会输出结果，需要接受echo函数，即`<?php echo shell_exec('whoami');?>`
```
# php index.php
# root
```

## passthru 函数
passthru 函数函数用于执行外部程序，并显示原始输出。其用法如下：
`void passthru(string $command[,int &return_var])`

例如index.php文件内容为`<?php passthru('whoami');?>`。执行后默认输出whoami的结果：root
```
# php index.php
# root
```

## popen 函数
popen 函数 用于打开进程文件指针。其用法如下：
`resource popen(string $command,string $mode)`

例如index.php文件内容为`<?php popen("touch test.txt","r");?>`。执行该命令会在当前文件夹创建test.txt 文件。
```
# php index.php
# ls
# index.php test.txt
```

## proc_open 函数
proc_open 函数用于执行一个函数，并且打开用来输入输出的文件指针。其用法如下：
``resource proc_popen(string $command,array $descriptorspec,array $pipes [,string $cwd [,array $env[,array $other_options]]])``

例如index.php 内容为：
```
<?php
$proc=proc_popen("whoami",
	array(
		array("pipe","r"),
		array("pipe","w"),
		array("pipe","w")
	),
	$pipes);
	print stream_get_contents ($pipes[1]);
?>
```

执行后输入：
```
# php index.php
# root
```

## 反单引号
反单引号(\`)是PHP的执行运算符，PHP将尝试将反引号中的内容作为shell命令来执行，并将其输出信息返回：
例如index.php文件内容为``<?php echo `whoami`;?>``。执行后输出：
```
# php index.php
# root
```
