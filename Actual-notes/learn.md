# Two Sum

Three approaches - 
1. Brute Force O(n^2)
2. Sort and two pointer O(n log n)
3. Hashmap O(n) (One pass)


  ```python 
    class Solution:
        def twoSum(self, nums: List[int], target: int) -> List[int]:
            for i in range(len(nums)):
                for j in range(i + 1, len(nums)):
                    if nums[i] + nums[j] == target:
                        return [i, j]
            return []
  ```

  ```python 
    class Solution:
        def twoSum(self, nums: List[int], target: int) -> List[int]:
            A = []
            for i, num in enumerate(nums):
                A.append([num, i])
            
            A.sort()
            i, j = 0, len(nums) - 1
            while i < j:
                cur = A[i][0] + A[j][0]
                if cur == target:
                    return [min(A[i][1], A[j][1]), 
                            max(A[i][1], A[j][1])]
                elif cur < target:
                    i += 1
                else:
                    j -= 1
            return []
  ```

    ```python 
    class Solution:
        def twoSum(self, nums: List[int], target: int) -> List[int]:
            prevMap = {}  # val -> index

            for i, n in enumerate(nums):
                diff = target - n
                if diff in prevMap:
                    return [prevMap[diff], i]
                prevMap[n] = i
  ```

------------------------------------------------------------------------------------------------------------------------------------------------------------

# 