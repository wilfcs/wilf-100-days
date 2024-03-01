
# [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/)

## Code ->
```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        vector<pair<int, int>> vec;

        for(int i=0; i<nums.size(); i++) mp[nums[i]]++;

        for(auto a: mp) vec.push_back({a.second, a.first});

        sort(vec.rbegin(), vec.rend());

        vector<int> ans;
        for(int i=0; i<vec.size() && k; i++){
            ans.push_back(vec[i].second);
            k--;
        }

        return ans;
    }
};
```

# [Connect n ropes with minimum cost](https://www.codingninjas.com/studio/problems/connect-n-ropes-with-minimum-cost_625783?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf)

## Code ->
```cpp
int minCost(int arr[], int n)
{
	/*Write your code here. 
	 *Don't write main().
	 *Don't take input, it is passed as function argument.
	 *Don't print output.
	 *Taking input and printing output is handled automatically.
	*/ 
	priority_queue<int, vector<int>, greater<int>> pq;

	for(int i=0; i<n; i++)pq.push(arr[i]);

	int ans = 0;

	while(pq.size()>1){
		int a = pq.top();
		pq.pop();
		int b = pq.top();
		pq.pop();

		ans+=a+b;

		pq.push(a+b);
	}

	return ans;
}
```
# [Merge K Sorted Arrays](https://www.codingninjas.com/studio/problems/merge-k-sorted-arrays_975379)
## [Approach](https://www.youtube.com/watch?v=l8CuET0jlDU)
Watch the vid above. But if you follow the - insert everything in a 1d vector and sort it approach then the TC will be O(n*k log(n*k)). But using pq we can take advantage of the arrays already being sorted and solve it in O(n*k log(k)) TC.

## Code ->
```cpp
class Solution
{
public:
    // Define a structure to store the value, row, and column of an element.
    struct data
    {
        int val; // value of the element
        int row; // row index of the element
        int col; // column index of the element

        data(int v, int r, int c) : val(v), row(r), col(c) {}
    };

    // Define a custom comparison function for the priority queue.
    struct myComp
    {
        // Compare elements based on their values.
        bool operator()(const data &a, const data &b)
        {
            return a.val > b.val;
        }
    };

    // Function to merge k sorted arrays.
    vector<int> mergeKArrays(vector<vector<int>> arr, int k)
    {
        // Initialize an empty vector to store the merged sorted array.
        vector<int> ans;

        // Priority queue to store elements along with their row and column information.
        // Using myComp as the custom comparison function.
        priority_queue<data, vector<data>, myComp> pq;

        // Push the first element from each array into the priority queue.
        for (int i = 0; i < k; i++)
        {
            data d(arr[i][0], i, 0);
            pq.push(d);
        }

        // Process the priority queue until it becomes empty.
        while (pq.size())
        {
            // Extract the minimum element from the priority queue.
            auto curr = pq.top();
            pq.pop();

            // Add the value of the current element to the result.
            ans.push_back(curr.val);

            // Get the row and column indices of the current element.
            int r = curr.row, c = curr.col;

            // If there are more elements in the same row, push the next element into the priority queue.
            if (c + 1 < arr[r].size())
            {
                data d(arr[r][c + 1], r, c + 1);
                pq.push(d);
            }
        }

        // Return the merged sorted array.
        return ans;
    }
};
```

# [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/description/)

## Approach ->
We have to return the median, so if n is odd we just have to return the middle element and if n is even we return the sum of two middle elements divided by 2. Coming to the question, we can imagine this large stream of data in two parts from the middle. eg-> 1,2,3,4,5 can be divided into two parts i.e. 1,2,3 and 4,5. To represent the left part we can take a max heap and to represent the right part from the middle we can take a min heap. That way the top of both the heaps will always give the middle element. So the top of both the heaps in our example will give us 3 and 4. That way we can easily find our median by looking at the value of n that we will also be maintaining. 

