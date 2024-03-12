
# [Ninja’s Training](https://www.codingninjas.com/studio/problems/ninja%E2%80%99s-training_3621003?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Approach ->
Now we have stepped our foot in the relms of 2D/3D DP and DP on Grids problems. 
The problem involves finding the maximum merit points a ninja can earn over 'N' days, where each day offers three activities with associated merit points.
The constraint is that the same activity cannot be performed on two consecutive days.

For example ->

![Eg.](images/ninja-training.jpg)

If we pick 50 on day 1 then we cannot pick 100 on day 2 because these are the same activites. So we will pick 10 on day 1 and 100 on day 2. Ans - 110.

Recursive Approach:

Express the Problem in Terms of Indexes:

Define a recursive function with parameters 'day' and 'last,' where day represents the row and last represents the column i.e. the particular activity we should avoid the next day. 

Try Out All Possible Choices at a Given Index:

For each day, consider all three activities, excluding the activity performed on the previous day.
Recursively call the function for the next day with the chosen activity.

Take the Maximum of All Choices:

Return the maximum merit points among all the choices for the current day.

Then apply memoisation, tabulation and space optimization as you go...

## Codes
1. 
```cpp
#include <bits/stdc++.h>

int helper(int day, int last, vector<vector<int>> &points, vector<vector<int>> &dp){
   // Base case: if it's the first day, return the maximum merit point for any activity
    if (day == 0) {
        int maxi = 0;
        for (int i = 0; i < 3; i++) {
            if (last != i)
                maxi = max(maxi, points[day][i]);
        }
        return maxi;
    }

    // Check if the result for the current day and last activity is already calculated
    if (dp[day][last] != -1) return dp[day][last];

    int maxi = 0;
    // Iterate over activities for the current day
    for (int i = 0; i < 3; i++) {
        // Ensure the same activity is not performed on two consecutive days
        if (last != i) {
            // Calculate the merit points for the current day and update the maximum
            int point = points[day][i] + helper(day - 1, i, points, dp);
            maxi = max(maxi, point);
        }     
    }

    // Memoize the result by storing it in the dp array
    return dp[day][last] = maxi;
} 


int ninjaTraining(int n, vector<vector<int>> &points)
{
    // Write your code here.
    vector<vector<int>> dp(n, vector<int>(4, -1));
    // Notice how we passed 3 insead of -1 at the place of last
    // This is because dp is checking for dp[day][last] and if we..
    // pass -1 then segmentation fault will occur.
    return helper(n-1, 3, points, dp);
}
```
Time Complexity: O(N*4*3)
Space Complexity: O(N) + O(N*4)

2. 
```cpp
#include <bits/stdc++.h>

int ninjaTraining(int n, vector<vector<int>> &points)
{
    // Initialize a 2D dp array with dimensions (n x 4) for dynamic programming
    // dp[i][j] represents the maximum merit points up to day 'i' with the last activity as 'j'
    vector<vector<int>> dp(n, vector<int>(4, -1));

    // Initialize base cases for the first day (day 0)
    // Preparing the initial values of dp table,
    // before the main dp loop, where we consider subsequent days.
    dp[0][0] = max(points[0][1], points[0][2]);
    dp[0][1] = max(points[0][0], points[0][2]);
    dp[0][2] = max(points[0][0], points[0][1]);
    dp[0][3] = max(points[0][0], max(points[0][1], points[0][2]));

    // Iterate over the days, starting from day 1
    for(int day = 1; day < n; day++) {
        // Notice how 'last' is iterated up to 4 to match the size of the dp array
        for(int last = 0; last < 4; last++) {
            // Initialize the merit points for the current day and last activity
            dp[day][last] = 0;
            
            // Iterate over the possible activities for the current day
            for(int i = 0; i < 3; i++) {
                // Avoid choosing the same activity as the last day
                if(last != i) {
                    // Calculate the merit points for the current day and update dp array
                    int point = points[day][i] + dp[day-1][i];
                    dp[day][last] = max(dp[day][last], point);
                }
            }
        }
    }

    // Return the maximum merit points for the last day with any activity
    return dp[n-1][3];
}
```
Time Complexity: O(N*4*3)

