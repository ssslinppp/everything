# Maximum Subarray 连续子数组的最大和

题目：
输入一个整形数组，数组里有正数也有负数。
数组中连续的一个或多个整数组成一个子数组，每个子数组都有一个和。
求所有子数组的和的最大值。要求时间复杂度为O(n)。

例如输入的数组为`1, -2, 3, 10, -4, 7, 2, -5`，和最大的子数组为`3, 10, -4, 7, 2`，
因此输出为该子数组的和18。

[leetcode-53 Maximum Subarray 连续子数组的最大和](https://blog.csdn.net/woliuyunyicai/article/details/50074737) 

《剑指Offer》题31   

# 思路

一个数值加上一个负值，所得到的结果自然小于其本身。
使用一个curSum变量来记录当前所考察的子数组的当前和，curMax变量来记录当前所得到的最大值；

遍历数组，若curSum为负值，则加上当前值num[i],所得到的和一定小于num[i],则直接将前面的子数组舍弃，取当前值为下一个继续考察的子数组的起点，则curSum=nums[i]；

若curSum为正值，可想而知，直接加上num[i]会得到更大值；

比较当前获得的curSum与之前获得的最大值，每一步都得到当前的最大值，则最终获得的最大值即为curMax所记录的值；

LeetCode代码：
```
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums == null || nums.length== 0){
            return 0;
        }
        
        int curMax = nums[0];
        int curSum = nums[0];
        
        for(int i=1;i<nums.length;i++){
            if(curSum < 0){//当前子序列结束
                curSum = 0;
            }
            
            curSum+=nums[i];

            if(curSum > curMax){
                curMax = curSum;
            }
        }
        
        return curMax;
    }
}
```
