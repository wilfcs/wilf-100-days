# [46. Permutations](https://leetcode.com/problems/permutations/description/)

## Approaches ->
1. Frequency array approach ->
- Create a frequency vector freq to keep track of whether an element has been used in the current permutation.

- If the size of the temporary vector temp becomes equal to the size of the input array nums, it means a valid permutation is found. Add it to the result vector ans.

- Iterate through each element in the nums array.
- If the element has not been used (frequency is 0), add it to the temporary vector temp.
- Mark the element as used by updating its frequency in the freq vector to 1.
- Recursively call the helper function with the updated temp and freq.
- After the recursive call, backtrack by marking the element as unused and removing it from temp.

Time Complexity:

The time complexity of the provided approach is O(N!), where N is the number of distinct integers in the input array nums. This is because, in each recursive call, the algorithm explores all possible choices for the next element, and there are N elements in the array. The recursive function is called N times for the first element, (N-1) times for the second element, (N-2) times for the third element, and so on, resulting in a total of N * (N-1) * (N-2) * ... * 1 = N! recursive calls.

Space Complexity:

The space complexity is O(N) for the recursion stack and O(N) for the temporary vectors used in each recursive call. The recursion stack depth is at most N, as the algorithm explores each element in the array. Additionally, each recursive call creates a temporary vector temp of size N. Therefore, the overall space complexity is O(N + N) = O(N).

## Code ->
```cpp
class Solution {
public:
    void helper(vector<int> nums, vector<int> temp, vector<vector<int>> &ans, vector<int> freq){
        if(temp.size()==nums.size()){
            ans.push_back(temp);
            return;
        }

        for(int i=0; i<nums.size(); i++){
            if(!freq[i]){
                temp.push_back(nums[i]);
                freq[i]=1;
                helper(nums, temp, ans, freq);
                freq[i]=0;
                temp.pop_back();
            }
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> temp;
        vector<vector<int>> ans;
        vector<int> freq(nums.size(),0);
        helper(nums, temp, ans, freq);
        return ans;
    }
};
```

