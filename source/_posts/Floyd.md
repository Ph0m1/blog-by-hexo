---
title: 弗洛伊德(Floyd)算法
date: 2024-03-01 18:27:30
updated: 2024-03-01 18:27:30
tags: 
    - 算法
    - c++
categories: 数据结构与算法
---



## 概览

弗洛伊德算法（Floyd's Algorithm），也称为弗洛伊德-沃尔沃什算法（Floyd-Warshall Algorithm），是一种用于解决图中所有节点间最短路径的动态规划算法。\
该算法能够计算出图中所有节点对之间的最短路径长度。

弗洛伊德算法的基本思想是通过遍历图中的所有节点对，并尝试通过中间节点来改进已知的最短路径。具体而言，该算法的步骤如下：

初始化一个二维数组 dist[][]，用来存储节点间的最短距离。初始时，如果节点 i 和节点 j 之间有直接的边，则 dist[i][j] 为该边的权重；否则，dist[i][j] 为一个很大的值，表示不可达。

对于每对节点 i 和 j，遍历所有可能的中间节点 k，如果通过节点 k 能够缩短从节点 i 到节点 j 的路径长度，则更新 dist[i][j] = dist[i][k] + dist[k][j]。
重复上述步骤，直到所有节点对之间的最短路径长度都已计算出。

## 时间复杂度

O(n^3) 其中 n 表示节点对数量。

## 实例

[leetcode 3015.按距离统计房屋对数目I](https://leetcode.cn/problems/count-the-number-of-houses-at-a-certain-distance-i/description/)

    给你三个 正整数 n 、x 和 y 。

    在城市中，存在编号从 1 到 n 的房屋，由 n 条街道相连。对所有 1 <= i < n ，都存在一条街道连接编号为 i 的房屋与编号为 i + 1 的房屋。另存在一条街道连接编号为 x 的房屋与编号为 y 的房屋。

    对于每个 k（1 <= k <= n），你需要找出所有满足要求的 房屋对 [house1, house2] ，即从 house1 到 house2 需要经过的 最少 街道数为 k 。

    返回一个下标从 1 开始且长度为 n 的数组 result ，其中 result[k] 表示所有满足要求的房屋对的数量，即从一个房屋到另一个房屋需要经过的 最少 街道数为 k 。

    注意，x 与 y 可以 相等 。

    提示：

        2 <= n <= 100
        1 <= x, y <= n

## 核心代码
```c
for (k = 1; k < n + 1; k++)
    {
        for (i = 1; i < n + 1; i++)
        {
            for (j = 1; j < n + 1; j++)
            {
                // 如果通过中间节点 k 能够缩短从节点 i 到节点 j 的路径长度，则更新路径长度
                int t = a[i][k] + a[k][j];
                if (t < a[i][j])
                    a[i][j] = t;
            }
        }
    }
```
***

## 题解

```c
   int *countOfPairs(int n, int x, int y, int *returnSize)
{
    // 创建一个二维数组 a，用于存储节点之间的最短路径长度
    int a[101][101];
    int i, j, k;

    // 初始化数组 a，设定初始值
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            // 如果节点 i 和节点 j 相同，则距离为 0；否则设为一个大值表示不可达
            if (i == j)
            {
                a[i][j] = 0;
            }
            else
            {
                a[i][j] = 999999;
            }
        }
    }

    // 初始化相邻节点之间的距离为 1
    for (i = 1; i < n; i++)
    {
        a[i][i + 1] = 1;
        a[i + 1][i] = 1;
    }
    // 初始化给定的两个节点之间的距离为 1
    a[x][y] = 1;
    a[y][x] = 1;

    // 使用弗洛伊德算法计算所有节点之间的最短路径长度
    for (k = 1; k < n + 1; k++)
    {
        for (i = 1; i < n + 1; i++)
        {
            for (j = 1; j < n + 1; j++)
            {
                // 如果通过中间节点 k 能够缩短从节点 i 到节点 j 的路径长度，则更新路径长度
                int t = a[i][k] + a[k][j];
                if (t < a[i][j])
                    a[i][j] = t;
            }
        }
    }

    // 统计每个节点对的最短路径长度出现的次数
    int *ans = (int *)malloc(sizeof(int) * (n + 1));
    for (int i = 0; i < n; i++)
    {
        ans[i] = 0;
    }
    for (i = 1; i < n + 1; i++)
    {
        for (j = 1; j < n + 1; j++)
        {
            // 统计除了节点自身以外的所有最短路径长度出现的次数
            if (i != j)
                ans[a[i][j] - 1]++;
        }
    }
    *returnSize = n;

    return ans;
}


```

<style>
  h3, h4 {text-align: center;}
  p {text-indent: 2em;}
</style>