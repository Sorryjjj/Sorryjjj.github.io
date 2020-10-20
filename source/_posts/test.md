---
title: n皇后
date: 2020-04-20 22:26:11
tags:
categories:
---



```c++


#include <cstdio>
#include <limits.h>
#include <algorithm>
#include <iostream>
#include <iomanip>
#include <stack>
#include <string>
#include <cstring>
#include <vector>
#include <map>
#include <set>
#include <cmath>
#include <queue>
#include <stdlib.h>
#include <stdio.h>
typedef long long ll;
using namespace std;

int n;//输入n
int cnt;//记录解个数
int vis[12];//记录皇后位置

//判断是否满足条件
bool check(int row) {
	for (int i = 1; i < row; i++) {
		if (abs(vis[row] - vis[i]) == abs(row-i) || vis[row] == vis[i]) {
			return false;
		}
	}
	return true;
}

//递归摆放皇后
void dfs(int row) {
  		//遍历列的位置
		for (int i = 1; i <= n; i++) {
			vis[row] = i;
			if (check(row)) {
				if (row == n) {//到达最后一行
					cnt++;
				}
				else
				{
					dfs(row + 1);//
				}
			}
		}
}

int arr[11];
int main() {
    //这里hdu-2553需要打表避免超时
	for (int i = 1; i < 11; i++) {
		cnt = 0;
		n = i;
		dfs(1);
		arr[i] = cnt;
	}
	while (cin>> n && n)
	{
		cout << arr[n] << endl;
	}
	return 0;
}
```

n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。（每个皇后不在同一行、同一列以及对角线上）

递归写法:

1. 将棋盘存储为一个一维数组啊a[n],第i个元素表示第i行的皇后位置
2. 遍历下一行位置,判断是否满足条件,是则记录一个解,直到走到最后一行
3. 冲突计算: ①行列相减的绝对值相等判断是否在对角线 ②列是否相等