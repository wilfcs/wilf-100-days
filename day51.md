
# [733. Flood Fill](https://leetcode.com/problems/flood-fill/description/)

Easy peasy

# Code ->
```cpp
class Solution {
public:
    bool isValid(vector<vector<int>>& ans, int sr, int sc, int color, int elem){
        // Check if the pixel is within the bounds of the image and has the same color as the starting pixel.
        // Also, check if the pixel is not already colored with the new color.
        if(sr<0 || sc<0 || sr>=ans.size() || sc>=ans[0].size() || ans[sr][sc]!=elem || ans[sr][sc]==color)
            return false;
        else return true;
    }
    void bfs(vector<vector<int>>& image, int sr, int sc, int color, vector<vector<int>> &ans, int elem){
        // Set the color of the current pixel to the new color.
        ans[sr][sc] = color;

        // Check and recursively call DFS on the neighboring pixels in 4 directions.
        if(isValid(ans, sr+1, sc, color, elem)) bfs(image, sr+1, sc, color, ans, elem);
        if(isValid(ans, sr-1, sc, color, elem)) bfs(image, sr-1, sc, color, ans, elem);
        if(isValid(ans, sr, sc+1, color, elem)) bfs(image, sr, sc+1, color, ans, elem);
        if(isValid(ans, sr, sc-1, color, elem)) bfs(image, sr, sc-1, color, ans, elem);
    }
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        // Create a copy of the original image.
        vector<vector<int>> ans(image.size(), vector<int>(image[0].size()));
        ans = image;

        // Start the flood fill from the specified pixel.
        bfs(image, sr, sc, color, ans, image[sr][sc]);
        return ans;
    }
};
```

# [994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges/description/)

## Approach ->
The approach here is to use Breadth-First Search (BFS) traversal to simulate the rotting process of oranges.
BFS ensures that the traversal direction is consistent: left, right, up, and down and we are able to find minutes for each traversal. That way we can find out that in exactly how much time all the oranges are rotting. Using DFS this is not possible because we are going depth wise and not into a consistent 4 direction.

```cpp


class Solution {
public:
    // Helper function to check if a given position (i, j) is a valid fresh orange.
    bool isValid(vector<vector<int>>& grid, int i, int j){
        // Check if (i, j) is within the bounds of the grid and if it represents a fresh orange (1).
        return i >= 0 && j >= 0 && i < grid.size() && j < grid[0].size() && grid[i][j] == 1;
    }
    
    // Main function to calculate the minimum minutes required to rot all oranges.
    int orangesRotting(vector<vector<int>>& grid) {
        if(grid.empty()) return 0; // Handle the case where the grid is empty.
        
        int minutes = 0, total = 0, rotten = 0, row = grid.size(), col = grid[0].size();
        queue<pair<int, int>> q; // Queue to store the indexes of rotten oranges.
        
        // Push all already rotten oranges into the queue and calculate the total number of oranges (both fresh and rotten).
        // Finding total number of oranges is crucial because at the end we will compare total oranges with total oranges we have rotten.
        // If the total oranges present and total oranges we have rotten is equal, we will return minutes taken to rot all oranges, else return -1
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                if(grid[i][j] == 2){
                    q.push({i, j});
                    total++;
                }
                else if(grid[i][j] == 1)
                    total++;
            }
        }       

        // Perform BFS to rot adjacent fresh oranges and update their status. Also, push their indexes into the queue.
        while(!q.empty()){
            int sizeOfQ = q.size();
            rotten += sizeOfQ; // Update the count of rotten oranges.

            // Rot adjacent fresh oranges and push their indexes into the queue.
            while(sizeOfQ--){
                int i = q.front().first, j = q.front().second;
                q.pop();
                
                if(isValid(grid, i + 1, j)) q.push({i + 1, j}), grid[i + 1][j] = 2;
                if(isValid(grid, i - 1, j)) q.push({i - 1, j}), grid[i - 1][j] = 2;
                if(isValid(grid, i, j + 1)) q.push({i, j + 1}), grid[i][j + 1] = 2;
                if(isValid(grid, i, j - 1)) q.push({i, j - 1}), grid[i][j - 1] = 2;
            }

            // If the queue is not empty, there are more rounds of rotting, so increase the minutes.
            if(!q.empty()) minutes++;
        }
        
        // If the total number of oranges equals the number of rotten oranges, return the minutes, else return -1.
        return total == rotten ? minutes : -1;
    }
};
```

