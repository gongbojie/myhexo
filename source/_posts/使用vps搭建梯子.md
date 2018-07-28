---
title: 使用vps搭建梯子
date: 2018-01-25 10:28:41
updated: 2018-01-31 17:48:41
categories: 
tags:
---

由于工作学习生活上使用YouTube，Google较多，在前两天使用的某上网利器到期后，发现套餐贵且流量少遂而萌生自建梯子的打算。话不多说，直接进入步骤。
<!-- more -->
## 介绍
### <i class="fas fa-globe"></i> VPS与Vultr
VPS(Virtual private server) 译作虚拟专用伺服器。你可以把它简单地理解为一台在远端的强劲电脑。当你租用了它以后，可以给它安装操作系统、软件，并通过一些工具连接和远程操控它。

[Vultr](https://www.vultr.com/)是一家 VPS 服务器提供商，有美国、亚洲、欧洲等多地的 VPS。它家的服务器以性价比高闻名，按时间计费，最低的资费为每月 $5。相当于国内的阿里云和腾讯云。
### SSH
SSH 是一种网络协议，作为每一台 Linux 电脑的标准配置，用于计算机之间的加密登录。当你为租用的 VPS 安装 Linux 系统后，只要借助一些工具，就可以用 SSH 在你自己的 Mac/PC 电脑上远程登录该 VPS 了。
## 购买和连接 VPS
### 注册 Vultr
可以使用我的[推荐链接](https://www.vultr.com/?ref=7316038)进行注册，之前双方都有活动。
### 部署 VPS
注册完成后，在左边选择“Servers”标签，点击右边的加号部署新的服务器。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnso0zksdqj20x108nmyn.jpg)

接下来选择一个你想要的节点。个人认为近一点的地区比较快点，试了东京和新加坡的，感觉东京的明显更快，但第一次创建东京分配的IP就被墙了。

（PS:我在选择的时候出现了一点小状况，就是创建节点的IP被墙，也就是无法连接网络了，这里创建好节点后建议去[ping.me](http://ping.pe)测试一下，根据地点查看如果国外节点全部能够访问（绿色），国内节点无法访问（红色）则被墙无疑，可以再次创建节点，重新分配IP试试）
	
![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnswtq6ecnj20ox0i3q5i.jpg)

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnso91ccy0j20tz0bxtap.jpg)

为 VPS 选择安装系统。这里选 CentOS 6x64，因为相较其他 Linux 系统更适配「锐速」，一个提高连接 VPS 速度的软件。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnsoa5qty7j20tn0g4dhr.jpg)

根据需求选择套餐。只供个人科学上网用的话，$5 绰绰有余。2.5刀的售罄只能选这个，足够用。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnsoc75iwyj20tx0fnjui.jpg)

剩下都可以使用默认值。最后一项你可以自定义服务器的名字和标签，以方便区分。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnsodr1owuj20tx0iemz2.jpg)

另外，上图第 6 步中，SSH Keys 的作用是，可以让你登录 VPS 时不用每次手动输密码。若只将其用作 Shadowsocks 服务器，仅需要在配置时登录一次，可以完全忽略它。

	PS:生成SSH KEY
	1. 在终端输入ssh-keygen -t rsa 
	2. 接着就会提示你想要保存的目录可以直接复制提示默认目录一般都在 ~/.ssh 这个文件夹里面。
	3. 为你的key添加密码，直接回车可以不输入密码。 
	4. 然后就在上面的这个目录中找到你的公钥~/.ssh/id_rsa.pub复制好粘贴你的ssh key中就好了。
	

点击就可以创建虚拟主机了。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnsotxeo32j20tz02qwes.jpg)

等待系统安装完成。当显示“Running”时，就表示部署已完成。之后你可以随时停用、重启或销毁它。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnsov1wuorj20u203qq36.jpg)

### <i class="fas fa-terminal"></i> 连接 VPS
Mac 只要打开「终端」应用即可开始使用 SSH 连接 VPS。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnsp3w4867j20ks0cuglw.jpg)

首先，在 Vultr 上你刚部署的服务器右边点“Manage”，找到服务器的 IP 地址和密码。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnsp7iu792j20ua0b475u.jpg)

在终端中，输入下面的命令并回车。注意把<host>替换成你服务器的IP地址。

```
ssh root@<host>
```
接下来屏幕上会出现一连串提示，意为无法确认 host 主机的真实性，只知道它的公钥指纹，问你还想继续连接吗？直接输入 yes 并回车。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnspd7e4ujj20kb021t8x.jpg)

然后输入密码，也就是把服务器状态页中密码那块复制一下，再 ⌘ + v 贴过来。这里不管是输入还是粘贴，屏幕上都不会显示字符，所以贴完后也是看不到字符的，回车就行。

