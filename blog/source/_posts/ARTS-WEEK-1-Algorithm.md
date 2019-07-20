---
title: ARTS-WEEK 1-Algorithm
date: 2019-07-20 23:23:12
tags: ARTS Algorithm
---
# 什么是ARTS？

ARTS：
（也就是 Algorithm、Review、Tip、Share 简称ARTS）

1. 每周至少做一个 leetcode 的算法题
1. 阅读并点评至少一篇英文技术文章
1. 学习至少一个技术技巧
1. 分享一篇有观点和思考的技术文章

刚刚加入左耳朵耗子叔的ARTS打卡挑战群，群里的小伙伴说：

>在这条路上，你会有一万个理由来让你应付交差，还会有十万个理由放弃，过去的几个群，能坚持下来的也就2%-4%的样子，所以，你们要做好挑战自己的艰难准备，有问题互相探讨，互相鼓励。

我参与这个打卡挑战，首先是想通过持续的练习，提升自己的数据结构和算法基本功，让自己的技术更加精进，也是希望希望能够看一下，自己能不能够坚持下来，培养自己持续记录、持续输出的好习惯

废话不多说，开始正题

## Algorithm

LeetCode No. 905: Sort Array By Parity

> Given an array A of non-negative integers, return an array consisting of all the even elements of A, followed by all the odd elements of A.

>  You may return any answer array that satisfies this condition.
  
   
```
  Example 1:
  
  Input: [3,1,2,4]
  
  Output: [2,4,3,1]
  
  The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted. 
```
  
Note:

1. `1 <= A.length <= 5000`
1. `2 <= A[i] <= 5000`

翻译一下：分割一个整数数组，使得偶数在前奇数在后

### 思路：

#### 解法1 

可以通过开辟一个新的数组B，从头遍历原数组，遇到偶数，从前往后放到数组B中，遇到奇数，从后往前放到数据B中。

遍历结束得到新的数据就是结果。

**时间复杂度:** 因为只遍历一遍，时间复杂度为O(N)

**空间复杂度:** 需要新开辟一个数组，所以空间复杂度为O(N)

实现如下：

```java
class Solution {
    public int[] sortArrayByParity(int[] A) {
        int[] B = new int[A.length];
        int j = 0, k = A.length - 1;
        for (int i = 0; i < A.length; i++) {
            if (A[i] % 2 == 0) {
                B[j] = A[i];
                j++;
            } else {
                B[k] = A[i];
                k--;
            }
        }
        return B;
    }
}
```

#### 解法2

解法1需要新开辟一个数组，空间复杂度为O(N)，如果想减少空间复杂度，实现原地排序，那么可以用如下方法：

设置两个指针j和k，j从前往后遍历，k从后往前遍历

当且仅当j遇到的奇数，且k遇到的是偶数时，交换j、k所在位置的元素

否则，当j遇到偶数时右移一个元素，k遇到奇数时左移一个元素

直到j与k相遇为止

**时间复杂度:** 因为每个元素只遍历一遍，时间复杂度为O(N)

**空间复杂度:** 原地排序，空间复杂度为O(1)

实现如下：

```java
class Solution {
    public int[] sortArrayByParity(int[] A) {
        int j = 0, k = A.length - 1;
        while (j < k) {
            if (A[j] % 2 == 1 && A[k] % 2 == 0) {
                swap(A, j, k);
            }
            if (A[j] % 2 == 0) {
                j++;
            }
            if (A[k] % 2 == 1) {
                k--;
            }
        }
        return A;
    }
}
```

