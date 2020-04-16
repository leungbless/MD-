# Nmap -- 上帝之眼



## 1. 功能和作用

主动向扫描的网络发送指定的数据包，用来探测目标网络的信息。

> 1. 找出存活主机
> 2. 开放的端口和服务
> 3. 目标主机的操作系统
> 4. 安全过滤机制
> 5. 目标主机服务的版本信息
> 6. 利用脚本扫描漏洞

前期信息收集，为后续分析和利用漏洞打下基础，如果能分析出整个拓扑结构就更好了，对于内网的渗透，就会方便很多。

如果在前期通过信息收集找到了已存在的漏洞，可直接用 EXP 去试试看能不能拿下。





## 2. 安装和下载



[点击此处下载](https://nmap.org/download.html)



如果像在命令行使用 nmap 的命令，可以把目录添加到 环境变量 path 里面。

![添加环境变量](C:%5CUsers%5Clenovo%5CDesktop%5Cnm1.png)



## 3. Nmap主要参数介绍



Nmap的功能参数主要分为：

1. 目标说明
2. 主机发现
3. 端口扫描
4. 端口说明和扫描顺序
5. 服务与版本探测
6. 脚本扫描
7. 操作系统探测
8. 时间和性能
9. 防火墙/IDS规避和欺骗
10. 输出选项
11. 杂选项



## 4. TARGET SPECIFICATION  目标说明

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm2.png)



```shell
-iL  从文件中导出扫描的地址
-iR  随机选择目标进行扫描，num hosts表示数目，设置为0表示一直扫描
--exclude  移除某个扫描地址
--excludefile  从文件中移除某些扫描地址
```



Nmap 默认发送一个arp的ping数据包来探测目标主机在1-10000范围内所开放的端口

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm13.png)



先讲第一个参数    -iL

创建一个txt文件，里面放要扫描的网址或者IP地址

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm14.png)



文件我是放在 F 盘下面的  `nmap -iL F:\scan-IP.txt`

>C:\Users\lenovo>nmap -iL F:\scan-IP.txt
>WARNING: Could not import all necessary Npcap functions. You may need to upgrade to the latest version from https://npcap.org. Resorting to connect() mode -- Nmap may not function completely
>Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-29 12:38 ?D1ú±ê×?ê±??
>Nmap scan report for qhdzz.top (127.0.0.1)
>Host is up (0.0052s latency).
>rDNS record for 127.0.0.1: ==**qhdzz.com**==
>Not shown: 991 filtered ports
>PORT      STATE SERVICE
>80/tcp    open  http
>135/tcp   open  msrpc
>443/tcp   open  https
>445/tcp   open  microsoft-ds
>902/tcp   open  iss-realsecure
>912/tcp   open  apex-mesh
>3306/tcp  open  mysql
>5357/tcp  open  wsdapi
>10000/tcp open  snet-sensor-mgmt
>
>Nmap scan report for ==**baidu.com (39.156.69.79)**==
>Host is up (0.051s latency).
>Other addresses for baidu.com (not scanned): 220.181.38.148
>Not shown: 998 filtered ports
>PORT    STATE SERVICE
>80/tcp  open  http
>443/tcp open  https
>
>Nmap scan report for ==**DVWA.top (127.0.0.1)**==
>Host is up (0.0051s latency).
>Other addresses for DVWA.top (not scanned): 127.0.0.1
>rDNS record for 127.0.0.1: qhdzz.com
>Not shown: 991 filtered ports
>PORT      STATE SERVICE
>80/tcp    open  http
>135/tcp   open  msrpc
>443/tcp   open  https
>445/tcp   open  microsoft-ds
>902/tcp   open  iss-realsecure
>912/tcp   open  apex-mesh
>3306/tcp  open  mysql
>5357/tcp  open  wsdapi
>10000/tcp open  snet-sensor-mgmt
>
>Nmap scan report for ==**qhdzz.com (127.0.0.1)**==
>Host is up (0.0039s latency).
>Not shown: 991 filtered ports
>PORT      STATE SERVICE
>80/tcp    open  http
>135/tcp   open  msrpc
>443/tcp   open  https
>445/tcp   open  microsoft-ds
>902/tcp   open  iss-realsecure
>912/tcp   open  apex-mesh
>3306/tcp  open  mysql
>5357/tcp  open  wsdapi
>10000/tcp open  snet-sensor-mgmt
>
>Nmap done: 4 IP addresses (4 hosts up) scanned in 165.33 seconds



