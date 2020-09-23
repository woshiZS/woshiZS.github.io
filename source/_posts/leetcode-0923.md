---
title: 9月23日leetcode每日一题
toc: true
mathjax: true
date: 2020-09-23 10:27:25
password:
summary:
tags:
    - tree
categories:
    - leetcode-daily
---
[原题链接: 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本道题其实类似于链表合并，但是这个链表变成了二叉树，当然最开始写还有点不太适应这种写法，但是写法确实还是十分类似的。
<!--more-->
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当遍历到某个节点，一个节点为空的时候就直接返回另一个节点。另外就改变其中一个子树节点的值，注意这里不要再次构造新的节点，会消耗多余的空间和时间，原地上进行改动就好了。
### 代码
```c++
class Solution {
public:
    //堆内分配内存并不会随着栈的返回二失效
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if(t1==nullptr)return t2;
        if(t2==nullptr)return t1;
        t1->val += t2->val;
        t1->left = mergeTrees(t1->left,t2->left);
        t1->right = mergeTrees(t1->right,t2->right);
        return t1;
    }
};
```
### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;时空复杂度取的两者覆盖新生成二叉树遍历所需的时空复杂度。