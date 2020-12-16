---

layout: post

title: "Ubuntu16.04环境下的科学上网"

date: 2018-05-21

tag: 教程

---

# 前言
笔者目前在学习过程中在Linux环境下下载一些资源的时候,由于某些众所周知的原因，资源的下载速度很慢或者甚至是基本无法下载，而在网上搜到的很多文档，要不就是如何在Linux系统下搭建SS服务端，就算是客户端很多也是语焉不详。所以倒腾了一下之后将方法分享在这里

### 你需要准备的：
> 一个SS服务器，搭建不是本文讲述的要点，请查阅 [上一篇](https://aye10031.github.io/2018/05/none/) 

# 开始
首先还是下载SS，不过多阐述：
> 我的系统是Ubuntu的，因此使用一下指令：
> ```
> apt-get install python-pip
> pip install shadowsocks
> ```
> Debian内核的其他系统同理      
> 而对于红帽内核的(例如CentOS)，则使用如下指令：
>```
>yum install python-setuptools && easy_install pip
> pip install shadowsocks
>```

接下来需要创建并编辑配置文件：
```
sudo vim /etc/shadowsocks.json 
```

此文件的配置与你的服务端一致，具体配置如下：
>{      
    "server":"你的IP",     
    "server_port":443,      
    "local_address": "127.0.0.1",       
    "local_port":1080,      
    "password":"你的密码",        
    "timeout":300,      
    "method":"aes-256-cfb",     
    "fast_open": false      
}

# 转换HTTP代理
SS的代理模式默认是以Socks5模式进行的。因此在此需要转为HTTP模式的代理，首先安装Polipo：      
```
sudo apt-get install polipo
```
接下来修改配置文件：
```
sudo vim /etc/polipo/config
```
具体配置文件内容如下：
> #This file only needs to list configuration variables that deviate       
>    #from the default values. See /usr/share/doc/polipo/examples/config.sample
>    #and "polipo -v" for variables you can tweak and further information.      
> 
>    logSyslog = false      
>    logFile = "/var/log/polipo/polipo.log"     
>
>    socksParentProxy = "127.0.0.1:1080"        
>    socksProxyType = socks5        
>
>    chunkHighMark = 50331648       
>    objectHighMark = 16384     
>
>    serverMaxSlots = 64        
>    serverSlots = 16       
>    serverSlots1 = 32      
>
>    proxyAddress = "0.0.0.0"       
>    proxyPort = 8123

接下来需要重启一下polipo代理：
```
/etc/init.d/polipo restart
```

验证方法业很简单，在你的浏览器中输入`127.0.0.1:8123`，如果弹出了polipo的配置网页，那么就说明成功了。

# 开启代理
首先在`设置-网络设置-代理`中选择代理模式为手动，在http一栏中填上127.0.0.1，端口就是上面的8123，勾选将“代理应用到全局”(~~大概就是叫这个，我可能记错了~~)

接下来在终端中开启SS代理：
```
sudo sslocal -c /etc/shadowsocks.json
```
此时可以尝试一下浏览谷歌等界面，应该已经可以了。        
其他的一些比如说让代理服务后台运行啊，开机启动啊等等就不是本文讲述的了，可以自己去尝试一下。