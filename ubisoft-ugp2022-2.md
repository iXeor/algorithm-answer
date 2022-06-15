# Ubisoft UGP 2022笔试二测 - 最长二值子序列长度 （特斯拉笔试）

如果每个数组最多包含两个数字，则称其为二值数组

**例如**：[4,5,5,4,5]是二值数组（它包含两个不同的数字：4 和 5），而[1,5,4,5]则不是二值数组（它包含三个不同的数字：1，4，5）

求输入数组**A**中最长的二值子序列的长度是多少？

实现一个函数：
```cpp
    int longestBinArray(vector<int> &A)
```
给定一个由N个整数组成的数组A，返回A中最长的二值子序列长度。

### 示例：

1. 若A = [4,2,2,4,2]，函数返回值应该是 `5` ，因为整个数组都是双值的。
2. 若A = [1,2,3,2]，函数的返回值应该是 `3` ，最长二值子序列为[2,3,2]。
3. 若A = [0,5,4,4,5,1,2]，函数的返回值应该是 `4` ，最长二值子序列为[5,4,4,5]。
4. A = [4,4]，函数的返回值应该是 `2`。

### 条件：

1. N为[1...100]范围内整数
2. 数组A的每个元素都是一个整数，取值范围为[-1,000,000,000,1,000,000,000]。

## 解题思路：
1. 设置存储二值标志值的数组
2. 通过size记录子数组不同元素数量
3. length记录长度
4. 设置一个开始节点，一个重新开始节点，遍历数组
需要考虑的情况：
   - 如果在同集合中数值相同，且与上一个节点值不同，则开始节点不变，更新重新开始节点
   - 如果在同集合中数值相同，且与上一个节点值相同，则节点均不改变
   - 如果在同集合中数值不同，且与上一个节点值不同，则开始节点更新为重新开始节点，重新开始节点更新为当前值，且出队二值标志值数组里的第一个元素，并压入新元素（当前值）。
5. 取最大长度

## 代码：

```cpp
class Solution
{
public:
    int longestBinArray(vector<int> &A)
    {
        vector<int> data = new vector<int>(2);
        int start = 0,rstart = 0;
        int length = 0,size = 0;
        for(int i = 0 ; i < (int)A.size() ; i++)
        {
            if(i == 0)
            {
                data[0] = A[i];
                start = i;
                size++;
            }
            else if(size < 2)
            {
                if(A[i] != A[i-1])
                {
                    data[1] = A[i];
                    rstart = i;
                    size++;
                }
            }
            else
            {
                if(A[i] == data[0] || A[i] == data[1])
                {
                    if(A[i] != A[i-1])
                    {
                        rstart = i;
                    }
                }
                else
                {
                    int tmp = data[1];
                    data[1] = A[i];
                    data[0] = tmp;
                    start = rstart;
                    rstart = i;
                }
            }
            length = length > (j-start+1) ? length : (j-start+1);
        }
        return length;
    }
};
```