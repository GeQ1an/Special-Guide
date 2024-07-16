# 更换中兴万兆 2.5GE 光猫

## 前言
随着 10G-EPON / XGPON 光纤线路的普及，千兆宽带价格越来越低，更多的伙伴办理了千兆宽带，但往往运营商提供的光猫没有 2.5GE 网口，成为限制外部网速超过 1000Mbps 无法跨越的瓶颈。

针对这种情况，同时避免和运营商费力扯皮，最直接有效的办法就是自行购买一款带有 2.5GE 网口的万兆光猫，现在中兴直接把万兆双模光猫价格打到 120 元左右，性价比很高。

## 使用
### 准备工作
* 确认自己的主要设备支持超千兆网络，路由器、设备拥有 2.5GE 或 10GE 网口、具有 2x2 MIMO 160MHz 或 4x4 MIMO 80Mhz 以上的 WiFi 规格。
* 如果当前光猫类型是普通 EPON / GPON，需要确认线路是否支持 10G-EPON / XGPON，否则不止折腾起来没意义，还会无法正常使用。
* 知道自己光猫的超级密码，或清楚地了解光猫认证方式（如 LOID 认证、SN 认证、密码认证等）和 VLAN ID 等必要信息。
* 确保使用满足规格的超六类以上网线，推荐使用七类屏蔽网线。

### 购买光猫
选择并购买一款中兴万兆双模光猫，以下型号均使用同款 CPU，根据需求选购即可。

|   型号   |  2.5GE  |  WiFi  |  千兆口  |  电话口  |   尺寸   |   价格   | 
|--------:|:--------:|:-------:|:-------:|:-------:|:--------:|:--------|
| F7615TV3 |    1    |  AX3000  |    3    |    1    |    中    | 约 120 元 |
| F7015TV3 |    1    |     X    |    3    |    1    |    小    | 约 125 元 |
| F7005TV3 |    1    |     X    |    3    |    X    |   更小   | 约 110 元 |

购买时请务必要卖家改好地区、开启 Telnet 固化（以上价格来自闲鱼，仅供参考，发文时间 2024 年 7 月）。

### 开始设置
#### 光猫基础设置
1. 光猫插电，将网线插入光猫任一网口，连接电脑与光猫

2. 浏览器打开 192.168.1.1，输入默认管理员账号 `telecomadmin` 和密码 `nE7jA%5m` 进入后台

3. 点击上方「管理——上行方式」，根据自己的宽带类型选择，点击右下方「保存」，光猫会重启
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/ZTE_Modem/ZTE_Modem_1.png)

4. 重启后重新打开光猫后台，点击上方「网络——网络设置」，新建一个连接，建议桥接由路由器拨号（少一层 NAT）
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/ZTE_Modem/ZTE_Modem_2.png)
○ 封装类型： PPPoE <br>
○ 连接模式： 桥接 <br>
○ 业务类型： 上网 <br>
○ IP模式： IPv4&IPv6 <br>
○ VLAN 模式： 改写(tag) <br>
○ VlanID： 148（根据自己情况填写，天津电信是 148） <br>
○ DHCP服务使能： 不勾选 <br>
○ host模式： 不勾选 <br>
○ LAN端口绑定： 不勾选 <br>
全部选好后，点击右下方「添加」<br>
*（不勾选 LAN 口绑定是因为中兴光猫勾选 LAN 口绑定后属于软桥接，重度使用可能会有性能问题，不勾选则可以有效解决这个问题。）*

5. 如果有 IPTV，再新建一个连接填入有关信息，注意绑定 LAN 端口，并打开「应用——IGMP设置」设置好组播，如果没有则可跳过直接进行下一步操作
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/ZTE_Modem/ZTE_Modem_3.png)

#### 光猫认证设置
光猫认证方法有很多种，最常见的是 LOID 认证、SN 认证、设备密码（ONT Password）认证，也许还有 MAC 组合认证，自己按需搭配使用即可
1. LOID 认证 <br>
点击顶部「网络——远程管理」，左侧「宽带识别码认证」，填入自己的 LOID，如果有密码，勾选使用密码一并填入
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/ZTE_Modem/ZTE_Modem_4.png)
点击右下方保存，光猫会进入获取 OLT 认证界面，但我们还没有接入光纤，所以会卡住，稍等十几秒关闭页面即可