Reason: There are three nested loops

Space Complexity: O(N*4)

We have eleminated the recursion stack space from the SC

3. 
The space optimization technique in this dynamic programming solution involves only keeping track of the information needed for the current day and the previous day, instead of storing the entire 2D array.

### Algorithm / Intuition:

1. **Original Dynamic Programming Equation:**
   - The original equation to update the dp array is:
     ```cpp
     dp[day][last] = max(dp[day][last], points[day][task] + dp[day-1][task]);
     ```
   - It calculates the maximum merit points for the current day and the last activity based on the merit points of the current activity and the merit points accumulated up to the previous day for different activities.

2. **Space Optimization:**
   - Instead of storing the entire 2D array 'dp' of size N*4, we use two 1D arrays 'prev' and 'temp' each of size 4.
   - 'prev' stores the maximum merit points up to the previous day.
   - 'temp' is a dummy array used to calculate the next row's values.
   - We iterate over the activities and update 'temp' based on the current day's activities and 'prev'.

3. **Update for Each Day:**
   - As we move to the next day, 'temp' becomes 'prev' for the next step.
   - This process is repeated for each day until we reach the last day.
   - At the end of the iterations, 'prev[3]' contains the maximum merit points for the last day with any activity.

### Code:
```cpp
#include <bits/stdc++.h>

int ninjaTraining(int n, vector<vector<int>> &points)
{
    // Initialize a 1D array 'prev' to store the maximum merit points up to the previous day
    vector<int> prev(n, -1);

    // Initialize base cases for the first day (day 0)
    prev[0] = max(points[0][1], points[0][2]);
    prev[1] = max(points[0][0], points[0][2]);
    prev[2] = max(points[0][0], points[0][1]);
    prev[3] = max(points[0][0], max(points[0][1], points[0][2]));

    // Iterate over the days, starting from day 1
    for(int day = 1; day < n; day++) {
        vector<int> temp(4, -1);

        for(int last = 0; last < 4; last++) {
            temp[last] = 0;

            // Iterate over the possible activities for the current day
            for(int i = 0; i < 3; i++) {
                if(last != i) {
                    // Calculate the merit points for the current day and update 'temp' array
                    int point = points[day][i] + prev[i];
                    temp[last] = max(temp[last], point);
                }
            }
        }

        // Update 'prev' array with the values from 'temp'
        prev = temp;
    }

    // Return the maximum merit points for the last day with any activity
    return prev[3];
}
```

The only thing to keep in mind here is that wherever we have to replace the previous values we use prev vector and wherever we are dealing in the current value we are using the temp vector.
eg :

Replaced

```cpp
int point = points[day][i] + dp[day-1][i];
dp[day][last] = max(dp[day][last], point);
```

With

```cpp
int point = points[day][i] + prev[i];
temp[last] = max(temp[last], point);
```

### Complexity Analysis:
- Time Complexity: O(N) where N is the number of days.
- Space Complexity: O(1) additional space used for 'prev' and 'temp' arrays.

# [62. Unique Paths](https://leetcode.com/problems/unique-paths/description/)

## Codes ->
1. 
```cpp
class Solution {
public:
    int helper(int m, int n, vector<vector<int>> &dp){
        if(m==0 && n==0){
            return 1;
        }
        if(m<0 || n<0){
            return 0;
        }

        if(dp[m][n] != -1) return dp[m][n];

        return dp[m][n] = helper(m-1, n, dp) + helper(m, n-1, dp);
    }
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, -1));
        return helper(m-1, n-1, dp);
    }
};
```
2. 
```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, -1));

        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(i==0 && j==0){
                    dp[i][j]=1; // base case
                    continue;
                }
                int up = 0, left = 0;

                if(i>0) up = dp[i-1][j];
                if(j>0) left = dp[i][j-1];

                dp[i][j] = up+left;
            }
        }

        return dp[m-1][n-1];
    }
};
```

