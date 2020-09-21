---
title: Trie字典树实现
toc: true
mathjax: true
date: 2020-09-21 20:49:47
password:
summary:
tags:
    - Trie
categories:
    - data structure
---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;其实这个拖了很久了，今天总算有机会把字典树的实现自己写一遍了，毕竟这么好的数据结构放着不用确实有些可惜。**一次建树，多次查询**。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;和普通的树结构不一样，一般的树结构都是
```c++
template<class T>
struct TreeNode{
    T value;
    TreeNode *children[NUM];
};
```
<!--more-->
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;但是字典树的结构是这样的
```c++
struct TrieNode{
    bool isENnd;
    TrieNode* next[26];
};
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;大概意思就是说，我们可以通过父节点来得知他所有子节点的值，如果子节点中存在该字符，那么节点就不为空。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通过代码来讲述功能可能更为合适。
```c++
clas Trie{
private:
    bool isEnd;
    Trie* next[26];
public:
    Trie(){
        isEnd = false;
        memset(next,nullptr,sizeof(next));
    }

    void insert(string word){
        Trie* node = this;
        for(char& c: word){
            if(node->next[c-'a']==nullptr){
                node->next[c-'a'] = new Trie();
            }
            node = node->next[c-'a'];
        }
        node->isEnd = true;
    }

    bool search(string& word){
        Trie* node = this;
        for(char& c:word){
            if(node->next[c-'a']==nullptr){
                return false;
            }
            node = node->next[c-'a'];
        }
        return node->isEnd;
    }

    bool startWith(string& prefix){
        Trie* node = this;
        for(char& c:prefix){
            if(node->next[c-'a']==nullptr){
                return false;
            }
            node = node->next[c-'a'];
        }
        return true;
    }
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;插入过程其实就是从头结点开始遍历，如果当前单词中的字母在字典树中未出现，则新建相应的节点。另外就是查找和检查前缀，两者的区别主要在于前缀匹配不需要检查最后的节点是否是字符结尾。
### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;好像也没啥总结的，算是完成了之前的一个遗留任务吧，吐槽一句，work life balance真的很重要，今天算是半年以来第一次打球，体力真的差好多。。。