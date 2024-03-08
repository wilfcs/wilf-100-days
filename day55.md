
# [1091. Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/description/)

## Approach ->
We can use bfs in this question because there is no weight attached to the graph, but since we have to find the shortest path we can also use dijkstra. 

1. Using BFS ->
Use simple BFS, and as you expand your levels keep increasing the level variable. If you reach the end return level, else return -1. Simple, right?

2. Using Dijkstra's ->
Use normal dijkstra's with weights as 1. Keep in mind that the distance vector that we used to take in normal dijkstra's used to be a 1d matrix but this question's distance vector will be a 2d matrix.

## Code ->
1. 

```cpp
class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        // Lambda function of isValid
        auto isValid = [&](int row, int col){
            // retur true if row and col are in the range and the val there is 0
            return row >= 0 && col >= 0 && row < grid.size() &&
            col < grid[0].size() && grid[row][col]==0; 
        };

        // Code for main starts here

        if (grid[0][0] == 1 || grid[grid.size() - 1][grid[0].size() - 1] == 1) {
            return -1; // Check if the start or end point is blocked
        }

        int level = 0;

        // This will store the coordinates of gird in q
        queue<pair<int, int>> q;
        q.push({0,0});

        // As we keep visiting 0s we will mark them as 1s in grid itself
        grid[0][0] = 1;

        while(q.size()){
            int n = q.size();
            // iterate for the size of q because we are dealing in levels here.
            while(n--){
                int row = q.front().first;
                int col = q.front().second;
                q.pop();
                
                // if its the bottom right index, return level + 1
                if(row==grid.size()-1 && col==grid[0].size()-1) return level+1;

                // check the validity of all 8 directions and push them in the queue
                // also mark the grid corresponding to that position as 1
                if(isValid(row+1, col)) q.push({row+1, col}), grid[row+1][col]=1;
                if(isValid(row-1, col)) q.push({row-1, col}), grid[row-1][col]=1;
                if(isValid(row, col+1)) q.push({row, col+1}), grid[row][col+1]=1;
                if(isValid(row, col-1)) q.push({row, col-1}), grid[row][col-1]=1;
                if(isValid(row+1, col+1)) q.push({row+1, col+1}), grid[row+1][col+1]=1;
                if(isValid(row-1, col-1)) q.push({row-1, col-1}), grid[row-1][col-1]=1;
                if(isValid(row+1, col-1)) q.push({row+1, col-1}), grid[row+1][col-1]=1;
                if(isValid(row-1, col+1)) q.push({row-1, col+1}), grid[row-1][col+1]=1;
            }
            // increase the level
            level++;
        }
        //  if the last node wasn't visited then return -1
        return -1;
    }
};
```

2. 

```cpp
class Solution {
public:
    typedef pair<int, pair<int, int>> P;  // Define a pair to represent distance and coordinates
    vector<vector<int>> directions{{1, 1}, {0, 1}, {1, 0}, {0, -1}, {-1, 0}, {-1, -1}, {1, -1}, {-1, 1}};  // Define possible directions to move

    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();

        if (m == 0 || n == 0 || grid[0][0] != 0)
            return -1;  // If the grid is empty or the start point is blocked, return -1

        auto isSafe = [&](int x, int y) {
            return x >= 0 && x < m && y >= 0 && y < n;  // Check if the coordinates are within the grid boundaries
        };

        vector<vector<int>> result(m, vector<int>(n, INT_MAX));  // Initialize a 2D vector to store the shortest distances
        priority_queue<P, vector<P>, greater<P>> pq;  // Priority queue to store nodes with minimum distance

        pq.push({0, {0, 0}});  // Push the start node with distance 0
        result[0][0] = 0;  // Set the distance to the start node as 0

        while (!pq.empty()) {
            int d = pq.top().first;  // Current distance
            pair<int, int> node = pq.top().second;  // Current node's coordinates
            pq.pop();  // Pop the current node from the priority queue

            int x = node.first;
            int y = node.second;

            for (auto dir : directions) {
                int x_ = x + dir[0];  // New x-coordinate
                int y_ = y + dir[1];  // New y-coordinate
                int dist = 1;  // Distance between adjacent nodes

                if (isSafe(x_, y_) && grid[x_][y_] == 0 && d + dist < result[x_][y_]) {
                    // If the new coordinates are safe, the cell is not blocked, and the new distance is smaller,
                    pq.push({d + dist, {x_, y_}});  // Push the new node with updated distance
                    grid[x_][y_] = 1;  // Mark the cell as visited
                    result[x_][y_] = d + dist;  // Update the shortest distance to the new node
                }
            }
        }

        if (result[m - 1][n - 1] == INT_MAX)
            return -1;  // If there is no valid path to the destination, return -1

        return result[m - 1][n - 1] + 1;  // Return the shortest path length to the destination
    }
};
```


