# Bypass-Campus-Network-Authentication-With-IPv6

## 服务端

### 第一步

实验室电脑具备IPv6地址。比如设置里查看

![image](https://github.com/LihengChen9/Bypass-Campus-Network-Authentication-With-IPv6/blob/main/1.jpg)

也可以在终端通过ifconfig命令查看IPv6地址。

### 第二步

在实验室电脑上的ubuntu(18.04及以下，20.04我没找到ssserver apt源)系统上安装ssserver（推荐ubuntu系统安装ssserver，似乎支持udp转发，不然你可能看不了腾讯视频等等）。安装如下

```
sudo apt install shadowsocks
```

直接在apt源里面就能找到这个，其中包含了ss的server端和client端，这里只用它的server端（注：如果想要科学上网，推荐使用shadowsocks-qt5，可以去github上下载。不过我现在只用v2ray科学上网了，好用的客户端比如Qv2ray）。

### 第三步

在终端输入

```
sudo ssserver -s :: -k yourpassword -d start
```

其中-s代表服务端IP地址，::代表本地的IPv6地址。-k代表服务端密钥，yourpassword输入你自定义的密钥。-d代表守护进程模式（后台模式），start代表启动。默认端口是8388。默认加密方式是aes-256-cfb。

我个人写成了一个shell脚本，比如my_ssserver.sh。里面的内容如下

```
echo "0" | sudo -S ssserver -s :: -k yourpassword -d start
```

其中0代表sudo密码，-S代表直接输入sudo密码。

然后我把这个shell脚本加入了开启启动。可以在终端输入gnome-session-properties，添加开机启动项。

![image](https://github.com/LihengChen9/Bypass-Campus-Network-Authentication-With-IPv6/blob/main/2.jpg)

## 客户端

输入你服务端的IPv6地址、ssserver密码、加密方式、端口。连上校园Wifi，不要登录校园网账号，直接使用SS、SSR、v2rayNG（Github上有）进行SS连接。

![image](https://github.com/LihengChen9/Bypass-Campus-Network-Authentication-With-IPv6/blob/main/3.jpg)

## 提醒

PC端系统（Windows、ubuntu），大部分软件不走系统代理，需要你去软件独立的代理功能中设置。如果软件不走系统代理，并且不支持软件内设置代理功能，那么你可以尝试使用Proxifier工具。

## 路由器连校园网网口，怎么让舍友各自登录自己的校园网账号？

路由器关闭DHCP，网线连接Wan口。重启。（细节有点忘了）
