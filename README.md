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

H:

Brute force

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

Two pointer

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

Binary search

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



### [18.4Sum](https://leetcode.com/problems/4sum)

H:

2 pointer / hash set

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

- H:

  - Pesudo Code

  Originally: 

  ```
  for i = 1; i < nums.length;i++:
  	pre = cur;
  	cur = nums[i]
  	if cur == pre
  		remove duplicate and bring rest of array forward
  ```

  Improved solution:

  - Improvement:
    - Fast pointer to skip duplicated value
    - Slow pointer stops when detecting multiple duplicates, so that it can handle repeated numbers like 1,1,1,1,2,2.

  ```
  slow_pointer = 0;
  for fast_pointer = 1; fast_pointer < nums.length; fast_pointer++
  	if nums[slow_pointer] != nums[fast_pointer]
  		slow_pointer++;
  		nums[slow_pointer] = nums[fast_pointer]
  return slow_pointer + 1
  ```

  

- Analysis

  - Time: O(n)
  - Space O(1)

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

  ```java
  public int removeElement(int[] nums, int val) {
      int i = 0;
      int n = nums.length;
      while (i < n) {
          if (nums[i] == val) {
              nums[i] = nums[n - 1];
              // reduce array size by one
              n--;
          } else {
              i++;
          }
      }
      return n;
  }
  ```

- Analysis

  - Time O(n)
  - Space O(1)

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

Hanfei:

Intution:

- Perhaps need to swap current digit with others. i.e 1,2,3
  - if index = 0, array[index] = 1, might need to swap index = 0 with either index = 1 or 2
- Start from test cases: 1234567, ended with case 7654321
  - Observation 1: this is in a loop, with max and min boundary. They have a pattern for the next number. Find the pattern, then it becomes 1 step solution.
  - Observation 2: this ordering happens from lowest digit to highest digit.
    - input 7654321, we examine digit using slow, fast pointer, there we cannot find array[fast] < array[slow], thus, we reach the end 
    - input 6754321, we find when array[fast] = 6, array[slow] = 7 so the next step should make swap between slow & fast pointer
    - input 7564321, we find when array[fast] = 5, array[slow] = 6 so the next step should make swap between slow & fast pointer, then sequence become 7654321

```java
    public void nextPermutation(int[] nums) {
        int slow = nums.length -1;
        boolean noSwap = true;
        for(int fast = nums.length -2; fast >=0;fast--){
            if(nums[fast] >= nums[slow]){
                slow--;
            } else {
                int temp = nums[fast];
                nums[fast] = nums[slow];
                nums[slow] = temp;
                noSwap = false;
                break;
            }
        }
        
        if(noSwap){
            int i = 0;
            int j = nums.length - 1;
            while (i < j){
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++;
                j--;
            }
            
        }
    }
```

Correct solution:

- Should simply swap fast slow point position, instead, track back and find the proper spot

  ```java
      class Solution {
          public void nextPermutation(int[] nums) {
              int slow = nums.length -1;
              int fast = nums.length -2;
              boolean noSwap = true;
              for(; fast >=0;fast--){
                  if(nums[fast] < nums[slow]){
                      break;
              } else {
                         slow--;
              }
          }
              if(fast >= 0){
                         //trace back 
                      int i = nums.length -1;
                      while( nums[i] <= nums[fast]){
                          i--;
                      }
                      noSwap = false;
                      swap(nums,fast,i);
              }
                     reverse(nums,fast+1);
          }
           public void reverse(int[] nums, int start) {
              int i = start, j = nums.length - 1;
              while (i < j) {
                  swap(nums, i, j);
                  i++;
                  j--;
              }
          }
            private void swap(int[] nums, int i, int j) {
              int temp = nums[i];
              nums[i] = nums[j];
              nums[j] = temp;
          }
  
      }
  ```

- Analysis

  - Time O(n)
  - Space O(1)

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

### [33.Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array)

Hanfei:

Original idea:

1. This is a rotation array, we can use two pointer from 2 ends
2. Stop condition is find the rotate or left >= right
3. Since sorted if target > array[left] left ++, if target < array[right], right--



Correct solution: :star:

1. Binary search to find rotate
2. Binary Search on each part of sorted array to find target

