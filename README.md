No license, just for fun. 

deepin is convenient, but lost something.

## 基本操作
### FAQ（常见问题）
[深度操作系统新手指引（FAQ）](https://bbs.deepin.org/forum.php?mod=viewthread&tid=146921)

deepin 15.5 的文件管理器还有不小的问题，比如打开一个内容很多的文件夹的时候，慢的一匹。电脑开机久了，可能还会导致删除目录之后没有立即刷新当前文件夹中的内容，要切换目录然后在切换回来才能看得到。
### 更换软件源，处理问题源
````sh
sudo gedit /etc/apt/sources.list   
deb http://mirrors.aliyun.com/deepin panda main contrib non-free   
deb-src http://mirrors.aliyun.com/deepin panda main contrib non-free   
sudo rm /etc/apt/sources.list.d/spotify.list
````
### 替换 apt  
apt-fast 是用来代替 `apt-get` 的的一个 shell 脚本程序，它通过多线程的方式改善了更新和下载安装包的速度。如果你经常用终端和 apt-get 来安装和升级软件的话，可以试试 apt-fast 。
````sh
sudo add-apt-repository ppa:apt-fast/stable  
sudo apt update  
sudo apt install apt-fast
````
### 软件源支持 add-apt-repository ppa
````sh
sudo apt install software-properties-common
````
apt 安装 java、katoolin 时会用到 
### 直接 root 用户登录系统
两种思路：
- 将默认的登录用户永久提升到 root 权限
- 调整 Lightdm（配置文件 `/etc/lightdm/lightdm.conf`，[详细步骤请看这篇文章](http://www.cnblogs.com/EasonJim/p/7128317.html)。  

重要提示：[《linux下如何添加一个用户并且让用户获得root权限》](https://www.cnblogs.com/johnw/p/5499442.html) 中的方法三直接将uid改为0，是一个坑，会导致开机无法登录。因为 lightdm 有限制，我将 uid 改为 1001，能够显示登录界面但是登录不进去，估计 home 目录里面也有限制要调整。
### 不能做的事情
````sh
ln -f /usr/bin/python3 /usr/bin/python
````
因为一些东西依赖于 python，且他们认为系统的 python 是指 python2，所以不要做这个改动。  
不要随意删除 `/usr/share/applications` 下的 `*.desktop` 文件，不要以为这只是一个为了在 launcher 中显示的文件。比如你删了File manager，你会发现系统设置中对应的 Super+E 快捷键失效；你删了 Text Editor 你会发现文本文件右键打开方式出了点问题；删除了 deepin-font-install 之后，就不能直接通过双击安装软件。当然也有一些真的放着碍眼，你就删掉吧，如果要恢复，使用 `apt install --reinstall <package-name>`
### bug修复
> [《Deepin 下解决Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=gasp》](https://blog.csdn.net/liuestcjun/article/details/53515589) 

deepin 15.5.1还存在这个bug，导致我不能使用 java 命令运行class文件。


> [《为英语环境下的日历添加农历支持》](https://bbs.deepin.org/forum.php?mod=viewthread&tid=154593&extra=page%3D1&page=2)
### 引导修复
见 [Repair grub](https://github.com/ExplosiveBattery/custom-deepin-linux/tree/master/Repair%20grub) 文件夹（信不信？不管你遇到过的还是没遇到过的）
### apt remove/purge 手贱 修复
- 方法一
````sh
cat /var/log/apt/history.log  # 查看 apt 操作历史
````
- 方法二
````sh
history # 查看所有命令操作历史
````
### 内核定制（好处，教程）
> [《内核定制 简单教程》](https://bbs.deepin.org/forum.php?mod=viewthread&tid=155660&extra=)
>
> [《LINUX KERNEL内核编译步骤》](http://smilejay.com/2011/05/linux_kernel_compilation/)

内核通常会涉及很多问题，比如合上笔记本盖子之后并且拔掉外置电源接口可能导致系统直接进入关机而不是待机，这个问题很奇特，我还原系统之后居然莫名其妙碰到，升级到最新的内核可以解决问题，但是因为最新的内核存在一些毛病又退回来，相当于重装原来版本的内核，最终问题得到了解决。  

### 开机动画
见 [plymouth](https://github.com/ExplosiveBattery/custom-deepin-linux/tree/master/plymouth) 文件夹  
### 切换桌面壁纸（脚本、内存泄露问题）
> [壁纸自动切换的内存泄漏问题](https://bbs.deepin.org/forum.php?mod=viewthread&tid=155474) 
### 动态桌面壁纸
> [火萤视频桌面](http://bbs.huoying666.com/portal.html) 
>
> [设置 Linux 动态桌面的几种办法](https://www.jianshu.com/p/d6ff45e983ce)
>
> [komorebi](https://github.com/iabem97/komorebi)
自己通过ffmpeg等工具截取、合成好看的视频文件  
几乎所有的动态壁纸都仅仅支持通过CPU来播放运行，估计是懒得写，而且Linux下GPU还不一定能够很好地使用，以及有些人是关闭了GPU来省电。
### 桌面美化
Conky 类似于 Windows下 的 [Rainmeter](https://www.rainmeter.net)。

具体教程见 [conky](https://github.com/ExplosiveBattery/custom-deepin-linux/tree/master/conky) 文件夹  
### 开机自启
包含开机应用自启动，比如说conky、壁纸自动切换脚本等的开机自启。

具体教程见 [autostart](https://github.com/ExplosiveBattery/custom-deepin-linux/tree/master/autostart) 文件夹。
### 配合conky的 [show desktop](https://github.com/ExplosiveBattery/deepin-desktop-toggle)
程序会读取 `~/.toggleconfig` 中的每一行作为一个顶级窗口名，如果存在同样的窗口名则不会被 [ShowDesktop](https://github.com/ExplosiveBattery/deepin-desktop-toggle)（win+d按键）影响。
### 钉宫语音包
> [《Deepin Linux【钉宫语音包】V2.0 死宅限定》](https://bbs.deepin.org/forum.php?mod=viewthread&tid=154264)

这个帖子楼主的包有点不完善，拉到后面有人发了比较好的。

类似的还有
> [《deepin替换elementaryOS音效》](https://bbs.deepin.org/forum.php?mod=viewthread&tid=134778)
### Launcher
想要给launcher自定义分类，从[代码](https://github.com/linuxdeepin/dde-launcher/search?utf8=%E2%9C%93&q=chat&type=)看出是写死的，而不是读取配置文件，很遗憾这种写法，如果要实现自定义分类就需要**重新编译**这个项目。
### 顶栏和任务栏
> [《社区软件-topbar alpha》](https://bbs.deepin.org/forum.php?mod=viewthread&tid=141816)
>
> [《找到dock栏彻底透明的办法了》](https://bbs.deepin.org/forum.php?mod=viewthread&tid=149493)

自定义dock大小：
> [deepin linux怎么调整dock栏图标大小](https://jingyan.baidu.com/article/f7ff0bfc3f49132e26bb1338.html) （还可以使用 gsettings 命令)

### 天气Dock插件
> [【准官方】天气Dock插件](https://bbs.deepin.org/forum.php?mod=viewthread&tid=156934)
### 应用图标定制
具体教程见 [icons](https://github.com/ExplosiveBattery/custom-deepin-linux/tree/master/icons) 文件夹
### 无线网卡工作模式
````sh
iwconfig # 显示无线网络设备信息
`````  
如果发现有一行 `Power Management:powersave` 说明网卡工作在低功耗，emmm，我觉得这可能是我信号不好的原因。
````sh
sudo iw dev wlp2s0 set power_save off # 关闭无线网卡省电模式
````  
> [《无线网卡用户如果觉得网速很慢，尝试这样修改一下》](https://bbs.deepin.org/forum.php?mod=viewthread&tid=166203)
### 省电
tlp、linux-cpupower 没准安装了之后是一种省电方式。
### 定时任务
`cron` 了解一下，相关命令
````
crontab -e # 编辑当前用户的 cron 配置
crontab -l # 列出当前用户的 cron 配置
````
### 自定义手势
手势有触摸板手势以及鼠标手势（相当于点着鼠标左键的触摸板手势）  
我幻想着能够在触摸板上画一个五角星，电脑就会关机....emmm...妥妥的利器  
触摸板手势：
- xSwipe（老旧系统使用）
- Fusuma
- Touchégg  

鼠标手势：
- Easystroke（类似于Windows中的 WGestures ）

我当时设置了Easystroke的五角星关机，每次关机...emmm....按着触摸板左键，再画一个五角星。
### 屏保 
推荐屏保主题：[cmatrix](https://github.com/abishekvashok/cmatrix)（装逼利器）
### 提高音质
> [《简单提升linux音效，实测有效》](https://bbs.deepin.org/forum.php?mod=viewthread&tid=137002)
### 主题  
- [Arc-theme](https://github.com/horst3180/Arc-theme)
- [365os](https://bbs.deepin.org/forum.php?mod=viewthread&tid=166024)
    （本主题已上架深度商店，可以直接在商店搜到）
- [adapta-gtk-theme](https://github.com/adapta-project/adapta-gtk-theme)  
- [Matcha](https://bbs.deepin.org/forum.php?mod=viewthread&tid=159620 ) 
### 替换最大化最小化按钮（仿mac）
> [《拿来主义，小白自己动手修改mac主题》](https://bbs.deepin.org/forum.php?mod=viewthread&tid=156341)
### 移动launcher中图标分类示例
使用文本编辑器改变 `/usr/share/applications/` 中对应 desktop 文件的 Categories 的值（[详细介绍请看这个](http://deepin.lolimay.cn/booklet/desktop.html#Categories-%E5%8F%82%E6%95%B0)）。 
如果安装的是 wine 应用，那么 desktop 文件在 `/usr/local/share/applications` 下 ，比如通过商店安装的百度云、迅雷。

如果上面改了 Categories 后还是没有改变，我们还要把这个 desktop 文件名字稍微改一下，如：
````sh  
sudo mv /usr/share/applications/edrawmax-zh.desktop /usr/share/applications/edrawmax.desktop
````
### 精简系统
可以删除系统多余的语言包，有几百MB大小  
````sh
sudo apt purge thunderbird # 卸载"雷鸟邮件"
sudo apt purge dde-introduction # 卸载"欢迎"
sudo apt purge deepin-manual # 卸载"深度帮助手册"
sudo apt puege deepin-clone # 卸载"深度备份还原工具"
sudo apt purge simple-scan # 卸载"扫描易"
````
这里只是提供一些建议，大家需要根据自己的实际需要确定该卸载的软件。

````sh
sudo rm /usr/share/applications/skype.desktop /usr/share/applications/dde-trash.desktop  /usr/share/applications/deepin-toggle-desktop.desktop /usr/share/applications/org.gnome.FileRoller.desktop /usr/share/applications/dde-computer.desktop /usr/share/applications/deepin-feedback.desktop  
````

我不支持deepin的flatpak，删了之后自己进应用市场搜deepin，去安装deb格式。
````sh
flatpak uninstall org.deepin.flatdeb.deepin-calculator org.deepin.flatdeb.deepin-calendar org.deepin.flatdeb.deepin-image-viewer org.deepin.flatdeb.deepin-music org.deepin.flatdeb.deepin-screen-recorder org.deepin.flatdeb.deepin-screenshot org.deepin.flatdeb.deepin-voice-recorder org.deepin.flatdeb.Base.Platform  
````
### 解决wireshark（dumpcap）的权限问题
````sh
sudo chmod 4755 /usr/bin/dumpcap
````

<p>&nbsp;</p><p>&nbsp;</p>

## 应用推荐
### Java
apt安装：https://www.cnblogs.com/diyunfei/p/7618508.html  
压缩包安装（推荐）：https://bbs.deepin.org/forum.php?mod=viewthread&tid=136610  
### gradle
压缩包安装： https://gradle.org/install/   
如果有Android Studio，可以通过这个IDE下载安装  
不推荐通过apt install 安装  
### docker
https://github.com/ExplosiveBattery/docker  
仓库中有两个pdf，一个是介绍docker，一个是如何安装docker，我的deepin安装使用了：  
`wget -qO- https://get.docker.com/ | sh` 一条命令解决  
`cat /etc/debian_version` 可以看到deepin15.5是debian 8.0 sid（也就是jessie unstable）  
### katoolin
https://github.com/LionSec/katoolin  
自己选择部分工具安装即可，不推荐使用kali docker，因为docker不适合会变动的东西，比如部分经常有更新的软件：metasploit，nmap  
### QT Creator
应用商店的软件常常版本落后，就需要我们自己去安装：  
`sudo apt-get install libgl1-mesa-dev build-essential`(见http://doc.qt.io/qt-5/linux.html 虽然不是编译安装，但是我发现还是需要libgl1-mesa-dev)   
接着安装指导：https://www.jianshu.com/p/afbc42ad2cfd  
下载页面：http://download.qt.io/archive/qt/  
最后记得编辑/etc/profile(或者~/.bashrc)，来实现每一次开机自动export Qt安装目录中的bin文件。或者自己一个个ln -s到/usr/bin目录中
- 注意：
- 选择安装的时候，QT Creator和Qt下的desktop gcc是必选的，后者会为你提供qmake等工具
- qmake: could not exec ‘/usr/lib/x86_64-linux-gnu/qt4/bin/qmake’: No such file or directory  
 https://blog.csdn.net/zhuquan945/article/details/52818786 </b>
### 思维导图
- Edraw MindMaster
- XMind
- mindmaps（网页版）
- 百度脑图（网页版）
### 流程图等
- Edraw Max
相当于 Windows 中的 visio，Edraw支持多方跨平台、值得付费。
### Office 办公
- [WPS](http://www.wps.cn/product/wpslinux)
- LibreOffice
- [UZER.ME](https://uzer.me) 在线办公平台
### 文件比较
- Beyond Compare
- VS code内置插件
- Meid
### 数据库连接工具
- datagrip
- Navicat
- dbeaver
### 笔记
- 为知笔记
- 有道云笔记
- 印象笔记
### Markdown
- [Cmd Markdown](https://www.zybuluo.com/mdeditor)
- [Typora](https://typora.io/#linux)
- [VS Code](https://code.visualstudio.com/)
- [Yu Writer Pro](https://ivarptr.github.io/yu-writer.site/index.html)
### 邮箱客户端
- Foxmail（wine模拟、耗电，所以如果脱离电源不要开着）
### 阅读器
- 福昕阅读器Foxit Reader
- Comix 漫画阅读器  
- 追书神器安卓版  

### 计算机网络学习
- [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer)
思科交换机模拟器
- [GNS3](https://www.gns3.com/)
### 输入法
- 搜狗输入法
- Rime
### 系统临时状态
专门用于使用真机进行自己内心不想进行的操作  
通过shell命令进入系统临时状态，并且记录其间shell的交互情况  【难度巨大，等待完成】
### BT系列
- 迅雷  
- [WebTorrent](https://webtorrent.io/desktop/) (BT流媒体播放器)  
### 驱动
Driver Manager  
支持SAMSUNG t5的使用：
````sh
sudo apt install exfat-utils exfat-fuse
````
### 分区
Gparted 这款软件还可以单独写到PE里面  
### 清理垃圾
deepin repair 一般一键备份前用一下，减少备份包体积  
wine 应用比如QQ，实时会产生垃圾，直接删除QQ账号文件夹，没事的（记得重要下载文件先移出来）。
### 系统配置文件更改
- dconf editor
````sh
sudo apt install dconf-editor -y # 安装 dconf系统配置编辑器
````
改桌面的各种配置都可以在这通过UI操作。
### 字体查看
font viewer
### 虚拟机
> [VMware Workstation Pro for Linux14.1.3](https://download.pchome.net/system/sysenhance/download-75584.html)
>
>[VMWare注册码](http://beikeit.com/post-513.html)
### 下载 YouToBe、bilibili 等视频
1. you-get
请自己安装好 pip3
````
pip3 install  you-get
````
但是有一个问题，pip3安装的you-get、scrapy等脚本程序都放到了~/.local/bin下，而这里还需要
````sh
sudo echo "export PATH=$PATH:$HOME/.local/bin" >> /etc/profile  
source /etc/profile  #但是这条命只是刷新当前shell，要完全范围生效还是重启，如果不想重启，我推荐直接export PATH=$PATH:$HOME/.local/bin顶一下 
````
2. youtube-dl  
  
> [M3U文件下载](https://www.zhihu.com/question/48914419)

- [Tide](https://github.com/yuhuili-lab/Tide)

## 常用快捷键

````
Shift+Win+左右方向键    在多任务视图中移动当前应用  
Win+方向上键            当前窗口最大化    
Win+方向下键            当前窗口正常化 
Win+= 与 Win+-          放大镜
Win+E                   文件管理器  
Win+W                   当前任务视图中的所有窗口  
Ctrl+Alt+T              终端  
````
能用常用快捷键打开的东西，就可以不必放在dock中