2. Swapping to generate all permutations Approach ->
[Look at the recursive tree without fail](https://www.youtube.com/watch?v=f2ic2Rsc9pU&t=26s)

- If the current index idx reaches the size of the array, it means a valid permutation is found. Add it to the result vector ans.

- Iterate through each element in the array starting from the current index idx.
- Swap the current element with the element at index idx.
- Recursively call the helper function with the updated array and the next index (idx+1).
- After the recursive call, backtrack by swapping the elements back to their original positions.

- After the recursion, the ans vector contains all the valid permutations, and it is returned.

The time complexity is O(N!), where N is the number of distinct integers in the input array nums. The space complexity is O(N) for the recursion stack, as the recursion explores each element in the array. The space complexity is optimized compared to the previous approach as it doesn't use additional temporary vectors in each recursive call.

## Code ->
```cpp
class Solution {
public:
    void helper(vector<int> nums, vector<vector<int>> &ans, int idx){
        if(idx==nums.size()){
            ans.push_back(nums);
            return;
        }

        for(int i=idx; i<nums.size(); i++){
            swap(nums[idx], nums[i]);
            helper(nums, ans, idx+1);
            swap(nums[idx], nums[i]);
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> ans;
        helper(nums, ans, 0);
        return ans;
    }
};
```

# [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/description/)
## Approach ->
The helper function is called recursively with three parameters: the remaining counts of open parentheses (open), close parentheses (close), and the current combination string (str). The base case is when both open and close counts are zero, at which point the current combination is added to the result vector (ans).

The recursion explores two possibilities at each step:

If there are remaining open parentheses (open > 0), it appends an open parenthesis to the current combination and calls the helper function with reduced open count.
If there are remaining close parentheses (close > 0 and there are more open parentheses than closed ones), it appends a close parenthesis to the current combination and calls the helper function with reduced close count.

## Code ->
```cpp
class Solution {
public:
    void helper(int n, int open, int close, vector<string> &ans, string str){
        if(open==0 && close==0){
            ans.push_back(str);
            return;
        }
        if(open){
            helper(n, open-1, close, ans, str+'(');
        } 
        
        if(close && open<close){
            helper(n, open, close-1, ans, str+')');
        }
    }
    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        helper(n, n, n, ans, "");
        return ans;
    }
};
```
# [51. N-Queens](https://leetcode.com/problems/n-queens/description/)
## [Approaches](https://takeuforward.org/data-structure/n-queen-problem-return-all-distinct-solutions-to-the-n-queens-puzzle/)

---
## Codes ->
Code 1
```cpp
class Solution {
public:
    bool isValid(int row, int col, int n, vector<string>board){
        // checking left
        int r = row;
        int c = col;
        while(c>=0){
            if(board[r][c]=='Q') return false;
            c--;
        }

        // checking down-left diagonal
        r = row;  
        c = col;
        while(r<n && c>=0){
            if(board[r][c]=='Q') return false;
            r++;
            c--;
        }
        
        // checking up-left diagonal
        r = row;
        c = col;
        while(r>=0 && c>=0){
            if(board[r][c]=='Q') return false;
            r--;
            c--;
        }
        return true;
    }
    void helper(int col, vector<string> &board, vector<vector<string>> &ans, int n){
        if(col == n){
            ans.push_back(board);
            return;
        }

        for(int row=0; row<n; row++){
            if(isValid(row, col, n, board)){
                board[row][col]='Q';
                helper(col+1, board, ans, n);
                board[row][col]='.';
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> ans;
        vector<string> board(n);
        string s (n, '.');
        for(int i=0; i<n; i++) board[i]=s;
        helper(0, board, ans, n);
        return ans;
    }
};
```

Code 2
```cpp
class Solution {
public:
    void helper(int col, vector<string> &board, vector<vector<string>> &ans, int n, vector<int> &leftrow, vector<int> &upperDiagonal, vector<int> &lowerDiagonal){
        if(col == n){
            ans.push_back(board);
            return;
        }

        for(int row=0; row<n; row++){
            if(leftrow[row]==0 && upperDiagonal[n-1+col-row]==0 && lowerDiagonal[row+col]==0){
                board[row][col]='Q';
                leftrow[row]=1;
                upperDiagonal[n-1+col-row]=1;
                lowerDiagonal[row+col]=1;
                helper(col+1, board, ans, n, leftrow, upperDiagonal, lowerDiagonal);
                board[row][col]='.'; // backtract to reset
                leftrow[row]=0;
                upperDiagonal[n-1+col-row]=0;
                lowerDiagonal[row+col]=0;
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> ans;
        vector<string> board(n);
        string s (n, '.');
        for(int i=0; i<n; i++) board[i]=s;
        vector<int>leftrow(n, 0);
        vector<int> upperDiagonal(2*n-1, 0);
        vector<int> lowerDiagonal(2*n-1, 0);
        helper(0, board, ans, n, leftrow, upperDiagonal, lowerDiagonal);
        return ans;
    }
};
```

# [131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

## [Approach](https://takeuforward.org/data-structure/palindrome-partitioning/)
Note that here we need to partition the string, so this is how we partition. Remember this pattern for further partitioning questions.

## Code ->
```cpp
class Solution {
public:
    bool isPalindrome(int idx, int i, string s){
        while(idx<i){
            if(s[idx++] != s[i--]) return false;
        }
        return true;
    }
    void helper(string s, vector<vector<string>> &ans, vector<string> &temp, int idx){
        if(s.size()==idx){
            ans.push_back(temp);
            return;
        }

        for(int i=idx; i<s.size(); i++){
            if(isPalindrome(idx, i, s)){ // if the substring from start to end is palindrome
                temp.push_back(s.substr(idx, i-idx+1)); // push substring start to end in temp
                helper(s, ans, temp, i+1); // call recursion to find other partitions
                temp.pop_back(); // reset temp
            }
        }
        
    }
    vector<vector<string>> partition(string s) {
        vector<vector<string>> ans;
        vector<string> temp;
        helper(s, ans, temp, 0);
        return ans;
    }
};
```

# [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

## Approach ->
Make a mapping for all the numbers. Extract the alphabets from given digit one by one and for every letter for a given digit, call the recursive function.
[Recursive tree](https://www.youtube.com/watch?v=vgnhZzw-kfU)

## Code ->
```cpp
class Solution {
public:
    void helper(string digits, vector<string> digitMapping, string temp, int idx, vector<string> &ans){
        if(idx==digits.size()){
            ans.push_back(temp);
            return;
        }

        string curStr = digitMapping[digits[idx]-'0']; // extract the alphabets from given digit to curStr. Also don't forget to subtract digits[idx] with '0' because it is not a number it is a character.

        for(int i=0; i<curStr.size(); i++)
            helper(digits, digitMapping, temp+curStr[i], idx+1, ans); // recursive call for next index and temp+curStr[i]
    }
    vector<string> letterCombinations(string digits) {
        vector<string> ans; // declare answer to return
        if(digits.size()==0) return ans; // important base condition
        vector<string> digitMapping = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"}; // declaring a string mapping of size 10
        helper(digits, digitMapping, "", 0, ans);
        return ans;
    }
};
```
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