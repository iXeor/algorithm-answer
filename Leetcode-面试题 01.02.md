# Leetcode 面试题 01.02. 判定是否互为字符重排
## 题目内容
给定两个字符串 `s1` 和 `s2`，请编写一个程序，确定其中一个字符串的字符重新排列后，能否变成另一个字符串。
```
示例 1：
输入: s1 = "abc", s2 = "bca"
输出: true

示例 2：
输入: s1 = "abc", s2 = "bad"
输出: false
```
```
说明：

0 <= len(s1) <= 100
0 <= len(s2) <= 100
```
## 解题思路
判定重排，按照本人的偷懒思路，继续用现成方法。

1）把两组字符串重新排序

2）如果两组字符串是互为重排的，两字符串必然相等，直接返回true就结束了

3）不相等返回false

## 结果截图
![image.png](https://pic.leetcode-cn.com/1632877320-QcCQSk-image.png)

## 代码

```cpp
class Solution {
public:
    bool CheckPermutation(string s1, string s2) {
        sort(s1.begin(),s1.end());
        sort(s2.begin(),s2.end());
        if(s1 == s2) return true;
        return false;
    }
};
```
应某老哥需求，试着撕一下RB-Tree.
手撕咱也撕不好，就瞎鸡儿写一个吧。本人垃圾代码制造机。还得继续学习，不然丢人（写完自己看一遍确实也挺丢人，还请老哥们见谅，多加指导）
![image.png](https://pic.leetcode-cn.com/1638885813-sMddpu-image.png)
0）判断两字符串长度是否相等，相等就继续下述内容，不等直接`false`

1）使用`insert`将`s1`装入RBTree容器

2）使用`remove`照着`s2`的内容移除

3）能移除干净，可以重排至相等，返回`true`，否则不可以，返回`false`。

## 代码

```cpp
#include <iomanip>
using namespace std;

enum RBTColor{RED, BLACK};

template <class T>
class RBTNode{
    public:
        RBTColor color;    // 颜色
        T key;            // 关键字(键值)
        RBTNode *left;    // 左节点
        RBTNode *right;    // 右节点
        RBTNode *parent; // 父节点

        RBTNode(T value, RBTColor c, RBTNode *p, RBTNode *l, RBTNode *r):
            key(value),color(c),parent(),left(l),right(r) {}
};

template <class T>
class RBTree {
    private:
        RBTNode<T> *mRoot;    // 根节点
    public:
        RBTree();
        ~RBTree();
        RBTNode<T>* search(T key);
        RBTNode<T>* iterativeSearch(T key);
        T minimum();
        T maximum();
        RBTNode<T>* successor(RBTNode<T> *x);
        RBTNode<T>* predecessor(RBTNode<T> *x);
        void insert(T key);
        void remove(T key);
        void destroy();
        bool isNull();
    private:
        RBTNode<T>* search(RBTNode<T>* x, T key) const;
        RBTNode<T>* iterativeSearch(RBTNode<T>* x, T key) const;
        RBTNode<T>* minimum(RBTNode<T>* tree);
        RBTNode<T>* maximum(RBTNode<T>* tree);
        void leftRotate(RBTNode<T>* &root, RBTNode<T>* x);
        void rightRotate(RBTNode<T>* &root, RBTNode<T>* y);
        void insert(RBTNode<T>* &root, RBTNode<T>* node);
        void insertFixUp(RBTNode<T>* &root, RBTNode<T>* node);
        void remove(RBTNode<T>* &root, RBTNode<T> *node);
        void removeFixUp(RBTNode<T>* &root, RBTNode<T> *node, RBTNode<T> *parent);
        void destroy(RBTNode<T>* &tree);
        bool isNull(RBTNode<T>* &tree);

#define rb_parent(r)   ((r)->parent)
#define rb_color(r) ((r)->color)
#define rb_is_red(r)   ((r)->color==RED)
#define rb_is_black(r)  ((r)->color==BLACK)
#define rb_set_black(r)  do { (r)->color = BLACK; } while (0)
#define rb_set_red(r)  do { (r)->color = RED; } while (0)
#define rb_set_parent(r,p)  do { (r)->parent = (p); } while (0)
#define rb_set_color(r,c)  do { (r)->color = (c); } while (0)
};

template <class T>
RBTree<T>::RBTree():mRoot(NULL)
{
    mRoot = NULL;
}

template <class T>
RBTree<T>::~RBTree()
{
    destroy();
}

template <class T>
RBTNode<T>* RBTree<T>::search(RBTNode<T>* x, T key) const
{
    if (x==NULL || x->key==key)
        return x;

    if (key < x->key)
        return search(x->left, key);
    else
        return search(x->right, key);
}

template <class T>
RBTNode<T>* RBTree<T>::search(T key)
{
    search(mRoot, key);
}

template <class T>
RBTNode<T>* RBTree<T>::iterativeSearch(RBTNode<T>* x, T key) const
{
    while ((x!=NULL) && (x->key!=key))
    {
        if (key < x->key)
            x = x->left;
        else
            x = x->right;
    }

    return x;
}

template <class T>
RBTNode<T>* RBTree<T>::iterativeSearch(T key)
{
    iterativeSearch(mRoot, key);
}

template <class T>
RBTNode<T>* RBTree<T>::minimum(RBTNode<T>* tree)
{
    if (tree == NULL)
        return NULL;

    while(tree->left != NULL)
        tree = tree->left;
    return tree;
}

template <class T>
T RBTree<T>::minimum()
{
    RBTNode<T> *p = minimum(mRoot);
    if (p != NULL)
        return p->key;

    return (T)NULL;
}

template <class T>
RBTNode<T>* RBTree<T>::maximum(RBTNode<T>* tree)
{
    if (tree == NULL)
        return NULL;

    while(tree->right != NULL)
        tree = tree->right;
    return tree;
}

template <class T>
T RBTree<T>::maximum()
{
    RBTNode<T> *p = maximum(mRoot);
    if (p != NULL)
        return p->key;

    return (T)NULL;
}

template <class T>
RBTNode<T>* RBTree<T>::successor(RBTNode<T> *x)
{
    if (x->right != NULL)
        return minimum(x->right);
    RBTNode<T>* y = x->parent;
    while ((y!=NULL) && (x==y->right))
    {
        x = y;
        y = y->parent;
    }

    return y;
}

template <class T>
RBTNode<T>* RBTree<T>::predecessor(RBTNode<T> *x)
{
    if (x->left != NULL)
        return maximum(x->left);
    RBTNode<T>* y = x->parent;
    while ((y!=NULL) && (x==y->left))
    {
        x = y;
        y = y->parent;
    }

    return y;
}

template <class T>
void RBTree<T>::leftRotate(RBTNode<T>* &root, RBTNode<T>* x)
{
    RBTNode<T> *y = x->right;
    x->right = y->left;
    if (y->left != NULL)
        y->left->parent = x;
    y->parent = x->parent;

    if (x->parent == NULL)
    {
        root = y;
    }
    else
    {
        if (x->parent->left == x)
            x->parent->left = y;
        else
            x->parent->right = y;
    }
    y->left = x;
    x->parent = y;
}

template <class T>
void RBTree<T>::rightRotate(RBTNode<T>* &root, RBTNode<T>* y)
{
    RBTNode<T> *x = y->left;
    y->left = x->right;
    if (x->right != NULL)
        x->right->parent = y;
    x->parent = y->parent;

    if (y->parent == NULL)
    {
        root = x;
    }
    else
    {
        if (y == y->parent->right)
            y->parent->right = x;
        else
            y->parent->left = x;
    }
    x->right = y;
    y->parent = x;
}

template <class T>
void RBTree<T>::insertFixUp(RBTNode<T>* &root, RBTNode<T>* node)
{
    RBTNode<T> *parent, *gparent;
    while ((parent = rb_parent(node)) && rb_is_red(parent))
    {
        gparent = rb_parent(parent);
        if (parent == gparent->left)
        {
            {
                RBTNode<T> *uncle = gparent->right;
                if (uncle && rb_is_red(uncle))
                {
                    rb_set_black(uncle);
                    rb_set_black(parent);
                    rb_set_red(gparent);
                    node = gparent;
                    continue;
                }
            }
            if (parent->right == node)
            {
                RBTNode<T> *tmp;
                leftRotate(root, parent);
                tmp = parent;
                parent = node;
                node = tmp;
            }
            rb_set_black(parent);
            rb_set_red(gparent);
            rightRotate(root, gparent);
        }
        else
        {
            {
                RBTNode<T> *uncle = gparent->left;
                if (uncle && rb_is_red(uncle))
                {
                    rb_set_black(uncle);
                    rb_set_black(parent);
                    rb_set_red(gparent);
                    node = gparent;
                    continue;
                }
            }
            if (parent->left == node)
            {
                RBTNode<T> *tmp;
                rightRotate(root, parent);
                tmp = parent;
                parent = node;
                node = tmp;
            }
            rb_set_black(parent);
            rb_set_red(gparent);
            leftRotate(root, gparent);
        }
    }
    rb_set_black(root);
}

template <class T>
void RBTree<T>::insert(RBTNode<T>* &root, RBTNode<T>* node)
{
    RBTNode<T> *y = NULL;
    RBTNode<T> *x = root;
    while (x != NULL)
    {
        y = x;
        if (node->key < x->key)
            x = x->left;
        else
            x = x->right;
    }

    node->parent = y;
    if (y!=NULL)
    {
        if (node->key < y->key)
            y->left = node;
        else
            y->right = node;
    }
    else
        root = node;
    node->color = RED;
    insertFixUp(root, node);
}

template <class T>
void RBTree<T>::insert(T key)
{
    RBTNode<T> *z=NULL;
    if ((z=new RBTNode<T>(key,BLACK,NULL,NULL,NULL)) == NULL)
        return ;

    insert(mRoot, z);
}

template <class T>
void RBTree<T>::removeFixUp(RBTNode<T>* &root, RBTNode<T> *node, RBTNode<T> *parent)
{
    RBTNode<T> *other;

    while ((!node || rb_is_black(node)) && node != root)
    {
        if (parent->left == node)
        {
            other = parent->right;
            if (rb_is_red(other))
            {
                rb_set_black(other);
                rb_set_red(parent);
                leftRotate(root, parent);
                other = parent->right;
            }
            if ((!other->left || rb_is_black(other->left)) &&
                (!other->right || rb_is_black(other->right)))
            {
                rb_set_red(other);
                node = parent;
                parent = rb_parent(node);
            }
            else
            {
                if (!other->right || rb_is_black(other->right))
                {
                    rb_set_black(other->left);
                    rb_set_red(other);
                    rightRotate(root, other);
                    other = parent->right;
                }
                rb_set_color(other, rb_color(parent));
                rb_set_black(parent);
                rb_set_black(other->right);
                leftRotate(root, parent);
                node = root;
                break;
            }
        }
        else
        {
            other = parent->left;
            if (rb_is_red(other))
            {
                rb_set_black(other);
                rb_set_red(parent);
                rightRotate(root, parent);
                other = parent->left;
            }
            if ((!other->left || rb_is_black(other->left)) &&
                (!other->right || rb_is_black(other->right)))
            {
                rb_set_red(other);
                node = parent;
                parent = rb_parent(node);
            }
            else
            {
                if (!other->left || rb_is_black(other->left))
                {
                    rb_set_black(other->right);
                    rb_set_red(other);
                    leftRotate(root, other);
                    other = parent->left;
                }
                rb_set_color(other, rb_color(parent));
                rb_set_black(parent);
                rb_set_black(other->left);
                rightRotate(root, parent);
                node = root;
                break;
            }
        }
    }
    if (node)
        rb_set_black(node);
}

template <class T>
void RBTree<T>::remove(RBTNode<T>* &root, RBTNode<T> *node)
{
    RBTNode<T> *child, *parent;
    RBTColor color;
    if ( (node->left!=NULL) && (node->right!=NULL) )
    {
        RBTNode<T> *replace = node;
        replace = replace->right;
        while (replace->left != NULL)
            replace = replace->left;
        if (rb_parent(node))
        {
            if (rb_parent(node)->left == node)
                rb_parent(node)->left = replace;
            else
                rb_parent(node)->right = replace;
        }
        else
            root = replace;
        child = replace->right;
        parent = rb_parent(replace);
        color = rb_color(replace);
        if (parent == node)
        {
            parent = replace;
        }
        else
        {
            if (child)
                rb_set_parent(child, parent);
            parent->left = child;

            replace->right = node->right;
            rb_set_parent(node->right, replace);
        }

        replace->parent = node->parent;
        replace->color = node->color;
        replace->left = node->left;
        node->left->parent = replace;

        if (color == BLACK)
            removeFixUp(root, child, parent);

        delete node;
        return ;
    }
    if (node->left !=NULL)
        child = node->left;
    else
        child = node->right;
    parent = node->parent;
    color = node->color;

    if (child)
        child->parent = parent;
    if (parent)
    {
        if (parent->left == node)
            parent->left = child;
        else
            parent->right = child;
    }
    else
        root = child;

    if (color == BLACK)
        removeFixUp(root, child, parent);
    delete node;
}

template <class T>
void RBTree<T>::remove(T key)
{
    RBTNode<T> *node;
    if ((node = search(mRoot, key)) != NULL)
        remove(mRoot, node);
}

template <class T>
void RBTree<T>::destroy(RBTNode<T>* &tree)
{
    if (tree==NULL)
        return ;

    if (tree->left != NULL)
        return destroy(tree->left);
    if (tree->right != NULL)
        return destroy(tree->right);

    delete tree;
    tree=NULL;
}

template <class T>
void RBTree<T>::destroy()
{
    destroy(mRoot);
}

template <class T>
bool RBTree<T>::isNull(RBTNode<T>* &tree)
{
    if(tree == NULL) return true;
    else return false;
}

template <class T>
bool RBTree<T>::isNull()
{
    return isNull(mRoot);
}

class Solution {
public:
    bool CheckPermutation(string s1, string s2) {
        if(s1.size() != s2.size()) return false;
        RBTree<char>* rbt = new RBTree<char>();
        for(int i = 0 ; i < s1.size() ; i++)
        {
            rbt->insert(s1[i]);
        }
        for(int i = 0 ; i < s2.size() ; i++)
        {
            rbt->remove(s2[i]);
        }
        return rbt->isNull() ? true : false;
    }
};
```
