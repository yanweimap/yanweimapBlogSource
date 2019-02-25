---
title: Linux包管理器
description: apt apt-get apt-cache dpkg yum dnf npm
toc: true
---
# 包管理器介绍
每个linux发行版都有自己的软件包管理器,软件通常都是存放在存储库中，并通过包的形式进行分发。处理包的工作被称为包管理。包提供了操作系统的基本组件，以及共享的库、应用程序、服务和文档。  
大多数包系统都是围绕包文件的集合构建的。包文件通常是一个存档文件，它包含已编译的二进制文件和软件的其他资源，以及安装脚本。包文件同时也包含有价值的元数据，包括它们的依赖项，以及安装和运行它们所需的其他包的列表  
如下是常见linux发行版使用的包管理器:

|操作系统|软件包格式|包管理器|
|:--:|:--:|:--:|
|Ubuntu|.deb|apt, apt-cache, apt-get, dpkg|
|Debian|.deb|apt, apt-cache, apt-get, dpkg|
|CentOS|.rpm|yum|
|Fedora|.rpm|dnf|
|FreeBSD|Ports, .txz|make, pkg|
Debian 及其衍生版，如 Ubuntu、Linux Mint 和 Raspbian，它们的包格式是 .deb。APT 这款先进的包管理工具提供了大多数常见的操作命令：搜索存储库、安装软件包及其依赖项，并管理升级。在本地系统中，我们还可以使用 dpkg 程序来安装单个的 deb 文件，APT 命令作为底层 dpkg 的前端，有时也会直接调用它  
最近发布的 debian 衍生版大多数都包含了 apt 命令，它提供了一个简洁统一的接口，可用于通常由 apt-get 和 apt-cache 命令处理的常见操作。这个命令是可选的，但使用它可以简化一些任务  
CentOS、Fedora 和其它 Red Hat 家族成员使用 RPM 文件。在 CentOS 中，通过 yum 来与单独的包文件和存储库进行交互  
在最近的 Fedora 版本中，yum 已经被 dnf 取代，dnf 是它的一个现代化的分支，它保留了大部分 yum 的接口  
FreeBSD 的二进制包系统由 pkg 命令管理。FreeBSD 还提供了 Ports 集合，这是一个存在于本地的目录结构和工具，它允许用户获取源码后使用 Makefile 直接从源码编译和安装包  

# apt
## apt 介绍
apt可以理解为简化的apt-get,具有更精减但足够的命令选项，而且参数选项的组织方式更为有效。除此之外，它默认启用的几个特性对最终用户也非常有帮助。例如，可以在使用 apt 命令安装或删除程序时看到进度条。  
## apt 常见用法
```
$ apt install   	#安装软件包
$ apt remove 	  #移除软件包
$ apt purge 	 	#移除软件包及配置文件
$ apt update 	 	#刷新存储库索引
$ apt upgrade 	 	#升级所有可升级的软件包
$ apt autoremove 	 	#自动删除不需要的包
$ apt full-upgrade 	 	#在升级软件包时自动处理依赖关系
$ apt search 	 	#搜索应用程序
$ apt show      #显示安装细节
$ apt list    	#列出包含条件的包（已安装，可升级等）
$ apt edit-sources 	   #编辑源列表
```
# apt-get
## atp-get 介绍
Ubuntu系统使用apt-get作为包管理器,apt-get适用于deb包管理，主要用于自动从互联网的软件仓库中搜索、安装、升级、卸载软件或操作系统  
## apt-get相关的配置文件
1. /etc/apt/sources.list  
apt-get获取包位置的软件源站点
2. /var/cache/apt/archives  
用 apt-get install 安装软件时，软件包的临时存放路径
3. /var/lib/dpkg/available  
软件包的描述信息, 该软件包括当前系统所使用的安装源中的所有软件包,其中包括当前系统中已安装的和未安装的软件包.
4. /var/lib/apt/lists
apt-get update命令会从/etc/apt/sources.list中下载软件列表，并保存到该目录

