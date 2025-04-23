---
title: Largest Rectangle in Histogram
---

### 描述

Given `n` non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![Above is a histogram where width of each bar is 1, given height = `[2,1,5,6,2,3]`.](/img/histogram.png)

![The largest rectangle is shown in the shaded area, which has area = 10 unit.](/img/histogram-area.png)

For example, given height = `[2,1,5,6,2,3]`, return 10.

### 分析

简单的，类似于 [Container With Most Water](../../dual-pointers/container-with-most-water.md)，对每个柱子，左右扩展，直到碰到比自己矮的，计算这个矩形的面积，用一个变量记录最大的面积，复杂度`O(n^2)`，会超时。

如上图所示，从左到右处理直方，当`i=4`时，小于当前栈顶（即直方 3），对于直方 3，无论后面还是前面的直方，都不可能得到比目前栈顶元素更高的高度了，处理掉直方 3（计算从直方 3 到直方 4 之间的矩形的面积，然后从栈里弹出）；对于直方 2 也是如此；直到碰到比直方 4 更矮的直方 1。

这就意味着，可以维护一个递增的栈，每次比较栈顶与当前元素。如果当前元素大于栈顶元素，则入栈，否则合并现有栈，直至栈顶元素小于当前元素。结尾时入栈元素 0，重复合并一次。

### 代码

import Tabs from "@theme/Tabs";
import TabItem from "@theme/TabItem";

<Tabs
defaultValue="python"
values={[
{ label: 'Python', value: 'python', },
{ label: 'Java', value: 'java', },
{ label: 'C++', value: 'cpp', },
]
}>
<TabItem value="java">

```java
// Largest Rectangle in Histogram
// 时间复杂度O(n)，空间复杂度O(n)
class Solution {
    public int largestRectangleArea(int[] heights) {
        Stack<Integer> s = new Stack<>();
        int result = 0;
        for (int i = 0; i <= heights.length; ) {
            final int value = i < heights.length ? heights[i] : 0;
            if (s.isEmpty() || value > heights[s.peek()])
                s.push(i++);
            else {
                int tmp = s.pop();
                result = Math.max(result,
                        heights[tmp] * (s.isEmpty() ? i : i - s.peek() - 1));
            }
        }
        return result;
    }
}
```

</TabItem>
<TabItem value="cpp">

```cpp
// Largest Rectangle in Histogram
// 时间复杂度O(n)，空间复杂度O(n)
class Solution {
public:
    int largestRectangleArea(vector<int> &heights) {
        stack<int> s;
        heights.push_back(0);
        int result = 0;
        for (int i = 0; i < heights.size(); ) {
            if (s.empty() || heights[i] > heights[s.top()])
                s.push(i++);
            else {
                int tmp = s.top();
                s.pop();
                result = max(result,
                        heights[tmp] * (s.empty() ? i : i - s.top() - 1));
            }
        }
        return result;
    }
};
```

</TabItem>

<TabItem value="python">

```python
# Largest Rectangle in Histogram
# 时间复杂度O(n)，空间复杂度O(n)
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        s = []
        result = 0
        i = 0
        while i <= len(heights):
            value = heights[i] if i < len(heights) else 0
            if not s or value > heights[s[-1]]:
                s.append(i)
                i += 1
            else:
                tmp = s.pop()
                result = max(result,
                        heights[tmp] * (i if not s else i - s[-1] - 1))
        return result
```

</TabItem>
</Tabs>

### 相关题目

- [Trapping Rain Water](../../dual-pointers/trapping-rain-water.md)
- [Container With Most Water](../../dual-pointers/container-with-most-water.md)