当出现上图那串 [root@vultr ~]# 时，说明已成功登录。

## 部署 Shadowsocks <i class="far fa-paper-plane"></i>
### 部署 Shadowsocks 服务器端 <i class="fas fa-code"></i>
以下是3条命令，每次输入一行、回车，等待屏幕上的操作完成后再输入下一条。

```
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
chmod +x shadowsocks.sh
./shadowsocks.sh 2>&1 | tee shadowsocks.log
```

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnspi023foj20k8010mxb.jpg)
![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnspjv4l8zj20kb0190sw.jpg)

最后一步输完，你应该会看到下图中内容──是要你为 Shadowsocks 服务设置一个个人密码。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnspmwbvw1j20g00ld40y.jpg)

依次输入密码 端口（1-635535之间的数字都行），然后选择加密模式。
遵照上图指示，按任意键开始部署 Shadowsocks。这时你什么都不用做，只需要静静地等它运行完就好。结束后就会看到你所部署的 Shadowsocks 的配置信息。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnsppvo7wij20kc06174z.jpg)

记住其中黄框中的内容，也就是服务器 IP、服务器端口、你设的密码和加密方式。

### TCP Fast Open
但是为了更好的连接速度，你还需要多做几步。

首先是打开 TCP Fast Open，输入以下命令，意为用 nano 这个编辑器打开一个文件。

```
nano /etc/rc.local
```
用方向键把光标移到最末端，粘贴下面这一行内容，然后按 Ctrl + X 退出。

```
echo 3 > /proc/sys/net/ipv4/tcp_fastopen
```
输入“Y”并回车确认退出。

接着

```
nano /etc/sysctl.conf
```
在文末加上下面的内容，保存退出。

```
net.ipv4.tcp_fastopen = 3
```

再打开一个 Shadowsocks 配置文件。

```
nano /etc/shadowsocks.json
```

把其中 "fast_open" 一项的 false 替换成 true。

```
"fast_open":true
```

如果你希望添加多用户的话，可以将 "password" 字段如下图修改。其中，"22345":"password1"意为该用户使用 22345 端口、以“password1”为密码连接登录 Shadowsocks。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnsptpz7xnj20fu09jjro.jpg)

保存退出。最后，输入以下命令重启 Shadowsocks。

```
/etc/init.d/shadowsocks restart
```

### 设置Shadowsocks 客户端
![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnspwtms4tj20gf0ekq3o.jpg)

## 开启锐速
锐速 ServerSpeeder 是一个 TCP 加速软件，对 Shadowsocks 客户端和服务器端间的传输速度有显著提升。
首先需要用 SSH 登录 VPS。

```
ssh root@<host>
```

CentOS更换内核，提供锐速可用的内核下载

CentOS6 内核更换为： 2.6.32-504.3.3.el6.x86_64

```
rpm -ivh http://soft.91yun.org/ISO/Linux/CentOS/kernel/kernel-firmware-2.6.32-504.3.3.el6.noarch.rpm
rpm -ivh http://soft.91yun.org/ISO/Linux/CentOS/kernel/kernel-2.6.32-504.3.3.el6.x86_64.rpm --force
```
CentOS7 内核更换为： 3.10.0-229.1.2.el7.x86_64

```
rpm -ivh http://soft.91yun.org/ISO/Linux/CentOS/kernel/kernel-3.10.0-229.1.2.el7.x86_64.rpm --force
```

查看内核是否安装成功
```
rpm -qa | grep kernel
```
如果显示里面有这个内核就对了。

![](http://ww1.sinaimg.cn/large/880fe8a4ly1fnsw8qxr97j20kh03edgf.jpg)

重启并且查看内核版本

```
reboot
uname -r
```

更换好内核后，开始安装脚本

```
wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh
```

安装完成后，输入以下命令打开配置文件。

```
nano /serverspeeder/etc/config
```

将 advinacc 的 0 改为 1，保存并退出。
退出「终端」程序。
至此，整个搭建过程就完成了。

<i class="fas fa-plane fa-10x"></i>
*****

参考文章：
[教程：CentOS更换内核，提供锐速可用的内核下载](https://www.91yun.co/archives/795)
[科学上网的终极姿势:在 Vultr VPS 上搭建 Shadowsocks](https://zoomyale.com/2016/vultr_and_ss/)
[怎样检查IP被墙](https://www.banwago.com/1265.html)
[解决SSH出现HOST KEY VERIFICATION FAILED错误的方法](https://www.centos.bz/2017/08/ssh-host-key-verification-failed/)

<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">



