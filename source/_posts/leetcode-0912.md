---
title: 9月12号leetcodeme每日一题
toc: true
mathjax: true
date: 2020-09-12 07:34:40
password:
summary:
tags:
    - breadth-first-search
    - queue
categories:
    - leetcode-daily
---
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题就没有什么好说的，经典层序遍历，记录每层的一个size值即可。没有什么价值可言...（就随便贴一下代码好了233）
<!--more-->
### 代码
```c++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> ans;
        queue<TreeNode*> q;
        if(root==nullptr)return ans;
        q.push(root);
        while(!q.empty()){
            int cnt = q.size();double sum = 0;
            for(int i = 0;i<cnt;++i){
                TreeNode* temp = q.front();
                sum+=temp->val;
                if(temp->left)q.push(temp->left);
                if(temp->right)q.push(temp->right);
                q.pop();
            }
            ans.push_back(sum/cnt);
        }
        return ans;
    }
};
```

### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;时间复杂度感觉就O（n），n为树中所有节点的数量，今天的博客就先水一水，看楼教主打比赛去了。