3. 
Read this carefully --->
```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<int> prev(n, 0);

        for(int i=0; i<m; i++){
            vector<int> temp(n, 0);
            for(int j=0; j<n; j++){
                if(i==0 && j==0) {
                    temp[j] = 1;
                    continue;
                }
                int up = 0, left = 0;
                
                // replacement of ->
                // if (i > 0) up = dp[i - 1][j];
                // if (j > 0) left = dp[i][j - 1];
                if(i>0) up = prev[j];
                if(j>0) left = temp[j-1];
                // simple logic: Replace old with prev and new with temp

                temp[j] = up+left;
            }
            prev = temp;
        }

        return prev[n-1];
    }
};
```

# [63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/description/)

## Codes ->
1. 
```cpp
class Solution {
public:
    int helper(int m, int n, vector<vector<int>> &dp, vector<vector<int>> &obstacleGrid){
        if(m==0 && n==0){
            return 1;
        }
        if(m<0 || n<0 || obstacleGrid[m][n]){
            return 0;
        }

        if(dp[m][n] != -1) return dp[m][n];

        return dp[m][n] = helper(m-1, n, dp, obstacleGrid) + helper(m, n-1, dp, obstacleGrid);
    }

    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        if(obstacleGrid[0][0]==1) return 0;
        int m = obstacleGrid.size(), n = obstacleGrid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, -1));
        return helper(m-1, n-1, dp, obstacleGrid);
    }
};
```
2. 
```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        if(obstacleGrid[0][0]==1) return 0;
        int m = obstacleGrid.size(), n = obstacleGrid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, -1));

        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(i==0 && j==0) dp[i][j] = 1;
                else if(obstacleGrid[i][j]) dp[i][j] = 0;
                else{
                    int up = 0, left = 0;
                    if(i>0) up = dp[i-1][j];
                    if(j>0) left = dp[i][j-1];
                    dp[i][j] = up+left; 
                }
            }
        }

        return dp[m-1][n-1];
    }
};
```

3. 
```cpp
class Solution {
public:

    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        if(obstacleGrid[0][0]==1) return 0;
        int m = obstacleGrid.size(), n = obstacleGrid[0].size();
        vector<int> prev(n, -1);

        for(int i=0; i<m; i++){
            vector<int> temp(n, -1);
            for(int j=0; j<n; j++){
                if(i==0 && j==0) temp[j] = 1;
                else if(obstacleGrid[i][j]) temp[j] = 0;
                else{
                    int up = 0, left = 0;
                    if(i>0) up = prev[j];
                    if(j>0) left = temp[j-1];
                    temp[j] = up+left; 
                }
            }
            prev = temp;
        }

        return prev[n-1];
    }
};
```

# [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/description/)

## Codes ->
1. 
```cpp
class Solution {
public:
    int helper(int m, int n, vector<vector<int>>& grid, vector<vector<int>>& dp){
        if(m==0 && n==0) return grid[m][n];
        if(m < 0 || n < 0) return INT_MAX;

        if(dp[m][n]!=-1) return dp[m][n];

        int up = helper(m-1, n, grid, dp);
        int left = helper(m, n-1, grid, dp);

        return dp[m][n] = grid[m][n] + min(up, left);
    }
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, -1));
        return helper(m-1, n-1, grid, dp);
    }
};
```
2. 
```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, -1));

        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(i==0 && j==0){
                    dp[i][j] = grid[i][j];
                    continue;
                }

                int up = INT_MAX, left = INT_MAX;
                if(i>0)up = dp[i-1][j];
                if(j>0)left = dp[i][j-1];

                dp[i][j] = grid[i][j] + min(up, left);
            }
        }

        return dp[m-1][n-1];
    }
};
```
3. 
```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<int> prev(n, -1);

        for(int i=0; i<m; i++){
            vector<int> temp(n, -1);
            for(int j=0; j<n; j++){
                if(i==0 && j==0){
                    temp[j] = grid[i][j];
                    continue;
                }

                int up = INT_MAX, left = INT_MAX;
                if(i>0)up = prev[j];
                if(j>0)left = temp[j-1];

                temp[j] = grid[i][j] + min(up, left);
            }
            prev = temp;
        }

        return prev[n-1];
    }
};
```

