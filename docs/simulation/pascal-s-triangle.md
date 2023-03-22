---
title: Pascal's Triangle
---

### 描述

Given `numRows`, generate the first `numRows` of Pascal's triangle.

For example, given `numRows = 5`,

Return

```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

### 分析

本题可以用队列，计算下一行时，给上一行左右各加一个 0，然后下一行的每个元素，就等于左上角和右上角之和。

另一种思路，下一行第一个元素和最后一个元素赋值为 1，中间的每个元素，等于上一行的左上角和右上角元素之和。

### 从左到右

import Tabs from "@theme/Tabs";
import TabItem from "@theme/TabItem";

<Tabs
defaultValue="java"
values={[
{ label: 'Java', value: 'java', },
{ label: 'C++', value: 'cpp', },
]
}>
<TabItem value="java">

```java
// Pascal's Triangle
// 时间复杂度O(n^2)，空间复杂度O(n)
public class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> result = new ArrayList<>();
        if(numRows == 0) return result;

        result.add(new ArrayList<>(Arrays.asList(1))); //first row

        for(int i = 2; i <= numRows; ++i) {
            Integer[] current = new Integer[i]; // 本行
            Arrays.fill(current, 1);
            List<Integer> prev = result.get(i - 2); // 上一行

            for(int j = 1; j < i - 1; ++j) {
                current[j] = prev.get(j-1) + prev.get(j); // 左上角和右上角之和
            }
            result.add(new ArrayList<>(Arrays.asList(current)));
        }
        return result;
    }
}
```

</TabItem>
<TabItem value="cpp">

```cpp
// LeetCode, Pascal's Triangle
// 时间复杂度O(n^2)，空间复杂度O(n)
class Solution {
public:
    vector<vector<int> > generate(int numRows) {
        vector<vector<int> > result;
        if(numRows == 0) return result;

        result.push_back(vector<int>(1,1)); //first row

        for(int i = 2; i <= numRows; ++i) {
            vector<int> current(i,1);  // 本行
            const vector<int> &prev = result[i-2];  // 上一行

            for(int j = 1; j < i - 1; ++j) {
                current[j] = prev[j-1] + prev[j]; // 左上角和右上角之和
            }
            result.push_back(current);
        }
        return result;
    }
};
```

</TabItem>
</Tabs>

### 从右到左

<Tabs
defaultValue="java"
values={[
{ label: 'Java', value: 'java', },
{ label: 'C++', value: 'cpp', },
]
}>
<TabItem value="java">

```java
// Pascal's Triangle
// 时间复杂度O(n^2)，空间复杂度O(n)
public class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> array = new ArrayList<>();
        for (int i = 1; i <= numRows; i++) {
            for (int j = i - 2; j > 0; j--) {
                array.set(j, array.get(j - 1) + array.get(j));
            }
            array.add(1);
            result.add(new ArrayList<Integer>(array));
        }
        return result;
    }
}
```

</TabItem>
<TabItem value="cpp">

```cpp
// LeetCode, Pascal's Triangle
// 时间复杂度O(n^2)，空间复杂度O(n)
class Solution {
public:
    vector<vector<int> > generate(int numRows) {
        vector<vector<int> > result;
        vector<int> array;
        for (int i = 1; i <= numRows; i++) {
            for (int j = i - 2; j > 0; j--) {
                array[j] = array[j - 1] + array[j];
            }
            array.push_back(1);
            result.push_back(array);
        }
        return result;
    }
};
```

</TabItem>
</Tabs>

### 相关题目

- [Pascal's Triangle II](pascal-s-triangle-ii.md)
