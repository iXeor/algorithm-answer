# 阿里笔试题——完美数字
### 解题思路
在有限的次数内可以通过对有限个数字进行+1操作实现整个数组内没有奇数。按照题目要求，我们需要对是否能在规定次数内完成修改进行判断。那么很明显，我们判断全部修改完成需要的次数即可。对于已经变成偶数的部分，不需要对其进行+1，可以将其清出队列（因为不用输出修改后的数组，所以可以这样做），`main()`略，仅展示核心判断部分。
### 代码
```cpp
class Solution
{
private:
    int count = 0;
public:
    /*使用迭代器去除容器内偶数元素*/
    vector<int> clearduo(vector<int> list)
    {
        vector<int>::iterator itr = list.begin();
        while (itr != list.end())
        {
            if (*itr % 2 == 0)
            {
                itr = list.erase(itr);
            }
            else itr++;
        }
        return list;
    }
    /*检查操作次数*/
    int checkOpera(vector<int> list, int m)
    {
        count++;
        vector<int> temp = clearduo(list);
        if (temp.size() > m)
        {
            for (int i = 0; i < m; i++)
            {
                temp.at(i)++;
            }
            checkOpera(temp, m);
        }
        return count;
    }
};
```