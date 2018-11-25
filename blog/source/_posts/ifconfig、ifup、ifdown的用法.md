---
title: ifconfig、ifup、ifdown的用法
date: 2018-01-30 16:36:30
tags: Linux
---

**ifconfig**

	ifconfig 主要是可以手动的启动、观察与修改网络接口的相关参数，可以修改的参数很多，包括 IP 参数以及 MTU 等等都可以修改.对应的配置文件为/etc/sysconfig/network-scripts/ifcfg-ethx
	ifconfig {interface} {up|down}  <== 观察与启动接口
	ifconfig interface {options}    <== 设定与修改接口
	选项与参数：
	interface：网络卡接口代号，包括 eth0, eth1, ppp0 等等
	options  ：可以接的参数，包括如下：
	up, down ：启动 (up) 或关闭 (down) 该网络接口(不涉及任何参数)
	mtu      ：可以设定不同的 MTU 数值，例如 mtu 1500 (单位为 byte)
	netmask  ：就是子屏蔽网络；
	broadcast：就是广播地址啊
	[root@redhat6 ~]# ifconfig
	eth0      Link encap:Ethernet  HWaddr 08:00:27:4C:C5:88 
	inet addr:192.168.1.217  Bcast:192.168.1.255  Mask:255.255.255.0
	inet6 addr: fe80::a00:27ff:fe4c:c588/64 Scope:Link
	UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
	RX packets:218811 errors:0 dropped:0 overruns:0 frame:0
	TX packets:85 errors:0 dropped:0 overruns:0 carrier:0
	collisions:0 txqueuelen:1000
	RX bytes:14322456 (13.6 MiB)  TX bytes:11859 (11.5 KiB)
	
 
上面出现的各项数据是这样的(数据排列由上而下、由左而右)：

	eth0：就是网络卡的代号，也有 lo 这个 loopback ；
	HWaddr：就是网络卡的硬件地址，俗称的 MAC 是也；
	inet addr：IPv4 的 IP 地址，后续的 Bcast, Mask 分别代表的是 Broadcast 与 netmask 喔！
	inet6 addr：是 IPv6 的版本的 IP ，我们没有使用，所以略过；
	MTU：就是第二章谈到的 MTU 啊！
	RX：那一行代表的是网络由启动到目前为止的封包接收情况， packets 代表封包数、errors 代表封包发生错误的数量、 dropped 代表封包由于有问题而遭丢弃的数量等等
	TX：与 RX 相反，为网络由启动到目前为止的传送情况；
	collisions：代表封包碰撞的情况，如果发生太多次， 表示你的网络状况不太好；
	txqueuelen：代表用来传输数据的缓冲区的储存长度；
	RX bytes, TX bytes：总接收、发送字节总量

	透过观察上述的资料，大致上可以了解到你的网络情况，尤其是那个RX, TX 内的 error 数量，以及是否发生严重的collision 情况，都是需要注意的喔！
	临时修改网络参数：ifconfig eth0 192.168.1.217 netmask 255.255.255.0 broadcast 192.168.1.255
	启动、关闭网卡：ifconfig eth0 up/down
	查看DNS地址：cat /etc/resolv.conf
	重启网络服务：/etc/init.d/network restart
 
 **ifup、ifdown**

	ifup   {interface}
	ifdown {interface}
	这2个程序主要是搜寻/etc/sysconfig/network-scripts目录下的配置文件 (ifcfg-ethx) 来进行启动与关闭的， 所以在使用前请确定 ifcfg-ethx 是否真的存在于正确的目录内，否则会启动失败喔！ 另外，如果以 ifconfig eth0 .... 来设定或者是修改了网络接口后， 那就无法再以 ifdown eth0 的方式来关闭了！因为 ifdown 会分析比对目前的网络参数与ifcfg-eth0 是否相符，不符的话，就会放弃该次动作。因此，使用 ifconfig 修改完毕后，应该要以 ifconfig eth0 down 才能够关闭该接口喔。