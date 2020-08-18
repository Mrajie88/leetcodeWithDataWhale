# 打卡记录

## Task01
### pow(x,n)
- 主要复习一下快速幂。写的时候踩了个小坑，忘记判断n的正负性，没能一次a过。附代码。
```class Solution:
     def myPow(self, x: float, n: int) -> float:
        # 复习快速幂
        base = x
        result = 1
        tag = False
        if n<0:
            tag = True
            n = -1*n
        
        while(n>0):
            if(n%2==0):
                base = base*base
                n = n//2
            else:
                result = result*base
                n = n-1
        if tag:
            return 1/result
        return result
 ```
 ### 最大子序和
```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        if(len(nums)<1):
            return 0
        dp = [0 for i in range(len(nums))]
        result = nums[0]
        dp[0] = nums[0]
        for i in range(1,len(nums)):
            dp[i] = max(dp[i-1]+nums[i],nums[i])
            if(dp[i]>result):
                result = dp[i]
        
        return result
```