```cpp

class Solution {
public:
    typedef pair<int, pair<int, int>> P;
    vector<vector<int>> dirs = {
                {-1,0},
        {0,-1},         {0,1},
                {1, 0}
    };
   // Interesting right :-) 
    
    int minimumEffortPath(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        
        auto isSafe = [&](int x, int y) {
            return x>=0 && x<m && y>=0 && y<n;
        };
        
        vector<vector<int>> result(m, vector<int>(n, INT_MAX));
        priority_queue<P, vector<P>, greater<P>> pq;
        
        pq.push({0, {0, 0}});
        result[0][0] = 0;
  
        while(!pq.empty()) {
            int diff  = pq.top().first;
            auto node = pq.top().second;
            pq.pop();

            int x = node.first;
            int y = node.second;
            
            //Why returning now ?
            //Because there is no way that the rest of elements can update the weight of destination cell even smaller due to the min heap.
            if(x == m-1 && y == n-1)
                return diff;
            
	    for(auto dir:dirs) {
		int x_   = x + dir[0];
		int y_   = y + dir[1];

		if(isSafe(x_, y_)) {

		    int newDiff = max(diff, abs(heights[x][y] - heights[x_][y_]));
		    if(result[x_][y_] > newDiff) {
			result[x_][y_] = newDiff;
			pq.push({result[x_][y_], {x_, y_}});
		    }
		}
	     }
        }
   
        return result[m-1][n-1];

    }
};
```

# [1631. Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/description/)

## Approach ->
Another impl of dijkstra's. Try solving it when you see it. Couldn't solve in the first go.

## Code ->
```cpp
class Solution {
public:
    // Define a custom pair for priority queue
    typedef pair<int, pair<int, int>> P;

    // Define the possible directions to move (up, left, right, down)
    vector<vector<int>> dirs = {
        {-1, 0},
        {0, -1},
        {0, 1},
        {1, 0}
    };

    int minimumEffortPath(vector<vector<int>>& heights) {
        // Get the number of rows and columns in the matrix
        int m = heights.size();
        int n = heights[0].size();

        // Lambda function to check if a cell is within bounds
        auto isSafe = [&](int x, int y) {
            return x >= 0 && x < m && y >= 0 && y < n;
        };

        // Initialize a matrix to store the minimum effort for each cell
        vector<vector<int>> result(m, vector<int>(n, INT_MAX));

        // Initialize a min heap priority queue
        priority_queue<P, vector<P>, greater<P>> pq;

        // Push the starting cell (0,0) with 0 effort to the priority queue
        pq.push({0, {0, 0}});
        result[0][0] = 0;

        // Continue until the priority queue is empty
        while (!pq.empty()) {
            // Extract the minimum effort and corresponding cell from the priority queue
            int diff = pq.top().first;
            auto node = pq.top().second;
            pq.pop();

            // Extract the coordinates of the cell
            int x = node.first;
            int y = node.second;

            // Check if the current cell is the destination cell (bottom-right)
            // If so, return the minimum effort
            if (x == m - 1 && y == n - 1)
                return diff;

            // Explore all possible neighbors
            for (auto dir : dirs) {
                int x_ = x + dir[0];
                int y_ = y + dir[1];

                // Check if the neighbor is within bounds
                if (isSafe(x_, y_)) {
                    // Calculate the new effort for the neighbor
                    int newDiff = max(diff, abs(heights[x][y] - heights[x_][y_]));

                    // If the new effort is smaller than the recorded effort, update and push to the priority queue
                    if (result[x_][y_] > newDiff) {
                        result[x_][y_] = newDiff;
                        pq.push({result[x_][y_], {x_, y_}});
                    }
                }
            }
        }

        // This line is reached only if the destination cell is not reached, but it should not affect the logic
        return result[m - 1][n - 1];
    }
};
```
# [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/description/)

