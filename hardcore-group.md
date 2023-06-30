
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
![image](https://github.com/pagexv/leetcode-Team/assets/33244427/4e717b46-bb76-49b8-ae4a-4368edae8b3a)


## 2023/06/21 

补档

https://leetcode.com/problems/binary-tree-right-side-view/

### Question 1 

#### Thoughts 1: DFS

Doesn't work because it doesn't consider all possible nodes

```java
   public List<Integer> rightSideView(TreeNode root) {
    /*    
        DFS doesn't work
        if(root == null){
            return result;
        }
        
        result.add(root.val);
       // always add right element first
        if(root.right != null){
            return rightSideView(root.right);
        } else if(root.left != null){
            return rightSideView(root.left);
        }
        
        return rightSideView(root.right);
        }
        */
```

#### Thoughts 2 : Simple BFS

Doesn't work because it didn't capture any level information

```java
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    public List<Integer> rightSideView(TreeNode root) {
    /*    
        
        if(root == null){
            return result;
        }
        
        result.add(root.val);
       // always add right element first
        if(root.right != null){
            return rightSideView(root.right);
        } else if(root.left != null){
            return rightSideView(root.left);
        }
        
        return rightSideView(root.right);
        */
        
        // Need to use BFS
        //         1
        //       2    3
        //     4  5   6  7
        
        //  1 2 3 6 7
        
        stack.push(root);
        bfs();
        return result;

    }
    
    public void bfs(){
        
        
        if(stack.isEmpty()){
            return;
        }
        
        TreeNode next = stack.pop();
        
        if(next == null){
            return;
        }
        result.add(next.val);
        
        if(next.left != null){
            stack.push(next.left);
        }
        if(next.right != null){
            stack.push(next.right);
        }
        
        if(next.left == null && next.right == null){
            return;
        }
        bfs();
    }
```

#### Solution 1

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    Stack<Integer> level = new Stack<>();
    public List<Integer> rightSideView(TreeNode root) {
    /*    
        
        if(root == null){
            return result;
        }
        
        result.add(root.val);
       // always add right element first
        if(root.right != null){
            return rightSideView(root.right);
        } else if(root.left != null){
            return rightSideView(root.left);
        }
        
        return rightSideView(root.right);
        */
        
        // Need to use BFS
        //         1          level 0    curlevel 0  nextlevel = 1, result.size == 0
        //       2    3       level 1    curlevel 1, pop 3 , => no next level => check  nextlevel = 2, result.size = 1
        //     4  5            level 2    
        //       8
        //  1 2 3 6 7
        
        stack.push(root);
        level.push(0);
       
        
        bfs(0);
        return result;

    }
    
    // level 0: 1       result.size() = 0   before adding
    // level 1: 2 3     result.size() = 1   before adding
    // level 2: 4       result.size() = 2   before adding
    public void bfs(int curLevel){
        
        
        if(stack.isEmpty()){
            return;
        }
        
        
        // Popping the stack for latest item
        // case 1: it is the previous nodes
        // case 2: the previous nodes don't have children
        
        TreeNode next = stack.pop();
        int prevLevel = level.pop();
        
        
        while(curLevel != prevLevel){
            
            // Consider alternatives: curl level > result.size()
            if(next.left != null){
                stack.push(next.left);
                level.push(prevLevel+1);
            }
            if(next.right != null){
                stack.push(next.right);
                level.push(prevLevel+1);
            }
             if(stack.isEmpty()){
                break;
            }
    
            next = stack.pop();
            prevLevel = level.pop();
        }
        // If we can't find any next level, then return false
        if(curLevel != prevLevel){
            return;
        }
        if(next == null){
            return;
        }
        result.add(next.val);
        
        if(next.left != null){
            stack.push(next.left);
            level.push(curLevel+1);
        }
        if(next.right != null){
            stack.push(next.right);
            level.push(curLevel+1);
        }
        
        
        
  
        bfs(curLevel+1);
    }
}
```

Time O(N) Space O(N)

#### Improved version

> No need to store level & all tree value information

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    Stack<Integer> level = new Stack<>();
    public List<Integer> rightSideView(TreeNode root) {
    /*    
        
        if(root == null){
            return result;
        }
        
        result.add(root.val);
       // always add right element first
        if(root.right != null){
            return rightSideView(root.right);
        } else if(root.left != null){
            return rightSideView(root.left);
        }
        
        return rightSideView(root.right);
        */
        
        // Need to use BFS
        //         1          level 0    curlevel 0  nextlevel = 1, result.size == 0
        //       2    3       level 1    curlevel 1, pop 3 , => no next level => check  nextlevel = 2, result.size = 1
        //     4  5            level 2    
        //       8
        //  1 2 3 6 7
        

        
        bfs(root, 0);
        return result;

    }
    
    // level 0: 1       result.size() = 0   before adding
    // level 1: 2 3     result.size() = 1   before adding
    // level 2: 4       result.size() = 2   before adding
    public void bfs(TreeNode tree, int curLevel){
        
        if(tree == null){
            return;
        }
        
        if(curLevel == result.size()){
            result.add(tree.val);
        }
        
        
       bfs(tree.right, curLevel+1);
       bfs(tree.left, curLevel+1); 
        
        
    }
}
```

