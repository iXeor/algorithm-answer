# Leetcode 164. 最大间距

### 解题思路
这次依然是按照题目提示解题：先排序，后计算最大差值。
1. 先看看数组里有几个元素，`如果数组元素个数小于 2，返回 0`。
2. 如果元素个数大于等于`2`，就可以初始化容器并对数组排序了。至于为什么要初始化容器？因为我要存储两元素之间距离。
3. 那么我依次遍历并计算得到所有相邻元素之间的差值了，对吧。它们现在已经躺平在容器里。
4. 这时候我引入一个`*max_element()`，它是取容器内最大值（`max_element()`取首次出现最大值的地址）。
5. 那我对存距离的容器使用`*max_element()`就好了啊。看起来没有那么难，是吗？
6. 上代码：

### 代码
```c++
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        if(nums.size()<2)
        {
            return 0;
        }
        vector<int> length;
        sort(nums.begin(),nums.end());
        for(int i = 1 ; i < nums.size() ; i++)
        {
            int temp = abs(nums.at(i-1) - nums.at(i));
            length.push_back(temp);
        }
        return *max_element(length.begin(),length.end());;
    }
};
```