第三个参数是 exclude 表示从我扫描的名单中移除

`nmap www.baidu.com qhdzz.top --exclude qhdzz.top`

表示：我一开始准备扫描 baidu.com 和 qhdzz.top 的 后面移除了qhdzz.top，就不扫描它了。可以用于一个扫描列表中的白名单。

> C:\Users\lenovo>nmap www.baidu.com qhdzz.top --exclude qhdzz.top
> WARNING: Could not import all necessary Npcap functions. You may need to upgrade to the latest version from https://npcap.org. Resorting to connect() mode -- Nmap may not function completely
> Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-29 12:45 ?D1ú±ê×?ê±??
> Nmap scan report for www.baidu.com (14.215.177.38)
> Host is up (0.033s latency).
> Other addresses for www.baidu.com (not scanned): 14.215.177.39
> Not shown: 998 filtered ports
> PORT    STATE SERVICE
> 80/tcp  open  http
> 443/tcp open  https
>
> Nmap done: 1 IP address (1 host up) scanned in 42.07 seconds

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm16.png)



## 5. HOST DISCOVERY 主机发现

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm3.png)



```shell
-sL  列表扫描，仅将指定的目标IP列举出来，不进行主机发现
-sn  和-sP一样，只利用Ping命令进行主机发现，不扫描目标主机的端口
-Pn  将所有指定的主机视为已开启状态，跳过主机发现的过程，可用于网段的扫描     -好慢
-PS TCP SYN ping  发送一个设置了 SYN 标志位的空 TCP 报文，默认端口为80，可以指定端口
-PA TCP ACK ping  发送一个设置了 ACK 标志位的 TCP 报文，默认端口为80，可以指定端口
-PU UDP ping  发送一个空的 UDP 报文到指定端口，可以穿透只过滤 TCP 的防火墙
-P0  使用 IP 协议ping
-PR  使用 ARP 来ping
-n/-R  -n不用域名解析，加速扫描，-R为目标IP做反向域名解析，扫描会慢一点
-dns-servers  自定义域名解析服务器地址
-traceroute  目标主机路由追踪
```



`nmap -sL baidu.com` 列举出 IP 地址

> C:\Users\lenovo>nmap -sL baidu.com
> WARNING: Could not import all necessary Npcap functions. You may need to upgrade to the latest version from https://npcap.org. Resorting to connect() mode -- Nmap may not function completely
> Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-29 12:56 ?D1ú±ê×?ê±??
> Nmap scan report for baidu.com (220.181.38.148)
> Other addresses for baidu.com (not scanned): 39.156.69.79
> Nmap done: 1 IP address (0 hosts up) scanned in 0.11 seconds

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm17.png)



`nmap -sn baidu.com` 测试主机之间的连通性，相当于 Ping 命令

> C:\Users\lenovo>nmap -sn baidu.com
> WARNING: Could not import all necessary Npcap functions. You may need to upgrade to the latest version from https://npcap.org. Resorting to connect() mode -- Nmap may not function completely
> Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-29 12:57 ?D1ú±ê×?ê±??
> Nmap scan report for baidu.com (220.181.38.148)
> Host is up (0.034s latency).
> Other addresses for baidu.com (not scanned): 39.156.69.79
> Nmap done: 1 IP address (1 host up) scanned in 0.11 seconds

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm18.png)

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm15.png)



## 6. SCAN TECHNIQUES  端口扫描

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm4.png)

Nmap将目标端口分为6种状态：

1. ==open== （开放）
2. ==closed== （关闭）
3. ==filtered== （被过滤的）
4. ==unfiltered== （未被过滤） 可访问但不确定开放情况
5. ==open|filtered== （开放或者被过滤） 无法确定端口是开放的还是被过滤的
6. ==closed|filtered== （关闭或者被过滤） 无法确定端口是关闭的还是被过滤的

> Nmap产生的结果是基于目标机器响应的报文，而这些主机是不可信任的，会生成迷惑扫描器的报文

```shell
-sS  TCP SYN扫描，半开放扫描，速度快隐蔽性好（不完成 TCP 连接），能够明确区分端口状态
-sT  TCP连接扫描，容易产生记录，效率低 （慎用）
-sA  TCP ACK扫描，只设置 ACK 标志位，区别被过滤和没有被过滤
-sU  UDP服务扫描，例如 DNS/DHCP 等，效率低
```



