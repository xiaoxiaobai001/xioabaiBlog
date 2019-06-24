# puTTY #
PuTTY是一个Telnet、SSH、rlogin、纯TCP以及串行接口连接软件。
Putty远程连接服务器教程
　　ssh@username:hostip　　ssh访问局域网内的主机

- 上传/下载文件

1. 打开psftp,
输入命令： open hostname  eg:192.168.xxx.xxx
会连接目标地址，连接成功后即可进行文件上传或其他操作了。
连接成功后，会看到当前所在远程目录。

2. 开始上传文件，输入以下命令：
put E:/hello.txt 
回车后，会把本地路径 E:/hello.txt 文件上传到服务器的/home/XXX 目录下。
如果需要上传文件夹，在可以在put命令后增加参数 -r ,意思为循环递归。

2. 开始下载文件，输入以下命令：
get ./xiaobai/hello.txt D:/xxx
回车后，会把服务器上./xiaobai/hello.txt 文件下载到本机的安装psftp目录下。
如果需要下载文件夹，在可以在put命令后增加参数 -r ,意思为循环递归。

![](https://i.imgur.com/5KVpToW.png)


