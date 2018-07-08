---
layout: post
title:  "LeetCode初级算法-数组（上）"
date:   2018-07-07
author: MinicitY
categories: LeetCode
tags: 
---

* content
{:toc}

## **写在前面**

[**LeetCode**]( https://leetcode-cn.com/)是个好地方。这个系列会把LeetCode-探索-初级算法中的一些题目挑出来，并附上答案。没有什么值得讲解必要的题目将被忽略。出于大人的原因，代码的形式是java。**不定期更新。**




## **旋转数组**

>Rotate an array of n elements to the right by k steps.
>
>For example, with n = 7 and k = 3, the array [1,2,3,4,5,6,7] is rotated to [5,6,7,1,2,3,4].
>
>Note:
>Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
>
>Hint:
>Could you do it in-place with O(1) extra space?

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int temp;
        k = k%nums.length;
        for(;k>0;k--){
            for(int i = nums.length - 1; i >= 1 ; i--){
                temp = nums[i];
                nums[i] = nums[i - 1];
                nums[i - 1] = temp;
            }
        }
    }
}
```

因为要求用原址的算法，所以额外弄个数组是不行的。因此一开始想要用这种双循环，但在提交的时候超出时间限制了，也就是说必须要线性时间内完成。不得已用搜索引擎查到了下面这种奇淫技巧：

1. 将数组分为两部分，节点视k决定
2. 左部分反转，右部分反转
3. 最后整个数组反转，得到结果数组

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int newK = k % nums.length;
          if (newK == 0 || nums.length == 1)
            return;
        int middle = nums.length - newK - 1;
        reverse(nums, 0, middle);
        reverse(nums, middle + 1, nums.length - 1);
        reverse(nums, 0, nums.length - 1);
    }
    
    public void reverse(int[] array, int begin, int end){
        int temp;
        for(int i = 0; i < (end - begin)/2 + 1; i++){
            temp = array[begin + i];
            array[begin + i] = array[end - i];
            array[end - i] = temp;
        }
    }
}
```

## **存在重复**

>Given an array of integers, find if the array contains any duplicates.
>
>Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.
>
>Example:
>
>Input: [1,2,3,1] Output: true

同样地，双循环太浪费时间，所以借助哈希函数来达到每次查重消耗O(1)、最后在线性时间内结束的效果。

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        if(nums.length <= 1)
        		return false;
        	HashSet<Integer> newSet = new HashSet<>();
        	for(int i = 0; i < nums.length; i++){
        		if(!newSet.contains(nums[i])){
        			newSet.add(nums[i]);
        		}
        		else
        			return true;
        	}
        	return false;
        }
}
```

## **只出现一次的数字**

>Given a non-empty array of integers, every element appears twice except for one. Find that single one.
>
>Note:
>
>Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

这个算法想了很久，网上甚至也没有出现满足「线性时间以及O(1)空间」的讨论。然后从一个用三进制求解另一个问题的算法中得到启示，可以用长度为32的数组储存所有数字的二进制的每个位数的和（因为数组长度不随输入规模变化所以空间还是O(1)），因此只出现一次的数字必然会导致相应的位数是奇数值，再按二进制转十进制就得出结果了。

上面那个是比较复杂的思路，后来偶然看到异或，又有了更简单的思路，直接遍历异或，因为题目中重复的值都是只出现两次，而两个相同值异或会抵消，所以最终遍历结果就是单独的数字。

```java
class Solution {
    public int singleNumber(int[] nums) {
        int s = nums[0],i;
        for(i=1;i<nums.length;i++){
            s = s^nums[i];
        }
        return s;
    }
}
```