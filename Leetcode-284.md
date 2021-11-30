# Leetcode 284. 窥探迭代器
### 知识拓展——什么是迭代器？
迭代器（iterator）有时又称光标（cursor）是程序设计的软件设计模式，可在容器对象（container，比如链表或者数组）上遍访的接口。
### 解题思路
按照题干说明，我们这次需要实现一个`PeekingIterator`类（构造体），这个类（构造体）除了支持`hasNext`和`next`操作外，还需要支持`peek`操作。
那么现在有如下已知条件：
1. `int peek()` 返回数组中的下一个元素，但**不移动**指针。
2. `int next()` 返回数组中的下一个元素，并将指针**移动到下个元素**处。
3. `PeekingIterator(int[] nums)` 使用指定整数数组`nums`初始化迭代器。
4. 当我们把语言环境切换到**C**的时候，我们会发现好家伙，这力扣白送俩方法，不用白不用：    
```c
/*
 *	struct Iterator {
 *		// Returns true if the iteration has more elements.
 *      // 如果迭代器的内部指针往下走还有元素，就返回一个true
 *		bool (*hasNext)();
 *
 * 		// Returns the next element in the iteration.
 *      返回迭代器中位于当前元素之后的下一个元素（返回了一个整数）
 *		int (*next)();
 *	};
 */

```
好，`PeekingIterator`构造体喜加一。那我直接再在`PeekingIterator`里放一个专门存下一个元素的位置。
那么现在我们的`struct PeekingIterator`就是这样的了：
```c
struct PeekingIterator {
    struct Iterator *iterator;
    int nextElement;
};
```
然后按照题干，我们应该实现一个构造（Constructor）函数，那么我的思路是这样的：
1. 我的`PeekingIterator`构造体需要先初始化
2. 然后借用题目中给的`struct Iterator`
3. 我得立个全局的`flag`，这样可以在随便一个位置用来看是否还有下一个元素。
4. `nextElement`要给它传下一个元素的值
那么这部分就变成了这样：
```c
struct PeekingIterator* Constructor(struct Iterator* iter) {
    struct PeekingIterator* piter = malloc(sizeof(struct PeekingIterator)); 
    //初始化一个PeekingIterator* 型的参数piter，并给它一定的发展空间
    piter->iterator = iter; //把piter中的iterator和传入的struct Iterator* iter链接
    flag = piter->iterator->hasNext(); //flag先存首个元素之后是否还有下一个元素
    if(flag)
    {
        piter->nextElement = piter->iterator->next();//nextElement存入下一个元素的值
    }
    return piter;
}
```
准备工作结束了，那这个时候就需要开始实现`hasNext`，`next`和`peek`方法了。
先从简单的来
`bool hasNext()`如果数组中存在下一个元素，返回 `true`；否则，返回 `false`。
那我直接返回当前最新的`flag`就可以了：
```c
bool hasNext(struct PeekingIterator* obj) {
    return flag;
}
```
`int peek()` 返回数组中的下一个元素，但**不移动**指针。
既然不移动指针，那就直接返回当前指针之后一个位置的元素。刚才我们定义的`nextElement`马上闪亮登场了属于是：
```c
int peek(struct PeekingIterator* obj) {
    return obj->nextElement;
}
```
重头戏来了，我们的`int next()`还没有实现，往上翻费劲，那么我们在这复习一遍。
`int next()`和`int peek()`的区别是：`int next()`返回数组中的下一个元素，并将指针**移动到下个元素**处而`int peek()`**不移动**指针。
那么我们需要针对指针进行一些小动作：
1. 指针移动后`nextElement`中的数值需要更新
2. `flag`需要更新
那么这样就好办了，我们在`int peek()`的基础上进行修改：
```c
int next(struct PeekingIterator* obj) {
    int ret = obj->nextElement; //ret用于存储返回结果。
    if(flag)    //如果下面还有元素
    {
        flag = obj->iterator->hasNext(); //flag先更新
        obj->nextElement = obj->iterator->next(); //nextElement数值更新顺带指针下移
    }
    return ret; //返回结果值
}
```

### 整体代码如下：

```c
bool flag;

struct PeekingIterator {
    struct Iterator *iterator;
    int nextElement;
};

struct PeekingIterator* Constructor(struct Iterator* iter) {
    struct PeekingIterator* piter = malloc(sizeof(struct PeekingIterator));
    piter->iterator = iter;
    flag = piter->iterator->hasNext();
    if(flag)
    {
        piter->nextElement = piter->iterator->next();
    }
    return piter;
}

int peek(struct PeekingIterator* obj) {
    return obj->nextElement;
}

int next(struct PeekingIterator* obj) {
    int ret = obj->nextElement;
    if(flag)
    {
        flag = obj->iterator->hasNext();
        obj->nextElement = obj->iterator->next();
    }
    return ret;
}

bool hasNext(struct PeekingIterator* obj) {
    return flag;
}
```