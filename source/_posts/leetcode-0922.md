---
title: leetcode_0922
toc: true
mathjax: true
date: 2020-09-22 14:46:44
password:
summary:
tags:
    - traverse
    - Treelike-dp
categories:
    - leetcode-daily
---
[原题链接: 监控二叉树](https://leetcode-cn.com/problems/binary-tree-cameras/)
### 思考过程
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本题目和打家劫舍拿到题目很像，大致就是到每个节点会分不同的情况，比如说本道题中就是是否在该节点安装监控，打家劫舍则是是否在那个节点进行盗窃，每个节点的情况依赖于子节点的状态。
<!--more-->
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;相对来说这道题的情况稍微复杂一点，需要记录的状态也随要求来进行变化。首先，每个点可能不会安装监控，这就需要该节点的子节点至少有一个安装监控。另外就是该节点安装了监控，那么其左右两棵子树只需要将自身的子孙部分覆盖即可，这种情况下不用管子节点自身是否安装了监控，因为子节点已经被当前节点所安装的监控所监视，**这句话很重要，就是我不管他是否在子节点装了，反正当前节点的子节点的子孙部分一定要被覆盖。**所以，我们需要的状态有三个，第一个就是覆盖以当前节点为根的树的最小摄像头数目，第二个就是当前节点有摄像头时需要的总摄像头数目，第三个是当前节点的子孙部分被覆盖所需的最小摄像头数目。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;状态量确定之后应该寻找转移方成，分别设这三个状态量为a,b,c。显然a应该从节点安装摄像头与否之间进行选择。对于b而言。
>$$b = 1+L_c+R_c $$
>$$c = min(b,L_a+R_a) $$
>$$a = min(b,min(L_b+R_a,L_a+R_b)) $$
这里L，R分别代表当前节点的儿子节点,首先b等于当前节点一个摄像头加上可以将自己两个子节点的子孙包括的摄像头数量。而c等于以当前节点安装监控的情况和两个子树的a值（将子树覆盖的最小数量），最终a就从当前节点安装摄像头和未安装摄像头的两种情况中两者取最小值。安装的值就是b，未安装的分为两种，一种就是左子树根节点安装摄像头，右子树取刚好覆盖的最小值，不一定要求在右子树根节点安装监控，另一种情况刚好相反。注意这里还要考率一种特殊情况，当子节点为空时，无法在上边安装摄像头，将该节点的初始b状态设为一个较大的值即可解决这个问题，因为我们取的是最小值，所以就可以自动消除影响。
### 代码
```c++
class Solution {
public:
    vector<int> dfs(TreeNode* root){
        vector<int> temp{0,1000,0};
        if(root==nullptr){
            return temp;
        }
        auto l = dfs(root->left);
        auto r = dfs(root->right);
        temp[1] = 1 + l[2] + r[2];
        temp[2] = min(temp[1],l[0]+r[0]);
        temp[0] = min(temp[1],min(l[1]+r[0],l[0]+r[1]));
        return temp;
    }
    int minCameraCover(TreeNode* root) {
        auto ans = dfs(root);
        return ans[0];
    }
};
```
### 总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;树形dp实际上还是比较常规的，主要就是要稍微分析得仔细一点，将每个节点的状态列出来，然后找出父子节点之间的联系构造状态转移函式即可，时空复杂度就是常规的二叉树遍历的时空复杂度，但是这里因为返回值是vector需要拷贝，所以花费时间会比较久。