# [120. Triangle](https://leetcode.com/problems/triangle/description/)

## Approach ->
Very similar to the last question but the only mistake you might do in this is take the size of the dp array wrong. This is not a problem where the size of the grid is n*n. Here the rows are constantly changing with +1.

## Code ->
```cpp
class Solution {
public:
    int helper(vector<vector<int>>& triangle, vector<vector<int>>& dp, int i, int j, int m){
        // no need of outside the bounds case
        if(i==m) return triangle[i][j];
        if(dp[i][j]!=-1) return dp[i][j];

        int down = helper(triangle, dp, i+1, j, m);
        int downRight = helper(triangle, dp, i+1, j+1, m);

        return dp[i][j] = triangle[i][j] + min(down, downRight);
    }
    int minimumTotal(vector<vector<int>>& triangle) {
        int m = triangle.size();
        // notice the size here
        vector<vector<int>> dp(m, vector<int>(m, -1));

        return helper(triangle, dp, 0, 0, m-1);
    }
};
```

2. 
Now there is a twist here. In the memo approach, this was the base case -> if(i==m) return triangle[i][j];

So how many base cases did this code run? ans -> m base cases. Because we are comparing i with m i.e. the last row of the triangle and the last row will have m elements. So in our tabulation base case we assign our dp[m-1] with all the elements at the bottom of the triangle. 

One more thing to notice in all the questions above was that we go from n-1 to 0 in our recurance relation in memoization approaches but we went from 1 to n-1 in our tabulation approach. But in this q we are going from 0 to m-1 in our memoization approach so we will be going from m-2 to 0 in our tabulation approach. 

```cpp
class Solution {
public:
    // Main function to find the minimum path sum in the triangle
    int minimumTotal(vector<vector<int>>& triangle) {
        int m = triangle.size();  // Number of rows in the triangle
        vector<vector<int>> dp(m, vector<int>(m, -1));  // Memoization table

        // Initialize the last row of the dp array with the values of the last row in the triangle
        for (int i = m - 1; i >= 0; i--) {
            dp[m - 1][i] = triangle[m - 1][i];
        }

        // Iterate from the second-to-last row to the first row
        for (int i = m - 2; i >= 0; i--) {
            // j will run from i because total columns in any row == row number
            for (int j = i; j >= 0; j--) {
                // Calculate the minimum path sum by considering the two possible moves
                int down = dp[i + 1][j];
                int downRight = dp[i + 1][j + 1];

                // Update the dp array with the minimum path sum for the current element
                dp[i][j] = triangle[i][j] + min(down, downRight);
            }
        }

        // The top of the dp array now contains the minimum path sum
        // Note how we returned dp[0][0]. Everything is in reverse. Pretty easy right!
        return dp[0][0];
    }
};
```
3. 
```cpp
class Solution {
public:
    // Main function to find the minimum path sum in the triangle
    int minimumTotal(vector<vector<int>>& triangle) {
        int m = triangle.size();  // Number of rows in the triangle
        vector<int> next(m, -1);  // Temporary array to store the minimum path sums for the next row

        // Initialize the last row of the temporary array with the values of the last row in the triangle
        for (int i = m - 1; i >= 0; i--) {
            next[i] = triangle[m - 1][i];
        }

        // Iterate from the second-to-last row to the first row
        for (int i = m - 2; i >= 0; i--) {
            vector<int> temp(m, -1);  // Temporary array to store the updated minimum path sums for the current row
            // j will run from i because the total columns in any row == row number
            for (int j = i; j >= 0; j--) {
                // Calculate the minimum path sum by considering the two possible moves
                int down = next[j];
                int downRight = next[j + 1];

                // Update the temporary array with the minimum path sum for the current element
                temp[j] = triangle[i][j] + min(down, downRight);
            }
            next = temp;  // Update the next array with the values calculated for the current row
        }

        // The top of the temporary array now contains the minimum path sum
        // Note how we returned next[0]. Everything is in reverse. Pretty easy right!
        return next[0];
    }
};
```