2. SN 认证或密码认证 <br>
这些信息需要使用 Telnet 进行修改，请自行搜索所用平台使用 Telnet 的方法<br>
打开终端，输入 `telnet 192.168.1.1`，回车，出现 Login: 时输入默认账号 `root`，回车，输入默认密码 `Zte521`，回车
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/ZTE_Modem/ZTE_Modem_5.png)
修改 SN，输入 `setmac 1 2176 SN前四位`，回车，输入 `setmac 1 2176 SN后八位`，回车 <br>
修改设备密码输入 `setmac 1 2179 你的密码`，回车 <br>
修改 MAC 输入 `setmac 1 32769 MAC地址`，回车 <br>
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/ZTE_Modem/ZTE_Modem_6.png)
设置好认证信息后，我们就可以把光猫断电，接入光纤后重新接入电源

#### 光猫完善设置
光猫接入光纤后，我们首先需要确定 OLT 认证是否正常，打开光猫后台，查看状态总览中光路（OLT）认证是否显示认证成功。如果成功，那么恭喜你即将完成设置；如果未成功，请返回上一步核查设置。

![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/ZTE_Modem/ZTE_Modem_7.png)

此时 OLT 认证成功，但会 ITMS 注册会显示注册失败，我们重新打开 Telnet 解决这个问题。

打开终端输入 `telnet 192.168.1.1`，输入之前我们提到的账号和密码

输入 `sendcmd 1 DB set PDTCTUSERINFO 0 Status 0`，回车<br>
输入 `sendcmd 1 DB set PDTCTUSERINFO 0 Result 1`，回车<br>
输入 `sendcmd 1 DB save`，回车<br>
输入 `reboot` 回车，重启光猫

重启后就会看到 ITMS 也已经注册成功，此时用网线连接光猫 2.5GE 网口和路由器 WAN 网口，设置好拨号信息即可正常上网了。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/ZTE_Modem/ZTE_Modem_8.png)

### 测速
测速请确保设备速率瓶颈超过千兆，比如 iPhone，除了 iPhone 15 Pro 系列 WiFi 最高支持 2x2 MIMO 160Mhz，可以握手 2400Mbps 速率外，iPhone 11-15 最高只支持 2x2 MIMO 80Mhz，可以握手 1200Mbps 速率，实际测速大概只能到 800Mbps 左右。可以使用较新的支持 WiFi6 的 Android 设备进行测速，基本都能握手 2400Mbps 速率，足以跑满千兆宽带。目前手机端花瓣测速的节点较多，可以下载使用。

下图是我的测试结果。直连使用花瓣测速本地节点进行测速，代理使用 Dler Cloud 日本 AC 节点通过 Speedtest 进行测试。

![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/ZTE_Modem/ZTE_Modem_Speedtest.png)

不知是天津电信给的带宽余量少，还是我所在区域 ODN 或 ONU 的问题，测速最高只能到 1100Mbps 左右，一般来说千兆宽带可以跑到 1200Mbps 以上，多的甚至能到 1500Mbps，大家自行测试吧。

### 其他
#### 中兴光猫常用 Telnet 命令
1. 查看系统参数<br>
setmac show2

2. 修改管理员账户和密码<br>
sendcmd 1 DB set DevAuthInfo 0 User admin （改管理员名称为admin）<br>
sendcmd 1 DB set DevAuthInfo 0 Pass 12345678 （改管理员密码12345678）<br>
sendcmd 1 DB save （保存）

3. 修改连接数限制<br>
sendcmd 1 DB p CltLmt （查看当前用户数量）<br>
sendcmd 1 DB set CltLmt 8 Max 100 （修改最大用户数量为100，建议最大数目不超过255）<br>
sendcmd 1 DB save （保存）

#### 获取超密等骚操作
1. 打电话给装维师傅，说想要光猫的超级密码改桥接，理由可以说“玩游戏老是跳 ping 想改桥接试一下，只改桥接不会乱搞”，一般会给。
 
2. 如果装维师傅不给，或者说有规定之类的，那只好折腾一点重置一下光猫，然后报修，师傅来调试时，随时准备偷拍 LOID 认证信息或超级密码（记得静音）。如果是没有 WiFi 的光猫，可以准备好 Windows 电脑让师傅设置光猫，Edge 浏览器会自动记录填写的表格信息一段时间，双重保险。

3. 如果是电信光猫（不知道联通光猫是否也有这个入口）且只需要知道 VLAN ID 的话，可以打开光猫后台，点击底部「快速装维入口——网络侧信息」，连接名称后方的数字就是对应的 VLAN ID

## 免责声明
以上内容仅供参考，严禁用于非法用途。刷机、修改信息等操作可能会造成设备损坏，本人不承担任何责任，请认真思考后再决定是否尝试。