TC -> O(m * n), SC -> O(m * n)

# [Detect cycle in an undirected graph](https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1)

## Approach 1 ->
We will detect cycle using BFS.

The idea is simple, suppose this is a graph we are given: 

1 -- 2
|    |
3 -- 4

When we start checking from 1 using BFS we push 1 in queue and see in the adj list that it connects to 2 and 3 and 2 and 3 are non visited so we mark them as visited and push 2 and 3 in our queue. 

Now we check for 2 from our queue. 2 is connected to 4 and 1. 4 is not visited so we mark 4 as visited and push in our queue. 1 is visited but 1 is also the parent of 2 so because of the parent fact we ignore 1.

Now we check for 3 from our queue. 3 is connected to 1 and 3. 1 is visited but it is the parent of 3 so we ignore 1. 4 is also visited but its not the parent of 3. How can 4 be visited already and not be the parent? That clearly means that there is a cycle in our graph. So if someone was visited and it is not the parent then we have a cycle. 

We also have to keep in mind that there can be multiple provinces so run a loop for all the nodes in the main function and if they are not visited then call the isLoop/bfs function to check if loop exist.

## Code 1 ->
```cpp
class Solution {
private:
    // Helper function to perform BFS traversal and detect cycle
    bool isCycle(vector<int> adj[], vector<int> &visited, int elem) {
        visited[elem] = 1;
        queue<pair<int, int>> q;
        // Pushing the element and its parent into the queue
        q.push({elem, -1});

        while (q.size()) {
            elem = q.front().first;
            int parent = q.front().second;
            q.pop();

            // checking adj list of element
            for (int i = 0; i < adj[elem].size(); i++) {
                // If the neighbor is visited and not the parent, then there is a cycle
                if (visited[adj[elem][i]] && adj[elem][i] != parent)
                    return true;
                // If the neighbor is already visited and parent, continue to the next neighbor
                else if (visited[adj[elem][i]])
                    continue;

                // Push the neighbor and its parent (the element itself) into the queue and mark it as visited
                q.push({adj[elem][i], elem});
                visited[adj[elem][i]] = 1;
            }
        }
        return false;
    }

public:
    // Function to detect cycle in an undirected graph
    bool isCycle(int V, vector<int> adj[]) {
        vector<int> visited(V, 0);

        // Iterate through each vertex and check for cycles if they aren't visited ofcourse
        for (int i = 0; i < V; i++) {
            if (!visited[i])
                if (isCycle(adj, visited, i)) return true;
        }
        return false;
    }
};
```

Time Complexity: O(N + 2E) + O(N), Where N = Nodes, 2E is for total degrees as we traverse all adjacent nodes. In the case of connected components of a graph, it will take another O(N) time.

Space Complexity: O(N) + O(N) ~ O(N), Space for queue data structure and visited array.

# Approach 2 ->
Using DFS.

The approach is almost the same. We pass the parent of the elem too in the function call and we check for its adjacent elements. If the neighbour is visited and its not the parent we return true (i.e. cycle present), else we make the recursive call. If at any point our recursive call is returning true we finally return true.

## Code ->
```cpp
class Solution {
private:
    // Function to check for cycles using DFS.
    bool isCycle(vector<int> adj[], vector<int> &visited, int elem, int parent) {
        // Mark the current element as visited.
        visited[elem] = 1;

        // Iterate through the adjacent vertices.
        for (int i = 0; i < adj[elem].size(); i++) {
            // Check if the adjacent vertex is visited and not the parent.
            if (visited[adj[elem][i]] && adj[elem][i] != parent) {
                return true;  // Cycle detected.
            } else if (visited[adj[elem][i]]) {
                continue;  // Skip if already visited.
            }

            // Recursively call the function for the adjacent vertex.
            if (isCycle(adj, visited, adj[elem][i], elem)) {
                return true;  // if the recursive call is returning true, return true finally and break the recursive calls.
            }
        }

        return false;  // No cycle detected for the current vertex.
    }

public:
    // Function to detect cycle in an undirected graph.
    bool isCycle(int V, vector<int> adj[]) {
        vector<int> visited(V, 0);  // Initialize visited vector.

        // Iterate through each vertex.
        for (int i = 0; i < V; i++) {
            // Check if the vertex is unvisited.
            if (!visited[i]) {
                // Call the recursive DFS function to check for cycles.
                if (isCycle(adj, visited, i, -1)) {
                    return true;  // Cycle detected in the graph.
                }
            }
        }

        return false;  // No cycles found in the entire graph.
    }
};
```
Time Complexity: O(N + 2E) + O(N), Where N = Nodes, 2E is for total degrees as we traverse all adjacent nodes. In the case of connected components of a graph, it will take another O(N) time.

