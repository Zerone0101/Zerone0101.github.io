---
title: WAP Lucky Pairs
key: WAP Lucky Pairs
tags: online coding test
---

7 is known as the lucky number. And if a number is a multiple of 7, it's also lucky.
Now you have n different integers. You can choose an ordered pair of different integer and connect them to get a new integer.
For example, we can connect pair (11, 2) to get 112, which is lucky.
Note that:
(11, 2) -> 112 and (2, 11) -> 211 are considered as different pairs.
(1, 11) -> 111 and (11, 1) -> 111 are also different.
You want to know many lucky pairs?
输入描述:

    The input consists of two lines.
    The first line contains an integer n.
    The second line describes the n integers you have.
测试用例分布及其范围

    30%   n <= 100,    0 < ai <= 1000
    70%   n <= 1000,   0 < ai <= 10^9
    100%  n <= 100000, 0 < ai <= 10^9
输出描述:

    Output a single line containing the number of lucky pairs.
**示例1**
输入

    4
    1 2 11 22
输出

    2
说明

    112 and 21 are multiples of 7.


**Discuss:**



**Code:**

```java
import java.util.*;
public class Main{
    public static int luckyPairs(int[] nums){
        int n = nums.length;
        int cnt = 0;
        
        return cnt;
    }
    public static void main(String[] args) {
       Scanner in = new Scanner(System.in);
       int n = in.nextInt();
       int[] nums = new int[n];
       for(int i = 0; i < n; ++ i) nums[i] = in.nextInt();
       int res = luckyPairs(nums);
       System.out.println(res);
    }
}
```