---
title: linux计算器和时间显示
description: bc gnome-calculator cal ncal calendar
toc: true
tag: 计算器/日历
---
# 计算器
## bc
bc 命令是命令行任意精度计算器语言，通常在linux下当计算器用。  
它类似基本的计算器, 使用这个计算器可以做基本的数学运算。  
支持加减乘除,指数(^),取余(%),开根(sqrt(n))等操作
### 支持参数
1. -i 强制交互模式
2. -l 指定标准数学库
3. -w 给POSIX bc的扩展发出警告
4. -s 准确处理POSIX bc语言
5. -q 不要打印正常的GNU bc welcome
### 常见用法
#### 交互模式
```
$bc
bc 1.07.1
Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006, 2008, 2012-2017 Free Software Foundation, Inc.
This is free software with ABSOLUTELY NO WARRANTY.
For details type `warranty'.
5*7
35
quit
```
#### echo+管道
```
$ echo "3+1" | bc
4
```
#### bc+文件名
进入bc交互式界面并自动返回文本内算式的结果
```
$ echo 5*9*4>testcal.txt
$ bc testcal.txt
bc 1.07.1
Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006, 2008, 2012-2017 Free Software Foundation, Inc.
This is free software with ABSOLUTELY NO WARRANTY.
For details type `warranty'.
180
```
#### 设置有效数字
scale=n
```
$bc -q
4/3
1
scale=10
4/3
1.3333333333
quit
```

#### 进制转换
使用变量 ibase表示转换前数字的数制；obase表示转换后数字的数制(obase最好放在前面)  
```
obase=16
ibase=10
128
80
obase=10
ibase=16
80
128
```

#### 输入表达式
可以在bc中输入表达式,bc接受变量计算
```
$ bc -q
a=(15-9)*5+(32-5)
b=(4+8)/6
a*2-b
112
```

#### 内置函数
1. sqrt(n) 开根  
2. s(X) 正弦
3. c(X) 余弦
4. a(X) 反正弦
5. l(N) 自然对数
6. e(N) 指数对数


## gnome-calculator
ubuntu默认图形界面计算器
支持多种计算模式,打开直接操作即可

