---
title: 9月17日leetcod每日一题
toc: true
mathjax: true
date: 2020-09-17 15:44:49
password:
summary:
tags:
    - tree
    - grapth
    - disjoint-set
categories:
    - leetocde-daily
---
[原题链接: 冗余连接](https://leetcode-cn.com/problems/redundant-connection-ii/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题其实在七月的每日一题中已经出现，当时也是一次就自己写出来了。这次的思路和上次的差不太多，算是利用一点并查集的思想，将每个点父节点存储到一个数组之中，正常情况下，除了根节点没有父节点之外，其他节点有且只有一个父节点，但是多添加一条边之后，可能出现两种情况。
<!--more-->
* 第一种就是每个节点都父节点，这样可以肯定的是多出来的边肯定是指向根节点的那条边，这时候图中必定存在一个环，所以所以我么们应该找到这个环，严格来说去除这个环中的任意一条边都满足条件，所以我们应该找出那条最后出现的边进行删除。
* 第二种情况就是同一条边出现了多个父节点，此时情况相对来说比较简单，答案是发生冲突的两条边之一。注意这里为什么说是之一，因为可能不符合条件的边是先出现的那条边。所以还是要检测是否有环，如果没有，则说明开始那条边应该被保留。

### 代码
```c++
class Solution {
public:
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> ans;
        vector<int> pre(n+1,0);//记录每个点的父节点
        vector<bool> book(n+1,false);
        vector<vector<int>> cdd;
        for(auto edge:edges){
            if(pre[edge[1]]){cdd.push_back({pre[edge[1]],edge[1]});cdd.push_back(edge);continue;}//如果已经存在父节点，但是仍然有以该点作为出度的边的话，就将其加入到冗余集合里面去
            pre[edge[1]]=edge[0];
        }
        if(!cdd.empty()){
            int temp = cdd[0][1];//第一条边的出度点
            //这里之所以这么做是因为最开始出现的边不一定是有效的边。
            while(true){
                if(book[temp]){
                    if(temp)return cdd[0];
                    return cdd[1];
                }
                book[temp]=true;
                temp = pre[temp];
            }
        }
        //如果每个点都父节点，说明多出来的边肯定落在了
        int target = 1;
        while(true){
            if(book[target])break;
            book[target]=true;
            target = pre[target];
        }
        book.resize(n+1,false);
        while(!book[target]){
            book[target]=true;
            target = pre[target];
        }
        for(int i =n-1;i>=0;i--){
            if(book[edges[i][0]]&&book[edges[i][1]]){ans=edges[i];break;}
        }
        return ans;
    }
};
```

### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;因为这里1并没有说是根节点，所以有可能出现从一条路径网上回溯的时候走过了一段并不在环内的部分，所以只有在出现重复的点的时候才能确定该点在环路内部，然后再从环路内部开始进行回溯（类似于并查集中所说的找帮派），将所有在环中的点标记，然后从输入尾部开始进行遍历，如果两个顶点都属于环内顶点，则返回该边。时间复杂度首先遍历所有边是O(m)，然后判断是否有环，时间复杂度最差为O（h）为数的高度，整体时间复杂度还是O(M),空间复杂度为O（N），N为图中节点的个数。