## 7. PORT SPECIFICATION AND SCAN ORDER 端口说明和扫描顺序

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm5.png)

```shell
-p  指定扫描端口，可以是单个端口，也可以是范围，可以指定UDP或TCP协议扫描特定端口
-p<name>  指定扫描的协议，例如-p http 即可扫描http协议的端口状态
--exclude-port  不扫描某个端口
-F  快速模式，仅扫描100个常用的端口
```



`nmap -p 80,443,100-300 baidu.com`  扫描 baidu.com 的80,443还有100到300端口

 > C:\Users\lenovo>nmap -p 80,443,100-300 baidu.com
> WARNING: Could not import all necessary Npcap functions. You may need to upgrade to the latest version from https://npcap.org. Resorting to connect() mode -- Nmap may not function completely
> Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-29 14:35 ?D1ú±ê×?ê±??
> Nmap scan report for baidu.com (220.181.38.148)
> Host is up (0.038s latency).
> Other addresses for baidu.com (not scanned): 39.156.69.79
>Not shown: 201 filtered ports
> PORT    STATE SERVICE
> 80/tcp  open  http
> 443/tcp open  https
>
> Nmap done: 1 IP address (1 host up) scanned in 9.67 seconds

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm19.png)



## 8. SERVICE/VERSION DETECTION  服务与版本探测

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm6.png)

Nmap-services  包含大量服务的数据库，Nmap通过查询该数据库可以报告哪些端口可能开放了扫描服务，不一定正确。

```shell
-sV  会根据 nmap-services-probes 文件里面存储的是服务类型特征数据去判断具体扫描到的是哪种服务
--version-intensity<level>  设置版本扫描强度，范围0-9，默认是7，强度越高，耗时越长，越准确
```

`nmap -sV 192.168`  扫瞄目标主机的开启的服务和版本

> C:\Users\lenovo>nmap -sV baidu.com
> WARNING: Could not import all necessary Npcap functions. You may need to upgrade to the latest version from https://npcap.org. Resorting to connect() mode -- Nmap may not function completely
> Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-29 16:18 ?D1ú±ê×?ê±??
> Nmap scan report for baidu.com (220.181.38.148)
> Host is up (0.039s latency).
> Other addresses for baidu.com (not scanned): 39.156.69.79
> Not shown: 998 filtered ports
> PORT    STATE SERVICE  VERSION
> 80/tcp  open  http     Apache httpd
> 443/tcp open  ssl/http Baidu Front End httpd 1.0.8.18
>
> Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
> Nmap done: 1 IP address (1 host up) scanned in 62.25 seconds

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm20.png)



## 9. SCRIPT SCAN  脚本扫描

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm7.png)



Nmap允许用户自己编写脚本来执行自动化的操作或者扩展Nmap的功能，使用Lua脚本语音。

```shell
-sC  使用默认类别的脚本进行扫描
--script=<Lua scripts>  使用某个或某类脚本进行扫描，支持通配符描述
```



## 10. OS DETECTION  操作系统探测

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm8.png)

用 TCP/IP 协议栈 fingerprinting 进行探测，Nmap发送一系列 TCP 和 UDP 报文到远程主机，检查响应中的每一个比特。测试后 Nmap 把结果和数据库中超过1500个已知的 fingerprinting  比较，如果匹配则输出匹配结果。

```shell
-O  启用操作系统探测
-A  同时启用操作系统探测和服务版本探测
--osscan-limit  针对指定的目标进行操作系统探测
--osscan-guess  当Nmap无法确定所检测的操作系统时，会尽可能地提供最相近的匹配
```