# 日历
## cal
cal可以在终端显示本月的日历,当前日期高亮
![cal](https://img-blog.csdn.net/20170311192854659?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGlqaWExMTExMTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  
### 查看固定月的日历
默认不带参数是显示当前月的日历
如果要看某年某月的日历,直接在后面跟月 年
```
$ cal 2 2017
      二月 2017         
日 一 二 三 四 五 六  
          1  2  3  4  
 5  6  7  8  9 10 11  
12 13 14 15 16 17 18  
19 20 21 22 23 24 25  
26 27 28
```
### 查看全年的日历
也可以查看全年的日历,直接带年份就行
```
$ cal 2019
                            2019
         一月                    二月                    三月           
日 一 二 三 四 五 六  日 一 二 三 四 五 六  日 一 二 三 四 五 六  
       1  2  3  4  5                  1  2                  1  2  
 6  7  8  9 10 11 12   3  4  5  6  7  8  9   3  4  5  6  7  8  9  
13 14 15 16 17 18 19  10 11 12 13 14 15 16  10 11 12 13 14 15 16  
20 21 22 23 24 25 26  17 18 19 20 21 22 23  17 18 19 20 21 22 23  
27 28 29 30 31        24 25 26 27 28        24 25 26 27 28 29 30  
                                            31                    

         四月                    五月                    六月           
日 一 二 三 四 五 六  日 一 二 三 四 五 六  日 一 二 三 四 五 六  
    1  2  3  4  5  6            1  2  3  4                     1  
 7  8  9 10 11 12 13   5  6  7  8  9 10 11   2  3  4  5  6  7  8  
14 15 16 17 18 19 20  12 13 14 15 16 17 18   9 10 11 12 13 14 15  
21 22 23 24 25 26 27  19 20 21 22 23 24 25  16 17 18 19 20 21 22  
28 29 30              26 27 28 29 30 31     23 24 25 26 27 28 29  
                                            30                    

         七月                    八月                    九月           
日 一 二 三 四 五 六  日 一 二 三 四 五 六  日 一 二 三 四 五 六  
    1  2  3  4  5  6               1  2  3   1  2  3  4  5  6  7  
 7  8  9 10 11 12 13   4  5  6  7  8  9 10   8  9 10 11 12 13 14  
14 15 16 17 18 19 20  11 12 13 14 15 16 17  15 16 17 18 19 20 21  
21 22 23 24 25 26 27  18 19 20 21 22 23 24  22 23 24 25 26 27 28  
28 29 30 31           25 26 27 28 29 30 31  29 30                 


         十月                   十一月                   十二月           
日 一 二 三 四 五 六  日 一 二 三 四 五 六  日 一 二 三 四 五 六  
       1  2  3  4  5                  1  2   1  2  3  4  5  6  7  
 6  7  8  9 10 11 12   3  4  5  6  7  8  9   8  9 10 11 12 13 14  
13 14 15 16 17 18 19  10 11 12 13 14 15 16  15 16 17 18 19 20 21  
20 21 22 23 24 25 26  17 18 19 20 21 22 23  22 23 24 25 26 27 28  
27 28 29 30 31        24 25 26 27 28 29 30  29 30 31
```
### 查看全年中的天数
```$ cal -j -3
                         2019
            一月                           二月               
 日   一  二   三  四  五  六    日   一  二  三   四  五   六  
          1   2   3   4   5                        32  33  
  6   7   8   9  10  11  12    34  35  36  37  38  39  40  
 13  14  15  16  17  18  19    41  42  43  44  45  46  47  
 20  21  22  23  24  25  26    48  49  50  51  52  53  54  
 27  28  29  30  31            55  56  57  58  59          


            三月               
 日  一  二   三   四  五  六  
                     60  61  
 62  63  64  65  66  67  68  
 69  70  71  72  73  74  75  
 76  77  78  79  80  81  82  
 83  84  85  86  87  88  89  
 90
 ```

## ncal
类似cal,纵向日历
```
$ ncal
       二月 2019           
Sun      3 10 17 24   
Mon      4 11 18 25   
Tue      5 12 19 26   
Wed      6 13 20 27   
Thu      7 14 21 28   
Fri   1  8 15 22      
Sat   2  9 16 23
```
如果只想查看特定周几的日期，这个命令可能特别有用
```
$ ncal |grep Thu
Thu      7 14 21 28
```

## calendar
查看节日,但calendar有点像历史上的今天,显示世界上所有日期相关的重要事件
### 查看当天相关事件
-l 0表示仅看当天的节日
```
$ calendar -l 0
2月 17 	Federick Eugene Ives born, 1856, pioneer of halftone
2月 17 	Marion Anderson born, 1902
2月 17 	T. J. Watson, Sr. born, 1874
2月 17 	Death of Boromir
2月 17*	Parshat Terumah
2月 17 	Jazz great Thelonius Monk dies in Englewood, New Jersey, 1982
2月 17 	Prins Willem III (1817 - 1890)
2月 17 	N'oubliez pas les Alexis !
2月 17 	Donát
```
### 精简事件
但是这么查看也会把很多不相关和不关心的事件显示出来.可以通过修改配置文件精简过多的信息  
/usr/share/calendar/calendar.all  
比如把文件中多余的include注释掉  
```
#include <calendar.world>
//#include <calendar.argentina>
//#include <calendar.australia>
//#include <calendar.belgium>
//#include <calendar.birthday>
```
### 添加私人事件
**当然我们也可以通过calendar显示自己添加的事件,比如工作日历**  
比如在calendar.all配置文件中添加
```
#include <calendar.work>   
```
日历文件的格式非常简单 - mm/dd 格式日期，空格和事件描述  
```
$ cat calendar.work
03/26   Describe how the cal and calendar commands work
03/27   Throw a party!
```

参考:  
1.[【原创】linux命令bc使用详解](https://www.cnblogs.com/lovevivi/p/4359296.html)  
2.[linux BC命令行计算器](https://www.cnblogs.com/hrhguanli/p/4757059.html)
3.[在 Linux 命令行上使用日历](https://linux.cn/article-9576-1.html)
