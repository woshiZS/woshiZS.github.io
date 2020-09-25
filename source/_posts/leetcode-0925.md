---
title: 9月25日leetocde每日一题
toc: true
mathjax: true
date: 2020-09-25 13:05:33
password:
summary:
tags:
    - tree
categories:
    - leetcode-daily
---
[原题链接: 中序遍历和后序遍历还原二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题其实还挺常规的，大致的一个思路就是中序遍历根节点会二分左右子树，后序遍历的最后一个节点为根节点，然后利用分治的思想进行左右子树的递归即可。 题目很常规了，几乎没有难度，熟悉套路就好了。
<!--more-->
### 代码
```c++
class Solution {
vector<int> _in;
vector<int> _post;
public:
    TreeNode* build(int lm,int rm,int lp,int rp){
        if(lm>rm)return nullptr;
        TreeNode* ans = new TreeNode(_post[rp]);
        int i;
        for(i = lm;i<=rm;++i){
            if(_in[i]==ans->val)break;
        }
        ans->left = build(lm,i-1,lp,lp+i-1-lm);
        ans->right = build(i+1,rm,lp+i-lm,rp-1);
        return ans;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        _in = inorder;
        _post = postorder;
        return build(0,inorder.size()-1,0,postorder.size()-1);
    }
};
```
### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题的时间复杂度最坏情况下为O（N^2）,极端情况下为树只有左子树或者右子树。空间复杂度最会也为O(N)，一般情况下为O(H)，H是树的高度。