> C:\Users\lenovo>nmap -A baidu.com
> WARNING: Could not import all necessary Npcap functions. You may need to upgrade to the latest version from https://npcap.org. Resorting to connect() mode -- Nmap may not function completely
> Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-29 16:31 ?D1ú±ê×?ê±??
> Nmap scan report for baidu.com (220.181.38.148)
> Host is up (0.037s latency).
> Other addresses for baidu.com (not scanned): 39.156.69.79
> Not shown: 998 filtered ports
> PORT    STATE SERVICE  VERSION
> 80/tcp  open  http     Apache httpd
> | http-robots.txt: 8 disallowed entries
> |_/baidu /s? /ulink? /link? /shifen/ /homepage/ /cpro /
> |_http-server-header: Apache
> |_http-title: Site doesn't have a title (text/html).
> 443/tcp open  ssl/http Baidu Front End httpd 1.0.8.18
> |_http-server-header: bfe/1.0.8.18
> |_http-title: Did not follow redirect to http://www.baidu.com/
> | ssl-cert: Subject: commonName=www.baidu.cn/organizationName=BeiJing Baidu Netcom Science Technology Co., Ltd/stateOrProvinceName=Beijing/countryName=CN
> | Subject Alternative Name: DNS:baidu.cn, DNS:baidu.com, DNS:baidu.com.cn, DNS:w.baidu.com, DNS:ww.baidu.com, DNS:www.baidu.com.cn, DNS:www.baidu.com.hk, DNS:www.baidu.hk, DNS:www.baidu.net.au, DNS:www.baidu.net.ph, DNS:www.baidu.net.tw, DNS:www.baidu.net.vn, DNS:wwww.baidu.com, DNS:wwww.baidu.com.cn, DNS:www.baidu.cn
> | Not valid before: 2020-02-27T00:00:00
> |_Not valid after:  2021-02-26T12:00:00
> |_ssl-date: TLS randomness does not represent time
> | tls-alpn:
> |_  http/1.1
> | tls-nextprotoneg:
> |   spdy/3.1
> |_  http/1.1
>
> Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
> Nmap done: 1 IP address (1 host up) scanned in 74.78 seconds

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm21.png)



## 11. TIMING AND PERFORMANCE  时间和性能

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm9.png)

Nmap开发的最高优先级是性能，但实际应用中很多因素会增加扫描时间，比如特定的扫描选项，防火墙配置以及版本扫描等。

```shell
-T <0-5>  设置时间模板级数，在0-5中选择。T0，T1用于 IDS 规避，T2降低了扫描速度以使用更少的带宽和资源。默认为T3，未做任何优化。T4假设具有合适及可靠的网络从而加速扫描。T5假设具有特别快的网络或者愿意为速度牺牲准确性
-host-timeout <time>  放弃低速目标主机，时间单位为毫秒
```



## 12. FIREWALL/IDS EVASION AND SPOOFING  防火墙/IDS规避和欺骗

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm10.png)

```shell
-f(报文分段); --mtu(使用指定的MTU)  将 TCP 头分段在几个包中，使得包过滤器，IDS以及其他的检测工具更困难
-D <decoy1[,decoy2][,ME]...>  隐蔽扫描;使用逗号分隔每个诱饵主机，用自己真实的IP作为诱选项。如果6号或更后的位置使用ME选项，一些检测器就不报告真实IP。如果不使用ME，真实IP将随机放置
-S <IP_Address>  伪造数据包的源地址
-source-port<portnumber>/-g <portnumber>  伪造源端口
```



## 13. OUTPUT 输出选项

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm11.png)

```shell
-oN  标准输出
-oX  XML输出写入指定的文件
-oS  相当于交互工具输出
-oG  Grep输出
-oA  输出至所有格式
-v   提高输出信息的详细程度
-resume <filename>  继续中断的扫描
```



## 14. MISC 杂选项

![](C:%5CUsers%5Clenovo%5CDesktop%5Cnm12.png)

```shell
-6  开启IPv6的扫描
-A  开启操作系统探测，版本探测，脚本扫描和路由追踪
```



## 15. 常用扫描指令



**1、弱口令扫描**

nmap --script=auth ip 对某个主机或某网段主机的应用进行弱口令检测

```
如：nmap --script=auth 192.168.0.101
```

**2、暴力破解**

nmap --script=brute ip 可以对数据库、MB、SNMP等进行简单的暴力破解

```
如：nmap --script=brute 192.168.0.101
```

**3、扫描常见漏洞**

nmap --script=vuln ip 

```
如：nmap --script=vuln 192.168.0.101
```

**4、使用脚本进行应用服务扫描**

nmap --script=xxx ip 对常见应用服务进行扫描 如：VNC、MySQL、Telnet、Rync等服务

```
如VNC服务：nmap --script=realvnc-auth-bypass 192.168.0.101
```

**5、探测局域网内服务开放情况**

nmap -n -p xxx --script=broadcast ip

```
如：nmap --script=broadcast 192.168.0.101
```

**6、Whois解析**

nmap -script external url

```
如：nmap -script external baidu.com
```

**7、扫描Web敏感目录**

nmap -p 80 --script=http-enum.nse ip

```
nmap -p 80 --script=http-enum.nse 192.168.0.101
```



简单的介绍了Nmap，配合metasploit食用，味道会更奇妙。