---
title: 9月16号leetcode每日一题
toc: true
mathjax: true
date: 2020-09-16 08:58:14
password:
summary:
tags:
    - binary tree
    - recursion
categories:
    - leetcode-daily
---
[原题链接: 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;虽然这道题属于那种有手就行的，但是将核心递归函数融合在返回主函数中还是比较巧妙的，要翻转这个树，就先翻转他的左右子树，之后在把两边指针进行交换。递归边界就是遇到root为空。虽然简单，但是题目还是比较典型的。
<!--more-->
### 代码
```c++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(!root)return root;
        auto l = invertTree(root->left);
        auto r = invertTree(root->right);
        root->left = r;
        root->right = l;
        return root;
    }
};
```
### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里时间复杂度是树中节点的个数，因为我们要对每个点及其构成的子树进行翻转，空间复杂度就是栈的最大深度，这里应该是树的高度。