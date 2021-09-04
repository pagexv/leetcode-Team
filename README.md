# Introduction

Complete 14 leetcode questions weekly, no upper bound.

## 09-02 ~ 09-05
Minimum goal (6 questions)
### [15. 3Sum](https://leetcode.com/problems/3sum/)
Naicheng

分析：
1. 考虑特解，长度小于三。
2. 给list排序,是为了使用双指针法。
3. 之后用双指针法，首先做一个循环，如果当前循环`nums[i]`的数值大于0，就可以直接返回已有结果（这是一个排序的目的）
4. 确定两个指针，`left = nums[i + 1]` ，`right = nums[n]`,
	- if sum > 0, right is too big then we move the right to left
	- if sum < 0, left is too small then we move the left to right.
	- if sum == 0, first we need to store this result. to avoid duplication, we need to jeep all same values of left and right pointers. then to quit this loop, we move left and right to center.
5. 在最开始的时候可以跳过一次重复解，后面通过left and right pointers can jump all duplications(**但其实我还是有点不懂为什么只有一个不行**)

Python3 code
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        
        if len(nums) < 3: return []
        nums.sort()
        n = len(nums)
        res = []
        for i in range(n):
            if nums[i] > 0:
                return res
            if nums[i] == nums[i-1] and i > 0:
                continue
            left = i + 1
            right = n - 1
            while left < right:
                if nums[right] + nums[left] + nums[i] == 0:
                    res.append([nums[right], nums[left], nums[i]])
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1
                    left += 1
                    right -= 1
                elif nums[right] + nums[left] + nums[i] > 0:
                    right -= 1
                else:
                    left += 1
        return res
```



### [16.3Sum Closest](https://leetcode.com/problems/3sum-closest)

#### H:

###### Brute force

- Pesudo Code

  ```
  closest = Integer.MAX
  for i = 0 to n
  
  	for j = i + 1 to n
  
           for k = j + 1 to n
               sum = nums[i] + nums[j] + nums[k]
               
               closest = math.min(target - sum, closest)
               
  return closet;
  
  ```

  

- Analysis

  Time O(n*3)

  Space O(n)

###### Two pointer

Sort the array, then use two point

- Pesudo Code

  ```
  closest = Integer.MAX
  for i = 0 to n - 1
  	low = i + 1, high = n - 1
  	// Missed
  	// Array.sort(nums) sort array
  	while num[low] < num [high]
  		s = num [i] + num [low] + num [high]
  		diff = math.abs(target - s)
  		closet = math.min(closet, diff)
  		if diff = 0
  			break
  		if s < target
  			low ++
  		else
  			high++
  		
  return closet;
  			
  ```

- Analysis

  Time  O(n^2)

  Space O(nlogn ) ~ O(n)

###### Binary search

```
closest = Integer.MAX
for i = 0 to n - 1
	low = i + 1, high = n - 1
	// Missed
	// Array.sort(nums) sort array
	while num[low] < num [high]
	    i_close = target - (+ num [low] + num [high])
	    i = binary search + for index that is closet to i_close
		s = num [i] + num [low] + num [low]
		diff = math.abs(target - s)
		closet = math.min(closet, diff)
		if diff = 0
			break
		if s < target
			low ++
		else
			high++
return closet;
			
```





N:

Naicheng

```python

```



---



#### [18.4Sum](https://leetcode.com/problems/4sum)

##### H:

######  2 pointer / hash set

- Pesudo Code

  ```
  def solution{
  	sort array
  	ksum
  }
  def ksum(int []nums, int target, int start, int k){
  	List<List<Integer>> res // result arraty
  	if(start = nums.length-1 or nums[start] * 4 > target or nums[nums.length - 1] * 4 < target ){ // either reach end of array, or start of sorted array * 4 is too large or end of sorted array * 4 is too small
  		return res
  	}
  	if (k == 2)
  		return twosum
  	
  	for i = start; i < nums.length; i++
  		if i = start or current value != current value
  			for (each subset : ksum(nums, target, i+1, k - 1)){
  				subset.add(nums[i])
  				res.add(subset)
  			}
  			
  	return res
  }
  
  def twosum(nums, target, start){
  	low = start
  	high = nums.length - 1
  	while low < high
  		sum = low + high
  		if sum == target
  			res.add list[num[low++], num[high++] ] // find all combinations of 2 sum
  		else if sum < target or (low > start && num[low] == num[low -1])
  			++low
  		else if sum > targe or (high < length - 1 && nums[high] == nums[high + 1])
  			--high
  		
  		
  }
  
  def twoSumHashSet(nums, target, start){
  	hashset
  	List<List<int>> res
  	for i = start; i < nums.length - 1; i++
  		if res.size > 0 &&  res.get(res.size() - 1).get(1) != nums[i]{ // avoid duplicate
  			if(hashset.contains(target - nums[i])){
  		 		res.add(hashseht.get(target - numsi), nums[i])
  			}
  			
  		}
  		hashset.add(nums[i])
  		
  	return res
  }
  
  
  
  ```

  

- Analysis

 Time: O(n^3) same for both 2 pointer & hash set

 Space O(n)



N:

- Pesudo Code
- Analysis

---



### [26.Remove Duplicated from sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array)

H:

- Pesudo Code
- Analysis

N:
双指针法

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        
        slow = 1
        for fast in range(1, len(nums)):
            if nums[fast] != nums[fast - 1]:
                nums[slow] = nums[fast]
                slow += 1

        return slow
```

---

### [27.Remove element](https://leetcode.com/problems/remove-element)

H:

- Pesudo Code
- Analysis



Naicheng:

双指针法

1. This is a sorted list
2. 初始化fast = slow
3. fast一直循环，当遇到了需要remove的element的时候，slow就停下来不再增加
4. 直到下一个不需要remove的元素，把当前的值放在slow的位置。

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        slow = 0
        for fast in range(0, len(nums)):
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            
        return slow
```

-----

### [31. Next Permutation](https://leetcode.com/problems/next-permutation/)

Naicheng

分析：
这一题可不简单有好多边界条件需要考虑，Colin看看？

python3
```python
def nextPermutation(nums):
    n = len(nums)
    i = n - 2
    while nums[i] >= nums[i + 1] and i >= 0:
        i -= 1 # 这里可以确定i

    if i >= 0:
        j = n - 1
        while j >= 0 and nums[i] >= nums[j]:
            j -= 1
        nums[i], nums[j] = nums[j], nums[i]

    left, right = i + 1, n - 1
    while left < right:
        nums[left], nums[right] = nums[right], nums[left]
        left += 1
        right -= 1
```

## 09-06 ~ 09-12

Goal: compelet 14 problem

[33.Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array)

[34.Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array)

[39. Combination Sum](https://leetcode.com/problems/combination-sum)

[40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

[54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

[55. Jump Game](https://leetcode.com/problems/jump-game/)

[56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

[57. Insert Interval](https://leetcode.com/problems/insert-interval/)