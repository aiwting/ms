# 安装 Linux

> 目标：拥有一个Linux系统

## 实现方案

- Windows系统是你的主要生产环境，同时你还需要一个Linux环境
    - Windows子系统for Linux (WSL)
    - 双系统: 一台机器共存两个系统, 可在启动时选择
    - 虚拟机
- 舍弃Windows系统, 在一台机器上只安装一个Linux系统

**鉴于Windows系统的普及性和简便性, 大多是用户属于第一种情况. 如果你不是资深Linux用户, 强烈建议使用Windows子系统for Linux (WSL)**

## 安装WSL

WSL(Windows Subsystem for Linux)是微软开发的一项Windows功能，允许用户在 Windows 操作系统上直接运行 Linux 操作系统的二进制可执行文件，无需借助虚拟机或双启动配置。

注意事项:

- WSL有两个版本 WSL 1 和 WSL 2 。WSL2拥有完整的Linux内核，性能和兼容性更改好，鉴于现在的Windows系统几乎都是win10、win11，选择安装WSL2。
- 以下的命令均为 Windows PoweShell 命令, 可在cmd或powershell中执行

### 步骤1-选择安装方式

(1). 首先根据系统版本选择安装方式.

(2). 检查Windows系统版本：“Win键 + R” &gt;键入“winver” &gt;选择“确定” 可以查看当前版本信息.

<center style="color:#C0C0C0; text-decoration:underline"><img src="http://101.201.29.162:8899/i/2025/07/17/vsm9im-0.png" width=60%><br/>图1. 查看Windows版本信息</center>

(3). 检查计算机的类型（x64或者ARM64）："win键+R" &gt;搜索 cmd 或 PowerShell &gt;确定会打开命令行界面, 然后输入：`systeminfo`，在输出中查看“系统类型”。(个人用计算机几乎都是x64)

<center style="color:#C0C0C0;text-decoration:underline"><img src="http://101.201.29.162:8899/i/2025/07/17/vt541p-0.png" width=60%><br/>图2. systeminfo 命令输出</center>

(4). **安装 WSL2 要求高于一定版本的Windows10或者任何版本的Windows11**

- 对于win10 x64 系统：要求版本 1903 或更高版本，内部版本为 18362.1049 或更高版本。
- 对于win10 ARM64 系统：要求版本 2004 或更高版本，内部版本为 19041 或更高版本。
- 低于1903版本, 需要更新Windows版

(5). 然后根据Windows版本选择手动安装还是自动安装

- 如果是Windows 10 版本 2004 及更高版本（内部版本 19041 及更高版本）或 Windows 11，可以使用 *步骤2-命令行安装* 方式
- 介于上述版本之间的较低版本Windows10(即 1903-2004 之间)需要采用 *步骤4-手动安装* 方式

### 步骤2-命令行安装

(1). 一个命令安装运行 WSL 所需的一切内容：win键 &gt;搜索“powershell” &gt;“以管理员身份运行” &gt;输入命令: `wsl --install`

- 上述命令将默认安装 Linux 的 Ubuntu 发行版. 如果你对Linux发行版不了解, 保持默认即可; 若想安装其它发行版，可以使用下面的命令更改版本(这一步与上一步选择一个执行即可)：

```powershell
#列出可用的 Linux 发行版 <Distribution Name>
wsl --list --online
#选择一个版本安装
wsl --install -d <Distribution Name>    
```

(2). 然后重启计算机, 再次回到 PowerShell，输入 `wsl` 启动Linux，或者使用“开始”菜单打开该发行版（默认情况下为 Ubuntu）。首次启动新安装的 Linux 分发版时，将打开一个控制台窗口，系统会要求你等待一分钟或两分钟，以便文件解压缩并存储到电脑上。这时系统将要求你为 Linux 发行版创建“用户名”和“密码”。

<center style="color:#C0C0C0;text-decoration:underline"><img src="http://101.201.29.162:8899/i/2025/07/17/vu3ulq-0.png" width=90%><br/>图3. WSL(ubuntu)首次启动创建用户界面</center>

(3). 此用户名和密码特定于安装的每个单独的 Linux 分发版，与 Windows 用户名无关。

(4). 请注意，输入密码时，屏幕上不会显示任何内容。 **这称为盲人键入**。 你不会看到你正在键入的内容，这是完全正常的。

(5). 创建用户名和密码后，该帐户将是分发版的默认用户，并将在启动时自动登录。此帐户将被视为 Linux 管理员，能够运行 sudo (Super User Do) 管理命令。

