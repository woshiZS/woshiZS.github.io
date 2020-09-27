---
title: 9月27号leetcode每日一题
toc: true
mathjax: true
date: 2020-09-27 09:16:20
password:
summary:
tags:
    - tree
    - postorder-traversal
categories:
    - leetcode-daily
---
[原题链接: 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;刚开始还没有看到二叉搜索树这个条件，就按照普通二叉树去做的，但是其实仔细考虑二叉搜索树这个条件其实很好写递归，因为祖先节点的值肯定在目标两节点的中间，如果当前遍历节点的值在两个目标节点值的中间就一定是那个公共节点，这是因为这种情况下，p,q两个节点分布在左右子树，所以寻找的节点必然是root节点。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果不是分别在两棵子树上，然就是两个节点都在左边或者都在右边，所以此时root->val会同时大于或者同时小于两个节点的值。，按照这个去写递归即可。
<!--more-->
### 代码
```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root->val<p->val&&root->val<q->val)return lowestCommonAncestor(root->right,p,q);
        if(root->val>p->val&&root->val>q->val)return lowestCommonAncestor(root->left,p,q);
        return root;
    }
};
```
### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 这道题的时间复杂度，在最坏情况下为O（N），即在树退化成链表的的时候，平均时间复杂度是O(K),K为二叉树的高度。空间复杂度最坏和时间复杂度最坏的情况相同，平均空间复杂度也是O（K）。