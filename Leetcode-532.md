# Leetcode-532. 数组中的 k-diff 数对

给定一个整数数组和一个整数 `k`，你需要在数组里找到 不同的 `k-diff` 数对，并返回不同的 `k-diff` 数对 的数目。

这里将 `k-diff` 数对定义为一个整数对 `(nums[i], nums[j])`，并满足下述全部条件：

> 0 <= i < j < nums.length
> |nums[i] - nums[j]| == k
> 注意，|val| 表示 val 的绝对值。

```
示例 1：

输入：nums = [3, 1, 4, 1, 5], k = 2
输出：2
解释：数组中有两个 2-diff 数对, (1, 3) 和 (3, 5)。
尽管数组中有两个1，但我们只应返回不同的数对的数量。
示例 2：

输入：nums = [1, 2, 3, 4, 5], k = 1
输出：4
解释：数组中有四个 1-diff 数对, (1, 2), (2, 3), (3, 4) 和 (4, 5)。
示例 3：

输入：nums = [1, 3, 1, 5, 4], k = 0
输出：1
解释：数组中只有一个 0-diff 数对，(1, 1)。
```

**提示**：

> 1 <= nums.length <= 104
> -107 <= nums[i] <= 107
> 0 <= k <= 107


## 解法：

### 题干的两个条件可以这样解读：

数对的两个元素的下标值不同。当 `k=0` 时，数对的两个元素值可以相同，但下标值必须不同。
数对的两个元素差值为 `k`。
这样的解读之下，原来 `i` 和 `j` 的大小关系被抹除了，只要求 `i` 和 `j` 不相等。而差值为 `k` 这一要求则可以在排序后使用双指针来满足。

将原数组升序排序，并用新的指针 `x` 和 `y` 来搜索数对。即寻找不同的 $(\textit{nums}[x], \textit{nums}[y])$ 满足：

$x < y$
$\textit{nums}[x] + k = \textit{nums}$

记录满足要求的 $x$ 的个数并返回。

```cpp
class Solution {
public:
    int findPairs(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int n = nums.size(), y = 0, res = 0;
        for (int x = 0; x < n; x++) {
            if (x == 0 || nums[x] != nums[x - 1]) {
                while (y < n && (nums[y] < nums[x] + k || y <= x)) {
                    y++;
                }
                if (y < n && nums[y] == nums[x] + k) {
                    res++;
                }
            }
        }
        return res;
    }
};
```