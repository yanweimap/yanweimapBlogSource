---
title: 5 ways to kill process
toc: true
---
# kill
## 命令格式
kill[参数][进程号]
## 命令功能
发送指定的信号到相应进程。不指定信号将发送SIGTERM（15）终止指定进程。如果仍无法终止该程序可用“-KILL” 参数，其发送的信号为SIGKILL(9) ，将强制结束进程，使用ps命令或者jobs 命令可以查看进程号。root用户将影响用户的进程，非root用户只能影响自己的进程。  
也就是说kill并不等于杀死进程,它只是向进程发送一个信号,除了强制终止(9 SIGKILL),其他信号进程都可以选择响应或者忽略
## 信号类型
-l 参数可以查看kill支持的所有信号
```
$ kill -l
 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP
 6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
63) SIGRTMAX-1	64) SIGRTMAX
```
也可以通过信号类型反向查找其编号(编号查找信号类型也可以)
```
$ kill -l KILL
9
$ kill -l 15
TERM
```
默认不带信号编号参数的话,缺省为15,即终止进程  
进程捕获此信号时，进程在退出之前可以清理并释放资源  
-9信号使进程强行终止，这常会带来一些副作用，如数据丢失或者终端无法恢复到正常状态。发送信号时必须小心，只有在万不得已时，才用kill信号(9)，因为进程不能首先捕获它。  
要撤销所有的后台作业，可以输入kill 0。因为有些在后台运行的命令会启动多个进程，跟踪并找到所有要杀掉的进程的PID是件很麻烦的事。这时，使用kill 0来终止所有由当前shell启动的进程，是个有效的方法。

**常用的信号**：  
HUP    1    终端断线  
INT     2    中断（同 Ctrl + C）  
QUIT    3    退出（同 Ctrl + \）  
TERM   15    终止  
KILL    9    强制终止  
CONT   18    继续（与STOP相反， fg/bg命令）  
STOP    19    暂停（同 Ctrl + Z）  
## 命令参数
1. -l (l为数字)直接发送信号编号给进程
```
$kill -9 32253
```
2. -s signame 发送信号名/类型给进程
```
$kill -s SIGTERM 32253
```
# killall
## 命令格式
killall [ -Z CONTEXT ] [ -u USER ] [ -y TIME ] [ -o TIME ] [ -eIgiqrvw ] [ -s SIGNAL | -SIGNAL ] NAME...  
## 命令功能
killall命令用于向指定命令(进程)发送信号,如果未指定具体信号,则发送SIGTERM  
如果killall杀死了至少一个进程,会返回0
如果killall没有匹配到任何一个-u或-Z的command,那么返回非0值
## 信号类型
可以使用-l查看支持的信号
```
$ killall -l
HUP INT QUIT ILL TRAP ABRT BUS FPE KILL USR1 SEGV USR2 PIPE ALRM TERM STKFLT
CHLD CONT STOP TSTP TTIN TTOU URG XCPU XFSZ VTALRM PROF WINCH POLL PWR SYS
```
**但killall -l不像kill可以直接查询匹配signal和ID**  
## 命令参数
1. -e 匹配完整command名
2. -I 忽略大小写
3. -g 终止group相关process
4. -i 交互模式,杀死进程前先询问用户
5. -o 仅匹配时间较早(更老)的进程
6. -r 根据正则表达式匹配进程名
7. -s 直接发送信号名而不是信号ID
8. -u 根据用户user去kill
9. -v 打印verbose log,报告信号是否成功发送
10. -w 等待所有被杀死的进程死亡。如果任何被杀死的进程仍然存在，则killall每秒检查一次，如果没有剩余进程则仅返回。  
**如果信号被忽略，没有效果，或者进程处于僵尸状态，killall可能会永远等待。**  
11. -y 仅匹配时间较晚的进程
12. -Z (仅限SELinux)指定安全上下文：仅删除具有与给定扩展常规表达式匹配的安全上下文的进程。必须在命令行上的其他参数之前。命令名称是可选的
```
$killall -KILL nginx
 同
$killall -9 nginx
```
# killall5(仅做介绍)
## 命令功能
killall5是SystemV killall命令。 它向除内核线程和其自身会话中的进程之外的**所有进程**发送信号，因此它不会终止运行调用它的脚本的shell。 它的主要（仅）用途是在/etc/init.d目录中的rc脚本中。

## 命令参数
-o 忽略进程ID  

# pkill
## 命令格式
pkill [options] pattern
## 命令功能
类似killall,根据进程名去发送信号
pkill会将指定的信号（默认为SIGTERM）发送到每个进程，而不是将它们列在stdout上。
## 命令参数
1. -signal 定义要发送到每个匹配进程的信号。可以使用数字或符号信号名称  
2. -f 匹配完整进程名
3. -g 仅匹配列出的进程组ID中的进程。进程组0被翻译成pgrep或pkill自己的进程组。  
4. -G 仅匹配列出实际组ID的进程。可以使用数值或符号值。  
5. -i 匹配进程不区分大小写。
6. -n 仅选择最新（最近启动）的匹配流程
7. -o 仅选择最早（最近最少启动）的匹配流程  

pkill参数比较多,需要使用的话请直接查看man pkill  
**pkill可以结合pgrep一起使用**
```
$pgrep atom
2948 atom
2950 atom
2952 atom
2981 atom
3003 atom
3107 atom
$pkill atom
```
先使用pgrep查看所有进程相关pid,然后再kill  
# xkill
## 命令格式
xkill [-display displayname] [-id resource] [-button number] [-frame] [-all] [-version]

## 命令功能
Xkill是一个强制X服务器关闭客户端连接的实用程序。 此程序非常危险，但对于中止在用户屏幕上显示不需要的窗口的程序非常有用。 如果没有给出-id的资源标识符，则xkill将显示一个特殊光标作为用户选择要杀死的窗口的提示。 如果在非根窗口上按下指针按钮，则服务器将关闭与创建窗口的客户端的连接。  

## 命令参数
1. -display 此选项指定要联系的X服务器的名称。  
2. -id 此选项指定要中止其创建者的资源的X标识符。如果没有指定资源，xkill将显示一个特殊的光标，您应该使用该光标选择要杀死的窗口。  
3. -button 此选项指定在选择要杀死的窗口时应使用的指针按钮数。如果指定了单词“any”，则可以使用指针上的任何按钮。默认情况下，使用指针映射中的第一个按钮（通常是最左边的按钮）。  
4. -all 此选项表示应杀死屏幕上具有顶级窗口的所有客户端。 Xkill会要求您选择每个当前定义按钮的根窗口，以便为您提供多次中止的机会。强烈建议不要使用此选项。  
5. -frame 此选项表示xkill应忽略查找顶级客户端窗口的标准约定（通常嵌套在窗口管理器窗口中），并且只是认为您要杀死根的直接子级。

参考:  
1.[4 Ways to Kill a Process – kill, killall, pkill, xkill](https://www.thegeekstuff.com/2009/12/4-ways-to-kill-a-process-kill-killall-pkill-xkill/)  
2.[linux kill命令详解](https://www.cnblogs.com/wangcp-2014/p/5146343.html)  
3.[pkill详解](http://www.mamicode.com/info-detail-2315063.html)
