---
title: 9月8号leetcode每日一题
toc: true
mathjax: true
date: 2020-09-08 11:47:27
password:
summary:
tags:
  - depth-first-search
  - combination
categories:
  - leetcode-daily
---
[原题链接 组合](https://leetcode-cn.com/problems/combinations)

### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题刚拿到一看就比较典型，组合选k个数字，应该可以用dfs解决。
但是这道题要求的是组合，如果按照循环遍历选一个再用bool数组记录选取情况的话，会产生很多不必要的递归路径。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;因此要确定每个组合只有一个排列会push到最终结果，方法就是规定一个顺序即可，这里就规定递增顺序即可。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;但是做出来之后效率比较低下，应该是没有进行剪枝，仔细一想，每次要求遍历开始的部分直接从已有数字+1开始（数组为空时取1即可），修改之后最大时间缩短为十分之一。然而超过人数占比并不理想，所以在想了想，要是当前剩余可遍历元素少于所需元素个数，好像也可以直接返回，最后运行结果16ms,超过94%。

### dfs递归代码
```c++
class Solution {
private:
    vector<vector<int>> ans;
public:
    void dfs(int left,int n,vector<int>& target){
        if(left==0){
            ans.push_back(target);
            return;
        }
        int i = target.empty()? 1 :target.back()+1;
        int bound = n - left + 1;
        for(;i<=bound;++i){
            target.push_back(i);
            dfs(left-1,n,target);
            target.pop_back();
        }
    }
    vector<vector<int>> combine(int n, int k) {
        //注意不是排列是组合，所以要想个办法使得每个组合的排列只出现一次，
        //好像规定组合内数字递增即可，递增算是同一组合的唯一排序
        vector<int> temp;
        dfs(k,n,temp);
        return ans;
    }
};
```
### 非递归二进制组合代码
思路这里是官方题解中给出的思路，意思就是选取k个数字，转换为一个n位2进制数应该是k个1，n-k个0，我们要找到一种从最低k位全1到最高k位全1的变换方法，这种方法很好理解，就是先最高位加1，然后整体从高到低都加1。一轮过后最高位加1，再开始。***注意代码实现使用了一点小trick，即在尾部多添加一个n+1的数字，其实在1到n之外的数字都行，仔细品味2333***。
```c++
class Solution {
public:
    vector<int> temp;
    vector<vector<int>> ans;

    vector<vector<int>> combine(int n, int k) {
        // 初始化
        // 将 temp 中 [0, k - 1] 每个位置 i 设置为 i + 1，即 [0, k - 1] 存 [1, k]
        // 末尾加一位 n + 1 作为哨兵
        for (int i = 1; i <= k; ++i) {
            temp.push_back(i);
        }
        temp.push_back(n + 1);
        
        int j = 0;
        while (j < k) {
            ans.emplace_back(temp.begin(), temp.begin() + k);
            j = 0;
            // 寻找第一个 temp[j] + 1 != temp[j + 1] 的位置 t
            // 我们需要把 [0, t - 1] 区间内的每个位置重置成 [1, t]
            while (j < k && temp[j] + 1 == temp[j + 1]) {
                temp[j] = j + 1;
                ++j;
            }
            // j 是第一个 temp[j] + 1 != temp[j + 1] 的位置
            ++temp[j];
        }
        return ans;
    }
};
```

### 总结
题目算是在普通dfs上的一个变式，时间效率上的pogai也再次提醒我们剪枝的重要性，另外利用二进制数进行模拟这种方法还是比较难想到的，算是拓宽视野了。

### 碎碎念
今天终于收到字节面试通知了，有点高兴又有点紧张，努力准备，加油！