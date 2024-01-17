# [216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/description/)

## Approach ->
Generate all combinations of length k from the set {1, 2, ..., 9}, and filter those combinations where the sum equals the target n.

---
## Code ->
```cpp
#include <vector>

class Solution {
public:
    // Helper function to find combinations
    void helper(int k, int n, vector<vector<int>> &ans, vector<int> temp, int sum, vector<int> &hash, int idx) {
        // if the sum exceeds the target or the size of the combination is reached
        if (sum > n) return;
        if (temp.size() == k) {
            // If the combination size is k and the sum is n, add it to the answer
            if (sum == n)
                ans.push_back(temp);
            return;
        }

        // Iterate through numbers starting from idx
        for (int i = idx; i <= 9; i++) {
            // If the number is not used in the current combination
            if (hash[i] == 0) {
                // Add the number to the combination
                temp.push_back(i);
                hash[i] = 1; // Mark the number as used
                // Recursively call the helper function with updated parameters
                helper(k, n, ans, temp, sum + i, hash, i + 1);
                temp.pop_back(); // Backtrack: remove the last added number
                hash[i] = 0;    // Mark the number as unused for the next iteration
            }
        }
    }

    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> ans;
        vector<int> temp;
        vector<int> hash(10, 0);
        helper(k, n, ans, temp, 0, hash, 1);
        return ans;
    }
}
```

# [79. Word Search](https://leetcode.com/problems/word-search/description/)

## Approach ->
Approach:  We are going to solve this by using backtracking, in this approach first we will linearly search the entire matrix to find the first letters matching our given string. If we found those letters then we can start backtracking in all four directions to find the rest of the letters of the given string.

Step 1: Find the first character of the given string.

Step 2: Start Backtracking in all four directions until we find all the letters of sequentially adjacent cells.

Step 3: At the end, If we found our result then return true else return false.

Edge cases: Now think about what will be our stopping condition, we can stop or return false if we reach the end of the boundaries of the matrix or the letter at which we are making recursive calls is not the required letter.

We will also return if we found all the letters of the given word i.e. we found the number of letters equal to the length of the given word.

NOTE: Do not forget that we cannot reuse a cell again.

That is, we have to somehow keep track of our position so that we cannot find the same letter again and again.

In this approach, we are going to mark visited cells with some random character that will prevent us from revisiting them again and again.

---
## Code ->
```cpp
class Solution {
public:
    bool helper(vector<vector<char>> &board, string word, int row, int col, int idx){
        // if index reaches at the end that means we have found the word
        if(idx == word.size()) return true;
        // Checking the boundaries if the position at which we are placed is not out of bounds
        // checking if the letter is same as of letter required or not
        if(row<0 || col<0 || row>=board.size() || col>=board[0].size() || board[row][col]!=word[idx]) return false;

        // this is to prevent reusing of the same character
        char prevWord = board[row][col];
        board[row][col] = '#';

        // check all directions
        bool op1 = helper(board, word, row+1, col, idx+1);
        bool op2 = helper(board, word, row, col+1, idx+1);
        bool op3 = helper(board, word, row-1, col, idx+1);
        bool op4 = helper(board, word, row, col-1, idx+1);

        // undo change
        board[row][col] = prevWord;

        return (op1 || op2 || op3 || op4);
    }
    bool exist(vector<vector<char>>& board, string word) {
        // First search the first character to start recursion process
        for(int i=0; i<board.size(); i++){
            for(int j=0; j<board[0].size(); j++){
                if (board[i][j] == word[0]){
                    if(helper(board, word, i, j, 0)) return true;
                }      
            }
        }
        return false;
    }
};
```

# [Rat in a Maze Problem - I](https://www.geeksforgeeks.org/problems/rat-in-a-maze-problem/1)

## Approach ->
It is failry simple. ATQ the answer should be in lexicographically sorted order, so we will keep that in mind. We also cannot visit a path that has 0 on it, so we will have to keep that in mind too. Keeping these two things in mind we make our recursive calls.

