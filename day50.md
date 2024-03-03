
# [632. Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/description/) 

## Code ->
```cpp
// most difficult question of heap

class Solution {
public:
    // Define a structure 'node' to represent elements in the matrix
    struct node {
        int val;  // Value of the element
        int row;  // Row index of the element in the matrix
        int col;  // Column index of the element in the matrix

        // Constructor to initialize the 'node' structure
        node(int data, int r, int c) {
            val = data;
            row = r;
            col = c;
        }
    };

    // Define a structure 'compare' for custom comparison in the priority queue
    struct compare {
        bool operator()(node &a, node &b) {
            // Compare nodes based on their values in descending order
            return a.val > b.val;
        }
    };

    // Function to find the smallest range that includes at least one number from each list
    vector<int> smallestRange(vector<vector<int>>& nums) {
        // Initialize variables to keep track of the minimum and maximum values
        int mini = INT_MAX, maxi = INT_MIN;

        // Create a min-heap (priority queue) to keep track of the smallest elements
        priority_queue<node, vector<node>, compare> minHeap;

        // Initialize the min-heap with the first element from each list
        for (int i = 0; i < nums.size(); i++) {
            node n(nums[i][0], i, 0);
            minHeap.push(n);
            mini = min(mini, nums[i][0]);
            maxi = max(maxi, nums[i][0]);
        }

        // Initialize variables to store the start and end of the smallest range
        int start = mini, end = maxi;

        // Process the min-heap until it is not empty
        while (minHeap.size()) {
            // Extract the minimum element from the min-heap
            node curr = minHeap.top();
            minHeap.pop();

            // Update the minimum value
            mini = curr.val;

            // Check if the current range is smaller than the existing range
            if (maxi - mini < end - start) {
                // Update the start and end values if a smaller range is found
                start = mini;
                end = maxi;
            }

            // Check if there are more elements in the current list
            if (curr.col + 1 < nums[curr.row].size()) {
                // Update the maximum value and push the next element from the current list
                maxi = max(maxi, nums[curr.row][curr.col + 1]);
                node n(nums[curr.row][curr.col + 1], curr.row, curr.col + 1);
                minHeap.push(n);
            } else {
                // Break the loop if there are no more elements in the current list
                break;
            }
        }

        // Return the smallest range as a vector
        return {start, end};
    }
};

```

# Basic terminologies ->

1. **Graph**: A representation of connections between nodes or vertices. Types include undirected (no specific direction) and directed (with specific direction) graphs.

2. **Node/Vertex**: Represented by circles, these are the entities in a graph.

3. **Edge**: The connection between nodes. Can be undirected (bi-directional) or directed (one-way).

4. **Undirected Graph**: A graph where edges have no specific direction.

5. **Directed Graph**: A graph with edges having a specific direction.

6. **Cycle**: A path that starts and ends at the same node, leading to terms like undirected cyclic graph and directed cyclic graph.

7. **Path**: A sequence of vertices where each adjacent pair is connected by an edge.

8. **Degree**: In undirected graphs, it's the number of edges connected to a node. The total degree is twice the number of edges. In directed graphs, it's split into in-degree (incoming edges) and out-degree (outgoing edges).

9. **Edge Weight**: A numerical value assigned to edges, influencing the properties of the graph. If not specified, unit weight (1) is assumed.


# [BFS of graph](https://www.geeksforgeeks.org/problems/bfs-traversal-of-graph/1)

## Approach ->
To traverse in the graph breath wise just create a queue and a visited array of size n (number of nodes in the adjacency list). Now push the first element of the graph i.e. 0 in the queue and mark it as visited. Now keep popping from the queue and check if the popped element has some nodes attached to it in the adjacency list and keep marking the visited nodes to avoid duplication. You will have your answer.

## Code ->
```cpp
class Solution {
public:
    // Function to return Breadth First Traversal of given graph.
    vector<int> bfsOfGraph(int V, vector<int> adj[]) {
        // Code here

        // Vector to store the result of Breadth-First Traversal
        vector<int> ans;

        // Queue for BFS traversal
        queue<int> q;

        // Array to mark visited nodes
        int visited[V] = {0};

        // Start BFS from node 0
        q.push(0);
        visited[0] = 1; // Mark the starting node as visited

        // Perform BFS traversal
        while (q.size()) {
            // Get the front element of the queue
            int curr = q.front();
            q.pop();

            // Add the current node to the result
            ans.push_back(curr);

            // Iterate through the adjacency list of the current node
            for (int i = 0; i < adj[curr].size(); i++) {
                // Check if the neighbor is not visited
                if (visited[adj[curr][i]] == 0) {
                    // Push the neighbor to the queue and mark it as visited
                    q.push(adj[curr][i]);
                    visited[adj[curr][i]] = 1;
                }
            }
        }

        // Return the result of BFS traversal
        return ans;
    }
};
```
## Complexity analysis ->
Time Complexity: O(N) + O(2E), Where N = Nodes, 2E is for total degrees as we traverse all adjacent nodes. 2E because degree's formula is 2*Edges.

