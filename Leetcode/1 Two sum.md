### 指針法
```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        st = 0
        end = len(nums)-1
        while st < end:
            for i in range(end, st, -1):
                if nums[st] + nums[i] == target:
                    return [st, i]
            st+=1
```
