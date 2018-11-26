---
title: longest palindrome subsequence with follow-up and 平方串
key: longest sub sequence 
tags: online_coding_test 
---


字符子串和字符子序列的区别
字符字串指的是字符串中**连续**的n个字符；如palindrome中，pa，alind，drome等都属于它的字串

而字符子序列指的是字符串中**不一定连续但先后顺序一致**的n个字符:
如palindrome中，plind，lime属于它的子序列，而mod，rope则不是，因为它们与字符串的字符顺序不一致。
问题描述：

    给出一个字符串，求该字符串的最长回文子序列

Follow-Up:

    1.输出最长子序列长度的同时，打印出符合结果的子序列，如果有多条打印一条即可
    2.对于求出非回文子串，而是平方子串:
        xx形式:比如aabaab

Discuss：

动态规划，转移方程是

    if(s[i] == s[j]) : dp[i][j]=dp[i+1][j-1] 
    or : dp[i][j] = max(dp[i+1][j], dp[i][j-1])
    dp[i][j]的可能状态只有三个，为了打印路径信息，需要每一次记录当前状态由哪一个状态转义而来，如果有多个，只需要记录一个。

对于原始问题，只输出最长回文子序列长度代码如下:

```java
    public class Main {
        public static int longestSubSequence(String str){
        char[] s = str.toCharArray();
        int n = s.length;
        int[][] dp = new int[n][n];
        //初始化dp初始状态
        for(int i = 0; i < n; ++ i) {
            dp[i][i] = 1;
        }
        //初始化dp初始状态
        for(int i = 0; i < n-1; ++ i) {
            if(s[i] == s[i+1]) {
                dp[i][i+1] = 2;
            }
            else{
                dp[i][i+1] = 1;
            }
        }
        //状态转移
        for(int i = n-1; i >= 0; -- i){
            for(int j = i+2; j < n; ++ j){
                if(s[i] == s[j]){
                    dp[i][j] = dp[i+1][j-1]+2;
                }else{
                    dp[i][j] = Math.max(dp[i+1][j], dp[i][j-1]);
                }
            }
        }
        return dp[0][n-1];
    }

```
对于增加路径输入的操作:
```java
//Pair类，记录上一个状态，三种情况:
1. [i+1, j]
class Pair{
    boolean isTwo = false;
    int x;
    int y;
    public Pair(int x, int y){this.x = x; this.y = y;}
    public boolean equals(Object obj) {
        if(!(obj instanceof Pair)) return false;
        Pair o = (Pair)obj;
        return this.x == o.x && this.y == o.y;
    }
    public int hashCode() {
        return (Integer.hashCode(x)) ^ (Integer.hashCode(y));
    }
}
public class Main{
    public static int longestSubSequence(String str){
        char[] s = str.toCharArray();
        int n = s.length;
        Pair[][] path = new Pair[n][n]; //whether to be removed
        int[][] dp = new int[n][n];
        for(int[] row : dp) Arrays.fill(row, -1); //初始化
        for(int i = 0; i < n; ++ i) {
            dp[i][i] = 1;
            path[i][i] = null; // no sub transform starte
        }
        for(int i = 0; i < n-1; ++ i) {
            if(s[i] == s[i+1]) {
                dp[i][i+1] = 2;
                path[i][i+1] = null;
            }
            else{
                dp[i][i+1] = 1;
                path[i][i+1] = new Pair(i, i); // random select a transform state
            }
        }
        for(int i = n-1; i >= 0; -- i){
            for(int j = i+2; j < n; ++ j){
                if(s[i] == s[j]){
                    dp[i][j] = dp[i+1][j-1]+2;
                    path[i][j] = new Pair(i+1, j-1);
                    path[i][j].isTwo = true;
                }else{
                    if(dp[i+1][j] > dp[i][j]){
                        dp[i][j] = dp[i+1][j];
                        path[i][j] = new Pair(i+1, j);
                    }
                    if(dp[i][j-1] > dp[i][j]){
                        dp[i][j] = dp[i][j-1];
                        path[i][j] = new Pair(i, j-1);
                    }
                }
            }
        }
        System.out.println(helper(s, path, 0, n-1));
        return dp[0][n-1];
    }
    public static String helper(char[] s, Pair[][] path, int i, int j){
        if(i > j) return null;
        if(path[i][j] == null){
            StringBuilder res = new StringBuilder();
            for(int k = i; k <= j; ++ k){
                res.append(s[k]);
            }
            return res.toString();
        }
        Pair next = path[i][j];
        if(next.isTwo == false)
            return helper(s, path, next.x, next.y);
        return s[i]+helper(s, path, next.x, next.y)+s[i];
    }
}

```
对于平方字符串的长度判断:
字符串S是由两个字符串T连接而成,即S = T + T, 我们就称S叫做平方串,例如"","aabaab","xxxx"都是平方串.
如果要形成一个平方串，需要最少删除掉多少个字符串
首先把字符串每一个位置作为分割点，然后字符串被分割成了两个部分，然后从两个字符串中寻找最长公共子序列，取全局最优值

```java
/**
aabaab
**/
import java.util.*;
public class Main{
    public static int longestSquareSubSequence(String str){
        int n = str.length();
        int res = 0;
        for(int i = 1; i <= n; ++ i){
            res = Math.max(res, longesCommonSubSequence(str.substring(0, i), str.substring(i)));
        }
        return res;
    }
    public static int longesCommonSubSequence(String str1, String str2){
        if(str1.length() == 0 || str2.length() == 0) return 0;
        int n1 = str1.length();
        int n2 = str2.length();
        int[][] dp = new int[n1+1][n2+1];//注意初始状态值的初始化
        for(int i = 1; i <= n1; ++ i){
            for(int j = 1; j <= n2; ++ j){
                if(str1.charAt(i-1) == str2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1] + 1;
                }else{
                    dp[i][j] = Math.max(dp[i][j], Math.max(dp[i-1][j], dp[i][j-1]));
                }
            }
        }
        return dp[n1][n2]*2; // 注意总的长度，两部分求和
    }
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        String str = in.next();
        System.out.println(longestSquareSubSequence(str));
    }
}
```