## Code ->
```cpp
class Solution{
    public:
    void helper(vector<vector<int>>&m, int n, vector<string> &ans, string s, int row, int col){
        // If the destination is reached, add the current path to the result
        if(row==n-1 && col==n-1){
            ans.push_back(s);
            return;
        }
        
        // Check for out-of-bounds or obstacles(0 on any index) in the matrix
        if(row<0 || col<0 || row>=n || col>=n || m[row][col]!=1) return;
        
        // Mark the current cell as visited
        m[row][col]=2;
        // Recursion call in all four directions in lexicographical order
        helper(m, n, ans, s+'D', row+1, col);
        helper(m, n, ans, s+'L', row, col-1);
        helper(m, n, ans, s+'R', row, col+1);
        helper(m, n, ans, s+'U', row-1, col);
        // Backtrack: Mark the current cell as unvisited
        m[row][col]=1;
    }
    vector<string> findPath(vector<vector<int>> &m, int n) {
        vector<string> ans;
        // Important base condition
        if (m[0][0] == 0 || m[n - 1][n - 1] == 0) {
            // If the source or destination is blocked, return -1
            ans.push_back("-1");
            return ans;
        }
        
        helper(m, n, ans, "", 0, 0);
        return ans; 
    }
};
```

# [37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/description/)

## [Approach](https://takeuforward.org/data-structure/sudoku-solver/)

## Code ->
```cpp
class Solution {
public:
    // Function to solve Sudoku puzzle
    void solveSudoku(vector<vector<char>>& board) {
        // Call helper function to start solving
        helper(board);
    }

    // Recursive helper function to solve Sudoku
    bool helper(vector<vector<char>> & board) {
        // Loop through each cell in the board
        for(int i = 0; i < board.size(); i++) {
            for(int j = 0; j < board[0].size(); j++) {
                // Check if the cell is empty (indicated by '.')
                if(board[i][j] == '.') {
                    // Try filling the empty cell with digits 1 to 9
                    for(char x = '1'; x <= '9'; x++) {
                        // Check if placing x in the current cell is valid
                        if(isValid(i, j, board, x)) {
                            // Place x in the cell
                            board[i][j] = x;

                            // Recursively call the helper function
                            if(helper(board)) return true; // Continue with the next cell

                            // If the placement doesn't lead to a solution, backtrack and try a different digit
                            else board[i][j] = '.';
                        }
                    }
                    // If no valid digit can be placed, backtrack to the previous cell
                    return false;
                }
            }
        }
        // If all cells are filled, the Sudoku is solved
        return true;
    }

    // Function to check if placing x in the given cell is valid
    bool isValid(int row, int col, vector<vector<char>> & board, char x) {
        // Check if x is not present in the current row and column
        for(int i = 0; i < 9; i++) {
            if(board[row][i] == x) return false; // Check row
            if(board[i][col] == x) return false; // Check column

            // Check the 3x3 sub-box
            if(board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == x) return false;
        }
        // If x can be placed in the cell without violating Sudoku rules, it is valid
        return true;
    }
};
```

# [1849. Splitting a String Into Descending Consecutive Values](https://leetcode.com/problems/splitting-a-string-into-descending-consecutive-values/description/)

## Approach ->
As we are required to split the string in such a way that all the splits should have numbers in a decreasing manner (just by 1) so in the main function we can iterate through the entire string and call a recursive function helper for the first split. Inside the helper function, we check if the current substring, formed by converting a portion of the input string to a numerical value, is one less than the previous value. If this condition is met, the function is recursively called on the remaining portion of the string, exploring all possible valid splits.

---
## Code ->
```cpp
class Solution {
public:
    bool splitString(string s) {
        unsigned long long int val = 0;

        // Iterate through the string (excluding the last character)
        for(int i = 0; i < s.size() - 1; i++) {
            // Build val
            val = 10 * val + (s[i] - '0');

            // Check if splitting is possible using the helper function
            if(helper(s, val, i + 1)) {
                return true;  // If possible, return true
            }
        }

        return false; // Else return false
    }

    bool helper(string s, unsigned long long int val, int idx) {
        if(s.size() == idx) {
            return true;  // If the entire string has been processed, return true
        }

        unsigned long long int curVal = 0;

        // Iterate through the remaining characters of the string i.e. from idx
        for(int i = idx; i < s.size(); i++) {
            // Build the current numerical value
            curVal = 10 * curVal + (s[i] - '0');

            // Check if the difference between val and curVal is 1
            // If true, then and only then recursively check the remaining string else just iterate in the loop (that's why we used && operator so that if val - curVal == 1 then and only then call the recursion to make further splits in the string.)
            if(val - curVal == 1 && helper(s, curVal, i + 1)) {
                return true;  // If a valid split is found, return true
            }
        } 

        return false; // If no valid split is found, return false
    }
};
```

