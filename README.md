# 1. OpenCore EFI for AMD Ryzen Hackintosh (macOS 10.15.3 - 11.2)

Hackintosh EFI for MSI B450M BAZOOKA PLUS + R5 2600 + RX 5700

基于 [mikigal/ryzen-hackintosh](https://github.com/mikigal/ryzen-hackintosh) 的 efi 修改制作

## 1.1. 修改 20210118

显卡 5700 卖了,换成 RX 560,唯一需要改的是 `boot-args` 去掉 `agdpmod=pikera`

## 1.2. 配置

```
处理器              AMD Ryzen 5 2600 六核
主板                微星 B450M BAZOOKA PLUS (MS-7B90) ( LPC Controller B450芯片组 )
显卡                AMD RX 560
内存                32 GB ( GLOWAY DDR4 2666MHz )
主硬盘               HS-SSD-C2000Pro 512G ( 512 GB / 固态硬盘 )
声卡                瑞昱 ALC892 @ AMD High Definition Audio 控制器
网卡                瑞昱 RTL8168/8111/8112 Gigabit Ethernet Controller / 微星
无线网卡             Fenvi T919 (BCM94360CD)
```

## 1.3. 可用

- iMessage、iCloud
- 板载声卡声音输出
- 隔空投送,随航接力
- 有线网络
- Xcode & iOS Simulator

## 1.4. 不可用

暂无

# 2. 常见问题

## 2.1. 黑屏问题

### 2.1.1. 安装时跑码然后黑屏

如果用的是 5000 系显卡,在 `boot-args` 添加 `agdpmod=pikera`

### 2.1.2. 安装完成,进入系统界面黑屏

bios 进入 设置 -> 高级 -> 超级 io

选择 Serial/COM Port/Parallel Port（串口，并口）全部关闭

## 2.2. 以太网已连接，但没有分配 ip

目前不知道,出现两次后面没有复现

## 2.3. iCloud 无法登录

没找到文字版,参考 b 站视频 [OpenCore 引导修复 iMessage FaceTime iCloud](https://www.bilibili.com/video/BV1hi4y1g7PV?from=search&seid=14246758270184094809)

## 2.4. EasyUEFI 建立 mac 启动项,无法开机

[![rWsSjU.png](https://s3.ax1x.com/2020/12/25/rWsSjU.png)](https://imgchr.com/i/rWsSjU)

我也不知道为什么会这样,后来重新建立一个 efi 分区,重建引导,源文件选择 OpenCore.efi

这样就可以开机和引导,<del>但是 Clover Configurator 无法挂载,只能到 pe 或 win 下修改 oc 配置</del>

可以用 OpenCore Configurator 挂载
[![rf9oB6.png](https://s3.ax1x.com/2020/12/25/rf9oB6.png)](https://imgchr.com/i/rf9oB6)

## 2.5. 声音修复

    万能声卡驱动：https://sourceforge.net/projects/voodoohda/

    AppleALC：https://github.com/acidanthera/AppleALC/releases

    注入声卡 ID 表：https://github.com/acidanthera/AppleALC/wiki/Supported-codecs

找到自己主板声卡型号,在`注入声卡 ID 表`查询 对应 layout

举个例子：我的声卡型号是 Realtek ALC892

在 `boot-args` 替换 `alcid=1`,一个不行就换个再试

## 2.6. boot-args 相关

### 2.6.1. 开启啰嗦模式

### 2.6.2. agdpmod=pikera

用于禁用 Navi gpu（RX 5000 系列）上的 boardID，否则会出现黑屏。如果您没有 Navi，请不要使用（即 Polaris 和 Vega 卡不应使用此功能）

# 3. 跑分参考

单核 978 多核 5196

对照: https://browser.geekbench.com/v5/cpu/search?utf8=%E2%9C%93&q=ryzen+5+2600

[![rWrMYq.jpg](https://s3.ax1x.com/2020/12/25/rWrMYq.jpg)](https://imgchr.com/i/rWrMYq)

# 4. 小技巧

## 4.1. 使用【访达】的【显示】设置显示完整路径

打开【访达】，找到菜单栏中的【显示】-》【显示路径栏】，或者使用快捷键【option+command+p】显示路径栏。

## 4.2. 开启允许安装任何位置的应用

不打开此选项，非 apple 第三方应用，会显示文件损坏！
![1](https://box.kancloud.cn/a4f224692466148a597353b7d700334a_727x620.png)
默认情况下，是只有前两项的：即你不能安装一些，绿色版应用！
在终端中输入这样一行命令就可以了！

```bash
sudo spctl --master-disable

```

## 4.3. 一键开启 macOS HiDPI

先把 NVRAM -> UIScale 修改为 Ag==(Data 类型,值 02)

https://github.com/xzhih/one-key-hidpi/blob/master/README-zh.md

## 4.4. 触发角

打开「系统偏好设置」-「桌面与屏幕保护程序」-「屏幕保护程序」，选择「触发角」就可以进入设置页面了。

屏幕右上角是调度中心，鼠标向右上一甩就是全部桌面和和窗口，想要把窗口或者文件从一个桌面拖到另一个桌面，也只需要按住窗口，甩到右上角就好。
屏幕右下角是显示桌面，这个和 Win 逻辑一样，习惯了。拖动文件或者网页图片，也只需要向右下角一甩就好。

## 4.5. 计算 md5

windows cmd

```bash
certutil -hashfile  '.\macOS Catalina 10.15.7(19H2) Installer for Clover 5122 and WEPE.dmg'  MD5
```
