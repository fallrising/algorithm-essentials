---
title: Diagonal Traverse II
---

### 描述

给定一个列表的列表，其中包含整数，返回按对角线顺序遍历的所有元素。
例如：
输入: nums = [[1,2,3],[4,5,6],[7,8,9]]
输出: [1,4,2,7,5,3,8,6,9]
遍历顺序是从左下角到右上角的对角线顺序。

### 分析

这个问题的关键观察是：在同一对角线上的元素，其行索引和列索引的和是相等的。我们可以利用这个特性来解决问题。
解题思路：

使用一个数据结构（如 TreeMap）来按对角线分组元素。键是行索引和列索引的和，值是该对角线上的所有元素。
遍历输入的列表的列表，将每个元素添加到其对应的对角线组中。
按顺序遍历对角线组，将元素添加到结果列表中。

时间复杂度：O(N)，其中 N 是所有元素的总数。
空间复杂度：O(N)，用于存储所有元素。

### 代码
```java
import java.util.*;

class Solution {
    public int[] findDiagonalOrder(List<List<Integer>> nums) {
        Map<Integer, List<Integer>> groups = new TreeMap<>();
        int count = 0;
        
        for (int row = 0; row < nums.size(); row++) {
            for (int col = 0; col < nums.get(row).size(); col++) {
                int diagonal = row + col;
                if (!groups.containsKey(diagonal)) {
                    groups.put(diagonal, new ArrayList<>());
                }
                groups.get(diagonal).add(nums.get(row).get(col));
                count++;
            }
        }
        
        int[] result = new int[count];
        int index = 0;
        for (List<Integer> group : groups.values()) {
            for (int i = group.size() - 1; i >= 0; i--) {
                result[index++] = group.get(i);
            }
        }
        
        return result;
    }
}
```