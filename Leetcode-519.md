# Leetcode 519. 随机翻转矩阵

### 解题思路
我们可以考虑将矩阵转换为一个长度为 $m×n$ 的一维数组 `map`，对于矩阵中的位置 $(i,j)$，它对应了 `map` 中的元素 $map[i×n+j]$，这样就保证了矩阵和 `map` 的元素映射。在经过 $m×n−k$ 次翻转 `flip` 后，我们会修改 `map` 与矩阵的映射，使得当前矩阵中有 $m×n−k$ 个 `1` 和 $k$ 个 `0`。
此时我们可以利用数组中元素的交换，使得 `map[0⋯k−1]` 映射到矩阵中的 `0`，而`map[k⋯m×n−1]` 映射到矩阵中的 11。这样的好处是，当我们进行下一次翻转操作时，我们只需要在 $[0, k-1)$ 这个区间生成随机数 `x`，并将 `map[x]` 映射到的矩阵的位置进行翻转即可。

在将 `map[x]` 进行翻转后，此时矩阵中有 $k−1$ 个 `0`，所以我们需要保证 `map[0..k−2]` 都映射到矩阵中的 `0`。由于此时`map[x]` 映射到了矩阵中的 `1`，因此我们可以将`map[x]` 与`map[k−1]` 的值进行交换，即将这个新翻转的 `1` 作为 `map[k−1]` 的映射，而把原本 `map[k−1]` 映射到的 `0` 交给 $x$。这样我们就保证了在每一次翻转操作后，`map` 中的前 $k$ 个元素恰好映射到矩阵中的所有 $k$ 个 `0`。

那么我们如何维护这个一维数组 `map` 呢？我们可以发现，`map` 中的大部分映射关系是不会改变的，即矩阵中的 $(i, j)(i,j)$ 映射到 $A[i×n+j]$，因此我们可以使用一个 `HashMap` 存储那些 `map` 中那些被修改了的映射。对于一个数 `x`，如果 `x` 不是 `HashMap` 中的一个键，那么它直接映射到最开始的 $(x/n, x\%n)(x/n,x\%n)$；如果 `x` 是`HashMap` 中的一个键，那么它映射到其在 `HashMap` 中对应的值。实际运行中 `HashMap` 的大小仅和翻转次数成正比，因为每一次翻转操作我们会交换 `map` 中两个元素的映射，即最多有两个元素的映射关系被修改。

### 代码
```cpp
#include <hash_map>
class Solution {
private:
    typedef __gnu_cxx::hash_map<int, int> HashMap;
    int m,n,total;
    HashMap map;
public:
    Solution(int m, int n) {
        this->m = m;
        this->n = n;
        this->total = m * n;
        srand(time(nullptr));
    }
    
    vector<int> flip() {
        int x = rand() % total;
        total--;
        vector<int> ans;
        if (map.count(x)) {
            ans = {map[x] / n, map[x] % n};
        } else {
            ans = {x / n, x % n};
        }
        if (map.count(total)) {
            map[x] = map[total];
        } else {
            map[x] = total;
        }
        return ans;
    }
    
    void reset() {
        total = m * n;
        map.clear();
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(m, n);
 * vector<int> param_1 = obj->flip();
 * obj->reset();
 */
```