# [77. Combinations](https://leetcode.com/problems/combinations/description/)

## Code ->
```cpp
// figure out on yourself first kiddo, you've done these kinds of patterns.
class Solution {
public:
    void helper(int n, int k, vector<vector<int>> &ans, vector<int> temp, int idx){
        if(temp.size()==k){
            ans.push_back(temp);
            return;
        }

        for(int i=idx; i<=n; i++){
            temp.push_back(i);
            helper(n, k, ans, temp, i+1);
            temp.pop_back();
        }
        return;
    }
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        vector<int> temp;
        helper(n, k, ans, temp, 1);
        return ans;
    }
};
```
# [1239. Maximum Length of a Concatenated String with Unique Characters](https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/description/)

## Code ->
```cpp
class Solution {
public:
    // Helper function to check if a string has unique characters
    bool isValid(string curStr, vector<int> &selected) {
        // Self-check for unique characters in the string
        vector<int> selfCheck(26, 0);
        for (int i = 0; i < curStr.size(); i++) {
            if (selfCheck[curStr[i] - 'a'] == 1) return false;
            selfCheck[curStr[i] - 'a'] = 1;
        }

        // Check if the characters in the string are already selected
        for (int i = 0; i < curStr.size(); i++) {
            if (selected[curStr[i] - 'a'] == 1) return false;
        }

        return true;
    }

    // Recursive helper function to explore all possible combinations
    int helper(vector<string> &arr, int &ans, int idx, vector<int> &selected) {
        // If we reach the end of the array, return the current answer
        if (idx == arr.size()) {
            return ans;
        }

        // Get the current string
        string curStr = arr[idx];

        // If the current string is not valid, move to the next index
        if (isValid(curStr, selected) == false) {
            return helper(arr, ans, idx + 1, selected);
        } else {
            // Mark characters in the current string as selected
            for (int i = 0; i < curStr.size(); i++) selected[curStr[i] - 'a'] = 1;
            // Update the answer by adding the length of the current string
            ans += curStr.size();
            // Recursively explore with the current string selected
            int op1 = helper(arr, ans, idx + 1, selected);

            // Backtrack: Mark characters in the current string as not selected
            for (int i = 0; i < curStr.size(); i++) selected[curStr[i] - 'a'] = 0;
            // Backtrack: Subtract the length of the current string from the answer
            ans -= curStr.size();
            // Recursively explore without selecting the current string
            int op2 = helper(arr, ans, idx + 1, selected);

            // Return the maximum of the two options
            return max(op1, op2);
        }
    }

    // Main function to find the maximum length of a concatenated string with unique characters
    int maxLength(vector<string>& arr) {
        int ans = 0;
        vector<int> selected(26, 0);
        return helper(arr, ans, 0, selected);
    }
};
```

# [93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/description/)

## Code ->
```cpp
class Solution {
public:
void helper(string& s, int i, int dots, string res, vector<string>& ans) {
        // Base case: If four parts are formed, and the entire string is processed
        if (dots == 4 && i == s.length()) {
            // Add the valid IP address to the result
            ans.push_back(res.substr(0, res.length() - 1)); // Remove the trailing dot
            return;
        }

        // If more than 4 parts are formed, or if the string is not fully processed, return
        if (dots > 4) {
            return;
        }

        // Explore all possible parts by considering substrings of length 1 to 3
        for (int j = i; j < min(i + 3, static_cast<int>(s.length())); ++j) {
            // Convert the substring to an integer
            int currnum = stoi(s.substr(i, j - i + 1));

            // Check if the part is less than or equal to 255 and does not have leading zeros
            if (currnum <= 255 && (i == j || s[i] != '0')) {
                // Recursively call the helper function with the updated parameters
                helper(s, j + 1, dots + 1, res + s.substr(i, j - i + 1) + ".", ans);
            }
        }
    }

    vector<string> restoreIpAddresses(string s) {
        vector<string> ans;
        helper(s, 0, 0, "", ans);
        return ans;
    }

};
```

