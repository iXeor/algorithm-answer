# Leetcode 218. 天际线问题（西山居开发岗笔试大题）

## 题目内容：
城市的天际线是从远处观看该城市中所有建筑物形成的轮廓的外部轮廓。给你所有建筑物的位置和高度，请返回由这些建筑物形成的 天际线 。
每个建筑物的几何信息由数组 `buildings` 表示，其中三元组 `buildings[i] = [lefti, righti, heighti]` 表示：

    - lefti 是第 i 座建筑物左边缘的 x 坐标。
    - righti 是第 i 座建筑物右边缘的 x 坐标。
    - heighti 是第 i 座建筑物的高度。
**天际线** 应该表示为由 “关键点” 组成的列表，格式 `[[x1,y1],[x2,y2],...]` ，并按 `x` 坐标进行排序 。关键点是水平线段的左端点。列表中最后一个点是最右侧建筑物的终点，`y` 坐标始终为 `0` ，仅用于标记天际线的终点。此外，任何两个相邻建筑物之间的地面都应被视为天际线轮廓的一部分。

注意：输出天际线中不得有连续的相同高度的水平线。例如 `[...[2 3], [4 5], [7 5], [11 5], [12 7]...]` 是不正确的答案；三条高度为 5 的线应该在最终输出中合并为一个：`[...[2 3], [4 5], [12 7], ...]`

>### 示例 1：
![](https://assets.leetcode.com/uploads/2020/12/01/merged.jpg)
>```
>输入：buildings = [[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]
>输出：[[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]
>```
>解释：
>图 `A` 显示输入的所有建筑物的位置和高度，
>图 `B` 显示由这些建筑物形成的天际线。图 B 中的红点表示输出列表中的关键点。
>### 示例 2：
>```
>输入：buildings = [[0,2,3],[2,5,3]]
>输出：[[0,3],[5,0]]
>```
### 提示：
```
1 <= buildings.length <= 104
0 <= lefti < righti <= 231 - 1
1 <= heighti <= 231 - 1
buildings 按 lefti 非递减排序
```

## 解题思路：
扫描线分割+优先队列维护当前最大高度。
在本题中，为了按顺序枚举横坐标，我们用数组 $boundaries$ 保存所有的边缘，排序后遍历该数组即可。过程中，我们首先将`包含该横坐标`的建筑加入到优先队列中，然后不断检查优先队列的队首元素是否`包含该横坐标`，如果不`包含该横坐标`，我们就将该队首元素弹出队列，直到队空或队首元素`包含该横坐标`即可。最后我们用变量 $maxn$ 记录最大高度（即纵坐标的值），当优先队列为空时，$maxn=0$，否则 $maxn$ 即为队首元素。最后我们还需要再做一步检查：如果当前关键点的纵坐标大小与前一个关键点的纵坐标大小相同，则说明当前关键点无效，我们跳过该关键点即可。
我们通过垂直线段将建筑分割成不同块，分割线被用作加入优先队列时的依据，当其加入优先队列后，我们只需要用到有效的高度信息（维护最大高度）以及下一条分割线的信息（用于弹出优先队列），因此只需要在优先队列中保存这两个元素即可。
## 代码：
```cpp
class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
        auto cmp = [](const pair<int, int>& a, const pair<int, int>& b) -> bool { return a.second < b.second; };
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> que(cmp);

        vector<int> boundaries;
        for (auto& building : buildings) {
            boundaries.emplace_back(building[0]);
            boundaries.emplace_back(building[1]);
        }
        sort(boundaries.begin(), boundaries.end());

        vector<vector<int>> ret;
        int n = buildings.size(), idx = 0;
        for (auto& boundary : boundaries) {
            while (idx < n && buildings[idx][0] <= boundary) {
                que.emplace(buildings[idx][1], buildings[idx][2]);
                idx++;
            }
            while (!que.empty() && que.top().first <= boundary) {
                que.pop();
            }

            int maxn = que.empty() ? 0 : que.top().second;
            if (ret.size() == 0 || maxn != ret.back()[1]) {
                ret.push_back({boundary, maxn});
            }
        }
        return ret;
    }
};
```
