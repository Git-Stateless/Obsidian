# Tomcat 中间件漏洞

Tomcat 是Apache 软件基金会（Apache Software Foundation）的Jakarta
项目中的一个核心项目，由Apache、Sun 和其他一些公司及个人共同开发而成。

Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用服
务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用。

实际上Tomcat 是Apache 服务器的扩展，但运行时它是独立运行的，所以当你
运行Tomcat 时，它实际上作为一个与Apache 独立的进程单独运行的。

## CVE-2017-12615
影响版本： Apache Tomcat 7.0.0 to 7.0.79

漏洞说明：当Tomcat 运行在Windows 主机上，且启用了HTTP PUT 请求方法（例
如，将readonly 初始化参数由默认值设置为false），攻击者将有可能可通过精心构
造的攻击请求向服务器上传包含任意代码的JSP 文件。之后，JSP 文件中的代码将能被
服务器执行。

![](Pasted%20image%2020210607222812.png)

## 弱口令