### 步骤3-检查安装

1. 检查是否成功安装WSL2，打开 PowerShell，输入命令 `wsl --list --verbose`查看安装的WSL版本.
2. 输出应该如下：

```powershell
> wsl --list --verbos
  NAME      STATE           VERSION
* Ubuntu    Stopped         2
```

**当你看到Linux发行版的名字及版本，祝贺你！ 已成功安装了与 Windows 操作系统完全集成的 Linux ！**

### 步骤4-手动安装(可选)

(1). 开启Windows功能：桌面搜索框搜索“功能” &gt;打开“启用或关闭Windows功能” &gt;**在最下面找到并勾选“适用于 Linux 的 Windows 子系统”和“虚拟机平台(Virtual Machine Platform)”**

<center style="color:#C0C0C0;text-decoration:underline"><img src="http://101.201.29.162:8899/i/2025/07/17/vx2znu-0.png" width=60%><br/>图4. Windows功能</center>

- 下面是**命令行实现**(上一步做了这一步跳过)：“开始”菜单 &gt;“PowerShell” &gt;单击右键 &gt;“以管理员身份运行”，然后输入以下两条命令：

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

(2). **然后重启计算机**

(3). 下载 Linux 内核更新包

- [适用于 x64 计算机的 WSL2 Linux 内核更新包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
- [适用于ARM64 计算机的WSL2 Linux 内核更新包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_arm64.msi)

(4). 运行上一步中下载的更新包。（双击以运行 - 系统将提示你提供提升的权限，选择“是”以批准此安装。）

(5). 将 WSL 2 设置为默认版本: win键 &gt;搜索“powershell” &gt;“PowerShell” &gt;单击右键 &gt;“以管理员身份运行” &gt;在PowerShell中输入命令 `wsl --set-default-version 2`

(6). 打开微软商店，搜索你偏好的 Linux 分发版，选择“获取”即可安装。下面是网页连接，可以跳转到微软商店或者下载 `.exe`安装程序:(如果你不了解, 选择下面第一个Ubuntu即可)

- [Ubuntu 22.04 LTS](https://apps.microsoft.com/detail/9pn20msr04dw?rtc=1&hl=zh-CN&gl=CN)
- [Debian GNU/Linux](https://apps.microsoft.com/detail/9msvkqc78pk6?rtc=1&hl=zh-CN&gl=CN)

(7). 完成后可以在“开始”菜单找到刚安装的Linux发行版, 打开它。这时系统将要求你为 Linux 发行版创建“用户名”和“密码”。根据提示创建完毕后登录即可.

## 卸载WSL

- 想要删除一个WSL发行版与该发行版相关的所有数据，请运行 `wsl --unregister <distroName>`，其中 `<distroName>` 是你的 Linux 发行版的名称，可以用 `wsl -l` 命令的列表中查看已安装的发行版的名称。
- 此外，可以像卸载任何其他应用商店应用程序一样，在计算机上卸载 Linux 发行版应用
- 更多设置及命令在[微软WSL文档](https://learn.microsoft.com/zh-cn/windows/wsl/)中可以找到。

## WSL的备份与导入

Linux系统中一切皆文件, 操作系统本质上是一堆文件, 可以实现方便的移植. WSL 与Windows高度兼容, 基本不存在移植时的驱动问题.

1. 备份前保证发行版处于关闭状态, 使用 `wsl -l -v` 命令列出所有已安装的 WSL 发行版的名称、运行状态以及 WSL 版本。
2. 执行导出命令 `wsl --export <Distribution Name> <filename.tar> C:\backups\ubuntu_backup.tar`
3. 得到的 `.tar` 文件就是该 WSL 发行版的完整备份，包含了发行版的所有文件和配置，你可以将其拷贝到其他位置进行保存，如移动硬盘、云盘等，也可用于在同一台电脑或其他电脑上恢复或重新导入该发行版.
4. 执行导入命令: `wsl --import <Distribution Name> <安装位置> C:\backups\ubuntu_backup.tar`
5. 导入完成后，运行 `wsl -l -v` 查看是否成功导入.

注意事项:

- 即便你移植别人的WSL, 也需要先打开“适用于 Linux 的 Windows 子系统”和“虚拟机平台(Virtual Machine Platform)”两个windows功能.
- 确认移植的WSL版本是WSL1还是WSL2, 以及自己的Windows是否支持WSL2
- 移植别人的WSL虽然看起来简单, 但可能你并不适应他人的个人设置, 还是建议自己安装一个新的WSL
