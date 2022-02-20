# 解锁 macOS 的新特性

## 前言
我们知道，每年苹果发布新的 macOS 时都会增加一些新功能/特性，但部分功能/特性会对机型作出限制，这些限制通常并不是硬件不支持，~~而仅仅是因为设备上市时间较为久远，~~ 既然硬件支持，macOS 又没有 iOS 那么封闭，那我们能不能通过修改系统文件或使用命令解锁这些新特性呢？<br>
<br>
通过在搜索引擎搜索，确实找到了一个能够解锁「隔空投放接收器」的快捷指令，但使用它需要关闭 SIP，并且每次重新启动都要重新运行这个快捷指令，该选项也不会出现在「系统偏好设置——共享」中，可以说仅仅是能用，远谈不上完美。<br>
<br>
在折腾黑果的过程中，知道有一个 [FeatureUnlock](https://github.com/acidanthera/FeatureUnlock) 内核补丁，它的功能就是在部分机型上解锁 macOS 的新特性，具体如下：

```
# 解锁以下机型的随航 (Sidecar) 功能
MacBook8,1
MacBookAir5,x - MacBookAir7,x
MacBookPro9,x - MacBookPro12,x
Macmini6,x    - Macmini7,1
MacPro5,1     - MacPro6,1
iMac13,x      - iMac16,x

# 解锁以下机型的隔空投放接收器 (AirPlay to Mac) 功能
MacBook8,1
MacBookAir5,x - MacBookAir7,x
MacBookPro9,x - MacBookPro14,x
Macmini6,x    - Macmini8,1
MacPro5,1     - MacPro6,1
iMac13,x      - iMac18,x

# 解锁以下机型的夜览 (NightShift) 功能
MacBook1,1    - MacBook7,1
MacBookAir1,1 - MacBookAir4,x
MacBookPro1,1 - MacBookPro8,x
Macmini1,1    - Macmini5,x
MacPro1,1     - MacPro5,1
iMac4,1       - iMac12,x

# 解锁以下机型的通用控制 (Universal Control) 功能
MacBookAir7,x
MacBookPro11,4 - MacBookPro12,1
Macmini7,1
MacPro6,1
iMac16,x
```
虽然 macOS 没有 iOS 那么封闭，但我们也没办法直接启用这个内核补丁，而且它需要依赖 [Lilu](https://github.com/acidanthera/Lilu) 这个超级补丁引擎，那我们能不能直接用在黑果使用的引导 [OpenCore](https://github.com/acidanthera/OpenCorePkg) 启动白果，从而启用这个内核补丁呢？<br>
<br>
答案是可以的，而且有非常简单的方法。

## 使用
### 准备工作
跳转到  [OpenCore-Legacy-Patcher/Releases](https://github.com/dortania/OpenCore-Legacy-Patcher/releases) 下载最新的 GUI 版本 (截图中最新版为 0.4.2，并不是一定要下载这个版本)。

![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/OpenCore-Legacy-Patcher/Download_OCLP.png)

下载完成后解压 (一般使用系统下载器下载会自动解压)，将 OpenCore-Patcher.app 移动到「应用程序」中。

### 开始使用
1. 打开 OpenCore Patcher，图标短暂出现在程序坞后会消失，这是正常现象，大约等待 10 秒左右便会出现主页面。

2. 在主页面，我们直接点击 **Setting** 进入设置。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/OpenCore-Legacy-Patcher/OCLP_Home_1.png)

3. 在设置页面，先确定机型是否正确，如果不正确需要手动选择正确机型。关闭默认打开的 **Show Boot Picker**，开启 **Allow native models**，如图所示。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/OpenCore-Legacy-Patcher/OCLP_Settings.png)
之后点击最底下的 **Return to Main Menu** 返回主页面。

4. 在主页面点击顶部的 **Build and Install OpenCore** 进入构建和安装。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/OpenCore-Legacy-Patcher/OCLP_Home_2.png)

5. 在构建和安装页面点击 **🔨 Build OpenCore** 进行构建。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/OpenCore-Legacy-Patcher/OCLP_Build_1.png)
程序会针对当前设置和硬件自动构建 OpenCore 引导，此过程需要网络。

6. 构建成功后，点击顶部 **🔩 Install OpenCore** 进入安装步骤。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/OpenCore-Legacy-Patcher/OCLP_Build_2.png)

7. 先选择系统磁盘 (如果不知道哪个是系统磁盘，可推出所有外置磁盘后操作)，再选择 EFI 分区，输入本机密码即可进行安装。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/OpenCore-Legacy-Patcher/OCLP_Install.png)

8. 安装成功后如图所示，退出该程序即可。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/OpenCore-Legacy-Patcher/OCLP_Done.png)

9. 重新启动 Mac，按住 **Option** 键直至出现引导选择页面再松开，一般有以下两个选项。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/OpenCore-Legacy-Patcher/Boot_Picker_1.png)

10. 选中 EFI Boot 后，按住 Control 键，移动鼠标到下方点击类似刷新的图标，即可锁定该启动项。
![](https://raw.githubusercontent.com/GeQ1an/Special-Guide/master/Images/OpenCore-Legacy-Patcher/Boot_Picker_2.png)

11. 进入系统后，打开「系统偏好设置——共享」，查看是否有**隔空投放接收器**选项，若有请开启并通过其他苹果设备测试是否正常使用。

