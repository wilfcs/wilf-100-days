# [85. Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/description/)

## Approach ->
So basically this question is just an extension of the Largest Rectangle in Histogram i.e. the question above. We will simply traverse the 2d array and for each row try to make the blocks (rectangles) considering the numbers above. Then pass each row to our same old function of previous question to find the largest rectangle.

## Code ->
```cpp
class Solution {
public:
    vector<int> findNSL(vector<int>& heights){
        stack<int> st;
        vector<int> res;

        for(int i=0; i<heights.size(); i++){
            while(st.size() && heights[st.top()]>=heights[i]) st.pop();

            if(st.empty()) res.push_back(-1);
            else res.push_back(st.top());

            st.push(i);
        }
        return res;
    }
    vector<int> findNSR(vector<int>& heights){
        stack<int> st;
        vector<int> res;

        for(int i=heights.size()-1; i>=0; i--){
            while(st.size() && heights[st.top()]>=heights[i]) st.pop();

            if(st.empty()) res.push_back(heights.size());
            else res.push_back(st.top());

            st.push(i);
        }
        reverse(res.begin(), res.end()); // don't forget to reverse in the case of NSR
        return res;
    }
    int largestRectangleArea(vector<int>& heights) {
        // we consider every bar to be smaller and try to find how many rectangles it can cover
        vector<int> NSL = findNSL(heights);
        vector<int> NSR = findNSR(heights);

        int ans; 

        for(int i=0; i<heights.size(); i++){
            int res = heights[i] * (NSR[i] - NSL[i] - 1);
            ans = max(ans, res);
        }

        return ans;
    }
    // MAIN function (rest everything on top is same as above question)
    int maximalRectangle(vector<vector<char>>& matrix) {
        int ans = 0;
        // Convert the char matrix to an integer binary matrix for ease of calculation
        vector<vector<int>> vec(matrix.size(), vector<int>(matrix[0].size()));
        for(int i=0; i<matrix.size(); i++){
            for(int j=0; j<matrix[0].size(); j++){
                vec[i][j]=((matrix[i][j]-'0'));
            }
        }
        // Traverse each row and calculate the maximal rectangle area
        for(int i=0; i<vec.size(); i++){
            for(int j=0; j<vec[0].size(); j++){
                // Update the height for each row based on the elements above
                if(i && vec[i][j]==1){
                    vec[i][j] += vec[i-1][j];
                }
            }
            int res = largestRectangleArea(vec[i]);
            ans = max(res, ans);
        }

        return ans;
    }
};
```

# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/description/)

## Approach ->
These types of questions use a concept called monotonic deque. This is because monotonic deque allows for efficient tracking of the maximum element within the sliding window. The deque (double-ended queue) maintains elements in a way that preserves their monotonicity, ensuring that the front of the deque always holds the maximum element in the current window.

This question can be solved using the following 4 steps:

Step1: When a new element comes make room for it by adjusting your window and popping the old element from the front of deque.

Step 2: We know from the concept of monotonic stack that the greatest number in the stack should always be at the front of deque, so after adjusting the window size we must maintain this propery of monotonic deque. To do this we compare the new element from the back of the deque and if the new element is greater then we keep popping from the back. That way if there were no elements greater than the new element then the queue will get empty and if there was a greater element in the queue already then it will still be on the front of the deque maintaining the deque's propery.

Step 3: Now we add the new element to the back of the deque. If that element is the greatest it would also be the front of the queue and if somone else is greatest then that will be the front of the queue. Hence the propery of monotonic deque that the greatest element should be at the front in maintained.

Step 4: Add to your answer the front of the deque.

Read the code to understand the approach. TC->O(N)

## Code ->
```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ans;
        deque<int> q;

        for(int i = 0; i < nums.size(); i++){
            // Step 1: Make room for the new element by adjusting the window
            while(!q.empty() && q.front() <= (i - k))
                q.pop_front();
            
            // Step 2: Maintain monotonic deque property by popping from the back
            while(!q.empty() && nums[i] > nums[q.back()])
                q.pop_back();

            // Step 3: Add the new element to the back of the deque
            q.push_back(i);
            
            // Step 4: Add the front of the deque to the answer
            if(i >= k - 1) 
                ans.push_back(nums[q.front()]);
        }

        return ans;
    }
};
```

# [901. Online Stock Span](https://leetcode.com/problems/online-stock-span/description/)

## Approach ->
Using the monotonic stack again. Initialize a stack to store pairs of prices and spans. When we spot a price that is greater than the previous price that is on top of the stack, we add the span of that price in our current price and keep doing it till we not find a smaller price. Then we push the new price with its calculated span in the stack and return its stack. In other words here are the steps we are going to perform..

1. Initialize a stack: Start with an empty stack.

2. Process each price: As we go through each stock price:
- Check if it's greater than the previous price on the stack.
- If yes, add the span of that price to our current price.
- Keep doing this until we find a smaller or equal price.
- Then, push the new price along with its calculated span onto the stack.

3. Repeat for all prices: Go through all the prices, following the same steps.

4. Return the stack: The stack now holds pairs of prices and their corresponding spans.

