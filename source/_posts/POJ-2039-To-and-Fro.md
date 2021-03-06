﻿---
title: POJ 2039 To and Fro
date: 2014-08-06 14:33:00
categories: Exercise
toc: true
---
# 题目
源地址：

http://poj.org/problem?id=2039

# 理解
找规律的题目。
首先看一下加密的方式：
原文为`theresonoplacelikehomeonasnowynightx`，分成N列来写，没有写满的用x填充，写成如下队列：

```
t o i o y
h p k n n
e l e a i
r a h s g
e c o n h
s e m o t
n l e w x

```
对于奇数行，从左向后书写，对于偶数行，从右向左书写，得到密文：
`toioynnkpheleaigshareconhtomesnlewx`
显然，解密的方式就是原样还原回去。
一共进行N次读入，对于第i次读取，用j定位行数。
如果j为偶数，输出`N*j+i`；
如果j为奇数，输出`N*(j+1)-1-i`。

<!-- more -->

# 代码

```

#include <cstdio>
#include <cstdlib>
#include <cstring>

char str[210];
int N;
int i, j;
int len, M;

int main(int argc, char const *argv[])
{
    while (scanf("%d", &N), N != 0)
    {
        getchar();
        gets(str);
        len = strlen(str);
        M = (int)((double)N / (double)len + 0.999999);
        for (i = 0; i < N; i++)
        {
            for (j = 0; j * N < len; j++)
            {
                if (j % 2 == 0)
                {
                    printf("%c", str[N * j + i]);
                }
                else if (j % 2 == 1)
                {
                    printf("%c", str[N * (j + 1) - 1 - i]);
                }
            }
        }
        printf("\n");
    }
    return 0;
}

```

# 更新日志
- 2014年08月06日 已AC。