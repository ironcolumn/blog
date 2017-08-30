---
title: LeetCode笔记(一)
date: 2017-08-29 19:26:46
tags: [Leetcode,HashMap]
categories: Leetcode
mathjax: true
---
# <center>Two Sum</center>
## 题目
> Given an array of integers, return indices of the two numbers such that they add up to a specific target.  
> You may assume that each input would have exactly one solution, and you may not use the same element twice.  
> Example:
> >Given `nums = [2, 7, 11, 15], target = 9`,  
> Because` nums[0] + nums[1] = 2 + 7 = 9`,
> return `[0, 1]`.

## 解法一 Brute Force(暴力算法)
```java
public int[] twoSum(int[] nums, int target) {   
     for (int i = 0; i < nums.length; i++) {
         for (int j = i + 1; j < nums.length; j++) {
             if (nums[j] == target - nums[i]) {
                 return new int[] { i, j };
             }
         }
     }
     throw new IllegalArgumentException("No two sum solution");
 }
```
### 思路
  双重循环遍历数组,第一重循环遍历每个元素,每次循环执行第二重循环,从下标为第一重循环元素的后一个的元素开始,寻找满足与一重循环元素相加为目标值的元素,找到则返回两次循环的下标组.
  抛出一个非法参数异常`IllegalArgumentException`,表示没有符合要求的元素.
### 复杂度分析
时间复杂度: $O(n^2)$,对一重循环的每个元素都又执行了一次循环遍历剩余的元素,所以时间复杂度为$O(n^2)$

空间复杂度: $O(1)$
#### 参考资料: [算法的时间复杂度和空间复杂度-总结](http://blog.csdn.net/zolalad/article/details/11848739)

## 解法二 Two-pass Hash Table(双程哈希表)
``` java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        map.put(nums[i], i);
    }
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement) && map.get(complement) != i) {
            return new int[] { i, map.get(complement) };
        }
    }
    throw new IllegalArgumentException("No two sum solution");
}
```
### 思路
将原数组的"下标-元素"对应关系以键值对形式存储在 `HashMap` 中,这里将元素值作为键名,将对应下标作为键值,这样可以保证相同的元素只存在一个,满足题目 `each input would have exactly one solution`的要求.

通过上述方法,可将查找目标元素的时间复杂度从 $O(n)$ 减至 $O(1)$ (使用 `HashMap.containsKey(key)`方法 ).  哈希表的目的就是用空间复杂度换取时间复杂度, 它支持在几乎固定的时间内完成快速查找,除非发生碰撞(collision),此时查找的时间复杂度将退化至 $O(n)$,但由于发生碰撞的概率很低,因此只要查找方法(hash function) 选择得当,哈希表查找的时间复杂度应被平摊地(amortized)认为是 $O(1)$ .

一个简单的实现是使用两次循环,第一次循环将原数组以 "键名:元素值,键值:元素下标"的形式存在一个`HashMap`中,第二次循环查找`HashMap`中是否存在与循环元素相加为目标值的元素(`HashMap.containsKey(key)`),并且该元素对应的下标不等于循环元素的下标(满足`not use the same element twice`),若满足条件则返回下标数组,否则抛出非法参数异常`IllegalArgumentException`,表示没有符合要求的元素.
### 复杂度分析
时间复杂度: $O(n)$,对原数组 `int[] nums` 进行了两次遍历,由于哈希表将查询的时间复杂度降至了$O(1)$,因此总时间复杂度为$O(n)$.

空间复杂度: $O(n)$ 额外占用的空间取决于哈希表中存储的条目数量(原数组的长度).
#### 关于`HashMap`:  [HashMap深度解析(一)](http://blog.csdn.net/ghsau/article/details/16843543)
#### `HashMap`的时间复杂度思考:  [java中hashmap容器实现查找O(1)时间复杂度的思考](http://blog.csdn.net/u014633283/article/details/48549155)
#### 关于`HashMap`冲突:  [HashMap解决hash冲突的方法](http://blog.csdn.net/subuser/article/details/47188837)
## 解法三 One-pass Hash Table(单程哈希表)
``` java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        map.put(nums[i], i);
    }
    throw new IllegalArgumentException("No two sum solution");
}
```
### 思路
对解法二的优化，在第一次遍历插入 `HashMap` 的过程中同时判断表中是否已存在符合条件的元素（在 ` map.put(nums[i], i)` 之前进行条件判断），如果存在则直接返回下标组，退出循环，否则执行插入操作。

### 复杂度分析
时间复杂度: $O(n)$,对原数组 `int[] nums` 只进行了一次遍历,每次哈希表查询的时间复杂度仅为$O(1)$,因此总时间复杂度为$O(n)$.

空间复杂度: $O(n)$ 额外占用的空间取决于哈希表中存储的条目数量(原数组的长度).
