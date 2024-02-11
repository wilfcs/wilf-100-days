
# [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/description/)

## [Approach](https://takeuforward.org/data-structure/next-greater-element-using-stack/)

## Code ->
```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        // Map to store the next greater element for each element in nums2
        map<int, int> mp;
        
        // Stack to keep track of elements in nums2
        stack<int> st;
        
        // Iterate through nums2 in reverse order
        for(int i = nums2.size() - 1; i >= 0; i--){
            // If the stack is empty, set the next greater element to -1
            if(st.empty()){
                mp[nums2[i]] = -1;
            }
            // If the current element is smaller than the top of the stack
            else if(st.top() > nums2[i]){
                // Set the next greater element to the top of the stack
                mp[nums2[i]] = st.top();
            }
            // If the current element is greater or equal to the top of the stack
            else{
                // Pop elements from the stack until a greater element is found or the stack is empty
                while(!st.empty()){
                    if(st.top() < nums2[i]) st.pop();
                    else break;
                }
                // If the stack is empty, set the next greater element to -1, else set it to the top of the stack
                if(st.empty()) mp[nums2[i]] = -1;
                else mp[nums2[i]] = st.top();
            }
            // Push the current element onto the stack
            st.push(nums2[i]);
        }

        // Vector to store the results for nums1
        vector<int> ans;
        
        // Iterate through nums1 and push the corresponding next greater element from the map
        for(int i = 0; i < nums1.size(); i++){
            ans.push_back(mp[nums1[i]]);
        }
        
        return ans;
    }
};
```

# [503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/description/)

## Approach ->
This question is very similar to the previous question but the only difference is that we are given a circular array. The code takes advantage of the circular nature of the array by initially pushing all elements onto the stack.
This ensures that when the second iteration starts, the stack already contains all possible candidates for the next greater element.
The rest of the logic remains the same as in the previous explanation, with comparisons and stack operations considering the circular nature of the array.
The final result vector contains the next greater element for each element in the circular array.

## Code ->
```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        vector<int> ans;  // Result vector to store the next greater elements
        stack<int> st;    // Stack to keep track of elements

        // Iterate through the array and push all elements onto the stack at the beginning
        for (int i = nums.size() - 1; i >= 0; i--)
            st.push(nums[i]);

        // Iterate through the array again, considering its circular nature
        for (int i = nums.size() - 1; i >= 0; i--) {
            if (st.empty()) {
                // If the stack is empty, push -1 to indicate no greater element
                ans.push_back(-1);
            } else if (st.top() > nums[i]) {
                // If the top of the stack is greater than the current element, push it onto the result vector
                ans.push_back(st.top());
            } else {
                // Pop elements from the stack until a greater element is found or the stack is empty
                while (!st.empty()) {
                    if (st.top() <= nums[i]) st.pop();
                    else break;
                }
                // If the stack is empty, push -1, else push the top element of the stack
                if (st.empty()) ans.push_back(-1);
                else ans.push_back(st.top());
            }
            // Push the current element onto the stack
            st.push(nums[i]);
        }

        // Reverse the result vector to maintain the original order of elements
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```
# [Nearest Smaller Element](https://www.interviewbit.com/problems/nearest-smaller-element/)

## Approach ->
If you have solved the above two questions, then this should be easy peasy.

## Code ->
```cpp
vector<int> Solution::prevSmaller(vector<int> &A) {
    vector<int> ans;
    stack<int> st;
    for(int i=0; i<A.size(); i++){
        if(st.empty()) ans.push_back(-1);
        else if(st.top()<A[i]) ans.push_back(st.top());
        else{
            while(st.size()){
                if(st.top()<A[i]) break;
                else st.pop();
            }
            if(st.size()) ans.push_back(st.top());
            else ans.push_back(-1);
        }
        st.push(A[i]);
    }
    
    return ans;
}
```

# [735. Asteroid Collision](https://leetcode.com/problems/asteroid-collision/description/)

## Approach ->
Use a stack to simulate the collisions of asteroids while traversing the array.
If the current asteroid is positive (moving right) or the stack is empty, push it onto the stack.
If the current asteroid is negative (moving left), handle the collision logic:
Pop asteroids from the stack until a larger or equal-sized positive asteroid is encountered or the stack is empty.
If there's an equal-sized positive asteroid on top, explode both; otherwise, push the current negative asteroid.
Finally, construct the result vector from the remaining elements in the stack, reversing it to maintain order.

You could have used vector itself to replicate stacks

Solve this please because thinking about the logic here is easier and the coding part is a bit tricky and you might miss some edge cases.

## Code ->
```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& ast) {
        stack<int> st; // Stack to track asteroids during collisions
        for(int i = 0; i < ast.size(); i++) {
            // If asteroid is positive (moving right) or stack is empty, push onto stack
            if(ast[i] > 0 || st.empty()) {
                st.push(ast[i]);
            } 
            else {
                // Asteroid is negative (moving left)
                // Collisions happen when moving left asteroid is bigger than the top of the stack
                // Very important conditions, if you miss any then there will be edge cases
                while(!st.empty() && st.top() > 0 && st.top() < abs(ast[i])) {
                    st.pop(); // Explode the smaller asteroid
                }
                // Explode both asteroids if they are equal in size
                if(!st.empty() && st.top() == abs(ast[i]))
                    st.pop();
                // Push the moving left asteroid onto the stack 
                // if stack is empty or there are only left moving aesteroids left in stack
                else if(st.empty() || st.top() < 0)
                    st.push(ast[i]); 
            }
        }

        vector<int> ans;
        while(!st.empty()) {
            ans.push_back(st.top());
            st.pop();
        }
        reverse(ans.begin(), ans.end()); // Reverse the order of the stack to get the final result
        return ans;
    }
};
```

# [402. Remove K Digits](https://leetcode.com/problems/remove-k-digits/description/)

## Approach ->
The code aims to find the smallest possible integer after removing k digits from the given non-negative integer num. It utilizes a stack to maintain a decreasing order of digits while iterating through the input. The algorithm considers comparisons with the stack's top, handles removals, and takes care of leading zeroes. The final result is obtained by constructing a string from the stack and reversing it. Edge cases, such as when the length of num is less than or equal to k, are handled appropriately.
Solve this question please. You weren't able to solve the edge cases at all.

## Code ->
```cpp
class Solution {
public:
    string removeKdigits(string num, int k) {
        // number of operation greater than length we return an empty string
        if(num.length() <= k)   
            return "0";
        
        // k is 0 , no need of removing /  preforming any operation
        if(k == 0)
            return num;
        
        string res = "";// result string
        stack <char> s; // char stack
        
        s.push(num[0]); // pushing first character into stack
        
        for(int i = 1; i<num.length(); ++i)
        {
            while(k > 0 && !s.empty() && num[i] < s.top())
            {
                // if k greater than 0 and our stack is not empty and the upcoming digit,
                // is less than the current top than we will pop the stack top
                --k;
                s.pop();
            }
            
            s.push(num[i]);
            
            // popping preceding zeroes
            if(s.size() == 1 && num[i] == '0')
                s.pop();
        }
        
        while(k && !s.empty())
        {
            // for cases like "456" where every num[i] > num.top()
            --k;
            s.pop();
        }
        
        while(!s.empty())
        {
            res.push_back(s.top()); // pushing stack top to string
            s.pop(); // pop the top element
        }
        
        reverse(res.begin(),res.end()); // reverse the string 
        
        if(res.length() == 0)
            return "0";
        
        return res;
        
        
    }
};
```