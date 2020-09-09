---
title: 9月9日leetcode每日一题
toc: true
mathjax: true
date: 2020-09-09 10:22:43
password:
summary:
tags:
    - depth first search
    - cut branch
categories:
    - leetcodedaily
---
[原题链接: 组合总和](https://leetcode-cn.com/problems/combination-sum/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;先说句题外话，这道题和昨天的那道组合数基本一样，只不过要自己额外做点处理，甚至在剪枝的要求上还不如昨天那道题。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;拿到这道题的第一想法就是我先用大的数去减这个target，减不动了再用小一点的数字。这一点很好理解，相当于将所有可行解按照解的长度由小到大排列。但是这里没有保证数组有序，所以预先排序即可。但是第一次实现的时候，发现有重复组合，消除重复组合的方法昨天的博客里面已经提到，限定一种顺序即可，这里我们按照从大到小的顺序，**因为可以重复选取元素，所记得选元素的时候可以是小于等于**。
<!--more-->
* 代码
```c++
class Solution {
private:
    vector<vector<int>> ans;
public:
    void traverse(int left,vector<int>& candi,vector<int>& curr){
        if(left==0){
            ans.push_back(curr);
            return;
        }
        for(int i = candi.size()-1;i>=0;--i){
            if(left>=candi[i]&&(curr.empty()||curr.back()>=candi[i])){
                curr.push_back(candi[i]);
                traverse(left-candi[i],candi,curr);
                curr.pop_back();
            }
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        vector<int> temp;
        traverse(target,candidates,temp);
        return ans;
    }
};
```

### 总结
这里要注意一点要先判定curr是否为空再判断curr.back()与candi[i]的大小关系，不然会出现访问不存在元素的未定义行为，所以书本里面写的很有道理，**先检查边界有效性，再检查内容。**另外提一句，这里时间效率已经超过90%，进一步剪枝的话就把curr.empty()和curr.back()>=candi[i]这个判定拉出来，直接让i从符合条件的位置开始遍历，或者再在traverse中添加一个index参数，记录上次选取的index，这次选取就从index（想想为什么不是index-1?）开始递减遍历。