## Approach ->
First approach after seeing the weighted graph that will come to your head will be Dijkstra's but unlike the other questions, in this question our priority of judgement will not be distance, it will be stops. We will be using simple BFS and for that we will be checking our levels. We just need a queue for that, no pq required. 
Lets prioritize paths with fewer stops over shorter distances.

Here are the steps: 

1. Create an adjacency list, a queue storing {stops, {node, dist}}, and a distance array.
2. Initialize source distance to 0 and stops to 0, push into the queue.
3. Pop from the queue, update distances if better, and push adjacent nodes with increased stops.
4. Repeat until the queue is empty. Return calculated distance if stops reach the required limit; otherwise, return -1. Use a queue instead of a priority queue for efficiency.

## Code ->
```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        // Create an adjacency list to represent the graph
        vector<vector<pair<int, int>>> adj(n);  // node -> nextNode, dist

        // Populate the adjacency list based on the given flights
        for(int i = 0; i < flights.size(); i++){
            adj[flights[i][0]].push_back({flights[i][1], flights[i][2]});
        }

        // Use a queue to implement Dijkstra's algorithm
        queue<pair<int, pair<int, int>>> q;  // stops, {node, dist}

        // Initialize distance array with maximum values
        vector<int> dist(n, INT_MAX);

        // Push the source node with initial distance and stops into the queue
        q.push({0, {src, 0}});
        dist[src] = 0;  // Distance from source to itself is zero

        // Dijkstra's algorithm
        while(!q.empty()) {
            // Dequeue the front element
            int stop = q.front().first;
            int node = q.front().second.first;
            int weight = q.front().second.second;
            q.pop();

            // Check if the number of stops exceeds the limit
            if(stop > k) continue;

            // Explore neighbors and update distances
            for(int i = 0; i < adj[node].size(); i++){
                int adjNode = adj[node][i].first;
                int edW = adj[node][i].second;

                // Update the distance if a shorter path is found
                if(weight + edW < dist[adjNode] && stop <= k){
                    dist[adjNode] = weight + edW;
                    q.push({stop + 1, {adjNode, weight + edW}});
                }
            }
        }

        // Check if destination is reachable and return the minimum cost
        if(dist[dst] != INT_MAX) return dist[dst];
        else return -1;
    }
};
```

# [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/description/)

## Code ->
```cpp
// Normal dijkstra's impl
class Solution {
public:
    typedef pair<int, int> P;
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        // Create an adjacency list to represent the graph: node -> {adjNode, weight}
        vector<vector<P>> adj(n+1);

        // Populate the adjacency list based on the given times
        for(int i=0; i<times.size(); i++){
            adj[times[i][0]].push_back({times[i][1], times[i][2]});
        }

        // Use a priority queue to implement Dijkstra's algorithm: {weight, node}
        priority_queue<P, vector<P>, greater<P>> pq;

        // Initialize distance array with maximum values
        vector<int> dist(n+1, 1e9);

        // Push the source node with initial distance 0 into the priority queue
        pq.push({0, k});
        dist[k] = 0;

        // Dijkstra's algorithm
        while(pq.size()){
            int node = pq.top().second;  // Extract node with the minimum weight
            int weight = pq.top().first;
            pq.pop();

            // Explore neighbors and update distances
            for(int i=0; i<adj[node].size(); i++){
                int adjElem = adj[node][i].first;  // Adjacent node
                int adjW = adj[node][i].second;    // Weight of the edge

                // Update the distance if a shorter path is found
                if(adjW + weight < dist[adjElem]){
                    dist[adjElem] = adjW + weight;
                    pq.push({adjW + weight, adjElem});
                }
            }
        }

        // Find the maximum distance in the distance array
        int maxElem = INT_MIN;
        for(int i=1; i<=n; i++) maxElem = max(maxElem, dist[i]);

        // Check if any nodes are unreachable, return -1; otherwise, return the maximum distance
        if(maxElem == 1e9) return -1;
        else return maxElem;
    }
};
```