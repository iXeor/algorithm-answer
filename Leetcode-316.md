# Leetcode 316. 去除重复字母 (Intel 研发岗笔试题原型)

## 题目内容：
给你一个字符串 s ，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证 返回结果的字典序最小（要求不能打乱其他字符的相对位置）。
注意：该题与 [Leetcode.1081](https://leetcode-cn.com/problems/smallest-subsequence-of-distinct-characters) 相同。

>### 示例 1：
>```
>输入：s = "bcabc"
>输出："abc"
>```
>### 示例 2：
>```
>输入：s = "cbacdcbc"
>输出："acdb"
>```

## 解题思路：
### 单调栈维护
从前向后扫描原字符串。每扫描到一个位置，我们就尽可能地处理所有的`关键字符`。假定在扫描位置 `s[i-1]` 之前的所有`关键字符`都已经被去除完毕，在扫描字符 `s[i]` 时，新出现的`关键字符`只可能出现在 `s[i]` 或者其后面的位置。
于是，我们使用单调栈来维护去除`关键字符`后得到的字符串，单调栈满足栈底到栈顶的字符递增。如果栈顶字符大于当前字符 `s[i]`，说明栈顶字符为`关键字符`，故应当被去除。去除后，新的栈顶字符就与 `s[i]` 相邻了，我们继续比较新的栈顶字符与 `s[i]` 的大小。重复上述操作，直到栈为空或者栈顶字符不大于 `s[i]`。
我们还遗漏了一个要求：原字符串` s `中的每个字符都需要出现在新字符串中，且只能出现一次。为了让新字符串满足该要求，之前讨论的算法需要进行以下两点的更改。
在考虑字符 `s[i]` 时，如果它已经存在于栈中，则不能加入字符 `s[i]`。为此，需要记录每个字符是否出现在栈中。
在弹出栈顶字符时，如果字符串在后面的位置上再也没有这一字符，则不能弹出栈顶字符。为此，需要记录每个字符的剩余数量，当这个值为 `0` 时，就不能弹出栈顶字符了。

## 代码：
```cpp
class Solution {
public:
    string removeDuplicateLetters(string s) {
        vector<int> vis(26), num(26);
        for (char ch : s) {
            num[ch - 'a']++;
        }

        string stk;
        for (char ch : s) {
            if (!vis[ch - 'a']) {
                while (!stk.empty() && stk.back() > ch) {
                    if (num[stk.back() - 'a'] > 0) {
                        vis[stk.back() - 'a'] = 0;
                        stk.pop_back();
                    } else {
                        break;
                    }
                }
                vis[ch - 'a'] = 1;
                stk.push_back(ch);
            }
            num[ch - 'a'] -= 1;
        }
        return stk;
    }
};
```