## apt-get 常见用法
```
$ apt-get install packagename   #安装一个新软件包  
$ apt-get -f install   #修复安装  
$ apt-get install package - - reinstall   #重新安装包
$ apt-get remove packagename   #卸载一个已安装的软件包（保留配置文档）  
$ apt-get remove --purge packagename   #卸载一个已安装的软件包（删除配置文档）  
$ apt-get autoremove packagename   #删除包及其依赖的软件包  
$ apt-get autoremove --purge packagname   #删除包及其依赖的软件包+配置文件，比上面的要删除的彻底一点  
$ apt-get clean   #删除下载的存档文件
$ apt-get autoclean    #删除旧的下载档案文件
$ apt-get update   #检索新的包列表  
apt-get update命令会扫描每一个软件源服务器，并为该服务器所具有软件包资源建立索引文件，存放在本地的/var/lib/apt/lists/目录中。 使用apt-get执行安装、更新操作时，都将依据这些索引文件，向软件源服务器申请资源
$ apt-get upgrade   #将系统中的所有软件包一次性升级到最新版本  
可以很方便的完成在相同版本号的发行版中更新软件包。在依赖关系检查后，命令列出了目前所有需要升级的软件包，在得到用户确认后，便开始更新软件包的下载和安装。当然，apt- get upgrade命令会在最后以合理的次序，安装本次更新的软件包。系统更新需要用户等待一段时间。
$ apt-get dist-upgrade   #升级系统
$ apt-get check    #验证没有损坏的依赖项  
$ apt-get source    #下载源档案
$ apt-get download   #将二进制包下载到当前目录中
$ apt-get changelog   #下载并显示给定包的更改日志
```

# apt-cache
## apt-cache 介绍
apt-cache是apt软件包管理工具，它可查询apt的二进制软件包缓存文件。APT包管理的大多数信息查询功能都可以由apt-cache命令实现,通过apt-cache命令配合不同的子命令和参数的使用,可以实现查找,显示软件包信息及包依赖关系等功能
## apt-cache 常见用法
```
$ apt-cache show packagename   #显示指定软件包的信息，包括版本号，安装状态和包依赖关系等.
$ apt-cache search packagename   #搜索软件包，可以按关键字查找软件包,通常用于查询的关键字会使用软件包的名字或软件包的一部分
$ apt-cache showpkg packagename   #显示命名包的包记录,包括版本,完整包名(包含镜像地址),依赖等
$ apt-cache depends packagename   #显示一个包含每个依赖关系的列表,以及可以实现该依赖关系的所有其他可能的包
$ apt-cache rdepends packagename   #反向查询当前包(lib)被哪些软件包依赖
$ apt-cache policy packagename    #显示软件包版本信息和下载源镜像地址
$ apt-cache dump   #显示缓存中的每个软件包的简要描述信息(信息很多,建议重定向到文件中查看)
$ apt-cache unmet   #显示不符合一致性的依赖关系
```

# dpkg
## dpkg 介绍
dpkg用来安装,删除,构建和管理deb软件包  
dpkg不像apt 或者apt-get可以直接匹配远程软件镜像源自动下载软件包,只能安装和配置本地用户已经下载好的deb软件包

## dpkg 常见用法
```
$ dpkg -i packagewholename.deb   #安装软件包
$ dpkg -r packagename   #删除软件包(保留配置)
$ dpkg -P packagename   #删除软件包(不保留配置)
$ dpkg -s packagename   #显示软件包相关信息(版本号,安装大小,依赖,描述,主页等)
$ dpkg -L packagename   #列出软件包所有相关文件
$ dpkg -l packagename   #显示软件包版本和软件描述
$ dpkg -c packagewholename.deb   #查看deb包中的内容
$ dpkg -unpack packagewholename.deb   #解出deb包中的内容  
更多详细用法请查看 参考-3  
```

# yum
## yum 介绍
Yum(全称为 Yellow dogUpdater, Modified)是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。  
## yum 常见用法
```
$ yum install packagename 	 #安装某个软件包
$ yum install package1 package2 ... 	#安装所有列出来的包
$ yum install -y package 	  #在 yum 提示是否继续的地方直接默认 yes
$ yum install package.rpm   #直接安装本地软件包
$ yum update    #更新已安装的包
$ yum remove packagename    #删除已安装的包
$ yum search search_string   #通过关键字搜索软件包名
$ yum search all search_string   #通过关键字搜索软件包所有信息,包括描述
$ yum info packagename   #查看软件包的信息
$ yum deplist packagename   #列出包的依赖
```

