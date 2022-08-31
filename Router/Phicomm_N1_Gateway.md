# 使用斐讯 N1 做旁路由

## 前言
对于部分用户而言，可能对于「旁路由」的概念并不是很清楚，下面摘抄一段文字来简单了解一下旁路由。

*我们经常在各种文章和视频里面看到听到「旁路由」这个词，但并不清楚他究竟是何方神圣。其实「旁路由」这并不是一个严谨的词汇，在官方的技术用语里，正确的叫法应该是「旁路网关」（为了方便，后续将依旧以旁路由为准）。而所谓的「旁路网关」，是指挂靠在主路由网络下的一个旁系网络，他分担了一部分路由器的功能，因此被大众简称为「旁路由」，本质上它是一个通过 LAN 口与主路由连接的一个客户端设备。(摘抄自 [从听说到上手，人人都能看懂的旁路由入门指南](https://zhuanlan.zhihu.com/p/122233420))*

说白了，旁路由可以在不对主路由做出复杂修改的前提下作为网关接管局域网，可以为局域网内的全部设备提供科学上网服务。

至于为什么选择原本是电视盒子的斐讯 N1 作为旁路由，则是因为它可能是目前性价比最高的能跑满 1000M 以下宽带的旁路由设备。

## 使用
### 准备工作
* 准备一台 (或多台 Mesh 组网) 能进入管理后台且能手动指定 DHCP 服务中**网关**和 **DNS** 的路由器；
* 准备一台刷有 OpenWrt 的斐讯 N1 盒子，拼多多购买约 150 元，建议指定卖家刷 73+o 版本 (发文时最新版本为75+o)；
* 准备一根超六类纯铜网线，否则你的 N1 很有可能只握手 100Mbps 速率。

*P.S. 家用 WiFi6 路由器推荐选购小米，要高性能——Redmi AX6000，要 2.5G 网口——小米 AX6000，要性价比——Redmi AX5400。这三款都可以通过拼多多每日摇红包获取全网最低价，在 500-100 优惠券的加持下通过凑单购买前两款不到 400 元，在 200-40 优惠券加持下直接购买第三款不到 290 元。*

### 开始使用
1. 进入主路由后台，查看 DHCP 服务的「起始和结束 IP」和「局域网 IP 地址」，小米路由器在「常用设置——局域网设置」内。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/Router_DHCP_01.png)

2. 将 N1 通电，使用设备 (电脑或手机) 通过网线或 WiFi (密码：password) 连接 N1，手动设置设备的 IP 地址为`192.168.1.X`(建议 X 大于 10 小于 200)，子网掩码为`255.255.255.0`，网关为`192.168.1.1`。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/Network_01.png)

3. 使用设备的浏览器输入`192.168.1.1`打开 N1 后台 (账号：root，密码：password)，点击左侧「网络——接口」，再点击右侧「LAN」的「修改」。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/OpenWrt_Network_Interfaces_01.png)
打开后先修改上方「基本设置」中的选项：<br>
○ 协议：静态地址<br>
○ IPv4 地址：192.168.31.2 (你喜欢的 DHCP 内任一有效地址，因为 N1 是第二网关，所以我设置的 .2)<br>
○ 子网掩码：255.255.255.0<br>
○ IPv4 网关：192.168.31.1 (局域网地址/主路由 IP)<br>
○ DNS 服务器：192.168.31.1 (局域网地址/主路由 IP)
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/OpenWrt_Network_Interfaces_02.png)
再修改上方「高级设置」中的选项，如下图所示：
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/OpenWrt_Network_Interfaces_03.png)
接着修改上方「物理设置」中的选项，取消勾选「桥接接口」并选中「以太网适配器 eth0」(非常重要)。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/OpenWrt_Network_Interfaces_04.png)
最后修改下方「基本设置」中的选项，勾选「忽略此接口」，然后点击右下角的「保存&应用」。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/OpenWrt_Network_Interfaces_05.png)
因为修改了 IP 段，此时界面会卡柱，等待 1 分钟左右拔掉 N1 电源即可。

4. 通过网线将 N1 连接至主路由的 LAN 口并接通电源，等待 1 分钟左右启动后，在设备的浏览器输入`192.168.31.2`(你设置的 N1 地址) 进入 N1 后台，点击左侧「网络——防火墙」，再点击右侧上方「自定义规则」。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/OpenWrt_Network_Firewall_01.png)
在「自定义规则」最下方添加规则`iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE`，点击界面右下角「重启防火墙」。

5. 进入主路由后台，修改 DHCP 服务的「DNS」和「默认网关」为`192.168.31.2`(你设置的 N1 地址)，点击保存。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/Router_DHCP_02.png)

等待主路由重启，设备再次接入局域网后，获取到的网关应该是我们指定的 N1 地址，此时 N1 已经接管了局域网网络，可以使用 OpenClash 或 ShadowSocksR Plus+ 等软件创建科学上网环境。

但目前只是能用，还不够完善，所以你需要再通过几个小步骤对 N1 的接管进行优化。

## 优化
### 关闭 N1 的 WiFi
N1 的 WiFi 性能很差，开启还会对网络造成一定干扰，所以推荐直接关闭。

打开 N1 后台，点击左侧「网络——无线」，直接点击「停用」即可。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/OpenWrt_Network_Wireless.png)

### 删除负载均衡规则
N1 作为旁路由时，启用负载均衡后开机一段时间会造成防火墙错误，需要重启防火墙 (有时甚至无法访问 N1 只能重启 N1) 才能正确连接 N1 访问或联网，所以我们直接删掉相关内容。

打开 N1 后台，点击左侧「网络——负载均衡」，逐一点击右侧「接口」「成员」「策略」「规则」，删掉里面的所有条目。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/OpenWrt_Network_LoadBalancing.png)

### 关闭 IPv6（可选）
当前 Clash 对 IPv6 的支持并不友好，当你的优先使用 IPv6 的设备或软件 (如 macOS 中的 Safari) 遇到访问网络或开启网页变慢时，你可能需要关闭 IPv6 来获得正常的访问速度。

进入主路由后台，关闭局域网 IPv6 功能，小米路由器在「常用设置——上网设置」中。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/Router_IPv6_01.png)

进入 N1 后台，点击左侧「网络——接口」，再点击右侧「LAN」的「修改」，将上方「基础设置」中的「IPv6 分配长度」、下方「IPv6 设置」中的服务全部修改为`已禁用`。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/OpenWrt_Network_Interfaces_06.png)

## 尾声
到了这里我们的工作基本做完了，其余功能的开发全看你的折腾能力。截止发文时，N1 已稳定运行 5 天 12 小时。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/OpenWrt_Status_Overview.png)

下面是设备测速截图，网络：天津电信 500M 宽带，代理：Dler Cloud 日本 AC 节点，跑满带宽无压力。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/Phicomm_N1/Speedtest.png)

以上就是全部内容，后续可能会因使用中的经验对本文进行修改，如果你对此旁路由解决方案感兴趣，赶快行动起来吧！
