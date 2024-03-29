# DP Basics ->

**Key Points:**
1. **Dynamic Programming Introduction:**
   - Definition: Storing answers to sub-problems for efficient solving of the main problem.
   - Two approaches: Memoization and Tabulation.

2. **Memoization (Top-Down):**
   - Recursive solution with overlapping sub-problems.
   - Steps:
      - Create dp array.
      - Check if the answer is already calculated (dp[n] != -1).
      - If not, calculate and store in dp array.
   - Time Complexity: O(N), Space Complexity: O(N).

3. **Tabulation (Bottom-Up):**
   - Iterative approach starting from base cases.
   - Steps:
      - Declare dp array.
      - Initialize base conditions.
      - Iterate to calculate dp[i] using dp[i-1] and dp[i-2].
   - Time Complexity: O(N), Space Complexity: O(N).

4. **Space Optimization:**
   - Utilize only the last two values (prev and prev2) instead of maintaining an entire array.
   - Time Complexity: O(N), Space Complexity: O(1).

**Code Examples:**
1. **Memoization:**
   ```cpp
   int f(int n, vector<int>& dp){
       if(n<=1) return n;
       if(dp[n]!= -1) return dp[n];
       return dp[n] = f(n-1, dp) + f(n-2, dp);
   }
   ```

2. **Tabulation:**
   ```cpp
   vector<int> dp(n+1, -1);
   dp[0] = 0;
   dp[1] = 1;
   for(int i=2; i<=n; i++)
       dp[i] = dp[i-1] + dp[i-2];
   ```

3. **Space Optimization:**
   ```cpp
   int prev2 = 0;
   int prev = 1;
   for(int i=2; i<=n; i++){
       int cur_i = prev2 + prev;
       prev2 = prev;
       prev = cur_i;
   }
   ```

# [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)

## Approaches ->
This q is v similar to fibonacci.

## Codes ->
1. Recursion
```cpp
class Solution {
public:
    int fibo(int n, vector<int> &dp){
        // Base Case:
        // when we go to the 0th stair we should return 1 
        //When there are 0 stairs (n=0), there is only one way to climb them, 
        //which is to not climb at all. Returning 1 in this case signifies that there is one way to climb 0 stairs.
        // We will go to 0th stair when we move 1 step i.e. only one way to go to 0
        if(n==0 || n==1) return 1;

        if(dp[n]!=-1) return dp[n];

        return dp[n] = fibo(n-1, dp) + fibo(n-2, dp);

    }
    int climbStairs(int n) {
        vector<int> dp(n+1,-1);
        return fibo(n, dp);
    }
};
```

2. Tabulation
```cpp
class Solution {
public:
    int climbStairs(int n) {
        vector<int> dp(n+1,-1);

        dp[1] = 1;
        dp[2] = 2;

        for(int i=3; i<n+1; i++){
            dp[i] = dp[i-1] + dp[i-2];
        }

        return dp[n];
    }
};
```

3. Space Optimization ---> In space optimization we will just replace dp[i] with curr, dp[i-1] with prev1 and dp[i-2] with prev2. And at the end we will shift the prevs by 1 position i.e. make prev2 = prev1 and make prev1 = curr.
And at the end just return the curr i.e. prev1
```cpp
class Solution {
public:
    int climbStairs(int n) {
        int prev1 = 2;
        int prev2 = 1;

        for(int i=3; i<n+1; i++){
            int curr = prev1+prev2;

            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }
};
```

# [Frog Jump](https://www.codingninjas.com/studio/problems/frog-jump_3621012?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Approach ->
In this question, we cannot apply greedy. Take an example -> 30, 10, 60, 10, 60, 50. ATQ we can move either 1 or 2 steps. So if we go greedily we will go from 30 to 10 to 10 to 50 === 20 + 0 + 40 = 60 height. But the best way to go would be 30 to 60 to 60 to 50 === 30 + 0 + 10 = 40 height. So we will use recursion