Let's break it down:

When we visit a node during the BFS traversal, we go through its adjacency list to visit its neighbors.
For each neighbor, we check if it has been visited before. If not, we add it to the queue and mark it as visited.
For an undirected graph, each edge contributes to the degree (number of neighbors) of both nodes it connects.
So, when we traverse the adjacency list of each node, we encounter each edge twice (once for each endpoint of the edge), leading to a factor of 2 in the time complexity analysis. Therefore, the time complexity is often expressed as O(N + 2E), but the constant factor is dropped, and it simplifies to O(N + E).

Simple example: Look at a graph right now and think if for each node say 0 aren't we visiting its one neighbour say 1 and for that neighbour 1 aren't we revisiting its neighbour 0. So aren't we visiting each edge twice?

Space Complexity: O(3N) ~ O(N), Space for queue data structure visited array and an adjacency list



# [DFS of Graph](https://www.geeksforgeeks.org/problems/depth-first-traversal-for-a-graph/1)

## Approach ->
Just use recursion and keep track of the visited nodes

## Code ->
```cpp
class Solution {
  private:
    void dfs(vector<int> &ans, int visited[], int node, vector<int> adj[]){
        ans.push_back(node);
        visited[node] = 1;
        
        for(int i=0; i<adj[node].size(); i++){
            if(!visited[adj[node][i]])
                dfs(ans, visited, adj[node][i], adj);
        }
    }
  public:
    // Function to return a list containing the DFS traversal of the graph.
    vector<int> dfsOfGraph(int V, vector<int> adj[]) {
        // Code here
        vector<int> ans;
        int visited[V] = {0};
        
        dfs(ans, visited, 0, adj);
        
        return ans;
    }
};
```
## Complexity analysis ->
Time Complexity: For an undirected graph, O(N) + O(2E), For a directed graph, O(N) + O(E), Because for every node we are calling the recursive function once, the time taken is O(N) and 2E is for total degrees as we traverse for all adjacent nodes.

Space Complexity: O(3N) ~ O(N), Space for dfs stack space, visited array and an adjacency list.

# [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/description/)

## Approach ->
Note that we are not given the adjacency matrix in this question, we are just given a matrix with the values 1 and 0 representing the adjacency matrix. So the dfs call will be a lot different from what we usually do in case of adjacency matrix. Or you can make yourself an adjacency matrix either but what's the fun in it? Try figuring out what we are doing in this q.

## Code ->
```cpp
class Solution {
public:
    // Depth-First Search (DFS) function to traverse the graph
    void dfs(int num, vector<int> &visited, vector<vector<int>> &isConnected) {
        // Mark the current city as visited
        visited[num] = 1;

        // Iterate through the isConnected list of the current city
        for (int i = 0; i < isConnected[num].size(); i++) {
            // Check if the current city is connected to city i and i is not visited
            if (isConnected[num][i] && !visited[i]) {
                // Recursively perform DFS on the connected city i
                dfs(i, visited, isConnected);
            }
        }
    }

    // Function to find the total number of provinces
    int findCircleNum(vector<vector<int>>& isConnected) {
        int ans = 0;
        int n = isConnected.size();

        // Vector to keep track of visited cities
        vector<int> visited(n, 0);

        // Iterate through all cities
        for (int i = 0; i < n; i++) {
            // Check if the current city is not visited
            if (!visited[i]) {
                // Increment the province count and perform DFS on the current city
                ans++;
                dfs(i, visited, isConnected);
            }
        }

        return ans;
    }
};
```

# [200. Number of Islands](https://leetcode.com/problems/number-of-islands/description/)

