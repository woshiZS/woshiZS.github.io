---
title: 9月13号leetcode每日一题
toc: true
mathjax: true
date: 2020-09-13 08:29:23
password:
summary:
tags:
    - dynamic programming
    - depth-first-search 
categories:
    - leetcode-daily
---
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题一开始肯定想的是使用dfs，外加bool型数组记录是否被选取，但是发现我自己原来写的并没有剪枝，但是时间效率还不错...看了题解，发现也是简单dfs加回溯。。。无语，那就这样写吧，这样的话感觉这道题就没有什么意义了，纯粹考察熟练度。
<!--more-->
### 代码
```c++
class Solution {
public:
    bool ans = false;
    int direct[4][2]={
        {1,0},{0,1},{-1,0},{0,-1}
    };
    bool book[200][200]={false};
    
    void dfs(const string& word,const vector<vector<char>>& target,int index,int x,int y){
        if(ans)return;//部分剪枝
        if(index+1==word.size()){ans=true;return;}
        for(int i=0;i<4;i++){
            int tx= x+direct[i][0],ty = y+direct[i][1];
            if(tx<0||tx>=target.size()||ty<0||ty>=target[0].size())continue;
            if(!book[tx][ty]&&target[tx][ty]==word[index+1]){
                book[tx][ty]=true;
                dfs(word,target,index+1,tx,ty);
                book[tx][ty]=false;
            }
        }
    }
    bool exist(vector<vector<char>>& board, string word) {
        for(int i=0;i<board.size();i++){
            for(int j=0;j<board[0].size();++j){
                if(board[i][j]==word[0]){
                    book[i][j]=true;dfs(word,board,0,i,j);book[i][j]=false;
                }
            }
        }
        return ans;
    }
};
```
### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里感觉时间复杂度是O（M*N*L）,和官方题解不一样这里没有计算条件分支，感觉这里的条件分支可一并到一起，加不加那个指数感觉无关紧要。空间复杂度很明显，栈的深度最大是L，bool型数组大小为M*N。