Now, how do we insert in the heaps? We know for a fact that the left heap will always be equal in size or one greater than the right heap. So first when we encounter an element just check if it is smaller than the top of left max heap, if it is then push the element in the left heap because it belongs to the left part of the array (data stream) else push it in the right part. Then check if the size of right heap became greater than the left heap, if yes then pop one element from the top and push it in left heap. If the size of left heap is two greater than the right heap then do the same, pop and push in right. We can now easily find out our answer. Look at the code and you'd understand.

## Code ->
```cpp
class MedianFinder {
public:
    // Max heap for the left part of the array (data stream)
    priority_queue<int> leftMaxHeap;
    // Min heap for the right part from the middle of the array (data stream)
    priority_queue<int, vector<int>, greater<int>> rightMinHeap;
    // Total number of elements in the data stream
    int n = 0;

    // Constructor to initialize the MedianFinder object
    MedianFinder() {
    }
    
    // Function to add an integer 'num' from the data stream to the data structure
    void addNum(int num) {
        // Check if the leftMaxHeap is empty or if the current element belongs to the left part
        if (leftMaxHeap.size() == 0 || leftMaxHeap.top() > num)
            leftMaxHeap.push(num);
        else
            rightMinHeap.push(num);

        // Balance the heaps if necessary
        if (rightMinHeap.size() > leftMaxHeap.size()) {
            int temp = rightMinHeap.top();
            rightMinHeap.pop();
            leftMaxHeap.push(temp);
        } else if (leftMaxHeap.size() > rightMinHeap.size() + 1) {
            int temp = leftMaxHeap.top();
            leftMaxHeap.pop();
            rightMinHeap.push(temp);
        }

        // Increment the total count of elements
        n++;
    }
    
    // Function to return the median of all elements so far in the data stream
    double findMedian() {
        // Check if the total count of elements is odd
        if (n % 2 == 1)
            return leftMaxHeap.top();
        // If even, return the average of the two middle elements
        else
            return double(double(leftMaxHeap.top() + rightMinHeap.top()) / 2);
    }
};
```

# [621. Task Scheduler](https://leetcode.com/problems/task-scheduler/description/)

## Approach ->
This question is based on a few observations. First observation is the question itself which you might interpret wrong. To understand the question let's take a custom testcase. 

tasks = A, A, A, A, A, B, B, B, C, C. n = 2.

Here we have 5 A's, 3 B's and 2 C's. ATQ we can repeat a number after n times. So we can repeat let's say A after 2 times. Another observation can be that the element with the most frequency should be taken first and given the most priority because we want to finish them first. So in this case A we will try to finish first.

So the CPU Cycles should be -> A, B, C, A, B, C, A, B, IDLE, A, IDLE, IDLE, A.
Did you notice something here? We are making a window of size n+1 with unique elements here. Here the value of n is 2 so the window size is 3. We can use this for solving the question. 

How to code this question?
To code the question follow these following steps, keep in mind that we don't need the characters to solve the question we will just need their frequencies so we will simply extract the frequencies of the chars and store them in our max heap to solve the question.

Steps:
1. Frequency Map: Start by creating a frequency map (unordered_map) to count the occurrences of each task.

2. Priority Queue: Use a priority queue (max heap) to store their frequency. This is because we want to process the most frequent tasks first. We don't need the characters at this point, we will proceed with the frequencies only.

3. Greedy Approach: In each iteration, create a window of size (n+1) to represent one cycle. This window ensures that we adhere to the cooling constraint.

4. Process Tasks: In each cycle, pop tasks from the priority queue and decrease their frequency. Append these tasks with one decreased frequency to a temporary vector.

5. Reinsert Tasks: After processing tasks in the cycle, reinsert the tasks with remaining frequency back into the priority queue.

6. Idle Time: If the priority queue is empty after processing tasks in the cycle, it means there are no more tasks left and we were able to finish all tasks before the n+1 cycle. In this case, add the actual time taken in the cycle to the final answer. Otherwise, add the maximum possible time for a full cycle (n+1).

7. Repeat: Continue this process until there are no more tasks left in the frequency map.

