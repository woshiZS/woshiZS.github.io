---
title: 9月24日leetcode每日一题
toc: true
mathjax: true
date: 2020-09-24 17:36:00
password:
summary:
tags:
    - tree
    - morris traversal
categories:
    - leetcode-daily
---
[原题链接: 二叉树中的众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;其实这种题目还蛮恶心的，因为虽然标注的难度是简单，但是往往还有进阶做法，像O（n）时间，O（1）空间这种。像今天这个就属于之前没学过就没法做的那种，题解采用的是Morris遍历以达到O（1）的时间复杂度。
<!--more-->
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Morris中序遍历有点像是将二叉树按照一个单链表展开，在进行中序遍历的时候会找到左子树的最右节点，将最右节点的右指针指向当前遍历节点，当然还会出现一种情况就是左子树的最右节点指向当前遍历节点说明成环了，所以这个时候我们应该将最右节点的右指针悬空来避免成环。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对于从这样一个非递减的序列中寻找众数，每一串相同的数字都是连续的，所以我们只要判断最长连续数字的长度即可。
### 代码
```c++
class Solution {
private:
    int base,cnt = 0,maxcnt = 0;
    vector<int> ans;
public:
    void update(int val){
        // printf("%d\n",val);
        if(val==base){
            ++cnt;
        }
        else{
            base = val;
            cnt = 1;
        }
        if(cnt==maxcnt)ans.push_back(val);
        else if(cnt > maxcnt){
            maxcnt = cnt;
            ans = {val};
        }
    }
    vector<int> findMode(TreeNode* root) {
        while(root){
            if(!root->left){
                update(root->val);
                root = root->right;
                continue;
            }
            TreeNode* pre = root->left;
            while(pre->right&&pre->right!=root)pre = pre->right;
            if(pre->right==nullptr){
                pre->right = root;
                root = root->left;
            }
            else{
                //避免成环
                pre->right = nullptr;
                update(root->val);
                root = root->right;
            }
        }
        return ans;
    }
};
```

### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;总的来说，时间复杂度还是O（n）,空间复杂度是O（1），感觉理解这个算法的关键在于自己动手去画一遍，不然将右节点右指针悬空那里确实不太好理解。