Space Complexity: O(N) + O(N) ~ O(N), Space for recursive stack space and visited array.

# [542. 01 Matrix](https://leetcode.com/problems/01-matrix/description/)

## Approach ->

Google asked this question so obviously hitting bfs from each cell when you find a 1 is not the approach its looking for, added that it will give TLE. So let's think of a better approach.

The intuition behind this solution is to use a multi-source BFS approach where you start from the positions of '0' and propagate outward to compute the distance of each cell from the nearest '0'. This is more efficient than running BFS from each '1' cell individually.

Here's a step-by-step explanation of the intuition:

1. Initialization:

Initialize a matrix (ans) to store the distances. Initialize it with -1, indicating that the distances are not yet computed.
Create a queue (q) to perform BFS.

2. Enqueue '0' Cells:

Iterate through the input matrix (mat).
Whenever you encounter a '0', enqueue its position into the queue (q) and set its distance in the ans matrix to 0.

3. BFS:

Start the BFS process by dequeuing a cell from the queue.

For the dequeued cell, check its neighboring cells:
-  If a neighboring cell is valid (within matrix boundaries and not visited yet), enqueue it.
-  Set the distance of the neighboring cell in the ans matrix to the current cell's distance plus 1.

4. Repeat BFS:

Continue the BFS process until the queue is empty.
The BFS will propagate from the '0' cells to their neighboring cells, updating the distances along the way.

5. Result:

The ans matrix will now contain the distances of each cell from the nearest '0'.


## Code -> 
```cpp
class Solution {
private:
    // Function to check if a cell is valid and has not been visited yet
    bool isValid(vector<vector<int>> &ans, int i, int j){
        if(i < 0 || j < 0 || i >= ans.size() || j >= ans[0].size() || ans[i][j] != -1)
            return false;
        else
            return true;
    }

public:
    // Function to update the matrix with distances from nearest 0
    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        // Initialize the answer matrix with -1 (unvisited) and a queue for BFS
        vector<vector<int>> ans(mat.size(), vector<int>(mat[0].size(), -1));
        queue<pair<int, int>> q;

        // Push the positions of '0' into the queue and set their distance to 0
        for(int i = 0; i < mat.size(); i++){
            for(int j = 0; j < mat[0].size(); j++){
                if(mat[i][j] == 0){
                    q.push({i, j});
                    ans[i][j] = 0;
                }
            }
        }

        // Perform BFS to update distances for other cells
        while(q.size()){
            int i = q.front().first;
            int j = q.front().second;

            q.pop();

            // Check and update distances for valid neighboring cells by simple if valid conditions
            // Could have used a while(size--) loop and checked isValid for each level but that that will give TLE
            // and there is no need also because we don't have anything to do with the levels specifically
            if(isValid(ans, i + 1, j)){
                q.push({i + 1, j});
                ans[i + 1][j] = ans[i][j] + 1;
            }
            if(isValid(ans, i - 1, j)){
                q.push({i - 1, j});
                ans[i - 1][j] = ans[i][j] + 1;
            }
            if(isValid(ans, i, j + 1)){
                q.push({i, j + 1});
                ans[i][j + 1] = ans[i][j] + 1;
            }
            if(isValid(ans, i, j - 1)){
                q.push({i, j - 1});
                ans[i][j - 1] = ans[i][j] + 1;
            }
        }

        return ans;
    }
};
```

