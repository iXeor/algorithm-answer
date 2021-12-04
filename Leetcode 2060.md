# Leetcode 2060. 同源字符串检测

## 题目内容：

原字符串由小写字母组成，可以按下述步骤编码：
任意将其**分割**为由若干**非空**子字符串组成的一个 **序列** 。
任意选择序列中的一些元素（也可能不选择），然后将这些元素替换为元素各自的长度（作为一个数字型的字符串）。
重新**顺次连接**序列，得到编码后的字符串。
例如，编码 `"abcdefghijklmnop"` 的一种方法可以描述为：

将原字符串分割得到一个序列：`["ab", "cdefghijklmn", "o", "p"]` 。
选出其中第二个和第三个元素并分别替换为它们自身的长度。序列变为` ["ab", "12", "1", "p"] `。
重新顺次连接序列中的元素，得到编码后的字符串：`"ab121p" `。
给你两个编码后的字符串` s1 `和` s2 `，由小写英文字母和数字` 1-9 `组成。如果存在能够同时编码得到` s1 `和` s2 `原字符串，返回` true `；否则，返回`false`。

**注意**：生成的测试用例满足` s1 `和` s2 `中连续数字数不超过` 3 `。
>### 示例 1：
>```
>输入：s1 = "internationalization", s2 = "idx18n"
>输出：true
>```
>解释：`"internationalization"` 可以作为原字符串
>>- "internationalization" 
>>  -> 分割：      ["internationalization"]
>>  -> 不替换任何元素
>>  -> 连接：      "internationalization"，得到 s1
>>- "internationalization"
>>  -> 分割：      ["i", "nternationalizatio", "n"]
>>  -> 替换：      ["i", "18",                 "n"]
>>  -> 连接：      "idx18n"，得到 s2
>### 示例 2：
>```
>输入：s1 = "l123e", s2 = "44"
>输出：true
>```
>解释：`"leetcode"` 可以作为原字符串
>>- "leetcode" 
>>  -> 分割：       ["l", "e", "et", "cod", "e"]
>>  -> 替换：       ["l", "1", "2",  "3",   "e"]
>>  -> 连接：       "l123e"，得到 s1
>>- "leetcode" 
>>  -> 分割：       ["leet", "code"]
>>  -> 替换：       ["4",    "4"]
>>  -> 连接：       "44"，得到 s2
>### 示例 3：
>```
>输入：s1 = "a5b", s2 = "c5b"
>输出：false
>```
>解释：不存在这样的原字符串
>>- 编码为 s1 的字符串必须以字母 'a' 开头
>>- 编码为 s2 的字符串必须以字母 'c' 开头
>### 示例 4：
>```
>输入：s1 = "112s", s2 = "g841"
>输出：true
>```
>解释：`"gaaaaaaaaaaaas"` 可以作为原字符串
>>- "gaaaaaaaaaaaas"
>>  -> 分割：       ["g", "aaaaaaaaaaaa", "s"]
>>  -> 替换：       ["1", "12",           "s"]
>> -> 连接：       "112s"，得到 s1
>>- "gaaaaaaaaaaaas"
>>  -> 分割：       ["g", "aaaaaaaa", "aaaa", "s"]
>>  -> 替换：       ["g", "8",        "4",    "1"]
>>  -> 连接         "g841"，得到 s2
>### 示例 5：
>```
>输入：s1 = "ab", s2 = "a2"
>输出：false
>```
>解释：不存在这样的原字符串
>>- 编码为 s1 的字符串由两个字母组成
>>- 编码为 s2 的字符串由三个字母组成


## 解题思路：
### DFS解决
dfs的状态里包括：标记 `s1` 和 `s2` 遍历的位置 `idx1` 和 `idx2`； 标记当前两个字符串展开长度的差距。
退出条件：`idx1` 和 `idx2` 都到了字符串的尾端；且两者展开的长度一样。
为了提高效率，需要加一个记忆化搜索。 事实上，dfs开始前，我们可以记录当前状态已经被访问过，下次遇到已经访问过的可以直接返回`false`。
这是因为要么当前情况之前出现的时候之前已经返回了`true`，要么就是没有成功，那么不用再进行计算也知道是`false`。
具体搜索的过程，就是从当前位置往后找所有的数字展开。
如果一遍长度比另一边长，遇到字母可以去抵消差距。如果本来就在长的一边，碰到字母就先不继续递归，等另一边长度追上来再进行递归。
中间还需要处理一下两边长度一样，且都是字母的情况。字母只有相同，才能继续递归；否则直接返回`false`。

## 代码：
```cpp
class Solution {
private:
    int n1 = 0;
    int n2 = 0;
    bool visited[41][41][2000];
    
    bool isNumber(char c) {
        return c <= '9' && c >= '0';
    }
    
    bool dfs(string& s1, int idx1, string& s2, int idx2, int d) {      
        if (idx1 == n1 && idx2 == n2) {
            if (d == 0) return true;
            visited[idx1][idx2][d+1000] = true;
            return false;
        }
        if (visited[idx1][idx2][d+1000]) {
            return false;
        }
        visited[idx1][idx2][d+1000] = true;
        int in1 = idx1;
        int in2 = idx2;
        while(in1 < n1 && isNumber(s1[in1])) {
            int num = stoi(s1.substr(idx1, in1-idx1+1));
            if (dfs(s1, in1+1, s2, idx2, d-num))
                return true;
            in1++;
        }
        if (idx1 < n1 && !isNumber(s1[idx1])) {
            if (d > 0 && dfs(s1, idx1+1, s2, idx2, d-1)) 
                return true;
        }
        while(in2 < n2 && isNumber(s2[in2])) {
            int num = stoi(s2.substr(idx2, in2-idx2+1));
            if (dfs(s1, idx1, s2, in2+1, d+num))
                return true;
            in2++;
        }
        if (idx2 < n2 && !isNumber(s2[idx2])) {
            if (d < 0 && dfs(s1, idx1, s2, idx2+1, d+1))
                return true;
        }
        if (d == 0 && !isNumber(s1[idx1]) && !isNumber(s2[idx2]) && s1[idx1] == s2[idx2]) 
                return dfs(s1, idx1+1, s2, idx2+1, 0);
        return false;
    }
public:
    bool possiblyEquals(string s1, string s2) {
        n1 = s1.size();
        n2 = s2.size();
        return dfs(s1, 0 , s2, 0, 0);
    } 
};
```