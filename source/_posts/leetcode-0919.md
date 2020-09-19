---
title: leetcode_0919
toc: true
mathjax: true
date: 2020-09-19 11:32:32
password:
summary:
tags:
    - binary-tree
    - recursion
categories:
    - leetcode-daily
---
[原题链接: 左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;树本身就是一个与递归概念联系紧密的数据结构，所以一般遇到这种问题都可以用递归的方法来解决，不过刚开始没有理清题意，还以为求的是最左边的子节点，结果WA了两次。明白了题意之后在普通dfs之上添加一个参数用于标识是左子节点还是右子节点即可。
<!--more-->
### 代码
```c++
class Solution {
private:
    int ans = 0;    
public:
    void dfs(TreeNode* root,int direct){
        if(!root->left&&!root->right&&direct==0){
            ans+=root->val;
            return;
        }
        if(root->left)dfs(root->left,0);
        if(root->right)dfs(root->right,1);
    }
    int sumOfLeftLeaves(TreeNode* root) {
        if(root==nullptr)return ans;
        dfs(root,1);
        return ans;
    }
};
```
实际上也可以用层序遍历来写，只不过queue中的元素应该是一个pair，即TreeNode*和int(用于标记当前节点属于左边还是右边即可),具体代码就不po了。

### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;时间复杂度很显然就是O(N),N为节点个数，相当于说每个节点都跑一遍。空间复杂度最坏情况下为O（N），此时树为链式结构，平均空间复杂度为O（logN）。