---
title: 9月26日leetcode每日一题
toc: true
mathjax: true
date: 2020-09-26 10:48:01
password:
summary:
tags:
    - depth-first-search
    - tree
categories:
    - leetcode-daily
---
[原题链接: 路径总和II](https://leetcode-cn.com/problems/path-sum-ii/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题之前也写过了，思维量也不大，使用简单dfs就可以解决。大致方法就是从根节点开始递归，然后对于非空的子节点进行递归，递归边界条件为遍历当前节点左右子节点均为空。仔细想了一下好像其他也没有什么要注意的点，另外就是root可能为空，所以要考虑一下cornerc case。
<!--more-->
### 代码
```c++
class Solution {
public:
    vector<vector<int>> ans;int target;
    void travel(TreeNode* root,vector<int>& path,int total){
        if(!root->left&&!root->right){
            if(total == target)ans.push_back(path);
            return;
        }
        if(root->left){
            path.push_back(root->left->val);
            travel(root->left,path,total+root->left->val);
            path.pop_back();
        }
        if(root->right){
            path.push_back(root->right->val);
            travel(root->right,path,total+root->right->val);
            path.pop_back();
        }
    }
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        if(!root)return ans;
        target = sum;
        vector<int> temp;
        temp.push_back(root->val);
        travel(root,temp,root->val);
        return ans;
    }
};
```
### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本题的时间复杂度O（N），因为递归要走遍所有的节点，还不能够剪枝，因为题目也没有保证节点的值不可以为负数，所以每一个节点都要遍历到。空间复杂度为O（H），H为树的最大高度，即递归所产生的的最大栈空间。