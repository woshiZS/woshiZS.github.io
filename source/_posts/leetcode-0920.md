---
title: leetcode_0920
toc: true
mathjax: true
date: 2020-09-20 15:39:11
password:
summary:
tags:
    - depth-first-search
    - dynamic programming
categories:
    - leetocde-daily
---
[原题链接: https://leetcode-cn.com/problems/subsets/]
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这种回溯类的题目都要写吐了，组合的问题其实也和之前大同小异，这里也可以按照一定顺序，对每一位进行选择与否，然后编写递归函数即可，返回条件应该是长度达到了n,这里的n指的是虚拟的长度，也就是是说，不选择的时候也算一步。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当然这道题用动态规划也是可以写的，但是总感觉vector拷贝会花不少时间，而且也不能在循环过程中改变vector的大小，这样会导致迭代器失效。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最后还有一种方法就是按二进制位标示选取与否，从全0到全1，一次push到答案中。
<!--more-->
### 代码
```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;
        ans.push_back({});
        for(int i:nums){
            vector<vector<int>> temp = ans;
            for(auto& item:temp){
                item.push_back(i);
            }
            ans.insert(ans.end(),temp.begin(),temp.end());
        }
        return ans;
    }
};
```
递归写法。
```c++
class Solution {
private:
vector<vector<int>> ans;
public:
    void dfs(int len,vector<int>& nums,vector<int> target){
        if(len==nums.size()){
            ans.push_back(target);
            return;
        }
        dfs(len+1,nums,target);
        target.push_back(nums[len]);
        dfs(len+1,nums,target);;
        target.pop_back();
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<int> temp;
        dfs(0,nums,temp);
        return ans;
    }
};
```

### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;时间复杂度的话，解法一的时间复杂度是O（2^N\*N）,解释就是N次拷贝，拷贝长度是从1到2^(N-1)。空间复杂度是O（2*N）解法二的时间复杂度和解法一相同,空间复杂的只有O（N）。