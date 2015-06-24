title: Rserve杂记
date: 2015-06-20 14:32:43
categories: R经验总结
tags: [Rserve,R,Java,rJava]
description: Rserve的一些经验及教训总结：启动方式，配置，多连接运行空间机制，编码问题等。
---
# Rserve启动
Rserve可以在terminal中使用命令行方式启动，也可以在R控制台中启动。
在R控制台中启动时，可以选择在当前R进程中启动Rserve，这个方式在调试程序时会提供一些便利。
   * 在terminal中启动
   ``` bash
   R CMD Rserve
   ```
   * 在R中启动
   单独启动一个守护进程作为Rserve实例：
   ```{R}
   library(Rserve)
   Rserve()
   ```
   在当前的进程中启动Rserve实例：
   ```{R}
   library(Rserve)
   run.Rserve()
   ```
# Rserve进程查看
   查看Rserve进程的linux命令:
   ``` bash
   ~ ps -aux | grep Rserve
   ```
   查看ip及端口:
   ``` bash
   ~ netstat -nltp | grep Rserve 
   ```
# Rserve配置
在文件/etc/Rserve.conf中，可配置以下选项，更多配置选项参考[官方文档](http://www.rforge.net/Rserve/doc.html#conf)：
* workdir: R工作目录, 每个链接会在此目录下建立子目录作为工作目录
* port: 通信端口
* remote: 是否允许远程访问
* auth: 是否开启认证登录,开启为required
* plaintext: 是否允许明文密码,开启为enable
* passwords file: 密码文件

# 独立运行空间
如果有两个客户端与Rserve服务器建立接口，那么Rserve会分别为每个客户端单独建立运行空间，运行空间之间相互隔离。
例如Rserve设置的运行目录为workdir="/analysis"， 那么，打开一个新的Rserve连接时，会自动产生一个子目录如"conn1234"，该连接的工作目录为"/analysis/conn1234"。

# 运行空间建立
接着说每个连接的工作目录，工作目录并不会立即建立，只是有一个虚拟的路径，在程序执行中如果有在工作目录中建立文件，那么才会真正建立这个目录。当连接关闭时，Rserve会检查这个工作目录中是否为空，如为空则删除该目录，否则保留该目录。

# 允许远程连接
在R控制台启动时，输入命令：
``` {R}
Rserve(args="--RS-enable-remote")
```
在terminal里启动，输入命令：
``` bash
R CMD Rserve --RS-enable-remote
```
# 需要注意编码问题
不同用户启动的Rserve的编码可能不同。与Java交互时，需要与tomcat编码一致，可以使用tomcat用户启动Rserve。

