---
title: 斐波那契函数
description: 剑指offer习题
toc: true
tags: Fibonacci
---
# Fibonacci
## 斐波那契函数:  
![fibo_fun](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570788251168&di=faa34a1c7f545dc68c64badaa7c97708&imgtype=0&src=http%3A%2F%2Fs13.sinaimg.cn%2Fmw690%2F001x0xHQzy7gxv0G9Puac%26690 "Fibonacci function")  

一般也用旋转叠加的图描述
![fibo_image](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570788408034&di=f29fbdad6ec22346118d207c335cc79a&imgtype=0&src=http%3A%2F%2Fi0.qhimg.com%2Ft01b927297ba2b6fe56.png "图形化的斐波那契")

## 迭代算法
迭代算法是比较常见的计算方式,根据Fibonacci的函数很容易就能想到
从n向前迭代,直到0和1
```
long long fibonacci_iteration(unsigned n){  
  if(n == 0)
    return 0;
  if(n == 1)
    return 1;

  return fibonacci_iteration(n-1) + fibonacci_iteration(n-2);  
}
```
但是递归的方式会导致重复计算,计算n需要先计算(n-1)和(n-2),计算(n-1)需要先计算(n-2)和(n-3)  
这样(n-2)的值就至少计算了2次,n越小计算的次数越多,这样就消耗了过多的资源,数字很大的情况会特别慢

## 循环算法
循环计算同一个值仅计算一次,就不会浪费多余资源
```
long long fibonacci_cycle(unsigned n){
  long long f_1, f_2, f_n;
  unsigned index;

  n_1 = 0;
  n_2 = 1;
  if(n == 0){
    return 0;
  }else if(n == 1){
    return 1;
  }

  index = 2;
  while(index <= n){
    f_n = f_1 + f_2;
    f_1 = f_2;
    f_2 = f_n;
    index++;
  }
  return f_n;
}
```

## 斐波那契题目
实际上一般不会直接说明是斐波那契,而是包成应用题的样子,让实现者自行判断
**另外求的f(n)可能是个很大的值,所以不要随便用int,可以使用long long**
### 求和并取余
斐波那契数列大家都非常熟悉。它的定义是：

f(x) = 1 .... (x=1,2)  
f(x) = f(x-1) + f(x-2) .... (x>2)  

对于给定的整数 n 和 m，我们希望求出：
f(1) + f(2) + ... + f(n) 的值。但这个值可能非常大，所以我们把它对 f(m) 取模(取模即求余数)。  
公式如下  
![ques1_image](http://lx.lanqiao.cn/RequireFile.do?fid=hyry39mn "题目1")  
但这个数字依然很大，所以需要再对 p 求模(**注意这里是对p求模,不是f(p)**)。  
输入格式  
　　输入为一行用空格分开的整数 n m p (0 < n, m, p < 10^18)  
输出格式  
　　输出为1个整数，表示答案  
样例输入  
2 3 5  
样例输出  
0  
样例输入  
15 11 29  
样例输出  
25  

*版权声明：本题目出自CSDN博主「楚楚楚歌」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。  
原文链接：https://blog.csdn.net/qq_38422570/article/details/79734569*

```
#include <stdio.h>

long long fibonacci(unsigned n,int sum){
  long long f_1 = 0;
  long long f_2 = 1;
  long long f_n;
  long long f_sum = 0;
  unsigned index;

  if(n == 0)
    return 0;
  if(n == 1)
    return 1;

  f_sum = 1;

  for(index = 2; index <= n; index++){
    f_n = f_1 + f_2;
    if(sum)
      f_sum += f_n;
    f_1 = f_2;
    f_2 = f_n;
  }

  if(sum)
    return f_sum;
  else
    return f_n;
}

int main(){
  unsigned int n, m, p;
  long long sum_n,f_m;

  scanf("%d %d %d",&n,&m,&p);
  printf("n=%d m=%d p=%d \n",n,m,p );

  if( n < 0 || m <= 0 || p <= 0){
    printf("invalid input\n");
    return -1;
  }

  sum_n = fibonacci(n,1);
  f_m = fibonacci(m,0);

  printf("result is %lld \n",sum_n%f_m%p );
  return 0;
}
```

### 应用题型
一只青蛙一次可以跳一个台阶,也可以跳两个台阶,求此青蛙跳上一个n级台阶的方法有多少种  
*此题取自剑指offer*  

拆解开来看  
1级台阶 - 1种跳法  
2级台阶 - 2种跳法  
把n个台阶的跳的方法数设为n的函数,f(n)  
假设第一次跳是1个台阶,那么剩下的方法是f(n-1)  
假设地一次跳2个台阶,那么剩下的台阶方法是f(n-2)  
这两种情况加到一起,就是总共的方法数量,即  
f(n) = f(n-1) + f(n-2)  
这样就可以看出这实际是个斐波那契原型题了
