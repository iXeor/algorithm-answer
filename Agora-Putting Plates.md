# Agora声网笔试题-Putting Plates（[codeforces #1530-B](https://codeforces.com/contest/1530/problem/B)）
声网直接用的原题，改都没改。直接上题干：

>To celebrate your birthday you have prepared a festive table! Now you want to seat as many guests as possible.
>The table can be represented as a rectangle with height `h` and width `w`, divided into $h×w$ cells. Let $(i,j)$ denote the cell in the i-th row and the j-th column of the rectangle $(1≤i≤h; 1≤j≤w)$.
>Into each cell of the table you can either put a plate or keep it empty.
>As each guest has to be seated next to their plate, you can only put plates on the edge of the table — into the first or the last row of the rectangle, or into the first or the last column. Formally, for each cell $(i,j)$ you put a plate into, at least one of the following conditions must be satisfied: `i=1, i=h, j=1, j=w`.
>To make the guests comfortable, no two plates must be put into cells that have a common side or corner. In other words, if cell $(i,j)$ contains a plate, you can't put plates into cells $(i−1,j)$, $(i,j−1)$, $(i+1,j)$, $(i,j+1)$, $(i−1,j−1)$, $(i−1,j+1)$, $(i+1,j−1)$, $(i+1,j+1)$.
>Put as many plates on the table as possible without violating the rules above.
>Input
>The first line contains a single integer $t (1≤t≤100)$ — the number of test cases.
>Each of the following t lines describes one test case and contains two integers $h$ and $w (3≤h,w≤20)$ — the height and the width of the table.
>Output
>For each test case, print $h$ lines containing $w$ characters each. Character `j` in line `i` must be equal to `1` if you are putting a plate into cell $(i,j)$, and `0` otherwise. If there are multiple answers, print any.
>All plates must be put on the edge of the table. No two plates can be put into cells that have a common side or corner. The number of plates put on the table under these conditions must be as large as possible.
>You are allowed to print additional empty lines.
>## Example
>>input
>>```
>>3
>>3 5
>>4 4
>>5 6
>>```
>## output
>>```
>>10101
>>00000
>>10101
>>
>>0100
>>0001
>>1000
>>0010
>>
>>010101
>>000000
>>100001
>>000000
>>101010
>>```
>
>## Note
>For the first test case, example output contains the only way to put `6` plates on the table.
>For the second test case, there are many ways to put `4` plates on the table, example output contains one of them.
>Putting more than `6` plates in the first test case or more than `4` plates in the second test case is impossible.

## 译文：
>为了庆祝生日，你准备了一张喜庆的桌子!现在你想让尽可能多的客人坐下。
>桌子可以表示为一个高`h`、宽`w`的矩形，分为$h×w$个单元。设$(i,j)$表示矩形$(1≤i≤h;1≤j≤w)$。
>在桌子的每个单元中，你可以放一个盘子，也可以让它空着。
>由于每个客人都必须坐在他们的盘子旁边，你只能把盘子放在桌子的边缘——放在矩形的第一排或最后一排，或者放在第一列或最后一列。正式地说，对于每个单元格$(i,j)$你放入一个盘子，至少必须满足下列条件之一:`i=1, i=h, j=1, j=w`。
>为了让客人感到舒适，不能把两个盘子放在有共同边或角落的小房间里。换句话说,如果单元$(i, j)$包含一个盘子,你不能把盘子进入单元$(−1 j)$,$(i, j−1)$,$(i + 1, j)$,$(i, j + 1)$,$(−1 j−1)$,$(−1 j + 1)$,$(i + 1, j−1)$,$(i + 1, j + 1)$里。
>在不违反上述规则的前提下，把盘子尽可能多地放在桌子上。
>输入
>第一行包含一个整数$t(1≤t≤100)$ -测试用例的数量。
>下面的每一行描述了一个测试用例，并包含两个整数$h$和$w(3≤h,w≤20)$ -表的高度和宽度。
>输出
>对于每个测试用例，打印每个包含$w$字符的$h$行。如果你将一个盘子放入单元格$(i,j)$中，行' i '中的字符' j '必须等于' 1 '，否则' 0 '。如果有多个答案，打印any。
>所有的盘子都必须放在桌子边上。两个盘子不可以放入有共同边或角的单元中。保证在这种情况下，摆在桌子上的盘子的数量必须尽可能多。
>你可以输出额外的空行。
> ## 使用样例
>>input
>>```
>>3
>>3 5
>>4 4
>>5 6
>>```
> ## 输出
>>```
>>10101
>>00000
>>10101
>>
>>0100
>>0001
>>1000
>>0010
>>
>>010101
>>000000
>>100001
>>000000
>>101010
>>```
>
> ## 注
>对于第一个测试用例，示例输出包含将`6`个盘子放在桌子上的唯一方法。
>对于第二个测试用例，有很多方法将`4`个盘子放在桌子上，示例输出包含其中之一。
>在第一个测试用例中放置超过`6`个盘子或者在第二个测试用例中放置超过`4`个盘子是不可能的。

### 解题思路
利用题干信息对`(i,j)`周围单元进行判断，如果满足题干摆放盘子的条件，就在单元`(i,j)`填`1`.
### 代码
```cpp
#include<bits/stdc++.h>
using namespace std;

int a[24][24];
int n, m;
int check(int x, int y) {
	if (a[x + 1][y + 1] == 1 && x + 1 < n && y + 1 < m)return 0;
    if (a[x + 1][y - 1] == 1 && y - 1 >= 0 && x + 1 < n)return 0;
    if (a[x - 1][y + 1] == 1 && y + 1 < m && x - 1 >= 0)return 0;
    if (a[x - 1][y - 1] == 1 && x - 1 >= 0 && y - 1 >= 0)return 0;
    if (a[x + 1][y] == 1 && x + 1 < n)return 0;
    if (a[x - 1][y] == 1 && x - 1 >= 0)return 0;
    if (a[x][y + 1] == 1 && y + 1 < m)return 0;
    if (a[x][y - 1] == 1 && y - 1 >= 0)return 0;
    return 1;
}

int main() {
    int T;
    cin >> T;
    while(T--) {
        memset(a, 0, sizeof a);
        cin >> n >> m;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (i == 0 || i == n - 1 || j == 0 || j == m - 1) {
                    if (check(i, j) == 1) {
                        a[i][j] = 1;
                    }
                }
            }
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                cout << a[i][j];
            }
            cout << endl;
        }
        cout << endl;
    }
}


```