## Code ->
```cpp
class StockSpanner {
public:
    stack<pair<int, int>> st;  // Monotonic stack to store pairs of prices and spans.

    StockSpanner() {
        // Constructor: Initializes the object of the class.
    }
    
    int next(int price) {
        int span = 1;  // Currently span of price is 1 i.e. itself
        
        // Iterate through the stack to find consecutive days where the stock price is less than or equal to the current price.
        while (!st.empty() && st.top().first <= price) {
            span += st.top().second;  // Add the span of the current price on top of the stack to the current span.
            st.pop();  // Pop the pair from the stack as we've accounted for its span.
        }
        
        // Push the current price and its calculated span onto the stack for future references.
        st.push({price, span});
        
        return span;  // Return the calculated span for the current day.
    }
};
```

# [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/description/)

## Approach ->
Using the monotonic stack

## Codes->
1. SC-> O(2n)
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<pair<int, int>> st;  
        vector<int> ans(temperatures.size());  

        // Traverse the temperatures array from right to left.
        for(int i = temperatures.size() - 1; i >= 0; i--){
            int days_to_wait = 1;  // Initialize the number of days to wait.

            // Main step: Iterate through the stack to find consecutive days with temperatures less than the current day.
            while(st.size() && temperatures[i] >= st.top().first){
                days_to_wait += st.top().second;  // Add the days to wait for the current temperature.
                st.pop();  // Pop the pair from the stack as we've accounted for its days.
            }

            // If the stack is empty, it means there is no future day with a warmer temperature.
            if(st.empty()) 
                days_to_wait = 0;

            ans[i] = days_to_wait;

            // Push the current temperature and its calculated days to wait onto the stack for future references.
            st.push({temperatures[i], days_to_wait});
        }

        return ans; 
    }
};

2. SC-> O(n)
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        vector<int> ans(temperatures.size(), 0);  // Result vector to store the number of days to wait for a warmer temperature.
        stack<int> st;  // Monotonic stack to store indices of temperatures.

        // Traverse the temperatures array from right to left.
        for(int i = temperatures.size() - 1; i >= 0; i--){
            // Main Step: Iterate through the stack to find indices of temperatures less than or equal to the current day's temperature.
            while(st.size() && temperatures[st.top()] <= temperatures[i])
                st.pop();

            // If the stack is not empty, calculate the number of days to wait for a warmer temperature.
            if(!st.empty()) 
                ans[i] = st.top() - i;

            // Push the current index onto the stack.
            st.push(i);
        }

        return ans; 
    }
};
```
# [844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/description/)

## Approach ->
Create 2 separate stacks s1 and s2. If you encounter a "#" pop the element from stack. Else push all the elements in the stack.

## Code ->
```cpp
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        stack<char> s1;
        stack<char> s2;

        for(int i=0; i<s.size(); i++){
            if(s[i]=='#'){
                if(s1.size()) s1.pop();
            }
            else s1.push(s[i]);
        }
        for(int i=0; i<t.size(); i++){
            if(t[i]=='#'){
                if(s2.size()) s2.pop();
            }
            else s2.push(t[i]);
        }

        return s1==s2;
    }
};
```

# [The Celebrity Problem](https://www.geeksforgeeks.org/problems/the-celebrity-problem/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=bottom_sticky_on_article)

## Approach ->
1. TC->O(N*N) -> Iterate through each row and check if all elements in that row are 0 i.e. the row's person doesn't know anyone. Then check all the columns for that row if all it's column other than the diagonal column (for example M[1][1] or M[2][2]) are 1. If yes then that confirms that the row we are currently on is our celebrity. Else keep checking for all the other rows.
2. TC->O(N) -> Take a stack and insert all from 0 to n-1 in the stack i.e. all possible people who can be a celebrity. Now pop two people (a and b) from the stack and check if a knows b. If a knows b then a cannot be the celebrity hence push b in the stack and vice versa. Then when we have just one element left in the stack then that element/person is our potential celeb but we're not sure yet. So we that person/row if that is the real celeb by the method above. If it is the celeb then we return its index else we return -1. 

## Code ->
```cpp
class Solution 
{
public:
    // Function to find if there is a celebrity in the party or not.
    int celebrity(vector<vector<int>>& M, int n) 
    {
        stack<int> st;  // Stack to store potential celebrity candidates.
        
        // Initializing the stack with all potential celebrity candidates.
        for(int i = 0; i < M.size(); i++)
            st.push(i);
        
        // Stack-based elimination process to find a potential celebrity.
        while(st.size() > 1)
        {
            int a = st.top();
            st.pop();
            
            int b = st.top();
            st.pop();
            
            // If a knows b, eliminate a; else, eliminate b.
            if(M[a][b] == 1) 
                st.push(b);
            else 
                st.push(a);
        }
        
        int potential_celeb = st.top();  // Potential celebrity candidate.
        
        // Final check to verify if the potential celebrity does not know anyone.
        for(int i = 0; i < M[0].size(); i++)
        {
            if(M[potential_celeb][i] == 1) 
                return -1;
        }
        
        int cnt = 0;
        
        // Final check to count the number of people who know the potential celebrity.
        for(int i = 0; i < M.size(); i++)
        {
            if(M[i][potential_celeb] == 1) 
                cnt++;
        }
        
        // If the potential celebrity is known by everyone except themselves, return their index; else, return -1.
        if(cnt == (M.size() - 1)) 
            return potential_celeb;
        else 
            return -1;
    }
};
```