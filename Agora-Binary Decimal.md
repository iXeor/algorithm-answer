# Agora声网笔试题-Binary Decimal（[codeforces #1530-A](https://codeforces.com/contest/1530/problem/A)）
声网直接用的原题，改都没改。直接上题干：
>Let's call a number a binary decimal if it's a positive integer and all digits in its decimal notation are either `0`or `1`. For example, `1010111` is a binary decimal, while `10201` and `787788` are not.
>Given a number `n`, you are asked to represent n as a sum of some (not necessarily distinct) binary decimals. Compute the smallest number of binary decimals required for that.
>## Input
>The first line contains a single integer `t` $(1≤t≤1000)$, denoting the number of test cases.
>The only line of each test case contains a single integer `n` $(1≤n≤109)$, denoting the number to be represented.
>## Output
>For each test case, output the smallest number of binary decimals required to represent n as a sum.
>## Example
>>## input
>>```
>>3
>>121
>>5
>>1000000000
>
>## output
>>```
>>2
>>5
>>1
>
>Note
>In the first test case, `121` can be represented as $121=110+11$ or $121=111+10$.
>In the second test case, `5` can be represented as $5=1+1+1+1+1$.
>In the third test case, `1000000000` is a binary decimal itself, thus the answer is `1`.
## 译文：
>我们称一个数为二进制化十进制数，如果它是一个正整数，并且它的十进制表示法中的所有数字都是`0`或`1`。例如，`1010111`是二进制化十进制数，而`10201`和`787788`不是。
给定一个数字`n`，请你用一些(不一定是不同的)二进制化十进制数的和来表示n。计算所需的最小二进制化十进制数个数。
> ## 输入
>第一行包含一个整数`t` $(1≤t≤1000)$，表示测试用例的数量。
>每个测试用例的唯一一行包含一个整数`n` $(1≤n≤109)$，表示要表示的数字。
> ## 输出
对于每个测试用例，输出将n表示为一个和所需要的最小的二进制化十进制数个数。
> ## 样例
> > ## 输入
>>```
>>3
>>121
>>5
>>1000000000
>
> ## 输出
>>```
>>2
>>5
>>1
>
> ## 注
>在第一个测试用例中，`121`可以表示为$121=110+11$或$121=111+10$。
>在第二个测试用例中，`5`可以表示为$5=1+1+1+1+1$。
>在第三个测试用例中，`1000000000`本身是一个二进制化十进制数，因此答案是`1`。

### 解题思路：
尽可能取大的二进制化十进制数，以减少使用数量。为了方便同时表示二进制化十进制数和普通数字，所以使用`string`类型进行存储，通过ASCII码转换回数字。
### 代码：
```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
	int t;
	string s;
	cin>>t;
	
	while(t--)
	{	int max=0;
		cin>>s;
		for(int i=0;i<s.length();i++)
		{
			if(s[i]-'0'>max)
			max=s[i]-'0';
		}
		cout<<max<<endl;
	}
}
```