In the start we had a fixed starting point and a fixed ending point. In the last question we had a fixed starting point and a variable ending point. What if we have a variable starting point and a variable ending point? Let's see...

# [931. Minimum Falling Path Sum](https://leetcode.com/problems/minimum-falling-path-sum/description/)

## Approach ->
Take the maximum of all choices:
As we have to find the minimum path sum of all the possible unique paths, we will return the minimum of all the choices we have from the bottom row.

## Code ->
1. 
```cpp
class Solution {
public:
    int helper(vector<vector<int>>& matrix, int m, int n, vector<vector<int>>& dp){
        // Base case: If the column index is out of bounds, return INT_MAX
        if(n<0 || n>=dp[0].size()) return INT_MAX;
        // Base case: If we are at the first row, return the value in the matrix
        if(m==0) return matrix[m][n];

        if(dp[m][n]!=-1) return dp[m][n];

        // Calculate the minimum falling path sum by exploring three possible moves
        int top = helper(matrix, m-1, n, dp);
        int topLeft = helper(matrix, m-1, n-1, dp);
        int topRight = helper(matrix, m-1, n+1, dp);

        return dp[m][n] = matrix[m][n] + min(top, min(topLeft, topRight));
    }
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n, -1));
        
        int ans = INT_MAX;

        // Answer will be the min path bw each element in the bottom row.
        // Iterate over each element in the bottom row to find the minimum path sum
        for(int i=0; i<n; i++){
            ans = min(ans, helper(matrix, m-1, i, dp));
        }

        return ans;
    }
};
```
But this approach will still give us TLE. TC of recursion ->  O(3 ^ n) because we have 3 options for n rows. Tc after memoization-> O(n*n)

Space Complexity: O(N) + O(N*M)
2. 
```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));

        for(int j=0; j<n; j++) dp[0][j] = matrix[0][j];

        for(int i=1; i<m; i++){
            for(int j=0; j<n; j++){
                int top=INT_MAX, topLeft=INT_MAX, topRight=INT_MAX;
                if(i>0) top = dp[i-1][j];
                if(i>0 && j>0) topLeft = dp[i-1][j-1];
                if(i>0 && j+1<m)topRight = dp[i-1][j+1];

                dp[i][j] = matrix[i][j] + min(top, min(topLeft, topRight));
            }
        }
        
        // IMPORTANT: We don't call any function for the bottom most row
        // Instead we convert the function call here to tabulation as well
        int ans = INT_MAX;

        for(int i=0; i<n; i++){
            ans = min(ans, dp[m-1][i]);
        }

        return ans;

    }
};
```
Time Complexity: O(N*M)

Space Complexity: O(N*M)

3. Easy Peasy - same as always 
```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        vector<int> prev(m, -1);

        for(int j=0; j<n; j++) prev[j] = matrix[0][j];

        for(int i=1; i<m; i++){
            vector<int> temp(m, -1);
            for(int j=0; j<n; j++){
                int top=INT_MAX, topLeft=INT_MAX, topRight=INT_MAX;
                if(i>0) top = prev[j];
                if(i>0 && j>0) topLeft = prev[j-1];
                if(i>0 && j+1<m)topRight = prev[j+1];

                temp[j] = matrix[i][j] + min(top, min(topLeft, topRight));
            }
            prev = temp;
        }
        

        // Even we convert the function call here to tabulation
        int ans = INT_MAX;

        for(int i=0; i<n; i++){
            ans = min(ans, prev[i]);
        }

        return ans;

    }
};
```