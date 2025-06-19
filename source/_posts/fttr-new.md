---
title: FTTR 冲击
date: 2025-06-19 15:57:24
updated: 2025-06-19 15:57:24
tags:
---
![cover](images/47974276_p0.jpg)
最近家里的宽带被运营商“突击升级”了——从500兆直接跳到千兆，还换上了号称“全屋Wi-Fi无缝覆盖”的FTTR设备。看着光猫上闪烁的绿灯和测速软件里飙到1000+Mbps的数字。以为能解锁网络自由，没想到很快就踩了坑;外网连不到家中的网络了。
<!-- more -->
# 设备信息
![FTTR](FTTR.jpg)
OptiXstar V175 XG-PON Terminal（下面那个大个的就是，上面小只是子AP），是个比较新的光猫网络上能寻找到的信息比较少。常规的 "CMCCAdmin“也不管用。网上几乎没有同类 FTTR 设备的破解帖子，我选择通过咨询宽带小哥得到了超级密码。

# 关闭IPv6防火墙
![FTTR](FTTR2.png)
找到这个选项关掉就可以了，有意思的是使用普通的账户登录也有这个页面，不过选项是灰色的不能选择和点击，使用F12修改页面去掉Disable的方式修改，会直接让你退出登录，回到光猫的登录界面。

# 禁用TR069
尚不清楚这个光猫禁用TR069之后能否脱离运营商的管理，不过还是禁用掉吧 保险一些QwQ

## 启用Telnet
![FTTR](FTTR3.png)

## 通过Telnet连接
> 用户名是root，密码是从宽带小哥那里获取到的超级密码
执行命令禁用TR069
    ip interface config interface wan1 status Disable

![FTTR](TR069_OFF.png)
## 效果图
![FTTR](FTTR1.png)

