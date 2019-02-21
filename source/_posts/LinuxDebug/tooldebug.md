---
title: ubuntu常见工具问题debug
description: 音频播放解码器 音频播放声卡选择
toc: true
tag: ubuntu工具配置
---

# Ubuntu Video 无法播放mkv视频
安装 h.264 decoder
sudo apt-get install h264enc
sudo apt install gstreamer1.0-libav
安装完重启播放器即可

# Ubuntu音频播放没有声音
## 声卡和音量选择
**alsamixer** 可以选择各个声卡和音量  
安装并打开alsamixer工具可以使用图形界面查看每路音频的音量,对应查看需要调试的音频音量(耳机/喇叭).如果要调试耳机声音,可以查看Headphone的音量那里，如果显示Headphone off，很可能时耳机设置为静音了,请使用pavucontrol关闭耳机静音  
另外查看下声卡的选择,F6可以选择声卡,有时默认声卡选项是空的,这时手动选择为电脑使用的声卡也能解决有声音的问题
如果通过此工具设置完仍然没有声音的话，也请使用pavucontrol尝试调试

## 输出设备信息查看
**pavucontrol** 可以图形界面查看输入输出设备的详细情况
进入pavucontrol的窗口后,首先查看下OutPut Device栏,看一下当前音频输出设备的音量,注意查看下设备右侧有个小喇叭的图标,如果是选中的,那么意味着此设备当前是静音状态,这时点一下小喇叭图标就能解决
如果显示的输出设备与当前使用的不同,也可以在此栏手动选择正确的音频输出设备

# Ubuntu中文输入法/搜狗输入法配置
如果安装的Ubuntu版本默认不支持中文输入法,可以手动安装fcitx输入法架构,然后根据需要安装具体的中文输入法.
## 切换输入法框架
Ubuntu默认的输入法框架是ibus,如果要切换成fcitx(没有安装的话请手动apt-get install fcitx),需要使用X window的输入法框架管理器im-config
```
$im-config
```
然后通过图形界面一路把默认的ibus设置为fcitx  
## 安装中文输入法
比较常见的linux中文输入法有google输入法和搜狗输入法  
google输入法直接通过命令就能安装
```
$sudo apt-get install fcitx-googlepinyin
```
搜狗输入法需要到搜狗linux输入法的官网上下载安装包  
[搜狗输入法for linux](https://pinyin.sogou.com/linux/)
下载下来是个deb的包(版本号可能会有不同)  
sogoupinyin_2.2.0.0108_amd64.deb
下载结束以后直接使用dpkg安装即可(双击下载也可以)
```
$dpkg -i sogoupinyin_2.2.0.0108_amd64.deb
```
在输入法安装结束之后,需要重启一下fcitx，再右上角小键盘(那个小键盘就是fcitx和输入法的小图标)显示的地方右键，restart即可，重启可能要花一点时间

## 配置输入法
具体使用哪个输入法需要在fcitx 中设置,还是那个小图标,右键-> Config Current input method菜单里面+或-平时使用的输入法,也可以上下调整输入法切换顺序.  
一般一个English,一个中文输入法就够用了,设置以后如果不能用再restart一下.  
如果fcitx restart后不管是手动切换输入法还是用快捷键切换，都不生效的话,需要修改下home 下的.xprofile文件(如果没有请手动创建)  
在.xprofile文件添加如下内容：
```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```
然后保存，注销一下系统

再次尝试快捷键和鼠标选择输入法，中文输入正常
