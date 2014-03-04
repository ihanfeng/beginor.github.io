---
layout: post
title: 冒泡法排序
description: 由于在排序过程中总是小数往前放，大数往后放，相当于气泡往上升，所以称作冒泡排序
tags: [算法]
keywords: 算法, 冒泡法排序, bubble sort
---

**冒泡排序的基本概念是**：  依次比较相邻的两个数， 将小数放在前面， 大数放在后面。 即在第一趟， 首先比较第1个和第2个数， 将小数放前， 大数放后。 然后比较第2个数和第3个数， 将小数放前， 大数放后， 如此继续， 直至比较最后两个数， 将小数放前， 大数放后。 至此地一趟结束， 将最大的数放到了最后。 在第二趟：仍从第一对数开始比较（因为可能由于第2个数和第3个数的交换，使得第1个数不再小于第2个数），将小数放前，大数放后，一直比较到倒数第二个数（倒数第一的位置上已经是最大的），第二趟结束，在倒数第二的位置上得到一个新的最大数（其实在整个数列中是第二大的数）。如此下去，重复以上过程，直至最终完成排序。

由于在排序过程中总是小数往前放，大数往后放，相当于气泡往上升，所以称作冒泡排序， 其执行过程如下图所示：

![冒泡法排序执行过程](/assets/post-images/bubble_sort_animation.gif)

冒泡法排序通常采用二重循环实现，外循环变量设为 `i` ，内循环变量设为 `j` 。外循环重复9次，内循环依次重复9，8，...，1次。每次进行比较的两个元素都是与内循环 `j` 有关的，它们可以分别用 `a[j]` 和 `a[j+1]` 标识，`i` 的值依次为1,2,...,9，对于每一个 `i` ,  `j` 的值依次为1,2,...10-i。

算法的 C# 代码如下：

    static void BubbleSort(int[] array) {
       var length = array.Length;
       for (var j = length - 1; j > 1; j--) {
          for (var i = 0; i < j; i++) {
             if (array[i] > array[i + 1]) {
                Swap(array, i, i + 1);
             }
          }
       }
    }
    
    static void Swap(int[] array, int i, int j) {
       var tmp = array[i];
       array[i] = array[j];
       array[j] = tmp;
    }

不过上面的实现还有可以改进的地方， 因为对于已经排序好的数组， 其内部循环也会拙略的执行， 通常如果能在内部循环第一次执行时，使用一个旗标来表示有无需要交换的可能， 这样在已经排好顺序（或者部分排序）的数组上执行时会提升效率， 如下所示：

    static void BubbleSort(int[] array) {
       var length = array.Length;
       for (var j = length - 1; j > 1; j--) {
          // 交换位置标识
          int swapPosition = 0;
          for (var i = 0; i < j; i++) {
             if (array[i] > array[i + 1]) {
                Swap(array, i, i + 1);
                // 记录下最后发生交换的位置， 在这个位置之后，应该已经是排序好的， 
                // 下次就不用再循环这部分了
                swapPosition = i + 1;
                // 下次外部循环到 swapPosition 即可
                j = swapPosition;
             }
          }
       }
    }

经过上面小小的改进， 冒泡法排序在已经排好顺序或者部分排序的数组上执行时效率会提高一些。