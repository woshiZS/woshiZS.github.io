---
title: 9月11号leetcode每日一题
toc: true
mathjax: true
date: 2020-09-11 10:27:51
password:
summary:
tags:
    - depth-first-search
    - combination
categories:
    - leetcode-daily
---
[原题链接: 组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;leetocde这几天感觉是要弄一个组合周...，总的来说还是换汤不换药，问题的关键在于去除重复的组合，重点是题目中说明不会出现重复的数字，这样就很简单了，递归函数包括两个参数，一个是剩余选取次数，一个是剩余选取大小，只有两者都为0时才能将结果push进最终答案，感觉实在没有什么好说的，确实太水了。。。
<!--more-->
### 代码
```c++
class Solution {
private:
    vector<vector<int>> ans;
public:
    void traverse(int left,int num_left,int last,vector<int>& target){
        if(left==0||num_left==0){
            if(!left&&!num_left)ans.push_back(target);
            return;
        }
        for(int i = last+1;i<=9;++i){
            if(num_left>=i){
                target.push_back(i);
                traverse(left-1,num_left-i,i,target);
                target.pop_back();
            }
        }
    }
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<int> temp;
        traverse(k,n,0,temp);
        return ans;
    }
};
```
### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里最后的时间效率超过了99%，主要是从大到小遍历进行了剪枝，按照选取次数进行时间复杂度分析，一共有C(9,k)条路径选择，每条最大深度为k，时间复杂度为O(C(9,k)*k),当然因为这里进行了剪枝，实际上会更小。空间复杂度，临时数组大小为k,栈的最大深度为k，所以空间复杂度为O(k)（终于赶在上飞机前写完了2333）。