## Approach ->
The main idea is to visit all the neigbouring lands and mark them in the visited matrix. That way when we complete once cycle or marking, we know for a fact that we have discovered an island. SO maintain a matrix of visited places in the grid. Iterate through each cell in the grid and If the cell is unvisited or the grid is land then call dfs to mark the neighbouring lands as 1. Now with each dfs call we are marking the neighbouring lands as 1. After the call is completed all the neighbour lands are marked 1 and those lands combined form an island.

## Code ->
```cpp
class Solution {
public:
    // Function to check if a cell is valid for DFS
    bool isValid(int row, int col, vector<vector<char>> &grid, vector<vector<int>> &visited){
        // return false if out of bounds or grid's value is 0 or is already visited
        if(row<0 || row>grid.size()-1 || col<0 || col>grid[0].size()-1 || grid[row][col]=='0' || visited[row][col]==1) return false;
        else return true;
    }
    // Depth-First Search (DFS) function to explore and mark connected land cells in our visited matrix
    void dfs(int row, int col, vector<vector<char>> &grid, vector<vector<int>> &visited){
        visited[row][col] = 1;

        // Explore neighboring cells if valid
        if(isValid(row+1, col, grid, visited)) dfs(row+1, col, grid, visited);
        if(isValid(row-1, col, grid, visited)) dfs(row-1, col, grid, visited);
        if(isValid(row, col+1, grid, visited)) dfs(row, col+1, grid, visited);
        if(isValid(row, col-1, grid, visited)) dfs(row, col-1, grid, visited);
    }
    int numIslands(vector<vector<char>>& grid) {
        int ans = 0;
        vector<vector<int>> visited(grid.size(), vector<int>(grid[0].size(),0));

        // Iterate through each cell in the grid
        for(int i=0; i<grid.size(); i++){
            for(int j=0; j<grid[0].size(); j++){
                // If the cell is unvisited and represents land, start DFS
                if(!visited[i][j] && grid[i][j]=='1'){
                    dfs(i, j, grid, visited);
                    ans++;
                }
            }
        }

        return ans;
    }
};
```

# OS ->
Page Replacement Algorithms 
1. Whenever Page Fault occurs, that is, a process tries to access a page which is not currently present in a
frame and OS must bring the page from swap-space to a frame.
2. OS must do page replacement to accommodate new page into a free frame, but there might be a possibility
the system is working in high utilization and all the frames are busy, in that case OS must replace one of the
pages allocated into some frame with the new page.
3. The page replacement algorithm decides which memory page is to be replaced. Some allocated page is
swapped out from the frame and new page is swapped into the freed frame.
4. Types of Page Replacement Algorithm: (AIM is to have minimum page faults)
a. FIFO
i. Allocate frame to the page as it comes into the memory by replacing the oldest page.
ii. Easy to implement.
iii. Performance is not always good
1. The page replaced may be an initialization module that was used long time ago
(Good replacement candidate)
2. The page may contain a heavily used variable that was initialized early and is in
content use. (Will again cause page fault)
iv. Belady’s anomaly is present.
1. In the case of LRU and optimal page replacement algorithms, it is seen that
the number of page faults will be reduced if we increase the number of
frames. However, Balady found that, In FIFO page replacement algorithm, the
number of page faults will get increased with the increment in number of
frames.
2. This is the strange behavior shown by FIFO algorithm in some of the cases.
b. Optimal page replacement
i. Find if a page that is never referenced in future. If such a page exists, replace this page
with new page.
If no such page exists, find a page that is referenced farthest in future. Replace this page
with new page.
ii. Lowest page fault rate among any algorithm.
iii. Difficult to implement as OS requires future knowledge of reference string which is
kind of impossible. (Similar to SJF scheduling)
c. Least-recently used (LRU)
i. We can use recent past as an approximation of the near future then we can replace the
page that has not been used for the longest period.
ii. Can be implemented by two ways
1. Counters
a. Associate time field with each page table entry.
b. Replace the page with smallest time value.
2. Stack
a. Keep a stack of page number.
b. Whenever page is referenced, it is removed from the stack & put on
the top.
c. By this, most recently used is always on the top, & least recently used
is always on the bottom.
d. As entries might be removed from the middle of the stack, so Doubly
linked list can be used.
d. Counting-based page replacement – Keep a counter of the number of references that have been
made to each page. (Reference counting)

i. Least frequently used (LFU)
1. Actively used pages should have a large reference count.
2. Replace page with the smallest count.
ii. Most frequently used (MFU)
1. Based on the argument that the page with the smallest count was probably just
brought in and has yet to be used.
iii. Neither MFU nor LFU replacement is common.
