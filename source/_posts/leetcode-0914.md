---
title: 9月14号leetcode每日一题
toc: true
mathjax: true
date: 2020-09-14 11:28:48
password:
summary:
tags:
    - binary tree
    - inorder traverse
categories:
    - leetcode-daily
---
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;很经典的题目，对于熟练一定的模板还是很有作用的，但刚开始的时候并没有看清楚题目，差点写成了前序遍历，这里就纯当练手了吧，做过太多遍了。。。
<!--more-->
### 代码
```c++
vector<int> inorderTraverse(TreeNode* root){
    vector<int> ans;
    vector<TreeNode*> stk;
    TreeNode* curr = root;
    while(curr||!stk.empty()){
        while(curr){
            stk.push(curr);
            curr = curr->left;
        }
        curr = stk.top();
        ans.push_back(curr->val);
        curr = curr->right;
        stk.pop();
    }
}
```

### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;时间复杂度很显然是树中所有节点的数量n，O（n）。空间复杂度就是栈所占用的空间，这里应该是与树的最大高度成正比O（h）。