## Code ->
```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        unordered_map<char, int> mp;

        // Step 1: Frequency Map
        for(int i = 0; i < tasks.size(); i++) 
            mp[tasks[i]]++;

        priority_queue<int> pq;

        // Step 2: Create Priority Queue with just the frequencies
        for(auto a: mp) 
            pq.push(a.second);

        int ans = 0;

        // Step 3-7: Greedy Approach
        while(pq.size()){
            int time = 0;
            vector<int> temp;

            // Step 3: Iterate for the Window of size n+1
            for(int i = 0; i < n + 1; i++){
                if(pq.empty()) break;
                // Push the updated frequency in the temp vector
                temp.push_back(pq.top() - 1);
                pq.pop();
                time++;
            }

            // Step 4: Repopulate the priority queue
            for(int i = 0; i < temp.size(); i++){
                if(temp[i] > 0) 
                    pq.push(temp[i]);
            }

            // Step 5-7: Reinsert Tasks, Idle Time, Repeat
            ans += pq.empty() ? time : n + 1;
        }

        return ans;
    }
};
```
## Complexity Analysis ->
The time complexity of the provided code is O(N log N), where N is the number of tasks.
- The creation of the frequency map takes O(N) time, where N is the number of tasks.
- The priority queue operations involve inserting and extracting elements, each of which takes O(log N) time. In the worst case, we might insert and extract all tasks, resulting in O(N log N) for priority queue operations.

# [355. Design Twitter](https://leetcode.com/problems/design-twitter/description/)

## Code ->
```cpp
class Twitter {
public:
    // Map to store the user's friends (followees).
    map<int, set<int>> friends;

    // Current timestamp for tweets.
    int curr = 0;

    // Priority queue to store tweets with timestamp for the news feed.
    priority_queue<vector<int>> timeline;

    // Constructor to initialize the Twitter object.
    Twitter() {
        
    }
    
    // Function to post a new tweet by a user.
    void postTweet(int userId, int tweetId) {
        // Push the tweet into the timeline with timestamp and user ID.
        timeline.push({curr++, tweetId, userId});
    }
    
    // Function to get the 10 most recent tweets in the user's news feed.
    vector<int> getNewsFeed(int userId) {
        // Vector to store the result tweets.
        vector<int> ans;
        
        // Create a copy of the timeline priority queue.
        priority_queue<vector<int>> userTimeline = timeline;
        
        // Count variable to track the number of tweets added to the result.
        int n = 0;

        // Iterate through the user's timeline.
        while (userTimeline.size() && n < 10) {
            // Get the top tweet from the timeline.
            auto topTweet = userTimeline.top();
            userTimeline.pop();

            // Check if the tweet is posted by the user or a friend (followee).
            if (topTweet[2] == userId || friends[userId].count(topTweet[2])) {
                // Add the tweet ID to the result.
                ans.push_back(topTweet[1]);
                n++;
            }
        }

        // Return the result.
        return ans;
    }
    
    // Function to make a user follow another user.
    void follow(int followerId, int followeeId) {
        // Insert the followee into the set of friends for the follower.
        friends[followerId].insert(followeeId);
    }
    
    // Function to make a user unfollow another user.
    void unfollow(int followerId, int followeeId) {
        // Erase the followee from the set of friends for the follower.
        friends[followerId].erase(followeeId);
    }
};
```

# [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/description/)

## Code ->
```cpp
class KthLargest {
public:
    // Min heap to store the k largest elements in the stream.
    priority_queue<int, vector<int>, greater<int>> min_heap;

    // Variable to store the value of k.
    int kth = 0;

    // Constructor to initialize the object with k and nums.
    KthLargest(int k, vector<int>& nums) {
        // Set the value of k.
        kth = k;

        // Iterate through the nums array.
        for (int i = 0; i < nums.size(); i++) {
            // Push the current element into the min heap.
            min_heap.push(nums[i]);

            // If the size of the min heap exceeds k, pop the smallest element.
            if (min_heap.size() > k)
                min_heap.pop();
        }
    }
    
    // Function to add a new element to the stream.
    int add(int val) {
        // Push the new element into the min heap.
        min_heap.push(val);

        // If the size of the min heap exceeds k, pop the smallest element.
        if (min_heap.size() > kth)
            min_heap.pop();

        // Return the top element of the min heap, representing the kth largest element.
        return min_heap.top();
    }
};
```