# dnf
## dnf 介绍
dnf是新一代的 rpm 软件包管理器，首先出现在 Fedora 18 这个发行版中，Fedora 22 中正式取代了 yum。dnf 包管理器相对 yum 来说，提升了用户体验、内存占用、依赖分析、运行速度等多方面的内容。dnf 使用 RPM, libsolv 和 hawkey 库进行包管理操作
dnf有很多指令跟yum都类似,只有少量有区别
## dnf 常见用法
```
$ dnf install packagename 	 #安装某个软件包
$ dnf install package1 package2 ... 	  #安装所有列出来的包
$ dnf install -y package 	  #在 dnf 提示是否继续的地方直接默认 yes
$ dnf install package.rpm   #直接安装本地软件包
$ dnf upgrade    #更新已安装的包
$ dnf erase package    #删除已安装的包
$ dnf search search_string   #通过关键字搜索软件包名
$ dnf search all search_string   #通过关键字搜索软件包所有信息,包括描述
$ dnf info package   #查看软件包的信息
$ dnf repoquery --requires package   #列出包的依赖
```

# npm
## npm 介绍
npm是JavaScript的开源软件包管理工具,它允许用户从npm服务器上下载并安装第三方的包或者程序到本地使用,同时也允许用户上传自己编写的包或程序到npm服务器分享给其他人.  
关于npm的使用可以直接查看npm官方中文文档
[npm 中文文档](https://www.npmjs.cn/)
## 安装 npm
linux默认没有npm组件,需要手动安装
```
$sudo apt-get install nodejs    #安装nodejs
$sudo apt-get install npm    #安装npm
```
安装完以后使用-v指令确认是否安装成功
```
$ node -v
v8.10.0
$ npm -v
3.5.2
```
## npm 常见用法
```
$ npm install <Module Name>   #直接安装模块在当前目录的node_modules
$ npm list -g    #查看全局安装模块
$ npm list <Module Name>   #查看某个模块的版本号
$ npm uninstall <Module Name>    #删除模块
$ npm update <Module Name>    #更新模块
$ npm search <Module Name>    #搜索模块
```

## package.json 配置文件
每个模块都有自己的package.json文件,在自己的包目录下,用于定义包的属性
相关的属性说明如下:
```
1. name - 包名  
2. version - 包的版本号
3. description - 包的描述
4. homepage - 包的官网 url
5. author - 包的作者姓名
6. contributors - 包的其他贡献者姓名
7. dependencies - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下
8. repository - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上
9. main - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js
10. keywords - 关键字
```

当然也可以使用package.json管理依赖包,这里可以参考hexo的package.json文件
```
{
  "name": "Amy-site",
  "version": "1.0.0",
  "private": true,
  "hexo": {
    "version": "3.8.0"
  },
  "dependencies": {
    "git": "^0.1.5",
    "gulp": "^3.9.1",
    "gulp-clean-css": "^3.9.0",
    "gulp-htmlclean": "^2.7.15",
    "gulp-htmlmin": "^3.0.0",
    "gulp-imagemin": "^3.4.0",
    "gulp-uglify": "^3.0.0",
    "hexo": "^3.7.0",
    "hexo-asset-image": "0.0.3",
    "hexo-autonofollow": "^1.0.1",
...
```
如果要添加模块到当前环境,可以直接追加模块名和版本号到**dependencies**部分,保存文件后,在当前目录运行
```
$npm install
```
npm就能自动检索需要的module,安装到node_modules下  

**参考以下大佬帖子:**
1. [【Linux】- apt-get命令 ](https://www.cnblogs.com/wangwust/p/9767892.html)  
2. [linux apt-cache使用方法](https://www.cnblogs.com/zzg-home/p/8032807.html)  
3. [ubuntu下的dpkg用法](https://blog.csdn.net/wanghuohuo13/article/details/78916821)  
4. [dpkg命令的用法](https://blog.csdn.net/yang3572/article/details/80991108)  
5. [NPM 使用介绍](http://www.runoob.com/nodejs/nodejs-npm.html)
6. [Linux 包管理基础：apt、yum、dnf 和 pkg ](https://linux.cn/article-8782-1.html)
7. [Linux中apt与apt-get命令的区别与解释](https://blog.csdn.net/maizousidemao/article/details/79859669)
8. [yum 命令讲解](https://blog.csdn.net/shuaigexiaobo/article/details/79875730)
