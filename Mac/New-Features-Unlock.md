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
虽然 macOS 没有 iOS 那么封闭，但我们也没办法直接启用这个内核补丁，而且它需要依赖 [Lilu](https://github.com/acidanthera/Lilu) 这个超级补丁引擎，那我们能不能直接用在黑果使用的引导 [OpenCore](https://github.com/acidanthera/OpenCorePkg) 启动白果，启用这两个内核补丁呢？<br>
<br>
答案是可以的，而且有非常简单的方法。

## 使用
### 准备工作
下载最新的 [OpenCore-Legacy-Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases) GUI 版本（截图中最新版为 0.4.2，并不是一定要下载这个版本）。
