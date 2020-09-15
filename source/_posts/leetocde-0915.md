---
title: 9月15号leetcode每日一题
toc: true
mathjax: true
date: 2020-09-15 15:41:19
password:
summary:
tags:
    - bit operation
    - hashtable
categories:
    - leetcode-daily
---
[原题链接](https://leetcode-cn.com/problems/sudoku-solver/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;说实话，这道题还是有一定的思考量的，刚开始想的是建立每行每列以及每个cell，之后就是把空的点push到一个队列里面，如果当前点可以进行填写（意思就是说其他8个数已经确定），就将节点pop出去，如果不能进行填写，pop出去之后就再push进入队尾。
<!--more-->
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;但是这样其实有一个问题就是有的数独他可能不会出现上述情况，比如说有的数读就是要你猜测某个点的值，你具有试错机会，所以说上面那种算法不仅耗时比较长，而且有可能会进入死循环。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;所以我们在这里采用的是题解中的方法，采用的是dfs式的递归方法，如果遇到冲突，还可以进行回溯试错。并且更进一步的还有bitset方法，占用空间较小（虽然最后出来的结果好像不是特别好2333）

```c++
class Solution {
private:
    vector<bitset<9>> rows;
    vector<bitset<9>> cols;
    vector<vector<bitset<9>>> cells;
public:
    bitset<9> status(int x,int y){
        return ~(rows[x]|cols[y]|cells[x/3][y/3]);
    }
    //返回有效位置bitset应该是没有问题的。
    pair<int,int> getNext(vector<vector<char>>& board){
        pair<int,int> ret;
        int min_cnt = 10;//代表10中可能选择的方案，我们这里选择的是最小选择数目的那个点
        for(int i = 0;i<9;++i){
            for(int j = 0;j<9;++j){
                if(board[i][j]=='.'){
                    auto temp = status(i,j);
                    if(temp.count()<min_cnt){
                        ret = {i,j};
                        min_cnt = temp.count();
                    }
                }
            }
        }
        return ret;
    }
    //每次返回下一个最少选择的位置没有问题
    void fillNum(int x,int y,int n,bool flag){
        rows[x][n] = flag ? 1 : 0;
        cols[y][n] = flag ? 1 : 0;
        cells[x/3][y/3][n] = flag ? 1 : 0;
    }

    bool dfs(vector<vector<char>>& board,int cnt){
        // printf("%d\n",cnt);
        if(cnt == 0)return true;
        auto next = getNext(board);
        auto bits = status(next.first,next.second);
        for(int i = 0;i<9;++i){
            if(bits.test(i)){
                //说明这个点可以选，选择这个点之后更新各个数组的状态，然后进行下一轮的递归
                fillNum(next.first,next.second,i,true);
                board[next.first][next.second] = i + '1';
                if(dfs(board,cnt-1))return true;
                board[next.first][next.second] = '.';
                fillNum(next.first,next.second,i,false);
            }
        }
        return false;
    }
    void solveSudoku(vector<vector<char>>& board) {
        rows.resize(9);
        cols.resize(9);
        cells.resize(3,vector<bitset<9>>(3));
        int cnt = 0;
        for(int i = 0;i<9;++i){
            for(int j  = 0;j<9;++j){
                if(board[i][j] == '.'){++cnt;continue;}
                int n = board[i][j] - '1';
                rows[i] |= (1<<n);
                cols[j] |= (1<<n);
                cells[i/3][j/3] |= (1<<n);
            }
        }
        // printf("Initialization is okay.\n");
        dfs(board,cnt);
    }
};
```

### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里比较精妙的就是设置了一个找出了一个可选数字最少的空白点，这样出错几率相对来说会更小。getNext函数，对全表进行扫描来选出目标点，而找到目标的方法就是对空点进行剩余可选个数计算，这就是status函数的作用。然后每次我们遍历一个点选择一个数的时候，应该要对相应的行，列以及块进行更新。这就是fillNum的功能，dfs形式选择参数为剩余的空白点。**这里有一个小技巧，如果有一个路径走通了，其他路径就不用走了，所以返回类型可以设置为bool，返回为true的时候就也直接return true,以达到剪枝的目的。**时间复杂度和空间复杂度，因为这里数独的维度是固定死的，所以都是常数级别。