Time O(N) Space O(H)

![image](https://github.com/pagexv/leetcode-Team/assets/33244427/30cc6cbf-a8a2-4d95-8e1b-462be43bbd81)


## 2023/06/22

### Question 1
https://leetcode.com/problems/h-index/solution/

Given an array of integers citations where citations[i] is the number of citations a researcher received for their ith paper, return the researcher's h-index.

Initial solution
```java
class Solution {
    public int hIndex(int[] citations) {
        // What is h index
        // [3,0,6,1,5]
//                    0 1 3 5 6
//  at least h paper  5 4 3 2 1
 // min value         0 1 3 2 1
        
          // 1  1  3
          // 3  3  1
//min value  1  1  1
        Arrays.sort(citations);
        // Need to consider previous citations equals
        int prevCitation = -1;
        int numberOfPaperAtLeastPublished = citations.length;
        int maxResult = 0;
  
        for(int i = 0; i < citations.length; i++){
            //If the previous citation isn't equal calculate how many paper greater or equal to current paper
            if(prevCitation != citations[i]){
                numberOfPaperAtLeastPublished = citations.length - i;
            }
            prevCitation = citations[i];
            // Take the min value of citation and # of papers greater than current paper
            int minValue = Math.min(citations[i], numberOfPaperAtLeastPublished);
            // Take the max values
            maxResult = Math.max(maxResult, minValue);
        }
        
        return maxResult;
    }
}
```
Time O(nlogn) Space O(1)

Optimal solution
![image](https://github.com/pagexv/leetcode-Team/assets/33244427/704a8ec1-f3ee-4258-8fed-833ad906f345)

```java
public class Solution {
    public int hIndex(int[] citations) {
        int n = citations.length;
        int[] count = new int[n + 1];
        // counting papers for each citation number
        for (int c: citations)
            count[Math.min(n, c)]++;
        // finding the h-index
        int k=n; // start with maximum possible answer i.e n and sum=count of number of papers having >=k citatations
        int sum=count[n];
        while(k>sum){
            k--;
            sum+=count[k];
        }
        return k;
    }
}
```
Time O(n) Space O(1)
![image](https://github.com/pagexv/leetcode-Team/assets/33244427/2ded2e96-174a-4c64-8e16-1562fa77614a)


## 2023/06/23

### Question 1
https://leetcode.com/problems/game-of-life/solution/

Initial solution
```java
class Solution {
    public void gameOfLife(int[][] board) {
        int rowLength = board.length;
        int colLength = board[0].length;
        
        int [] neighbor = {-1, 0, 1};
        int [][] nextGame = new int [rowLength][colLength];
        
        
        for(int i = 0; i < rowLength; i++){
            for(int j = 0; j < colLength; j++){
                nextGame[i][j] = board[i][j];
            }
        }
        
        for(int i = 0; i < rowLength; i++){
            for(int j = 0; j < colLength; j++){
                
                int countLiveNeighbor = 0;
                for(int k = 0; k < 3; k++){
                    for (int l = 0; l < 3; l++){
                        // Important to skip itself
                        if(k == 1 && l == 1){
                            continue;
                        }
                        int neighborRow = i + neighbor[k];
                        int neighborCol = j + neighbor[l];
                        if(neighborRow >= 0 && neighborRow < rowLength &&
                          neighborCol >=0 && neighborCol < colLength && nextGame[neighborRow][neighborCol] == 1){
                            countLiveNeighbor++;
                        }
                    }
                }
                
                if(nextGame[i][j] == 1 && countLiveNeighbor < 2){
                    board[i][j] = 0;
                } else if (nextGame[i][j] == 1 &&countLiveNeighbor <= 3){
                    board[i][j] = 1;
                } else if (nextGame[i][j] == 1 &&countLiveNeighbor > 3){
                   board[i][j] = 0;
                } else if (nextGame[i][j] == 0 && countLiveNeighbor == 3){
                    board[i][j] = 1;
                }
            }
        }
    
    }
}
```
Time & Space O(MN)

Improved solution:
Use -1 & 2 to mark those value turned from 0 to 1 & from 1 to 0


```java
class Solution {
    public void gameOfLife(int[][] board) {
        int rowLength = board.length;
        int colLength = board[0].length;
        
        int [] neighbor = {-1, 0, 1};
  

        
        for(int i = 0; i < rowLength; i++){
            for(int j = 0; j < colLength; j++){
                
                int countLiveNeighbor = 0;
                for(int k = 0; k < 3; k++){
                    for (int l = 0; l < 3; l++){
                        // Important to skip itself
                        if(k == 1 && l == 1){
                            continue;
                        }
                        int neighborRow = i + neighbor[k];
                        int neighborCol = j + neighbor[l];
                        if(neighborRow >= 0 && neighborRow < rowLength &&
                          neighborCol >=0 && neighborCol < colLength && Math.abs(board[neighborRow][neighborCol]) == 1){
                            countLiveNeighbor++;
                        } 
                    }
                }
                
                if(board[i][j] == 1 && countLiveNeighbor < 2){
                    board[i][j] = -1; 
                } else if (board[i][j] == 1 &&countLiveNeighbor > 3){
                   board[i][j] = -1;
                } else if (board[i][j] == 0 && countLiveNeighbor == 3){
                    board[i][j] = 2;
                }
            }
        }
        
        
        for(int i = 0; i < rowLength; i++){
            for(int j = 0; j < colLength; j++){
                if(board[i][j] <= 0){
                    board[i][j] = 0;
                } else {
                    board[i][j] = 1;
                }
            }
        }
    
    }
}
```
Time O(MN) Space O(1)

![image](https://github.com/pagexv/leetcode-Team/assets/33244427/6cca3acd-3cfd-4601-82a8-835efe8604f4)


## 2023/06/26

### Question 1
https://leetcode.com/problems/majority-element/

Initial solution
```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length/2];
        
    }
}
```
Time O(nlogn) Space O(1)


Optimal solution: Boyer-Moore Voting Algorithm
```java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        Integer candidate = null;

        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            count += (num == candidate) ? 1 : -1;
        }

        return candidate;
    }
}

```
Time O(N) Space O(1)

### Question 2
https://leetcode.com/problems/rotate-array/
Initial solution

```java
class Solution {
    public void rotate(int[] nums, int k) {
        LinkedList<Integer> result = new LinkedList<>();
        
        
        for(int i = nums.length - 1; i >=0; i--){
             result.offer(nums[i]);
        }

        
        for(int i = 0; i < k; i++){
            int temp = result.poll();
            result.offer(temp);
        }
        
         for(int i = nums.length - 1; i >=0; i--){
              nums[i] = result.poll();
        }
        
    
    }
}

```
Time O(N) Space O(N)

Reverse

```java
class Solution {
  public void rotate(int[] nums, int k) {
    k %= nums.length;
    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
  }
  public void reverse(int[] nums, int start, int end) {
    while (start < end) {
      int temp = nums[start];
      nums[start] = nums[end];
      nums[end] = temp;
      start++;
      end--;
    }
  }
}
```

Time O(N) Space O(1)

## 2023/06/27

### Question 1

https://leetcode.com/problems/candy/

Initial solution

Don't forget to stop when no changes happens
```java
class Solution {
    public int candy(int[] ratings) {
        
        int [] candies = new int[ratings.length];
        Arrays.fill(candies, 1);
        
        // Stop when no change happen (we may need to traverse multiple times)
        boolean changed = true;
        while(changed){
            changed = false;
            for(int i = 0; i < ratings.length ; i++){
            if( i < ratings.length - 1 && ratings[i] > ratings[i+1] && candies[i] <= candies[i+1]){
                candies[i] = candies[i+1] + 1;
                changed = true;
                
            }
            
            if(i > 0 && ratings[i] > ratings[i-1] && candies[i] <= candies[i-1]){
                candies[i] = candies[i-1] + 1;
                changed = true;
             }
          }
        }
        
        int sum = 0;
        for(int c : candies){
            sum += c;
        }
        
        return sum;
        
    }
}

```
Time O(n^2) Space O(n)

Optimal solution
![image](https://github.com/pagexv/leetcode-Team/assets/33244427/8781b3ef-7853-4e5e-b360-86c0b3aa3bc4)

```java
public class Solution {
    public int count(int n) {
        return (n * (n + 1)) / 2;
    }
    public int candy(int[] ratings) {
        if (ratings.length <= 1) {
            return ratings.length;
        }
        int candies = 0;
        int up = 0;
        int down = 0;
        int oldSlope = 0;
        for (int i = 1; i < ratings.length; i++) {
            int newSlope = (ratings[i] > ratings[i - 1]) ? 1 
                : (ratings[i] < ratings[i - 1] ? -1 
                : 0);

            if ((oldSlope > 0 && newSlope == 0) || (oldSlope < 0 && newSlope >= 0)) {
                candies += count(up) + count(down) + Math.max(up, down);
                up = 0;
                down = 0;
            }
            if (newSlope > 0) {
                up++;
            } else if (newSlope < 0) {
                down++;
            } else {
                candies++;
            }

            oldSlope = newSlope;
        }
        candies += count(up) + count(down) + Math.max(up, down) + 1;
        return candies;
    }
}
```
Time O(n) Space O(1)

![image](https://github.com/pagexv/leetcode-Team/assets/33244427/c130ea21-d50b-478a-b27f-00aff5119f64)


## 2023/06/27

### Question 1

https://leetcode.com/problems/minimum-size-subarray-sum/



```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int right = 0;
        int currentSum = 0;
        int result = Integer.MAX_VALUE;
        
        for(right = 0; right < nums.length; right++){
            currentSum += nums[right];
            
            while(currentSum >= target){
                result = Math.min(result, right - left + 1);
                currentSum -= nums[left++];
            }
            
        }
        
        return result == Integer.MAX_VALUE ? 0 : result;
    }
}
```

Time O(N) Space O(1)



## 2023/06/29

### Question 1
[133. Clone Graph](https://leetcode.com/problems/clone-graph/)https://leetcode.com/problems/clone-graph/

DFS

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    
    private HashMap <Node, Node> visited = new HashMap <> (); // Added to prevent infinite loop
    
    
    public Node cloneGraph(Node node) {
     
        
        if(node == null){
            return null;
        }
        return recursion(node);
    }
    
    
    public Node recursion(Node node){
        if(node == null){
  
            return node;
        }
        
          if (visited.containsKey(node)) { // Added to prevent infinite loop
            return visited.get(node);
        }
        
        Node result = new Node(node.val , new ArrayList());
        
        visited.put(node, result); // Added to prevent infinite loop
        // And it has to be before the for loop
        for(Node eachNode : node.neighbors){

            result.neighbors.add(recursion(eachNode));
            
        }

        return result;
        
    }
}
```
Time O(M + N) Space O(N)


BFS
```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;

    public Node() {}

    public Node(int _val,List<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/
class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) {
            return node;
        }

        // Hash map to save the visited node and it's respective clone
        // as key and value respectively. This helps to avoid cycles.
        HashMap<Node, Node> visited = new HashMap();

        // Put the first node in the queue
        LinkedList<Node> queue = new LinkedList<Node> ();
        queue.add(node);
        // Clone the node and put it in the visited dictionary.
        visited.put(node, new Node(node.val, new ArrayList()));

        // Start BFS traversal
        while (!queue.isEmpty()) {
            // Pop a node say "n" from the from the front of the queue.
            Node n = queue.remove();
            // Iterate through all the neighbors of the node "n"
            for (Node neighbor: n.neighbors) {
                if (!visited.containsKey(neighbor)) {
                    // Clone the neighbor and put in the visited, if not present already
                    visited.put(neighbor, new Node(neighbor.val, new ArrayList()));
                    // Add the newly encountered node to the queue.
                    queue.add(neighbor);
                }
                // Add the clone of the neighbor to the neighbors of the clone node "n".
                visited.get(n).neighbors.add(visited.get(neighbor));
            }
        }

        // Return the clone of the node from visited.
        return visited.get(node);
    }
}

```
Time O(M + N) Space O(N)

![image](https://github.com/pagexv/leetcode-Team/assets/33244427/04b57d53-0c59-4165-8fe5-44aed1e98b99)
