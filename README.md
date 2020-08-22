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
### 多数元素
```
# 偷懒写法
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        nums.sort()
        mid = len(nums)//2
        return nums[mid]
        
# 占空间写法
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        record = {}
        mid = len(nums)//2
        for i in range(len(nums)):
            if(nums[i] in record):
                record[nums[i]] +=1
            else:
                record[nums[i]] = 1
            if(record[nums[i]]>mid):
                return nums[i]
        return 0

```
## Task02 动态规划

### 打家劫舍
```
class Solution:
    def rob(self, nums: List[int]) -> int:
        if(len(nums)<1):
            return 0
        # 到第n晚能偷到最多的钱
        one,two = nums[0],None
        cur = nums[0]
        for i in range(1,len(nums)):
            if(i==1):
                cur = max(one,nums[i])
                two = one
                one = cur
                continue
            cur = max(one,two+nums[i])
            two = one
            one = cur
        return cur
```
### 打家劫舍2
```
class Solution:
    def rob(self, nums: List[int]) -> int:
        if(len(nums)<1):
            return 0
        # 分成两种情况 偷不偷第一家
        dp1 = [0]*len(nums)
        dp1[0] = nums[0]
        for i in range(1,len(dp1)):
            if(i==1 or i==len(dp1)-1):
                dp1[i] = dp1[i-1]
                continue
            dp1[i] = max(dp1[i-1],dp1[i-2]+nums[i])
        
        dp2 = [0]*len(nums)
        for i in range(1,len(dp2)):
            if(i==1):
                dp2[i] = nums[i]
                continue
            dp2[i] = max(dp2[i-1],dp2[i-2]+nums[i])
        
        return dp1[-1] if dp1[-1]>dp2[-1] else dp2[-1]
```
### 最长递增子序列
```
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        if(len(nums)<2):
            return len(nums)
        last = 1
        result = 1
        for i in range(1,len(nums)):
            if(nums[i]>nums[i-1]):
                last +=1
                if(last>result):
                    result = last
            else:
                last = 1

        return result
```
### 最长回文子串
```
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if(len(s)<2):
            return s
        dp = [[False]*len(s) for i in range(len(s))]

        for i in range(len(s)):
           dp[i][i] = True

        start = 0
        max_length = 1
        for j in range(1,len(s)):
            for i in range(0,j):
                if(s[i]==s[j]):
                    if(j-i<3):
                        dp[i][j] = True
                    else:
                        dp[i][j] = dp[i+1][j-1]
                else:
                    dp[i][j] = False

                if dp[i][j]:
                    cur_length = j-i+1
                    if(cur_length>max_length):
                        start = i
                        max_length = cur_length

        return s[start:start+max_length]
```
### 最长回文子序列
```
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        if(len(s)<2):
            return len(s)
        dp = [[0]*len(s) for i in range(len(s))]
        for i in range(len(s)):
            dp[i][i] = 1
        max_length = 1

        for i in range(len(s)-1,-1,-1):
            for j in range(i+1,len(s)):
                if(s[i]==s[j]):
                    dp[i][j] = dp[i+1][j-1]+2
                else:
                    dp[i][j] = max(dp[i+1][j],dp[i][j-1])

                if(dp[i][j]>max_length):
                    max_length = dp[i][j]
        
        return max_length
```
### 编辑距离
```
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        
        dp = [[0]*len(word2) for _ in range(len(word1))]

        def find(n,m):
            if(n==-1):
                return m+1
            if(m==-1):
                return n+1
            if(not dp[n][m]==0):
                return dp[n][m]
            
            if(word1[n]==word2[m]):
                return find(n-1,m-1)
            else:
                dp[n][m] = min(find(n-1,m),find(n,m-1),find(n-1,m-1))+1
                return dp[n][m]
        
        return find(len(word1)-1,len(word2)-1)
```
## Task03 查找
### 搜索插入位置
```
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        if(len(nums)<1): return -1

        left,right = 0,len(nums)-1
        while(left<right):
            mid = (left+right)//2
            if(nums[mid]==target):
                return mid 
            if(nums[mid]<target):
                left = mid+1
            else:
                right = mid-1

        return left if nums[left]>=target else left+1
```