# [473. Matchsticks to Square](https://leetcode.com/problems/matchsticks-to-square/description/)
## Approaches ->
1. The code uses a recursive approach to check if it's possible to form a square using matchsticks, ensuring each matchstick is used exactly once. The helper function explores different combinations by distributing matchsticks among four sides, and the main function checks if the total sum is divisible by 4 before calling the helper function. If successful, it returns true; otherwise, it returns false. Look at code 1 for better understanding. Although this approach will throw a TLE, so we will be optimizing the further. Also keep note of the optimization 1 done in the code below.

2. Now to solve the TLE we need to further optimize the code. Second optimization that we can do is that we will take a target variable that will suggest the value that should be in each matchSides array. In optimization 2 we will simply not call the recursive function if the value in the matchSides (after summing the already existing value in matchSides with matchsticks[i]) comes greater than target value. Third optimization we can do is that we will sort matchsticks array in decreasing order before passing it, that way if there is an element that is way larger than target will be encountered at the beginning itself and there will be no further recursion calls. Fourth and last optimization can be that for a given number in matchsticks array, if adding that number to a position of matchSides(let's say 0th having integer 5) gave us a false result, then if we encounter the same number (5 again) on matchSides on some other position then there is no point in calling the recursion. So we will simply continue our loop without calling the recursion. Now have a look at the code for better understanding.

## Codes ->

### 1. 
```cpp
class Solution {
public:
    bool helper(vector<int> &matchsticks, vector<int> matchSides, int idx){
        // When all matchsticks are used, check if sides are equal
        if(idx==matchsticks.size()){
            if(matchSides[0]==matchSides[1] && matchSides[1]==matchSides[2] && matchSides[2]==matchSides[3]) return true;
            else return false;
        }

        // Try distributing the current matchstick among the four sides and recursively explore
        for(int i=0; i<4; i++){
            matchSides[i]+=matchsticks[idx];
            if(helper(matchsticks, matchSides, idx+1)) return true;
            matchSides[i]-=matchsticks[idx];
        }
        return false;
    }
    bool makesquare(vector<int>& matchsticks) {
        // Calculate the sum of matchstick lengths
        int sum = accumulate(matchsticks.begin(), matchsticks.end(), 0);
        // Check if the number of matchsticks is not zero and the sum is divisible by 4 (Optimization 1)
        if(matchsticks.size()==0 || sum%4!=0) return false;

        vector<int> matchSides(4, 0);
        return helper(matchsticks, matchSides, 0);
    }
};
```
### 2. 
```cpp
// Same code with a 4 optimizations->
class Solution {
public:
    bool helper(vector<int> &matchsticks, vector<int> matchSides, int idx, int target){
        if(idx==matchsticks.size()){
            if(matchSides[0]==matchSides[1] && matchSides[1]==matchSides[2] && matchSides[2]==matchSides[3]) return true;
            else return false;
        }

        for(int i=0; i<4; i++){
            if(matchSides[i] + matchsticks[idx] > target) continue; // Optimization 2

            // Optimization 4 starts here
            int j = i-1;
            while(j>=0){
                if(matchSides[j] == matchSides[i]) break;
                j--;
            }
            if(j!=-1) continue;
            // Optimization 4 ends here

            matchSides[i]+=matchsticks[idx];
            if(helper(matchsticks, matchSides, idx+1, target)) return true;
            matchSides[i]-=matchsticks[idx];
        }
        return false;
    }
    bool makesquare(vector<int>& matchsticks) {
        int sum = accumulate(matchsticks.begin(), matchsticks.end(), 0);
        if(matchsticks.size()==0 || sum%4!=0) return false; // Optimization 1

        int target = sum/4;

        vector<int> matchSides(4, 0);

        sort(matchsticks.begin(), matchsticks.end(), greater<int>()); // Optimization 3
        return helper(matchsticks, matchSides, 0, target);
    }
};
```