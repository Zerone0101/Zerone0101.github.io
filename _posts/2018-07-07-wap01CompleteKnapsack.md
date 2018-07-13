---
title: WAP 01 Knapsack 
key: WAP
tags: online_coding_test
---

输入描述:

   Wendy has n magic balls, when he uses ith ball, it costs him Ci money, and make Di damage. each ball can use only once. Now he has m money. what is the maximum damage he can get.
测试用例分布及其范围

    Ci,     Di <= 10
    30%     n <= 20, m <= 200
    70%     n <= 2000, m <= 20000
    100%    n <= 200000, m <= 20000

输出描述:
    
    output a single line containing the maximum amount of damage
    
**示例1**
输入

    3 5
    4 7
    3 5
    2 3

输出

    8


**Discuss:**

typical multi 0/1  Knapsack problem.
Given two arrays about the space and value of n objects.
Given the total volume of the Knapsack, m
Return the maximum value of the Knapsack can hold.
背包九讲（https://raw.githubusercontent.com/tianyicui/pack/master/V2.pdf ）得到的思路
多重背包问题，对每一个物品的个数进行分堆，利用二进制拆分
10 拆分为， 1，2，4，3
15 拆分为， 1，2，4，8
**空间压缩时注意逆序更新，顺序更新的话，会出现覆盖**

**背包拆分**
dynamic programming
1. station definition:
    
        dp[i][j], the maximum value can achivec when put **the previous i objects** into the Knapsack 
        which volume is j
        表示前 i 件物品恰放入一个容量为 j 的背包可以获得 的最大价值


2. init state:

        dp[0][j] = 0 for j in [0, m]
        dp[i][0] = 0 for i in [0, n]

3. transform:
        
        if Cost[i-1] <= j:  // i th object can be put into rest volume 
            dp[i][j] = max(
                dp[i-1][j], // ith object not put into the Knapsack
                dp[i-1][j- Cost[i-1]] + Value[i-1] // i th object was put into the Knapsack.
            )
        else:
            dp[i][j] = dp[i-1][j] // rest volume is too small for any more coming object


4. return dp[n][m]

**Code:**

```java
import java.util.*;
public class Main{
    // public static int knapSack(int n, int m, int[][] nums){
    //     int[][] dp = new int[n+1][m+1];
    //     for(int i = 0; i <= n; ++ i){
    //         for(int j = 0; j <= m; ++ j){
    //             if(i == 0 || j == 0){
    //                 dp[i][j] = 0;
    //             }else if(nums[i-1][0] <= j){
    //                 dp[i][j] = Math.max(nums[i-1][1] + dp[i-1][j-nums[i-1][0]], dp[i-1][j]);
    //             }else {
    //                 dp[i][j] = dp[i-1][j];
    //             }
    //         }
    //     }
    //     return dp[n][m];
    // }
    public static int knapSackChaifen(int n, int m, int[][] nums){
        // 拆分
        int cnt = 0;
        for(int i  =1; i <= 10;  ++ i){
            for(int j = 1; j <= 10; ++ j){
                if(nums[i][j] == 0) continue;
                cnt += Integer.toBinaryString(nums[i][j]).length();
            }
        }
        int[] costs = new int[cnt];
        int[] values = new int[cnt];
        int idx = 0;
        for(int i  =1; i <= 10;  ++ i){
            for(int j = 1; j <= 10; ++ j){
                int base = 1;
                int rest = nums[i][j];
                while(rest >= base){
                    costs[idx] = i*base;
                    values[idx] = j*base;
                    idx += 1;
                    rest -= base;
                    base <<= 1;
                }
                if(rest != 0) {
                    costs[idx] = i*rest;
                    values[idx] = j*rest;
                    idx += 1;
                }
            }
        }
        // 空间优化 滚动更新
        int[] dp = new int[m+1];
        for (int i = 1; i <= cnt; ++ i){
            for(int j = m; j > 0; -- j){
                if(j >= costs[i-1])
                    dp[j] = Math.max(dp[j], dp[j-costs[i-1]] + values[i-1]);
            }
        }
        return dp[m];
    }
}



```