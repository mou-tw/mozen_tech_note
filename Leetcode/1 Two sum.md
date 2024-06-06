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


## Hash Table 法
### two-pass hash table
```
class Solution:

    def twoSum(self, nums: List[int], target: int) -> List[int]:

        #hash table solution
        numMap = {}
        for i in range(len(nums)):
            numMap[nums[i]] = i
        for i in range(len(nums)):
            complement = target - nums[i]
            if complement  in numMap and numMap[complement] != i:
                return [i, numMap[complement]]
        return []
```

### one-pass hash table
```
class Solution:

    def twoSum(self, nums: List[int], target: int) -> List[int]:

        #hash table solution

        numMap = {}

        for i in range(len(nums)):

            complement = target - nums[i]

            if complement in numMap:

                return [i, numMap[complement]]

            numMap[nums[i]] = i

        return []
```