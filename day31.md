
# [907. Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/description/)

## Approach ->
 The intuition behind the solution is to leverage the concept of the Next Smaller Element (NSE) on both the left and right sides for each element in the array. By finding the indices of the next smaller element to the left (NSL) and right (NSR), the algorithm efficiently determines the number of subarrays in which the current element is the minimum. Multiplying the counts of elements on the left and right provides the total ways each element contributes to the sum of minimums across all subarrays. The overall approach optimally handles the problem of finding the sum of minimums for all contiguous subarrays in the given array.
 [VIDEO SOLUTION](https://www.youtube.com/watch?v=HRQB7-D2bi0&t=1988s)

## Code ->
```cpp
class Solution {
public:

    // Function to find the Next Smaller to Left (NSL) for each element in the array
    vector<int> findNSL(vector<int>& arr, int n){
        vector<int> result(n);
        stack <int> st;

        for(int i=0; i<n; i++){
            if(st.empty()){
                result[i] = -1;
            }
            else{
                // Pop elements from the stack until a smaller element is found or the stack is empty
                // note how we used >= here 
                while(!st.empty() && arr[st.top()] >= arr[i]) 
                    st.pop();
                
                // If stack is empty, set NSL to -1, else set NSL to the top element of the stack
                result[i] = st.empty() ? -1 : st.top();
            }
            // Push the current index onto the stack
            st.push(i);
        }
        return result;
    }

    // Function to find the Next Smaller to Right (NSR) for each element in the array
    vector<int> findNSR(vector<int>& arr, int n){
        vector<int> result(n);
        stack <int> st;

        for(int i=n-1; i>=0; i--){
            if(st.empty()){
                result[i] = n;
            }
            else{
                // Pop elements from the stack until a smaller element is found or the stack is empty
                // note how we used > here
                while(!st.empty() && arr[st.top()] > arr[i]) 
                    st.pop();
                
                // If stack is empty, set NSR to n, else set NSR to the top element of the stack
                result[i] = st.empty() ? n : st.top();
            }
            // Push the current index onto the stack
            st.push(i);
        }
        return result;
    }

    // Main function to calculate the sum of minimums for all subarrays
    int sumSubarrayMins(vector<int>& arr) {
        int n = arr.size();

        // Find the Next Smaller to Left (NSL) and Next Smaller to Right (NSR) arrays
        vector<int> NSL = findNSL(arr, n);
        vector<int> NSR = findNSR(arr, n);

        long long sum = 0;
        int MOD = 1e9+7;

        // Iterate through each element in the array
        for(int i=0; i<arr.size(); i++){
            // Calculate the number of elements to the left and right of the current element
            long long ls = i - NSL[i]; // how many elements on left of i
            long long rs = NSR[i] - i; // how many elements on right of i

            // Calculate the total number of subarrays whose minimum is arr[i]
            long long totalWays = ls * rs;

            // Calculate the sum for the current element and add it to the overall sum
            long long totalSum = arr[i] * totalWays;
            sum = (sum + totalSum) % MOD;
        }

        return sum;
    }
};
```

# [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/description/)

## Approach ->
we consider every bar to be smaller and try to find how many rectangles it can cover. The algorithm considers each bar in the histogram as a potential height for a rectangle and aims to determine the maximum width it can cover. The process involves finding the next smaller bar on the left (NSL) and the next smaller bar on the right (NSR) for each bar. The area of the rectangle is then calculated by multiplying the height with the difference between the NSR and NSL indices. The maximum area across all bars is tracked, resulting in the area of the largest rectangle in the histogram.

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
        // Obtain Next Smaller to Left (NSL) and Next Smaller to Right (NSR) arrays
        vector<int> NSL = findNSL(heights);
        vector<int> NSR = findNSR(heights);

        int ans = 0;

        // Iterate through each bar in the histogram
        for(int i = 0; i < heights.size(); i++) {
            // Calculate the potential area of the rectangle formed by the current bar
            int currentArea = heights[i] * (NSR[i] - NSL[i] - 1);

            // Update the maximum area if the current area is larger
            ans = max(ans, currentArea);
        }

        // Return the area of the largest rectangle in the histogram
        return ans;
    }
};
```

# CN->
Intro to Process Scheduling | FCFS | Convoy Effect:
1. Process Scheduling
	a. Basis of Multi-programming OS.
	b. By switching the CPU among processes, the OS can make the computer more productive.
	c. Many processes are kept in memory at a time, when a process must wait or the time quantum expires, the OS takes the CPU away from that process & gives the CPU to another process & this pattern continues.
2. CPU Scheduler
	a. Whenever the CPU becomes ideal, OS must select one process from the ready queue to be executed.
	b. Done by STS.
3. Non-preemptive scheduling
	a. Once the CPU has been allocated to a process, the process keeps the CPU until it releases CPU either by
	terminating or by switching to wait-state.
	b. Starvation: a process with a long burst time may starve less burst time process.
	c. Low CPU utilization.
4. Preemptive scheduling
	a. CPU is taken away from a process after time quantum expires along with terminating or switching to wait-state.
	b. Less Starvation
	c. High CPU utilization.
5. Goals of CPU scheduling
	a. Maximum CPU utilization
	b. Minimum Turnaround time (TAT).
	c. Min. Wait-time
	d. Min. response time.
	e. Max. throughput of system.
6. Throughput: No. of processes completed per unit time.
7. Arrival time (AT): Time when process is arrived at the ready queue.
8. Burst time (BT): The time required by the process for its execution.
9. Turnaround time (TAT): Time taken from first time process enters ready state till it terminates. (CT - AT)
10. Wait time (WT): Time process spends waiting for CPU. (WT = TAT – BT)
11. Response time: Time duration between process getting into ready queue and process getting CPU for the first time.
12. Completion Time (CT): Time taken till process gets terminated.