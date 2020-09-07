---
title: 9月7号leetcode每日一题
toc: true
mathjax: true
date: 2020-09-07 16:28:16
password:
summary:
tags: 
  - min heap
  - quick sort
  - hashmap
categories: 
  - leetcode-daily
---
[原题链接:前k个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

### 思考过程
首先最直白的方法肯定是统计数字出现频率，然后排序选出频率最高的前K
个数字。但是既然会用到排序，那么复杂度就会来到 O(nlogn),不太符合题目的要求。
这时候很自然的就想到了快速的partition来进行划分，划分出来频率高的一边有k个即可。
另外一种想法就是(纯靠自己乱想的)，因为题目保证答案唯一，所以频率最高的前K个数字是确定的，使用map计数，然后每轮对元素数目每次减去1，这样某一轮过后，必定会在map中只剩K个元素，最后push到结果即可。
<!-- more -->

* 自行思考的方法
```c++
vector<int> topKFrequent(vector<int>& nums, int k) {
        int n = nums.size();
        unordered_map<int,int> mp;
        for(auto& item:nums){
            ++mp[item];
        }
        int cnt = mp.size();
        while(cnt>k){
            for(auto& item:mp){
                    item.second--;
                    if(item.second==0){
                        --cnt;
                    }
            }
        }
        vector<int> ans;
        for(auto& item:mp){
            if(item.second>0)ans.push_back(item.first);
        }
        return ans;
```
* quick sort
```c++
class Solution {
public:
    void qsort(vector<pair<int, int>>& v, int start, int end, vector<int>& ret, int k) {
        int picked = rand() % (end - start + 1) + start;
        swap(v[picked], v[start]);

        int pivot = v[start].second;
        int index = start;
        for (int i = start + 1; i <= end; i++) {
            if (v[i].second >= pivot) {
                swap(v[index + 1], v[i]);
                index++;
            }
        }
        swap(v[start], v[index]);

        if (k <= index - start) {
            qsort(v, start, index - 1, ret, k);
        } else {
            for (int i = start; i <= index; i++) {
                ret.push_back(v[i].first);
            }
            if (k > index - start + 1) {
                qsort(v, index + 1, end, ret, k - (index - start + 1));
            }
        }
    }

    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> occurrences;
        for (auto& v: nums) {
            occurrences[v]++;
        }

        vector<pair<int, int>> values;
        for (auto& kv: occurrences) {
            values.push_back(kv);
        }
        vector<int> ret;
        qsort(values, 0, values.size() - 1, ret, k);
        return ret;
    }
};
```
* minimum heap
```c++
class Solution {
public:
    /*
    提醒一下，这里定义成员函数的话要尾置const,定义priority的cmp函数都要注意这一点。
    */
    static bool cmp(pair<int, int>& m, pair<int, int>& n) {
        return m.second > n.second;
    }

    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> occurrences;
        for (auto& v : nums) {
            occurrences[v]++;
        }

        // pair 的第一个元素代表数组的值，第二个元素代表了该值出现的次数
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(&cmp)> q(cmp);
        for (auto& [num, count] : occurrences) {
            if (q.size() == k) {
                if (q.top().second < count) {
                    q.pop();
                    q.emplace(num, count);
                }
            } else {
                q.emplace(num, count);
            }
        }
        vector<int> ret;
        while (!q.empty()) {
            ret.emplace_back(q.top().first);
            q.pop();
        }
        return ret;
    }
};
```

### 总结
总的来说，快排的思路最好想，平均时间复杂度也只有O(n)，虽然最坏是O（n^2），小根堆则是利用了数据结构的特点，但是其时间复杂度并不太好O（nlogk），我自己想的方法就参考一下吧（其实有点像快乐模拟23333）。