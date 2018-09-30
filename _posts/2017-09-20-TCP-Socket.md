---
layout:     post
title:      TCP & Socket
subtitle:   
date:       2017-09-20
author:     BY annie dong
header-img: img/post-bg.jpg
catalog: true
tags:
    - tcp
    - socket
    
---
说tcp&socket的太多了，我只是想用自己的话来描述一下...

---

## TCP
### 网络模型
1. 物理层：将信息编码成电流脉冲或其它信号用于网上传输，RJ45等将数据转化成0和1 —— 集线器 网卡 网线 比特流  
2. 数据链路层：将源自网络层的数据可靠的传输到相邻的目标网络层，物理地址寻址、数据的成帧、进行错误检测和修正，规定了0和1的分包形式，确定了网络数据包的形式：    
- 保存的最主要是网卡的 mac 地址，mac 地址负责局域网通信的，发件人和收件人的mac地址    
- 基本数据单位为帧    
- 主要的协议：以太网协议    
- 两个重要设备名称：网桥和交换机    

3. 网络层：路径选择，路由，逻辑寻址        
- 保存的最主要的信息是IP地址，IP地址是负责外网通信的，发件人和收件人的IP地址    
- 基本数据单位为IP数据报；
- 包含的主要协议：
    IP协议（Internet Protocol，因特网互联协议）IPV4,IPV6
    ICMP协议（Internet Control Message Protocol，因特网控制报文协议）    
　 　ARP协议（Address Resolution Protocol，地址解析协议）    
　　 RARP协议（Reverse Address Resolution Protocol，逆地址解析协议）    
- 重要的设备：路由器    

4. 传输层：传输协议数据单元，确定端口号，确定传输协议（ TCP/IP可靠连接协议， UDP不可靠无连接协议），传输前的错误检测，流量控制    
5. 应用层：用户的应用程序和网络之间的接口老板    
- 数据传输基本单位为报文 应用协议数据单元    
- 包含的主要协议：FTP（文件传送协议）、Telnet（远程登录协议）、DNS（域名解析协议）、SMTP（邮件传送协议），POP3协议（邮局协议），HTTP协议（Hyper Text Transfer Protocol)    

### TCP vs UDP
#### TCP
- 面向连接   
三次握手，四次挥手, 中间数据分包传输:    
![img](https://dongjx.github.io/img/posts/tcp.jpg)    
 使用Wireshake进行tcp抓包：    
![img](https://dongjx.github.io/img/posts/tcp1.jpg)    
![img](https://dongjx.github.io/img/posts/tcp2.jpg)   
- 可靠传输    
- 分包传输    
- 适用传输大量数据    
- 速度慢    
`对应的应用层协议：http, https, ftp...`

#### UDP
- 面向非连接    
- 不可靠传输    
- 不分包传输    
- 适用传输少量数据    
- 数据快    
`对应的应用层协议：DNS...`    

## SOCKET
本质是编程接口(API)，对TCP/IP的封装，TCP/IP也要提供可供程序员做网络开发所用的接口，这就是Socket编程接口    