# [130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/description/)

## Approach ->

This is a very simple question if you are able to observe one thing. Think about it.

The only observation here is that the 0s that are on the boundary and 0s connected to the boundary 0s, all those 0s can never become X. All the other 0s can. So simply traverse the boundary and make every 0 as B and also the 0s connected to them as B recursively. When that is done, traverse the matrix and convert Bs to 0s and rest everything to X

## Code ->
```cpp
class Solution {
public:
    bool isValid(vector<vector<char>>& board, int i, int j){
        if(i>=0 && i<board.size() && j>=0 && j<board[0].size() && board[i][j]=='O')
            return true;
        return false;
    }
    void makeB(vector<vector<char>>& board, int i, int j){
        // make the current element B because it is a valid element i.e. lies in the range of the 2d array and also is a 'O'.
        board[i][j] = 'B';
        
        // check if the left, right, up and down elems are valid and if yes then call the function recursively.
        if(isValid(board, i+1, j))
            makeB(board, i+1, j);
        if(isValid(board, i-1, j))
            makeB(board, i-1, j);
        if(isValid(board, i, j+1))
            makeB(board, i, j+1);
        if(isValid(board, i, j-1))
            makeB(board, i, j-1);
    }
    void solve(vector<vector<char>>& board) {
        int row = board.size();
        int col = board[0].size();
        
        // Calling makeB for every corner elements of the 2d array
        // Make the cornor 'O's as 'B' and the 'O's connected to them as 'B' as well
        for(int i=0; i<row; i++){
            if(board[i][0] == 'O') makeB(board, i, 0);
            if(board[i][col-1] == 'O') makeB(board, i, col-1);
        }
        for(int i=0; i<col; i++){
            if(board[0][i] == 'O') makeB(board, 0, i);
            if(board[row-1][i] == 'O') makeB(board, row-1, i);
        }
        
        
        // Changing all the 'B' in the board to 'O'
        // Keep everything else as 'X'
        for(int i=0; i<row; i++)
            for(int j=0; j<col; j++)
                if(board[i][j] == 'B') board[i][j] = 'O';
                else board[i][j] = 'X';
    }
};
```

# [1020. Number of Enclaves](https://leetcode.com/problems/number-of-enclaves/description/)

## Approach ->
Exactly as same as the last one

## Code ->
```cpp
class Solution {
public:
    // Function to check if a cell is a valid land cell
    bool isValid(vector<vector<int>>& grid, int i, int j) {
        // Check if the indices are within the grid boundaries and the cell is land
        if (i >= 0 && j >= 0 && i < grid.size() && j < grid[0].size() && grid[i][j] == 1)
            return true;
        else
            return false;
    }

    // Function to mark connected land cells as water (set to 0)
    void makeZero(vector<vector<int>>& grid, int i, int j) {
        grid[i][j] = 0; // Mark the current land cell as water

        // Recursively mark neighboring land cells as water
        if (isValid(grid, i + 1, j)) makeZero(grid, i + 1, j);
        if (isValid(grid, i - 1, j)) makeZero(grid, i - 1, j);
        if (isValid(grid, i, j + 1)) makeZero(grid, i, j + 1);
        if (isValid(grid, i, j - 1)) makeZero(grid, i, j - 1);
    }

    // Main function to count the number of land cells that cannot reach the boundary
    int numEnclaves(vector<vector<int>>& grid) {
        int row = grid.size(), col = grid[0].size();

        // Mark connected land cells as water for the left and right boundaries
        for (int i = 0; i < row; i++) {
            if (grid[i][0] == 1) makeZero(grid, i, 0);
            if (grid[i][col - 1] == 1) makeZero(grid, i, col - 1);
        }

        // Mark connected land cells as water for the top and bottom boundaries
        for (int i = 0; i < col; i++) {
            if (grid[0][i] == 1) makeZero(grid, 0, i);
            if (grid[row - 1][i] == 1) makeZero(grid, row - 1, i);
        }

        int ans = 0;
        // Count the remaining land cells (not reachable from the boundary)
        for (int i = 0; i < row; i++)
            for (int j = 0; j < col; j++)
                if (grid[i][j] == 1) ans++;

        return ans;
    }
};
```