---
title: 9月18号leetcode每日一题
toc: true
mathjax: true
date: 2020-09-18 01:06:12
password:
summary:
tags:
    - permutation
    - depth-first-search
categories:
    - leetcode-daily
---
[原题链接: 全排列](https://leetcode-cn.com/problems/permutations-ii/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;感觉这道题有点回到了之前一大堆组合题目的那段时间，其实思路还是很明朗的，主要就是怎么去重，其他的就按照常规的dfs+回溯即可。
<!--more-->
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;个人认为去重的关键在于你在同一个位置上选取了相同的数，但是这个相同的数字处于不同的位置，并且这两个解是相同的。为了避免这种情况，我们可以在每次递归中再设置一个bool数组，用来标记本轮递归中已经出现的值，如果后面的位置中还出现相同的值，那么就直接continue。
### 代码
```c++
class Solution {
private:
     vector<vector<int>> ans;
     vector<bool> book;
public:
    //想一想每次递归前后要不要标记
    void dfs(int left,vector<int>& nums,vector<int>& target){
        if(left==0){
            ans.push_back(target);
            return;
        }
        unordered_map<int,bool> mp;
        for(int i = 0;i<nums.size();++i){
            if(book[i]==false&&mp[nums[i]]==0){
                book[i] = true;mp[nums[i]] = true;
                target.push_back(nums[i]);
                dfs(left-1,nums,target);
                target.pop_back();
                book[i] = false;
            }
        } 
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        //重点在于去重，然而去重的关键在于确定每一轮选择之中已经选择过的数字不要再次选择。
        int n = nums.size();
        book.resize(n,false);
        vector<int> temp;
        dfs(n,nums,temp);
        return ans;
    }
};
```
### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;时间复杂度在没有重复数字的情况下是O(n!)，空间复杂度，栈深度最大为n,每层栈里面还会开一个map，空间复杂度在最坏得情况下是O(n^2)。

### 碎碎念
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;今天收到字节的感谢信了，有点意外，因为毕竟就两个问题答得不是特别好，有可能是今天答的时候太随意了还是啥。总之有点不甘,垃圾评审给:older_man:爬（其实实习的原因主要贪字节钱多，但是这种想法可能不是太对，但是可以自由支配一定金钱的感觉确实不错，可以干更多想干的事情）。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;不过这样一来也有好处，就是我有更多时间去捣鼓自己的代码了，下一步应该是把填表代码开源到github，博客开通评论系统， 并且提前把机器学习课程的大作业做好，再按照学长的推荐，跟一个好点的小老板干活。不要气馁，继续认真做好自己的事就好了。