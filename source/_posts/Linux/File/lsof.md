---
title: lsof命令 - List open files
toc: true
---

## lsof介绍
一个列出当前系统打开文件的工具  
在linux环境下，任何事物都以文件的形式存在，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。  
在终端下输入lsof即可显示系统打开的文件，因为 lsof 需要访问核心内存和各种文件，所以必须以 root 用户的身份运行它才能够充分地发挥其功能  

由于lsof是查看所有系统中打开的文件,内容太多,这里我们只查看前10行
```
$ sudo lsof |head -n 10
COMMAND   PID   TID   USER   FD  TYPE  DEVICE  SIZE/OFF       NODE NAME
systemd     1         root  cwd   DIR    8,6      4096          2 /
systemd     1         root  rtd   DIR    8,6      4096          2 /
systemd     1         root  txt   REG    8,6   1440088    1048949 /lib/systemd/systemd
systemd     1         root  mem   REG    8,6   1623216    1054267 /lib/x86_64-linux-gnu/libm-2.28.so
systemd     1         root  mem   REG    8,6    125128    1048765 /lib/x86_64-linux-gnu/libudev.so.1.6.11
systemd     1         root  mem   REG    8,6    129232    1054245 /lib/x86_64-linux-gnu/libgpg-error.so.0.24.3
systemd     1         root  mem   REG    8,6     43304    1054256 /lib/x86_64-linux-gnu/libjson-c.so.3.0.1
systemd     1         root  mem   REG    8,6     34872    2760995 /usr/lib/x86_64-linux-gnu/libargon2.so.1
systemd     1         root  mem   REG    8,6    432640    1054226 /lib/x86_64-linux-gnu/libdevmapper.so.1.02.1
```
各列信息意义:

|COMMAND|PID|TID|USER|FD|TYPE|DEVICE|SIZE/OFF|NODE|NAME|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|进程名|进程标识符|线程标识符|进程所有者|文件描述符|文件类型|磁盘名称?|文件大小|索引节点|文件完整名字|

## 关于文件描述符FD
**文件描述符，应用程序通过文件描述符识别该文件**  
 (1）**cwd**：表示current work dirctory，即：应用程序的当前工作目录，这是该应用程序启动的目录，除非它本身对这个目录进行更改  
（2）**txt** ：该类型的文件是程序代码，如应用程序二进制文件本身或共享库，如上列表中显示的 /sbin/init 程序  
（3）lnn：library references (AIX);  
（4）er：FD information error (see NAME column);  
（5）jld：jail directory (FreeBSD);  
（6）ltx：shared library text (code and data);  
（7）mxx ：hex memory-mapped type number xx.  
（8）m86：DOS Merge mapped file;  
（9）**mem**：memory-mapped file;  
（10）mmap：memory-mapped device;  
（11）pd：parent directory;  
（12）**rtd**：root directory;  
（13）tr：kernel trace file (OpenBSD);  
（14）v86  VP/ix mapped file;  
（15）0：表示标准输出  
（16）1：表示标准输入  
（17）2：表示标准错误  
**一般在标准输出、标准错误、标准输入后还跟着文件状态模式：r、w、u等**  
（1）u：表示该文件被打开并处于读取/写入模式  
（2）r：表示该文件被打开并处于只读模式  
（3）w：表示该文件被打开并处于  
（4）空格：表示该文件的状态模式为unknow，且没有锁定  
（5）-：表示该文件的状态模式为unknow，且被锁定  
**同时在文件状态模式后面，还跟着相关的锁**  
（1）N：for a Solaris NFS lock of unknown type;  
（2）r：for read lock on part of the file;  
（3）R：for a read lock on the entire file;  
（4）w：for a write lock on part of the file;（文件的部分写锁）  
（5）W：for a write lock on the entire file;（整个文件的写锁）  
（6）u：for a read and write lock of any length;  
（7）U：for a lock of unknown type;  
（8）x：for an SCO OpenServer Xenix lock on part of the file;  
（9）X：for an SCO OpenServer Xenix lock on the entire file;  
（10）space：if there is no lock.  

## 关于文件类型TYPE
TYPE：文件类型，如DIR、REG等，常见的文件类型  
（1）DIR：表示目录  
（2）CHR：表示字符类型  
（3）BLK：块设备类型  
（4）UNIX： UNIX 域套接字
（5）FIFO：先进先出 (FIFO) 队列  
（6）IPv4：网际协议 (IP) 套接字

## 常用参数
### 直接加文件名
```
$lsof /home/amy/tool/atom
```
显示打开此文件的条目  
### 进程相关
1. -c   COMMAND
```
$lsof -c atom
```
显示某个进程打开的所有文件   
2. -u   user  
```
$lsof -u root
```
显示所属某个用户的进程打开的文件  
```
$lsof -u ^root
```
显示除此用户以外的用户打开的文件  

-c和-u也可以一起用,过滤某个用户的某个进程相关文件  
```
$lsof -u root -c atom
```
3. -p PID
```
$lsof -p 1
```
显示PID相关文件情况  
```
$lsof -p 1,2,3
```
显示多个PID相关文件

### 文件名,目录
1. +D 根据文件目录递归显示
```
$lsof +D /home/amy/
```
递归的显示所有目录下的打开的文件  

2. +d 显示此目录下打开的文件,非递归
```
$lsof +d /home/amy/
```

### 文件类型
1. -i 显示所有网络连接
```
$lsof -i
```
-i tcp 显示所有TCP连接
```
$lsof -i tcp
```
-i udp 显示所有UDP连接
```
lsof -i udp
```
-i:port 显示端口号相关连接
```
$lsof -i:60976
```
-i tcp:port
```
$lsof -i tcp:60976
```
完整的-i 网络参数
```
$lsof -i [protocol][@hostname|hostaddr][:service|port]  
[protocol]: tcp/udp 4/6(ipv4/ipv6)
```

2. -d 根据文件描述符
```
$lsof -d cwd
```

3. -N 文件系统
```
$lsof –N
```

### 显示条件
1. -t 仅显示符合条件的进程号
```
$lsof -t -u amy
```
这样仅会显示用户amy相关进程的进程号,可以配合kill一起使用
```
$kill -9 `lsof -t -u amy`
$kill -9 `-i udp`
```
2. -a 匹配不同条件&
-a相当于是与的条件,可以联系不同的参数使用,如果默认没有a,那就是以或满足条件
```
$lsof -u root -a -p 68684
```

3. -r repeat模式,每几秒重复操作和显示
```
$lsof -i tcp -a -r5
```
每5s显示一次当前tcp连接



## 参考以下大佬总结
1.[使用 lsof 查找打开的文件](https://www.ibm.com/developerworks/cn/aix/library/au-lsof.html)
2.[linux lsof命令详解](https://www.cnblogs.com/sparkbj/p/7161669.html)  
3.[Linux lsof命令详解](https://www.cnblogs.com/ftl1012/p/9247223.html)  
4.[lsof命令总结](https://www.cnblogs.com/onmyway20xx/p/4425330.html)  
