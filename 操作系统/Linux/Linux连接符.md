# Linux下命令连接符

## & 命令连接符
& 的作用是使命令后台运行，这样可以同时执行多条命令。
`id&whoami`

## &$ 命令连接符
&&前面的语句执行成功，则执行&&后的语句

## | 命令连接符
| 将前面命令的输出作为后面命令的输入，前面和后面的命令都执行，但是仅显示后面命令的执行结果

## || 命令连接符
|| 类似if-else语句，若前面的命令执行成功，则后面的命令不会执行，如果前面的执行失败，则执行后面的命令

## ; 命令连接符
; 使多个命令顺序执行，前后命令都执行