```java
class Solution {
    
    int [] nums;
    int target;
    
    
    public int findRotate(int left, int right){
        
        if(nums[left] < nums[right]){ // base case, there is no rotate
            return 0;
        }
        
        while (left <= right){ // start of the loop
            int pivot = (left + right) / 2;
            if (nums[pivot] > nums[pivot + 1]){// pivot + 1 is desired rotate
                return pivot + 1;
            }
            
            if(nums[left] <= nums[pivot]){// the left part is good, next time look to right part
                // very important this equal sign
                left = pivot + 1;
            } else { // the left part is not good, next time look to the left
                right = pivot - 1;
            }
        }
            
        return 0;
        
    }
    
    public int binSearch(int left, int right, int target){// this is a sorted array
        while(left <= right){
            int pivot = (left + right) / 2;
            if(nums[pivot] == target)
                return pivot;
            
            if(nums[pivot] > target){ // curr value > target look to left
                
                right = pivot - 1;
            } else { // curr value < target, look to right
                left = pivot + 1;
                
            }
        }
        
        return -1;
    }
    
    
    public int search(int[] nums, int target) {
        this.nums = nums;
        this.target = target;
        int left = 0;
        int right = nums.length - 1;
        //missing condition check
        if (this.nums.length == 1){
            if (nums[0] == target)
                return 0;
            return -1;
        }
        
        
        int pivot = findRotate(left,right);
        int result = -1;
        
        if(nums[pivot] == target){
            return pivot;
        }
        
        if(pivot == 0){
            return binSearch(left,right,target);
        }
        
        if(nums[0] > target){ //special notice: look right
            return binSearch(pivot, right,target);
        } 
        
        return binSearch(left, pivot,target);
    }
    
    
    
}
```

Analysis

- Time O(logN)
- Space: O(1)

### [34.Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array)

Hanfei

Original thought:

- use binary search

But it doesn't solve the first occurrence problem

```java
class Solution {
    
    public int binSearch(int left, int right, int[] nums, int target){
        
        while(left <= right){
            int pivot = (left + right) / 2;
            if(nums[pivot] == target){
                return pivot;
            }
            
            if(nums[pivot] < target){
                left = pivot + 1;
            } else {
                right = pivot -1 ;
            }
        }
        
        return -1;
    }
    
    
    
    public int[] searchRange(int[] nums, int target) {
    
        int left = 0;
        int right = nums.length - 1;
        
        int pivot = binSearch(left,right,nums,target);
        
        int [] result = new int [2];
        result[0] = -1;
        result[1] = -1;
        
        if(pivot == -1){
            return result;
        }
        
        
        int end = pivot;
        for(; end < nums.length;end++){
            if(nums[pivot] != nums[end]){
                end -= 1;
                break;
            }
              
                
        }
        result[0] = pivot;
        result[1] = end;
        
        return result;
        
        
    }
}
```

Correct solution

- correct the binary search with

```java
    public int binSearch(int left, int right, int[] nums, int target, boolean firstOccure){
        
        while(left <= right){
            int pivot = (left + right) / 2;
            if(nums[pivot] == target){
                
                if(firstOccure){
                    if(left == pivot || nums[pivot - 1] != target){ // if we cannot further extend to left, return current
                        return pivot;
                    }
                    right = pivot - 1; // otherwise, explore to left
                } else {
                    if(right == pivot || nums[pivot + 1] != target){ // if we cannot further extends to right, then return current
                        return pivot;
                    }
                    left = pivot + 1;// otherwise, explore to right
                }
            }  else if(nums[pivot] < target){
                left = pivot + 1;
            } else {
                right = pivot -1 ;
            }
        }
        
        return -1;
    }
```

- correct the main function with

  ```
      public int[] searchRange(int[] nums, int target) {
      
          int left = 0;
          int right = nums.length - 1;
          int [] result = new int [2];
          result[0] = -1;
          result[1] = -1;
          
          
          int first = binSearch(left,right,nums,target,true);
          
          
          
          if(first == -1){
              return result;
          }
          
          
          int end = binSearch(left,right,nums,target,false);
      
          result[0] = first;
          result[1] = end;
          
          return result;
          
          
      }
  ```

  Analysis:

  - Time O(logN)
  - Space O(1)

### [39. Combination Sum](https://leetcode.com/problems/combination-sum)

Hanfei

Original thoughts

- brute force. build a pool of input * 150, and generates the result

Correct solution

![image-20210914015734925](pictures\image-20210914015734925.png)

- back tracking

  - incrementally build candidates to solution
  - abandons candidates as soon as it fails

  ```java
  class Solution {
      
      
      public void backTrack(List<List<Integer>> result , LinkedList<Integer> combination, int target, int start, int [] candidates){
          
          if(target == 0){
              result.add(new ArrayList<Integer>(combination)); // need to use new ArrayList<Integer>(combination)) otherwise, it won't give input
              return;
          }
          
          if(target < 0){
              return;
          }
          
          for(int i = start; i < candidates.length ;i++){ 
              combination.add(candidates[i]); // each loop will first consider the start position of the candidate
              backTrack(result, combination, target - candidates[i], i, candidates);
              combination.removeLast(); // if candidates fails, then remove the last one and examine next element in the array
              
          }
      }
  
  public List<List<Integer>> combinationSum(int[] candidates, int target) {
      List<List<Integer>> result = new ArrayList<>();
      LinkedList<Integer> combination = new LinkedList<>(); // linked list has method removeLast
      backTrack(result,combination,target,0,candidates);
      
      
      
      return result;
  }
  }
  ```



Analysis

> Let *N* be the number of candidates, T be the target value, and *M* be the minimal value among the candidates.