## Code ->
1. Recursion (Top-Down ---> We went from n to 0 (or 0 to n doesn't matter) but we are going from top of the tree to the base condition i.e. down)
```cpp
#include <bits/stdc++.h> 
int helper(int ind, vector<int> &heights, vector<int> &dp){
    if(ind==0) return 0;
    
    if(dp[ind]!=-1) return dp[ind];

    int left = helper(ind-1, heights, dp) + abs(heights[ind]-heights[ind-1]);
    int right = INT_MAX;
    if(ind>1) right = helper(ind-2, heights, dp) + abs(heights[ind]-heights[ind-2]);

    return dp[ind] = min(left, right);
}
int frogJump(int n, vector<int> &heights)
{
    vector<int> dp(n+1, -1);
    return helper(n-1, heights, dp);
}
```
Time Complexity: O(N)
Space Complexity: O(N)

2. Tabulation (Bottom-Up ---> We are going from the base condition to the top i.e. the bottom up approach)
```cpp

int frogJump(int n, vector<int> &heights)
{
    vector<int> dp(n, -1);
    // return helper(n-1, heights, dp);
    dp[0] = 0;

    for(int i=1; i<n; i++){
        int left = dp[i-1] + abs(heights[i] - heights[i-1]);
        int right = INT_MAX;
        if(i>1) right = dp[i-2] + abs(heights[i] - heights[i-2]);

        dp[i] = min(left, right);
    }

    return dp[n-1];
}
```
Time Complexity: O(N)
Space Complexity: O(N)

If there is every something like an index-1 and index-2 in a tabulation, there will always be a possible space optimization
3. Space Optimization
```cpp

int frogJump(int n, vector<int> &heights)
{
    int prev1 = 0;
    int prev2 = 0;

    for(int i=1; i<n; i++){
        int left = prev1 + abs(heights[i] - heights[i-1]);
        int right = INT_MAX;
        if(i>1) right = prev2 + abs(heights[i] - heights[i-2]);

        int curr = min(left, right);

        prev2 = prev1;
        prev1 = curr;
    }

    return prev1;
}
```
Time Complexity: O(N)
Space Complexity: O(1)

# [Minimal Cost](https://www.codingninjas.com/studio/problems/minimal-cost_8180930?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)
This question is exactly same as last question but in the last q we could have moved 2 steps, in this question we can move k steps

## Codes
1. 
```cpp

#include <bits/stdc++.h>
using namespace std;
// Function to find the minimum cost to reach the end using at most 'k' jumps
int solveUtil(int ind, vector<int>& height, vector<int>& dp, int k) {
    // Base case: If we are at the beginning (index 0), no cost is needed.
    if (ind == 0) return 0;
    
    // If the result for this index has been previously calculated, return it.
    if (dp[ind] != -1) return dp[ind];
    
    int mmSteps = INT_MAX;
    
    // Loop to try all possible jumps from '1' to 'k'
    for (int j = 1; j <= k; j++) {
        // Ensure that we do not jump beyond the beginning of the array
        if (ind - j >= 0) {
            // Calculate the cost for this jump and update mmSteps with the minimum cost
            int jump = solveUtil(ind - j, height, dp, k) + abs(height[ind] - height[ind - j]);
            mmSteps = min(jump, mmSteps);
        }
    }
    
    // Store the minimum cost for this index in the dp array and return it.
    return dp[ind] = mmSteps;
}
int minimizeCost(int n, int k, vector<int> &height){
    // Write your code here.
    vector<int> dp(n, -1); // Initialize a memoization array to store calculated results
    return solveUtil(n - 1, height, dp, k); // Start the recursion from the last index
}
```

2. 

```cpp
int minimizeCost(int n, int k, vector<int> &height){
    // Write your code here.
    vector<int> dp(n, -1);
    dp[0] = 0;

    // Loop through the array to fill in the dp array
    for (int i = 1; i < n; i++) {
        int mmSteps = INT_MAX;

        // Loop to try all possible jumps from '1' to 'k'
        for (int j = 1; j <= k; j++) {
            if (i - j >= 0) {
                int jump = dp[i - j] + abs(height[i] - height[i - j]);
                mmSteps = min(jump, mmSteps);
            }
        }
        dp[i] = mmSteps;
    }
    return dp[n - 1]; // The result is stored in the last element of dp
}
```

# [198. House Robber](https://leetcode.com/problems/house-robber/description/)

## Codes->
1. 
```cpp
class Solution {
public:
    int helper(vector<int> &nums, int n, vector<int> &dp){
        if(n<0) return 0;
        if(n==0) return nums[0];

        if(dp[n]!=-1) return dp[n];

        int pick = nums[n] + helper(nums, n-2, dp);
        int notPick = 0 + helper(nums, n-1, dp);

        return dp[n] = max(pick, notPick);
    }
    int rob(vector<int>& nums) {
        int n = nums.size()-1;
        vector<int> dp(n+1, -1);

        return helper(nums, n, dp);
    }
};
```

2. 
```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n+1, -1);

        dp[0] = nums[0];

        for(int i=1; i<n; i++){
            int pick = nums[i];
            if(i>1) pick = nums[i] + dp[i-2];
            int notPick = 0 + dp[i-1];

            dp[i] = max(pick, notPick);
        }

        return dp[n-1];
    }
};
```

3. 
```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();

        int prev1 = nums[0];
        int prev2 = 0;

        for(int i=1; i<n; i++){
            int pick = nums[i];
            if(i>1) pick = nums[i] + prev2;
            int notPick = 0 + prev1;

            int curr = max(pick, notPick);

            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }
};
```

# [213. House Robber II](https://leetcode.com/problems/house-robber-ii/description/)

## Approach ->
The only difference in this q from the last one is that the array is circular. That tells us that we can either include the last index elem or the first index elem in our answer. So basically make two temp vectors by removing first and the last elements from nums and pass it in our function. Then return the max result.

## Code ->
```cpp
class Solution {
public:
    int helper(vector<int>& nums){
        int n = nums.size();
        int prev1 = nums[0];
        int prev2 = 0;

        for(int i=1; i<n; i++){
            int pick = nums[i];
            if(i>1) pick = nums[i] + prev2;
            int notPick = 0 + prev1;

            int curr = max(pick, notPick);

            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }
    int rob(vector<int>& nums) {
        // Don't forget to mention this base condition during interview
        // These are the small things that an interviewer looks for
        if(nums.size()==1) return nums[0];
        vector<int> temp1, temp2;
        for(int i=0; i<nums.size(); i++){
            if(i!=0) temp1.push_back(nums[i]);
            if(i!=nums.size()-1) temp2.push_back(nums[i]);
        }

        return max(helper(temp1), helper(temp2));
        
    }
};
```


# [57. Insert Interval](https://leetcode.com/problems/insert-interval/description/)

## Code ->
```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        int i = 0 ;
        int n = intervals.size();

        vector<vector<int>>result;

        while(i < intervals.size()){
            if(intervals[i][1] < newInterval[0])
                result.push_back(intervals[i]);

            else if(intervals[i][0] > newInterval[1])
                break;   

            else{
                newInterval[0] = min(newInterval[0] , intervals[i][0]);
                newInterval[1] = max(newInterval[1] , intervals[i][1]);
            }
            i++;
        }

        result.push_back(newInterval);

        while(i < n ){
          result.push_back(intervals[i]);
          i++;
        }

        return result;
    }
};
```