---
title: Single Number III
---

### 描述

Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

For example:

Given `nums = [1, 2, 1, 3, 2, 5]`, return `[3, 5]`.

**Note**:

1. The order of the result is not important. So in the above example, [5, 3] is also correct.
2. Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

### 分析

本题是有两个未知数，是 "Single Number" 这道题的扩展，直接做异或肯定是不行的。有没有办法把两个未知数分开，使得可以应用 Single Number 的解法呢？

设`x`, `y`是那两个未知数，那么如果对这个数组做异或的话，结果实质上等于`x ^ y`，因为其他数都出现了两次，被抵消了。

但是仅仅是通过最后异或出来的值，是没办法区分出`x`和`y`的，但是足以帮助我们把`x`和`y`划分到不停地子数组中去。对于`x`和`y`，由于二者不相等，那么二者至少存在一位不相同，如下图所示：

![](/img/single-number-iii.png)

当找到这个`k`以后，就可以按照第`k`位是否等于 1，将`nums`数组划分为两个子数组，然后把 Single Number 的算法直接应用到两个子数组上，就可以求出`x`和`y`了。

### 代码

```java
// Single Number III
// Time Complexity: O(log n), Space Complexity: O(1)
public class Solution {
    public int[] singleNumber(int[] nums) {
        int xorResult = 0;
        for (int x : nums) {
            xorResult ^= x;
        }

        // get the index of first 1
        int k = 0;
        for (k = 0; k < Integer.SIZE; ++ k) {
            if (((xorResult >>> k) & 1) == 1) {
                break;
            }
        }

        // use k to split the array into two subarrays
        // XOR result of the first subarray
        int xorResult2 = 0;
        for (int x : nums) {
            if (((x >>> k) & 1) == 1) {
                xorResult2 ^= x;
            }
        }
        return new int[] {xorResult2, xorResult ^ xorResult2};
    }
}
```
