---
title: 9月10号leetcode每日一题
toc: true
mathjax: true
date: 2020-09-10 11:24:11
password:
summary:
tags:
    - depth-first-search
    - combination
categories:
    - leetcode-daily
---
[原题链接: 组合总和||](https://leetcode-cn.com/problems/combination-sum-ii)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;今天还是组合类型题目，无语...唯一的区别就是每个数字之可以选取一次，并且可选数字中会出现重复的数字，所以这道题最大的难点在于去重。
<!--more-->
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;要去重就要分析可能出现重复的情况，举个例子来自说，target是11，candidates是[1,10,10]，这样就会出现重复的解，为了避免这样的情况，需要做的就是在每一轮递归的过程中不要选取重复的值（这里的意思并不是说排查了一个数字选多次的情况，因为同一个数字可以再下一轮递归中进行选取），如果同一轮递归选取了两个相同的值，刚好后面的几轮递归的数字是一样的话，就会产生重复的解。另外的话，为了避免选取重样的值，可以传入index作为下轮开始选取的起点，避免重读的同时还做到了剪枝。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;另外再提到一点就是这里如果顺序是按照从小到大遍历，如果left已经小于当前可选值，可以直接return，这也算一种剪枝。

### 代码
```c++
class Solution {
private:
    vector<vector<int>> ans;
public:
    void traverse(int left,vector<int>& candi,vector<int>& container,int index){
        if(left==0){
            ans.push_back(container);
            return;
        }
        //这里是防止每轮选取中避免选到相同的数字，除了，大于等于0之外，
        int pre = 0;
        for(int i = index;i<candi.size();++i){
            if(left>=candi[i]&&candi[i]!=pre){
                pre = candi[i];
                container.push_back(candi[i]);
                traverse(left-candi[i],candi,container,i+1);
                container.pop_back();
            }
            else if(left<candi[i])return;
        }
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        vector<int> temp;
        traverse(target,candidates,temp,0);
        return ans;
    }
};
```

### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;想分析一下这里的复杂度，考虑最坏情况，没有相等的数字，并且没有一种情况可以达成有效解（target很大），每条路径走到底应该要2^n倍时间复杂，从最初的遍历算一共有n个数可供选择，当然到最后由于index传入的剪枝，时间会小于2^n，总的上限应该是O(n*2^n),系数应该小于1，这个时间复杂度大于O(nlogn)。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;空间复杂度就是O(n),取的是递归最大深度。

