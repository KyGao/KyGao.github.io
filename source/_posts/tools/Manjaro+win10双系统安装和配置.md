---
title: Manjaro+Win10双系统安装和配置
date: 2020-01-30 20:30:43
tags: [tools,manjaro,kde]
abbrlink: manjaro_win10
categories: 
- tools
---

距离安装双系统已经过了好久...终于过来填这个坑了。在填坑之前，还是想感谢一下联想拯救者，想当初我就是因为在Ubuntu上一直和网卡驱动过不去，查了各种解决方法也解决不了问题，当然还是有收获的，就是知道了大多数遇到这个问题的都是拯救者电脑。所以就一直拖拖拖，大三的寒假才有时间重新上手linux。那么感谢拯救者的原因是什么呢？就是让我发现了宝藏manjaro，虽然manjaro对于显卡驱动的管理已经非常优质了，但可惜我还是薅不动nvidia，以后再说吧。
<!--more-->
下面开始正题：

## 安装
安装这块其实网上教程非常详细了，建议看底下放的B站链接，照着来就好了，我装的是KDE，感觉很棒。
#### U盘刻录
听说好像是新版的rufus没有dd模式刻录这个弹出了，当时也是折腾了挺久，最后用的USBWriter-1.3刻录的，没有问题
#### 分区
因为aur源下载的东西好像是安装到/opt目录下的，然后当时看各种教程，分区特别细，所以只分给了opt4个G的磁盘空间，所以装一个anaconda就满了... 最后还是重装系统，512M给efi分区，其他100多个G全给了根目录/，真香。

#### 语言选择
英文真的看的懂，还少了很多bug，安装时闪退的可以试试这个。



## 配置
#### 快捷键
> 注：meta指的是windows那个标标

 **alt+space**: search
 **meta+space** : 下拉式终端Yakuake (原本是F12的，建议换成这个，和search搭配)
 **ctrl+alt+t**：konsole
建议多去逛逛settings，算是初步了解一下manjaro
- 可以在输入输出设备中修改成双击确认和touchpad的垂直滚动，更符合人的习惯    
#### 下载
- 下载工具：yay
    可以替代pacman（下载AUR中的东西）：`sudo pacman -Sy yay`
- 更换各种源
    1. 国内镜像源
       `sudo pacman-mirrors -i -c China -m rank` 然后选中想要的源（清华中科大上交都可以）
	2. 软件源
    `./etc/pacman.conf`中添加：
    ```
    [archlinuxcn]
    SigLevel = Optional TrustedOnly
    Server = https://mirrows.utsc.edu.cn/archlinuxcn/$arch
    ```
    然后修改软件源配置
    `sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring`
- 更新一下系统（这个操作常用）
  `sudo pacman -Syy`

- 输入法 
  `/home/hugvgngj/.xprofile`中添加：

  ```
  - export GTK_IM_MODULE=fcitx
    export QT_IM_MODULE=fcitx
    export XMODIFIERS=@im=fcitx 
  ```
  然后安装fcitx
  ```cmd
  yay -Sy fcitx-im
  yay -Sy fcitx-configtool
  yay -Sy fcitx-googlepinyin
  ```
  可能重启还是没变，需要到输入法的config里去找

- 安装各种软件
  1. **chrome**: `yay -Sy google-chrome`
  2. **Tim**：对于折腾了很久还是装不上tim的小伙伴们可以看这里：
      反正最后只是想用tim传点东西，聊天什么的都在手机上，所以能满足基本功能个就行
	所以看这里：[github链接](https://github.com/askme765cs/Wine-QQ-TIM)，下载即用
  3. **VS code**：看[这里](https://www.jianshu.com/p/f033c52f5f3f)
  4. **anaconda**：看[这里](https://www.cnblogs.com/yangruiGB2312/p/9004335.html)
	（我是直接在pacmac应用商店下载的）
      对了，当时一直找不到navigator，后来才发现要进入自己的环境之后才可以用命令`anaconda-navigator`打开熟悉的gui界面
     
      

很好的值得参考的博客：
1. 首推B站上有个安装系统的讲解，对小白真的很有用：
2. [将干净的 Manjaro 快速配置为工作环境](https://blog.triplez.cn/manjaro-quick-start/#i-5)
3. [Manjaro折腾笔记：我的数据科学环境搭建之路](https://www.cnblogs.com/yangruiGB2312/p/9004335.html)
4. [Manjaro Linux + KDE 安装使用手记](https://queensferry.coding.me/2018/06/22/Manjaro-Linux-KDE-%E5%AE%89%E8%A3%85%E4%BD%BF%E7%94%A8%E6%89%8B%E8%AE%B0/)