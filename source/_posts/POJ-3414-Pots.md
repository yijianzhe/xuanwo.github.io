---
title: POJ 3414 Pots
date: 2014-08-24 21:11:39
categories: Exercise
toc: true
---
# 题目
源地址：

http://poj.org/problem?id=3414

# 理解
因为卡一道题然后整整两天没有做题目什么的太不科学了- -。
纠结的难点在于，我怎么样把实现操作的路径打印出来。事实上，标记全部的六种状态，然后在bfs的过程中，把每一个状态全都输出到一个数组中去，然后再进行输出。

<!-- more -->

# 代码

```

#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <cmath>
#include <ctime>
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
#include <deque>
#include <list>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <numeric>
#include <iomanip>
#include <bitset>
#include <sstream>
#include <fstream>
#define debug puts("-----")
#define pi (acos(-1.0))
#define eps (1e-8)
#define inf (1<<28)
using namespace std;

const int maxn = 110;
int vis[maxn][maxn];
int a, b, c;
int step;
int flag;


struct Status
{
    int k1, k2;
    int op;
    int step;
    int pre;
} q[maxn *maxn];
int id[maxn *maxn];
int lastIndex;

void bfs()
{
    Status now, next;

    int head, tail;
    head = tail = 0;

    q[tail].k1 = 0; q[tail].k2 = 0;
    q[tail].op = 0; q[tail].step = 0; q[tail].pre = 0;

    tail++;

    memset(vis, 0, sizeof(vis));
    vis[0][0] = 1;

    while (head < tail)
    {
        now = q[head];
        head++;

        if (now.k1 == c || now.k2 == c)
        {
            flag = 1;
            step = now.step;
            lastIndex = head - 1;
        }

        for (int i = 1; i <= 6; i++)
        {
            if (i == 1)
            {
                next.k1 = a;
                next.k2 = now.k2;
            }
            else if (i == 2)
            {
                next.k1 = now.k1;
                next.k2 = b;
            }
            else if (i == 3)
            {
                next.k1 = 0;
                next.k2 = now.k2;
            }
            else if (i == 4)
            {
                next.k1 = now.k1;
                next.k2 = 0;
            }
            else if (i == 5)
            {
                if (now.k1 + now.k2 <= b)
                {
                    next.k1 = 0;
                    next.k2 = now.k1 + now.k2;
                }
                else
                {
                    next.k1 = now.k1 + now.k2 - b;
                    next.k2 = b;
                }
            }
            else if (i == 6)
            {
                if (now.k1 + now.k2 <= a)
                {
                    next.k1 = now.k1 + now.k2;
                    next.k2 = 0;
                }
                else
                {
                    next.k1 = a;
                    next.k2 = now.k1 + now.k2 - a;
                }
            }

            next.op = i;
            if (!vis[next.k1][next.k2])
            {
                vis[next.k1][next.k2] = 1;
                next.step = now.step + 1;
                next.pre = head - 1;

                q[tail].k1 = next.k1; q[tail].k2 = next.k2;
                q[tail].op = next.op; q[tail].step = next.step; q[tail].pre = next.pre;
                tail++;

                if (next.k1 == c || next.k2 == c)
                {
                    flag = 1;
                    step = next.step;
                    lastIndex = tail - 1;
                    return;
                }
            }
        }
    }


}

int main(int argc, char const *argv[])
{
    while (scanf("%d%d%d", &a, &b, &c) != EOF)
    {
        flag = 0;
        step = 0;

        bfs();
        if (flag)
        {
            printf("%d\n", step);

            id[step] = lastIndex;
            for (int i = step - 1; i >= 1; i--)
            {
                id[i] = q[id[i + 1]].pre;
            }

            for (int i = 1; i <= step; i++)
            {
                if (q[id[i]].op == 1)
                    printf("FILL(1)\n");

                else if (q[id[i]].op == 2)
                    printf("FILL(2)\n");

                else if (q[id[i]].op == 3)
                    printf("DROP(1)\n");

                else if (q[id[i]].op == 4)
                    printf("DROP(2)\n");

                else if (q[id[i]].op == 5)
                    printf("POUR(1,2)\n");

                else if (q[id[i]].op == 6)
                    printf("POUR(2,1)\n");
            }
        }
        else printf("impossible\n");
    }
    return 0;
}

```

# 更新日志
- 2014年08月24日 已AC。