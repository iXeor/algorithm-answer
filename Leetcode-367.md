# Leetcode 367. 有效的完全平方数
### 说在前面
题解灵感源自Quake III的`rsqrt()`方法，一些大佬对于`0x5f375a86`和`0x1fbff0a3`两个数字的由来表示困惑。那么接下来我要讲的，大部分是关于这两个值是怎么来的。可能讲的不太清楚，还请补充。
### 解题思路
我们需要利用浮点数表示的标准，**IEEE 754**实现它的快速开方。
`float`型浮点数强制转换成`int`型整数的时候满足以下一种公式：
$$I_x ≈ L×log_2​x+L×(B−σ) $$
其中$I_x$表示从`float`型浮点数强转后的`int`型整数，$L$是常数，且在这个式子中$L = 2^{23}$，$x$是原始的浮点数（尚未表示成`float`型），$B=127$，$σ$是一个小量。通过化简公式我们得到下列式子：
$$log_2(x)≈I_x/L-(B−σ)$$
我们先说说`rsqrt()`方法，它要求的是$\frac{1}{\sqrt{x}}$的粗略值。那么现在我们假定$y = \frac{1}{\sqrt{x}}$,则$log_2y = -\frac{1}{2}log_2x$，带入上式可得：
$$I_y ≈ \frac{3}{2}L×(B−σ)-\frac{1}{2}I_X$$
在前面我们只知道σ是无穷小量，但是并不知道它的具体值，通过推导得到：当$σ ≈ 0.0450466$时，我们$\frac{3}{2}L×(B−σ)$的值为`0x5f3759df`，后来Lomont推导（有人说是暴力穷举）出了更好一些的`0x5f375a86`。
我们这道题实际需要求的不是$\frac{1}{\sqrt{x}}$，而是$\sqrt{x}$，同理，假设$y = \sqrt{x}$，则$log_2y = \frac{1}{2}log_2x$，同样带入公式$log_2(x)≈I_x/L-(B−σ)$得到：
$$I_y ≈ \frac{1}{2}L×(B−σ)+\frac{1}{2}I_x$$
此时，假设$σ = 0$，则$\frac{1}{2}L×(B−σ)$的值为`0x1fc00000`，假设$σ ≈ 0.0450466$，则$\frac{1}{2}L×(B−σ)$的值为`0x1fbd1e2d`。有人就问了，那这和你那个什么`0x1fbff0a3`也差太多了。我摊牌，我用暴力穷举法找到的`0x1fbff0a3`，它可以保证小数点后两位是相对准确的，对于这套题暂时来说还是够用的。我们需要的结果实际上是$I_y$的值，即代码中的`i =(i >> 1) + 0x1fbff0a3;`，但是这个值并不准确，那么接下来怎么处理呢？当然是牛顿法处理让它更准确咯。牛顿法大致使用的公式如下所示：
$$x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}$$
在这里$f(y) = y^2-x$,$x$是输入的定值，则$f'(y) = 2y$
带入牛顿法公式可得：
$$y_{n+1} = y_n - \frac{y_n^2-x}{2y_n} = \frac{1}{2}(y_n+\frac{x}{y_n})$$

即代码中的`y = 0.5*y+0.5*number/y;`

### 代码
``` c
    float mysqrt(float number)
    {
	    int i;
	    float y;
	    i = *(int*)&number;
	    i =(i >> 1) + 0x1fbff0a3;
	    y = *(float*)&i;
	    y = 0.5*y+0.5*number/y;
	    y = 0.5*y+0.5*number/y;
	    return y;
    }
    bool isPerfectSquare(int num) {
        int num0 = (int)mysqrt(num);
        int num1 = num/num0;
        int num2 = num%num0;
        return num0 == num1 && num2 == 0 ? true : false;
    }
```
