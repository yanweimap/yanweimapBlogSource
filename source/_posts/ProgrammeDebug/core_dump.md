---
title: Core Dump问题
description: 编程问题
toc: true
tags: dump
---

# 错误类型
Segmentation fault (core dumped)
# 产生原因
Core Dump 核心转储（是操作系统在进程收到某些信号而终止运行时，将此时进程地址空间的内容以及有关进程状态的其他信息写出的一个磁盘文件。这种信息往往用于调试），其实“吐核”这个词形容的很恰当，就是核心内存吐出来.  

出现这种错误可能的原因（其实就是访问了内存中不应该访问的东西):  
1. 内存访问越界：  
  - 数组访问越界，因为下标出超出了范围。  
  - 搜索字符串的时候，通过字符串的结尾符号来判断结束，但是实际上没有这个结束符。  
  - 使用strcpy, strcat, sprintf, strcmp,strcasecmp等字符串操作函数，超出了字符中定义的可以存储的最大范围。使用strncpy, strlcpy, strncat, strlcat, snprintf, strncmp, strncasecmp等函数防止读写越界。  

2. 多线程程序使用了线程不安全的函数。  

3. 多线程读写的数据未加锁保护。  
对于会被多个线程同时访问的全局数据，应该注意加锁保护，否则很容易造成核心转储  

4. 非法指针
  - 使用NULL指针  
  - 随意使用指针类型强制转换，因为在这种强制转换其实是很不安全的，因为在你不确认这个类型就应该是你转化的类型的时候，这样很容易出错，因为就会按照你强制转换的类型进行访问，这样就有可能访问到不应该访问的内存。  

5. 堆栈溢出  
不要使用大的局部变量（因为局部变量都分配在栈上），这样容易造成堆栈溢出，破坏系统的栈和堆结构，导致出现莫名其妙的错误。

以上部分引自互联网,具体信息见 *参考*

# 实际碰到的问题
## 数组使用
**数组访问越界**  
刷剑指offer的时候遇到的，题目是:  
在一个字符串前面添加一部分字符，并且不使用其他空间  
原理简单讲就是计算好添加字符后的总长度，然后从后向前去写字符数组，这样不需要其他中间变量或者多次写数组
```
#include <stdio.h>

void add_array(char ori[],char add[]){
  int i = 0;
  int length_ori;
  int length_add;
  char *p;

  if(ori == NULL || add == NULL){
    return;
  }

  while(ori[i] != '\0') //计算原字符串的长度
    i++;
  length_ori = i;

  i = 0;
  while(add[i] != '\0') //计算添加字符的长度
    i++;
  length_add = i;

  p = &ori[length_ori+length_add];  //指针移动到新字符串末尾的位置
  *p = '\0';  //写入截至符
  p--;

  for(i=1;i<=length_ori;i++){ //写入原始字符串
    *p = ori[length_ori-i];
    p--;
  }
  for(i=1;i<=length_add;i++){ //写入添加字符串
    *p = add[length_add-i];
    p--;
  }

  return;
}

int main(){
  char ori[20] = "I am beautiful!";
  char add[] = "Absolutely,";
  char *later;

  add_array(ori,add);
  printf("%s\n",ori);

  return 0;
}
```

运行结果是字符串顺利打出来了，但是检测到错误强制中断了
```
$ ./addString.o
Absolutely,I am beautiful!
*** stack smashing detected ***: <unknown> terminated
Aborted (core dumped)
```

我就奇怪了，字符串操作没问题，为啥会出错，后面检查发现我在给字符串内容的时候光注意给内容了（想写点厉害的），结果忽视了字符串长度...  
字符数组初始化时给的空间太小了，完全没考虑到添加那部分的字符长度
```
char ori[20] = "I am beautiful!";
```
后面长度增加到50就好了

## 链表初始化
**非法指针(NULL)调用**

写单向链表时，链表头使用指针的指针去存，这样添加或者删除指针的时候就能通过提供头指针的地址而修改第一个节点
```
#include <stdio.h>
#include <stdlib.h>

typedef struct Node{
  int num;
  struct Node *next;
}node;

node * add_node(int num){
  node *nnode;

  nnode = (node *)malloc(sizeof(node));
  if(nnode == NULL)
    return NULL;
  nnode->num = num;
  nnode->next = NULL;

  return nnode;
}

void add_node_to_list(node **listhead,int num){
  node * nnode = NULL;
  node * current;

  if(*listhead == NULL){
    *listhead = add_node(num);
    return;
  }

  for(current = *listhead;current->next != NULL;current = current->next)
    ;
  current->next = add_node(num);

  return;
}

void print_list(node **listhead){
  node *current;

  if(*listhead == NULL)
    return;
  for(current = *listhead;current != NULL;current = current->next)
    printf("%d \t",current->num);
}

int main(){
  node **listhead;
  int num;

  while(scanf("%d",&num),num >0){
    add_node_to_list(listhead,num);
  }
  print_list(listhead);

  return 0;
}
```
运行时报错
```
Segmentation fault (core dumped)
```
后面才发觉是初始化listhead时就是空的，没给他初始化内容，后面直接*listhead,肯定会dump
所以初始化的时候需要像这样：
```
node **listhead;
node *head;
*listhead = head;
```
这样listhead指向的是存head的地址，就不会有问题了  
另外除了初始化，代码中还存在其他问题，比如打印的时候只判断了\*listhead是不是NULL,而实际上，**应该最先判断listhead是不是NULL**,然后再判断\*listhead
目标就是所有指针再使用前都需要先判断其是否NULL:  
```
if(listhead == NULL || *listhead == NULL)
  return;
```
如果listhead为NULL，**或** 条件就成立了，不会再判断后面\*listhead的值  


# 参考
错误原因部分引用自csdn网友博客内容:  
1.[Segmentation fault (core dumped) 错误的一种解决场景](https://www.cnblogs.com/vancasola/p/9951763.html)
