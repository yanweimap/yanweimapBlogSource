---
title: 快速二叉树(二分法)排序
description: quick sort,剑指offer题
toc: true
tag: sort
---
# quick sort with recursively call binary tree sort
## 二叉树排序部分
### partition方法
在数组中选一个随机位置,以这个位置为中间,排序完左边的值都小于这个位置的值,右边的值都大于这个值
左右两边还是乱序
先把选中位置的元素放在最右侧,排序结束后再放到原位置
```
int partition(int data[], int length, int start, int end){
  int ra, small, index;

  if(data == NULL || length <= 0 || start <0 || end >= length)
    return -1;

  ra = random_range(start,end);
  if(ra < 0)
    return -1;

  swap(&data[ra],&data[end]);
  small = start -1;

  for(index = start; index < end; index ++){
    //only litter number(than data[end]) need move to left
    if(data[index] <= data[end]){
       small++;
      if(index != small){
        swap(&data[index],&data[small]);
      }
    }
  }
  small++;
  swap(&data[small],&data[end]);

  return small;
}
```

### 递归调用
递归调用partition方法,不断的把数组分割成两部分,分别排序,直到分割出来的数组是一个元素
递归调用时注意return条件
```
void quick_sort(int data[], int length, int start, int end){
  int small;

  if(start == end)
    return;

  small = partition(data, length, start, end);
  if(small < 0)
    return;
  if(small > start){
    quick_sort(data, length, start, small-1);
  }
  if(small < end){
    quick_sort(data, length, small+1, end);
  }
}
```

## 随机数index生成
使用时间生成随机数,保证每次使用的分割点不同
```
int random_range(int start, int end){
  int ra;

  if(end <= start)
    return -1;

  //use time get different random number
  srand((unsigned)time(NULL));
  ra = rand()%(end-start+1)+start;

  return ra;
}
```

## 交换函数swap
```
void swap(int *a, int *b){
  int tmp;
  tmp = *a;
  *a = *b;
  *b = tmp;
  return;
}
```

## 数组打印和排序调用(数组长度计算)
数组长度计算:用sizeof整个数组的长度/数组中单个元素的长度
```
void print_array(int data[], int length){
  int i = 0;

  if(data == NULL || length <= 0)
    return;

  while(i<length){
    printf("%d ",data[i] );
    i++;
  }
  printf("\n" );
  return;
}

int main(){
  int data[] = {4, 0, 2, 8, 0, 1, 3, 9, 7, 2};
  int length;

  length = sizeof(data)/sizeof(data[0]);

  quick_sort(data,length,0,length-1);
  print_array(data,length);

  return 0;
}
```
