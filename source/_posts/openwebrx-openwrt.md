---
title: OpenWRT(Docker)部署OpenWebRX指南
date: 2023-09-08 14:19:16
updated:
tags: ham
---
![cover](images/hello-world.md/hello.jpg)

白露时节，是农历八月的节气之一，意味着秋天的到来。太阳温和地照耀着大地，不再有夏日的炎热。这个时候野外小憩打开电台听听广播不失为一种享受。

<!-- more -->

# 何起
外出若是打包小包提着各种天线和收音机实在是不够优雅，若能以最轻量的装备获得随时随地收听无线电的效果那岂不是非常爽？比如说仅凭浏览器就可以收听，就像WebSDR。为解决这个问题我找到了开源项目[OpenWebRX](https://github.com/jketterl/openwebrx),这是一个非常强大的SDR接收软件兼容广泛的SDR设备同时能解码数字信号最为重要的是带有Web界面，这完美的符合了我的需求。

# 需要什么
1.OpenWebRX兼容的SDR设备，笔者使用的是RSP1
![兼容设备列表](Screenshot_20230908_145123.png)
2.接收天线及馈线，这个看你的需求和设备实际购买。
3.安装有Docker和带有USB2.0及以上接口的主机，本文中笔者采用的是树梅派4装有OpenWRT系统。
# 安装SDR
1.将SDR接入主机前后分布在终端中执行以下命令
```
lsusb
```
如果连接没问题的话，结果差不多应该是这样会多出来一条我们需要记下来红框内的数字在我的设备上他是001 004，那么在系统中他对应的位置就是/dev/bus/usb/001/004。
![lsusb](Screenshot_20230908_151546.png)
2.创建一个名为docker-compose.yml的文件，将以下内容填入。
  （建议直接在终端中使用nano或者vi）
```
version: "3"
services:
  openwebrx:
    image: jketterl/openwebrx:stable
    volumes:
	#配置文件目录，请自行替换/root/233
      - /root/233:/var/lib/openwebrx
    ports:
    #映射端口，讲XXXX替换成自己喜欢的端口号吧
      - XXXX:8073
    devices:
    #映射USB设备，请自行替换为自己的ID
      - /dev/bus/usb/001/004:/dev/bus/usb/001/004
    tmpfs:
    #缓存，建议设置到RAM DISK
      - /tmp/openwebrx
```
![docker-compose.yml](Screenshot_20230908_154025.png)
3.打开终端在docker-compose.yml所在目录执行
```
docker-compose up -d
```
![up -d](Screenshot_20230908_154127.png)
这样我们的OpenWebRX就启动了。
3.打开浏览器输入主机IP及你设置的端口号，笔者的是192.168.1.1:8073
![Web](Screenshot_20230908_155034.png)
如果你看到了如上界面那么代表这你已经成功部署了OpenWebRX，点击三角形播放键就能听到沙沙声音（如果提上没有有效设备可能是你使用的SDR设备预设配置中并没有包含参考[# 配置SDR](# 配置SDR)），在右下角有波段切换和解调器切换按键。第一次安装波段应该是只有20m,30m,40m,80m,48m。我们可以通过右上方Settings来自行添加。进入设置面板需要用户名与密码这个需要自己设定并没有默认的。
# 添加用户
源自官方[Wiki](https://github.com/jketterl/openwebrx/wiki/User-Management)

终端执行如下命令来找到容器ID
```
docker container ls
```
![[Screenshot_20230908_160010.png]]
可以看出ID为55519b7a8f64

接下来我们通过exec来进入容器，来添加一个用户名为kutina的管理员账户
```
docker exec -it 55519b7a8f64 python3 /opt/openwebrx/openwebrx.py admin adduser kutina
```
![ls](Screenshot_20230908_160439.png)
执行后他会要求你输入两边密码，密码不会回显无需惊慌输入完成回车既可创建完成。

# 配置SDR
下面我们来到Settings，输入刚才创建账户完成登录
![setting](Screenshot_20230908_160642.png)
SDR device settings便是我们要找的对象。

点开我们看到有几个预设的设备配置，如果你的SDR设备是预设列表中的可以直接用，没有的话就要Add New Device...
![Device](Screenshot_20230908_160949.png)