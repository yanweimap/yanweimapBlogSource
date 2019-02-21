---
title: Linux打包压缩相关命令
description: gzip bzip2 xz tar命令
toc: true
tag: 打包压缩
---
# gzip
## 压缩
gzip -v filename
```
$ gzip -v ker.log
ker.log:     79.1% -- replaced with ker.log.gz
```

注意 gzip默认压缩完会删除原文件

## 解压缩
gzip -d filename(.gz)
```
$ gzip -d ker.log.gz
```

同gzip压缩指令，解压缩后也会删除原压缩文件

## 用最佳的压缩比压缩,并保留原本的文件
```
$ gzip -9 -c ker.log > ker.log.gz
$ ls ker*
ker.log  ker.log.gz
```

如果压缩原文件是文本文件，可以直接用zcat查看
**用zgrep可以过滤字符串**
```
$ zgrep -n 'wcnss' ker.log.gz
```
还有zmore zless 也都可以使用

# bzip2
压缩比 比gzip还要高，用法几乎相同
## 压缩
bzip2 -v filename
```
$ bzip2 -v ker.log
  ker.log:  5.298:1,  1.510 bits/byte, 81.13% saved, 197622 in, 37299 out.
$ ls ker.log.bz2
ker.log.bz2
```

## 解压缩
bzip2 -d filename（.bz2）
```
$ bzip2 -d ker.log.bz2
$ ls ker.log
ker.log
```

## 保留源文件压缩
```
$ bzip2 -9 -c ker.log > ker.log.bz2
$ ls ker.log*
ker.log      ker.log.bz2  
```

**同样可以使用的字符查看，过滤命令**
bzcat  bzmore  bzless  bzgrep

# xz
使用方法同gzip和bzip2，压缩比更高
```
$ ls -hl ker.log*
-rw-r--r-- 1 android android 193K  5月 16 11:57 ker.log
-rw-rw-r-- 1 android android  37K  6月 21 16:14 ker.log.bz2
-rw-rw-r-- 1 android android  40K  6月 21 16:20 ker.log.gz
-rw-rw-r-- 1 android android  33K  6月 21 16:21 ker.log.xz
```
总结看来,三种压缩方式的压缩比的区别(按压缩文件比例排序): **gz<bz2<xz**
# tar
仅打包或打包并压缩
## 选项与参数
-c :创建打包文件,可搭配 -v 来察看过程中被打包的文件名(filename)  
-t :察看打包文件的内容含有哪些文件名,重点在察看“文件名”就是了;  
-x :解打包或解压缩的功能,可以搭配 -C (大写) 在特定目录解开特别留意的是, -c, -t,-x 不可同时出现在一串命令行中。  
-z :通过 gzip 的支持进行压缩/解压缩:此时文件名最好为 \*.tar.gz  
-j :通过 bzip2 的支持进行压缩/解压缩:此时文件名最好为 \*.tar.bz2  
-J :通过 xz的支持进行压缩/解压缩:此时文件名最好为 \*.tar.xz    特别留意, -z, -j,-J 不可以同时出现在一串命令行中  
-v :在压缩/解压缩的过程中,将正在处理的文件名显示出来!  
-f filename:**-f后面要立刻接要被处理的文件名!建议 -f 单独写一个选项啰!(比较不会忘记)**  
-C 目录 :这个选项用在解压缩,若要在特定目录解压缩,可以使用这个选项。  

## 打包并压缩
打包并压缩后的文件一般称为tarball  
```
$tar -czv -f compress_file.tar.gz filename
$tar -cjv -f compress_file.tar.bz2 filename
$tar -cJv -f compress_file.tar.xz filename
```
这里注意压缩后文件名都有加tar.* 主要用于区分是纯压缩（gzip，bzip2，xz）还是打包压缩（tar）

## 解压
```
$tar -xzv -f compress_file.tar.gz -C 解压目录（如果不用-C指向解压目录，默认加压到当前目录）
$tar -xjv -f compress_file.tar.bz2
$tar -xJv -f compress_file.tar.xz
```

## 打包
**打包后的文件一般称为tarfile**
不带压缩参数即可
```
$tar -cvf package_file.tar filename
```

## 拆包
```
$tar -xvf package_file.tar
```
