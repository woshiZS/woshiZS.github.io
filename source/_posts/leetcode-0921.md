---
title: 9月21号leetcode每日一题
toc: true
mathjax: true
date: 2020-09-21 10:14:27
password:
summary:
tags:
    - tree
    - traverse
categories:
    - leetcode-daily
---
[原题链接: 二叉搜索树转累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;很简单的题，二叉搜索树用有个特性就是比他大的节点都在他右边，所以这里使用反向的前序遍历即可。（好像实在想不出来有什么好解释的方法，因为确实没什么思路含量...）
<!--more-->
### 代码
```c++
class Solution {
int acc = 0;
public:
    //应该要加一个标签来标记左右路径
    void travel(TreeNode* root){
        if(root==nullptr)return;
        travel(root->right);
        root->val+=acc;
        acc = root->val;
        travel(root->left);
    }
    TreeNode* convertBST(TreeNode* root) {
        travel(root);
        return root;
    }
};
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里就是用acc来记录一个累加的值，按照递归顺序加上所遍历节点的值即可。

### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里时间空间复杂度就是常见的树的遍历那种，O（N）时间，O(logN)空间。