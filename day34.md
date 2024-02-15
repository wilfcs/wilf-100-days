# [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/)

## Approach ->
Just use stack, simple q.

## Code ->
```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> st;

        for(int i=s.size()-1; i>=0; i--){
            if(st.empty() || st.top() != s[i]) st.push(s[i]);
            else st.pop();
        }
        string ans = "";
        while(st.size()){
            ans+=st.top();
            st.pop();
        }
        return ans;
    }
};
```

# [71. Simplify Path](https://leetcode.com/problems/simplify-path/description/)

## Approach->
The code uses a technique called tokenization, which is like breaking down a sentence into individual words. In this case, it splits the input path using the '/' character as a separator.

Then, it goes through each part of the path. If it's an empty part or just a dot ('.'), it's ignored because they don't affect the path. If it's a double dot ('..'), it means going up one level in the directory. The code handles this by keeping track of the directories in a stack and popping one directory off the stack for each '..' encountered.

Finally, it constructs the simplified path by putting the directories back together, ensuring a clean and concise representation. If there's nothing left after this process, it defaults to the root directory ('/').

## Code ->
```cpp
class Solution {
public:
    string simplifyPath(string path) {
        string token = ""; // Variable to store each token during tokenization
        
        stringstream ss(path); // Create a stringstream with the input path
        stack<string> st; // Use a stack to keep track of directories in the simplified path
        
        // Tokenize the path using '/' as the delimiter
        while (getline(ss, token, '/')) {
            // Ignore empty tokens and '.' (current directory)
            if (token == "" || token == ".") continue;

            // Handle '..' (parent directory) by popping from the stack if it's not empty
            if (token != "..")
                st.push(token);
            else if (!st.empty())
                st.pop();
        }
        
        string result = ""; // Variable to store the simplified path
        
        // Build the simplified path by concatenating elements from the stack
        while (!st.empty()) {
            result = "/" + st.top() + result;
            st.pop();
        }
        
        if (result.length() == 0)
            result = "/"; // If no directory or file is present, minimum root directory must be present in result
        
        return result;
    }
};
```

# [946. Validate Stack Sequences](https://leetcode.com/problems/validate-stack-sequences/description/)

## Approach ->
The approach utilizes stack simulation to validate whether a given sequence of push and pop operations on an initially empty stack results in the target sequence. It iterates through the pushed array, simulating push and pop operations while checking for matching elements with the popped array. If the stack is empty at the end, the sequences are valid; otherwise, they are not. The time complexity is O(N), and the space complexity is O(N), where N is the length of the pushed array.

## Code->
```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> st; // Create a stack to simulate the stack operations

        int m = pushed.size(); // Get the size of the pushed array

        int i = 0, j = 0; // Initialize two pointers for pushed and popped arrays

        // Iterate through the pushed array
        while (i < m && j < m) {
            st.push(pushed[i]); // Simulate the push operation

            // Check if the stack top matches the current element in the popped array
            while (!st.empty() && j < m && st.top() == popped[j]) {
                st.pop(); // Simulate the pop operation
                j++; // Move to the next element in the popped array
            }
            i++; // Move to the next element in the pushed array
        }

        return st.empty(); // If the stack is empty, it means the sequences are valid
    }
};
```