- Time O(N ^(T/M + 1))
  - First of all, the fan-out of each node would be bounded to N*N*, *i.e.* the total number of candidates.
  - The total # of backtracking = # of node in the DFS tree
  - The maximum depth = T/M
  - The maximal number of nodes in N-ary tree of T/M is N^(T/M + 1)
- Space 
  - The number of recursive calls can pile up to T/M where we keep on adding the smallest element to the combination. As a result, the space overhead of the recursion is O(T/M)
  - Note that, we did not take into the account the space used to hold the final results for the space complexity.

### [40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

Hanfei

- Original thoughts:
  - Inherit combination sum backtrace  
  - change start index from i = start to i = start + 1
  - Didn't work since there is duplicate case [1,7,1] and target = 9
    - the solution will be [1,7] and [7,1]            

```java
        for(int i = start+1; i < candidates.length ;i++){ 
            combination.add(candidates[i]); // each loop will first consider the start position of the candidate
            backTrack(result, combination, target - candidates[i], i, candidates);
            combination.removeLast(); // if candidates fails, then remove the last one and examine next element in the array
        }
```

Correct solution

- either keep a counter

- or use index

  - sort the array first then skip duplicate value

  ```java
          for(int i = start; i < candidates.length ;i++){ 
              if(i > start && candidates[i] == candidates[i-1]){//skip duplicate
                  continue;
              }
              
              combination.add(candidates[i]); // each loop will first consider the start position of the candidate
              backTrack(result, combination, target - candidates[i], i+1, candidates);
              combination.removeLast(); // if candidates fails, then remove the last one and examine next element in the array
          }
  ```



Analysis

![image-20210915003350000](pictures/image-20210915003350000.png)





### [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

Original thoughts

- flatten array
- for array 1-9, it has pattern 1,2,3,4 -> 4+4, 9,10,11 -> 12
- marked visited as -1

Correct solution

```java
public List<Integer> spiralOrder(int[][] matrix) {
        int VISITED = 101;
        int rows = matrix.length;
        int columns = matrix[0].length;
        // Four directions that we will move: right, down, left, up.
        int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        // Initial direction: moving right.
        int currentDirection = 0;
        // The number of times we change the direction.
        int changeDirection = 0;
        // Current place that we are at is (row, col).
        // row is the row index; col is the column index.
        int row = 0;
        int col = 0;
        // Store the first element and mark it as visited.
        List<Integer> result = new ArrayList<>();
        result.add(matrix[0][0]);
        matrix[0][0] = VISITED;
        while (changeDirection < 2) {
            while (row + directions[currentDirection][0] >= 0 &&
                   row + directions[currentDirection][0] < rows &&
                   col + directions[currentDirection][1] >= 0 &&
                   col + directions[currentDirection][1] < columns &&
                   matrix[row + directions[currentDirection][0]]
                   [col + directions[currentDirection][1]] != VISITED) {
                // Reset this to 0 since we did not break and change the direction.
                changeDirection = 0;
                // Calculate the next place that we will move to.
                row = row + directions[currentDirection][0];
                col = col + directions[currentDirection][1];
                result.add(matrix[row][col]);
                matrix[row][col] = VISITED;
            }
            // Change our direction.
            currentDirection = (currentDirection + 1) % 4;
            // Increment change_direction because we changed our direction.
            changeDirection++;
        }
        return result;
    }
```

Analysis

- Time O(M*N) iterate each element once
- Space O(1), didn't use other space

### [55. Jump Game](https://leetcode.com/problems/jump-game/)

### [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

### [57. Insert Interval](https://leetcode.com/problems/insert-interval/)

### 35.[Search Insert Position](https://leetcode.com/problems/search-insert-position)

### 36.[Valid Sudoku](https://leetcode.com/problems/valid-sudoku)

### 45.[Jump Game II](https://leetcode.com/problems/jump-game-ii)

### 46.[Permutations](https://leetcode.com/problems/permutations)

### 47.[Permutations II](https://leetcode.com/problems/permutations-ii)

### 48.[Rotate Image](https://leetcode.com/problems/rotate-image)



## 09-13 - 09 - 17

### 53.[Maximum Subarray](https://leetcode.com/problems/maximum-subarray)

### 56.[ Merge Intervals](https://leetcode.com/problems/merge-intervals)

### 57.[Insert Interval](https://leetcode.com/problems/insert-interval)

### 59.[Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii)

### 63.[Unique Paths II](https://leetcode.com/problems/unique-paths-ii)

### 64.[ Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum)

### 66.[Plus One](https://leetcode.com/problems/plus-one)

### 73.[Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes)

### 74.[Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix)

### 75.[Sort Colors](https://leetcode.com/problems/sort-colors)

### 77.[Combinations](https://leetcode.com/problems/combinations)

### 78.[ Subsets](https://leetcode.com/problems/subsets)

### 79.[ Word Search](https://leetcode.com/problems/word-search)

### 80.[Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii)