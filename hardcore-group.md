
## 2023/06/19

### Question 1
https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/

Given an integer array nums sorted in non-decreasing order, remove some duplicates in-place such that each unique element appears at most twice. The relative order of the elements should be kept the same.

Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.

Initial Solution
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        // boolean hitDuplicateMax = true;
        int indexToFill = 0;
        int allowedDuplicate = 2;
        int previous = nums[0];
        
        int uniqueCount = 0;
        for(int i = 0; i < nums.length; i++){
            if(allowedDuplicate == 0){
                // // Current duplicate has reached maximum count
                // if(hitDuplicateMax){
                //     hitDuplicateMax = false;
                //     indexToFill = i;
                // }
                
                if(nums[i] != previous){
                    // current number isn't a duplicate
                    // The initial value needs to be 1
                    allowedDuplicate = 1;
                    nums[indexToFill++] = nums[i];
                    uniqueCount++;
                } else {
                    // current number is a duplicate
                }
            } else {
                if(nums[i] == previous){
                    // Current number is a duplicate
                    allowedDuplicate--;
                    uniqueCount++;
                    
                } else {
                    // Current number isn't a duplicate
                    
                    uniqueCount++;
                }
                   nums[indexToFill++] = nums[i];
            }
            previous = nums[i];
        }
        
        return uniqueCount;
        
    }
}
```

Improved solution:
Remove redundant parameter previous & uniqueCount
Overwrite values that we don't want, and return the pointer index


```java
class Solution {
    public int removeDuplicates(int[] nums) {
        // boolean hitDuplicateMax = true;
        int indexToFill = 1;
        int allowedDuplicate = 1;
      
        
     //   int uniqueCount = 1;
        for(int i = 1; i < nums.length; i++){
            
             if(nums[i] == nums[i-1]){
                    // Current number is a duplicate
                    allowedDuplicate--;
                    
             } else {
                 // current number isn't a duplicate
                    // The initial value needs to be 1
                   allowedDuplicate = 1;
                    // nums[indexToFill++] = nums[i];
                    // uniqueCount++;
             }
            
            
            if(allowedDuplicate >= 0){
                  nums[indexToFill++] = nums[i];
            }
        
          
        }
        
        return indexToFill;
        
    }
}
```
Time O(1) Space O(N)

![image](https://github.com/pagexv/leetcode-Team/assets/33244427/0c9898e0-2982-4b43-a2ae-8604e87e6444)


## 2023/06/20

### Question 1
https://leetcode.com/problems/edit-distance

Initial solution: Build DP table

```java
class Solution {
    public int minDistance(String word1, String word2) {
        
        // Tabulet soln
        // Given horse to ros
        // i = row, j = column
        //     "" h ho hor hors horse
        //  "" 0  1  2  3   4     5
        //  r  1  1  2  2   3     4
        //  ro 2  2  1
        //  ros        
        
        
        int word1Size = word1.length();
        int word2Size = word2.length();
        if(word1Size == 0){
            return word2Size;
        }
        
        if(word2Size == 0){
            return word1Size;
        }

        // initialize the tables
        int dp[][] = new int[word1Size + 1][word2Size + 1];

        // initialize all the left row
        for(int i = 1; i <= word1Size; i++){
            dp[i][0] = i;
        }
        // initialize all the top col
        for(int i = 1; i < word2Size; i++){
            dp[0][i] = i;
        }

        // calculating the optimal solution, and the result is in the right cornor
        for(int i = 1; i <= word1Size; i++){
            for(int j = 1; j <= word2Size; j++){
                if(word2.charAt(j-1) == word1.charAt(i - 1)){
                    dp[i][j] = dp[i-1][j-1];
                } else {
                    dp[i][j] = Math.min(dp[i-1][j], Math.min(dp[i][j-1], dp[i-1][j-1])) + 1;
                }
            }
        }
            
        return dp[word1Size][word2Size];
    }
}

```
Time O(M * N) Space O(M * N)

Alternative approach: Top down / recursion + memorization


```java
class Solution {
    
    int [][] memo;
    public int minDistance(String word1, String word2) {
        memo = new int [word1.length()+1][word2.length()+1];

        return dp(word1.length() ,  word2.length() , word1, word2);
    }
    
    
    public int dp(int word1Index, int word2Index, String word1, String word2){
        if(word1Index <= 0){
            return word2Index;
        }
        
        if(word2Index <= 0){
            return word1Index;
        }
        
        if(memo[word1Index][word2Index] != 0){
            return memo[word1Index][word2Index];
        }
        
        if(word1.charAt(word1Index-1) == word2.charAt(word2Index-1)){
            return dp(word1Index-1, word2Index-1, word1, word2);
        } else {
            // enumerate all possible cases
            // word1 = horse, word2 = ros
            // First scan
            // word1index = 4, word2index = 3
        //     horse
        //       ros
       //          ^
            // possible add: word1 add a "e" to match, word1 = horses, word2 = ros. Since the last char of word1 & word2 will match
       //      horses       word1's current postion = word1index + 1
       //         ros       word2's current position = word2Index
       //      move to next char word1's current postion becomes = word1index (unchanged)
      //                         word2's current position becomes = word2Index - 1

            
            // possible delete: word 1 delete last char, word1 = hors, word2 = ros. Since we are just deleting, no match

        //     hors              word1's current postion = word1index  - 1
       //       ros              word2's current position = word2Index
       //         ^
      //      move to next char word1's current postion becomes = word1index - 1
      //                         word2's current position becomes = word2Index  (unchanged)
            // possible replace: word1 replace last char to make them match, so both index - 1
      //      horss              word1's current postion = word1index 
      //        ros              word2's current postion = word2index
      //         ^
    //         move to next char word1's current postion becomes = word1index - 1
      //                         word2's current position becomes = word2Index  - 1
            // possible delete: word1 delete last
            int add = dp(word1Index, word2Index - 1, word1, word2);
            int delete = dp(word1Index - 1, word2Index, word1, word2);
            int replace = dp(word1Index -1 , word2Index -1 , word1, word2);
            
            int min = Math.min(add, Math.min(delete, replace)) + 1;
            // Save the optimal solution in memory
            memo[word1Index][word2Index] = min;
        }
        return memo[word1Index][word2Index];
    }
}
```

Time O(M * N) Space O(M * N)
