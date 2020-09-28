---
title: 9月27号leetcode每日一题
toc: true
mathjax: true
date: 2020-09-28 10:42:49
password:
summary:
tags:
    - tree
    - breadth-first-search
categories:
    - leetcode-daily
---
[原题链接:填充右侧节点](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;说实话这道题做的蛮差的，或者是我现在在写思路的时候心情有点不大好，午睡没睡好，头有点晕。普通的BFS当然没什么问题，但是不满足常数空间的要求，不过这道题出的要求也不是特别严谨，递归所产生的空间不算到空间复杂度里面，这本身就有点扯。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题思路还是借鉴官方题解之后才写出来的，其实还是层序遍历的板子，大概就是一行从左到右，在遍历这一行的时候将下一行的节点的next都连接起来了。然后使用一个nextstart来存储下一行开始的节点。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;不过感觉这道题的本质还是在考察BFS，递归产生的空间不算就当个笑话好了。
<!--more-->
### 代码
```c++
class Solution {
public:
    void link(Node *&last,Node *&cur,Node *&nstart){
        if(!nstart)nstart = cur;
        if(last)last->next = cur;
        last = cur;
    }
    Node* connect(Node* root) {
        Node* head = root;
        while(head){
            Node *last = nullptr,*nstart = nullptr;
            for(;head!=nullptr;head=head->next){
                if(head->left)link(last,head->left,nstart);
                if(head->right)link(last,head->right,nstart);
            }
            head = nstart;
        }
        return root;
    }
};
```
### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;时间复杂度是O（N）这肯定没得说的，空间复杂度是O（1），手动狗头。