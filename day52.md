
# [785. Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/description/)

## Question Explanation ->
Problem Statement: Given an adjacency list of a graph adj of V no. of vertices having 0 based index. Check whether the graph is bipartite or not.

If we are able to colour a graph with two colours such that no adjacent nodes have the same colour, it is called a bipartite graph.

## Example ->
Input 1:

![Input 1](images/bpGraph1.png)
![Output 1](images/bpGraph2.png)

Output: True


Input 2:

![Input 2](images/bpGraph3.png)
![Output 2](images/bpGraph4.png)

Output: False

## Approaches ->
1. BFS:

 This code checks if a graph is bipartite, meaning it can be split into two independent sets of nodes, ensuring that no two connected nodes share the same color. First thing to notice here is that the graph might be disconnected. It handles disconnected graphs by iterating through each unprocessed node and running a breadth-first search (BFS) for each component.

The approach involves using a visited vector to mark nodes as uncolored (-1). During BFS, nodes are colored alternatively (1 or 2). If, at any point, a node's neighbor has the same color as itself and the neighbour is not the parent of the node, the graph is not bipartite.

## Code ->
```cpp
class Solution {
public:
    // BFS function to check if a connected component is bipartite
    bool bfs(vector<vector<int>> &graph, vector<int> &visited, int i) {
        queue<pair<int, int>> q;
        q.push({i, 1}); // Start with node i, color 1
        visited[i] = 1; // Mark the current node as color 1

        while (q.size()) {
            int curr = q.front().first;
            int color = q.front().second;
            q.pop();

            for (int i = 0; i < graph[curr].size(); i++) {
                int neighbour = graph[curr][i]; // Extract the neighbour/child of the parent

                if (neighbour == curr) continue; // Skip parent node

                if (visited[neighbour] == -1) {
                    // If neighbour is not visited, color it with the opposite color in visited vector
                    int newColor = (color == 1) ? 2 : 1;
                    visited[neighbour] = newColor;

                    // Push the neighbour in queue with its corresponding color
                    q.push({neighbour, newColor});
                } else {
                    // If the neighbour has the same color as the current node, graph is not bipartite
                    if (visited[neighbour] == color) return false;
                }
            }
        }

        return true; // The connected component is bipartite
    }

    // Function to check if the entire graph is bipartite (handles disconnected graphs)
    bool isBipartite(vector<vector<int>> &graph) {
        vector<int> visited(graph.size(), -1); // Initialize visited vector

        // Iterate through each node in the graph
        for (int i = 0; i < graph.size(); i++) {
            // Check if the node is not already processed and run BFS for each unprocessed node
            if (visited[i] == -1 && !bfs(graph, visited, i)) return false;
        }

        return true; // The entire graph is bipartite
    }
};
```

2. DFS:

## Code ->
```cpp
class Solution {
private: 
    bool dfs(int node, int col, int color[], vector<int> adj[]) {
        color[node] = col; 
        
        // traverse adjacent nodes
        for(auto it : adj[node]) {
            // if uncoloured
            if(color[it] == -1) {
                if(dfs(it, !col, color, adj) == false) return false; 
            }
            // if previously coloured and have the same colour
            else if(color[it] == col) {
                return false; 
            }
        }
        
        return true; 
    }
public:
	bool isBipartite(int V, vector<int>adj[]){
	    int color[V];
	    for(int i = 0;i<V;i++) color[i] = -1; 
	    
	    // for connected components
	    for(int i = 0;i<V;i++) {
	        if(color[i] == -1) {
	            if(dfs(i, 0, color, adj) == false) 
	                return false; 
	        }
	    }
	    return true; 
	}
};
```
# [Detect cycle in a directed graph (Using DFS)](https://www.geeksforgeeks.org/problems/detect-cycle-in-a-directed-graph/1)
## Approach ->

![Example](images/directedGraphCycle.webp)

In the directed graph shown above, let's try to figure out if the normal cycle detection algo of a undirected graph work or not.  The undirected dfs algo won't work because look at the example, if you traverse in this graph using the directed graph DFS algo then it will detect the loop at 3,4,5,7 but if you carefully observe there is no loop because of the directions of the edges. But there is a loop at 8,9 and 10. 

In the directed graph cycle detection algorithm, we use two vectors: one to mark visited nodes and another to track nodes visited in the current path. As we traverse the graph, if we encounter a node that is already in the current path, it suggests a cycle. To prevent false positives, we reset the path vector to 0 when we complete the current path. This strategy ensures accurate cycle detection in directed graphs, addressing the limitations of the undirected cycle detection algorithm.

Look at the code to understand properly...

## Code ->
```cpp
class Solution {
public:
    // Function to detect cycle in a directed graph using DFS.
    bool dfs(vector<int> adj[], vector<int> &visited, vector<int> &pathVisited, int node, int V) {
        visited[node] = 1;          // Mark the current node as visited
        pathVisited[node] = 1;      // Mark the current node as visited in the current DFS path

        // Iterate through the neighbors of the current node
        for (int i = 0; i < adj[node].size(); i++) {
            // If the neighbor is not visited, recursively call DFS for that neighbor
            if (!visited[adj[node][i]]) {
                if (dfs(adj, visited, pathVisited, adj[node][i], V)) return true; // break the recursion and return true if dfs return true
            }
            // If the neighbor is already visited in the current DFS path, a cycle is detected
            else {
                if (pathVisited[adj[node][i]]) return true;
            }
        }

        pathVisited[node] = 0;  // Reset pathVisited for the current node after exploration
        return false;           // No cycle found in the current DFS path
    }

    // Function to check if a directed graph contains a cycle.
    bool isCyclic(int V, vector<int> adj[]) {
        vector<int> visited(V, 0);       // Array to track visited nodes
        vector<int> pathVisited(V, 0);   // Array to track visited nodes in the current DFS path

        // Iterate through each node in the graph
        for (int i = 0; i < V; i++) {
            // If the node is not visited, start DFS from that node
            if (!visited[i]) {
                if (dfs(adj, visited, pathVisited, i, V)) return true;  // If cycle found, return true
            }
        }

        return false;  // No cycle found in the entire graph
    }
};
```

# [802. Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/description/)

## Approaches ->
1. 
In this q we are simply checking for the cycle. Because if a node is safe then it must eventually stop at a terminal node. And if a node is not safe then ofcouse it will not stop at any node. A terminal node is the node which points to no other node. Hence if we do not detect a cycle for a given node, that node is safe. To check cycle we have applied dfs cycle detection technique. And we are checking the cycle for each nodes seperately. TC-> O(N^2)

## Code ->
```cpp
class Solution {
public:
    bool detectCycle(int i, vector<vector<int>> &adj, vector<int> &visited, vector<int> &curVisited){
        visited[i] = 1;
        curVisited[i] = 1;
        
        for(auto a: adj[i]){
            if(!visited[a]){
                if(detectCycle(a, adj, visited, curVisited)) return true;
            }
            else if(curVisited[a]) return true;
        }
        
        curVisited[i] = 0;
        return false;
    }
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        vector <int> ans;
        vector <int> visited(graph.size(), 0);
        vector <int> curVisited(graph.size(), 0);
        
        for(int i=0; i<graph.size(); i++){
            if(!detectCycle(i, graph, visited, curVisited)) ans.push_back(i);
        }
        
        return ans;
    }
};
```

2. We can improve our TC to O(N + 2E) by simply obversving the following: 
If we run the dfs for only the non visited nodes and we keep marking the visited nodes as 1, we can save a lot of time. We can do this because we are also maintaining curVisited array. CurVisited is always eventually 0 for the nodes that do not have cycle in it but is 1 for the path/nodes that have cycle. We can take advantage of that and write our code the following way ->

## Code ->
```cpp
class Solution {
public:
    // Same as before
    bool detectCycle(int i, vector<vector<int>> &adj, vector<int> &visited, vector<int> &curVisited){
        visited[i] = 1;
        curVisited[i] = 1;
        
        for(auto a: adj[i]){
            if(!visited[a]){
                if(detectCycle(a, adj, visited, curVisited)) return true;
            }
            else if(curVisited[a]) return true;
        }
        
        curVisited[i] = 0;
        return false;
    }
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        vector <int> ans;
        vector <int> visited(graph.size(), 0);
        vector <int> curVisited(graph.size(), 0);
        
        // if not visited then call the cycle detection dfs
        for(int i=0; i<graph.size(); i++){
            if(!visited[i])
                detectCycle(i, graph, visited, curVisited);
        }

        // the curVisited will always be 1 for the path that had a cycle
        // so it will obviously be 0 for the safe paths/nodes
        for(int i=0; i<curVisited.size(); i++){
            // simply push the nodes that has 0 in their curVisited array into our ans
            if(curVisited[i]==0) ans.push_back(i);
        }
        
        return ans;
    }
};
```

# [Topological sort - Kahn’s Algorithm/BFS](https://www.geeksforgeeks.org/problems/topological-sort/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=bottom_sticky_on_article)

## Approach ->
The code aims to perform a topological sort on a directed acyclic graph (DAG)

Topological Sort:

An ordering of the vertices in a directed graph where each directed edge goes from a vertex earlier in the order to a vertex later in the order. In simple words the algo states that the vertex that is the least dependent (i.e. the node that has the least indegree aka least number of incoming edges) will be printed first in the topological sort algo and the vertex that are dependent on other vertices, are printed later.

Directed Acyclic Graph (DAG):

A directed graph without cycles, meaning there are no closed loops in the relationships between vertices. This is important for a topological sort.

So we will take the following approach to solve this using Kahn's algo aka BFS:

1. We will make an indegree vector that will store the indegree of all the vertices present in the graph.  Each element at index i represents the in-degree of vertex i.
2. Create an empty queue (q). And for each node that had no dependency i.e. indegree == 0, push it in our queue.
3. Perform simple bfs and when we pop out an element/node from queue, push it in our ans. Then for all the adjacent elements of our node just keep reducing its indegree. 
4. When the indegree of an element is reduced to 0 then push it in our queue. Look at the code for further understanding...

## Code ->
```cpp
class Solution
{
	public:
	//Function to return list containing vertices in Topological order. 
	vector<int> topoSort(int V, vector<int> adj[]) 
	{
	    vector<int> ans;
        // Step 1: Create an indegree vector and calculate in-degrees for each element
        // Push it in our indegree vector
	    vector<int> indegree(V, 0);
	    for(int i=0; i<V; i++){
	        int size = adj[i].size();
	        for(int j=0; j<size; j++){
	            indegree[adj[i][j]]++;  
	        } 
	    }
	    
	   // Step 2: Create a queue and initially push all vertices with in-degree 0
	    queue<int> q;
	    for(int i=0; i<V; i++){
	        if(indegree[i]==0) q.push(i);
	    }
	    
	   // Step 3: Perform BFS (while the queue is not empty)
	    while(q.size()){
	        int curr = q.front();  // Take the front node from the queue
            q.pop();
            ans.push_back(curr);  // Add it to the topological order

            // Iterate through the adjacent vertices of the current node
            for(int i = 0; i < adj[curr].size(); i++){
                indegree[adj[curr][i]]--;  // Reduce in-degree of adjacent vertices

                // If the new in-degree becomes 0, push the vertex to the queue
                if(indegree[adj[curr][i]] == 0) 
                    q.push(adj[curr][i]);
            }
        }
	    
	    return ans;
	}
};
```

Time Complexity: O(V+E), where V = no. of nodes and E = no. of edges. This is a simple BFS algorithm.

Space Complexity: O(N) + O(N) ~ O(2N), O(N) for the indegree array, and O(N) for the queue data structure used in BFS(where N = no.of nodes).

# [Topological sort - Using DFS](https://www.geeksforgeeks.org/problems/topological-sort/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=bottom_sticky_on_article)

## Approach ->
We will be solving it using the DFS traversal technique. DFS goes in-depth, i.e., traverses all nodes by going ahead, and when there are no further nodes to traverse in the current path, then it backtracks on the same path and traverses other unvisited nodes. The intuition is that as we keep going the depthts of the tree, we will eventually reach a node which is the most dependant, so push it in our stack. And as we backtrack, we reach the nodes that are less dependant so keep pushing them and we'll have our dependant nodes in sorted order of their dependancy

The algorithm steps are as follows:

1. We must traverse all components of the graph.
2. Make sure to carry a visited array(all elements are initialized to 0) and a stack data structure, where we are going to store the nodes after completing the DFS call.
3. In the DFS call, first, the current node is marked as visited. Then DFS call is made for all its adjacent nodes.
4. After visiting all its adjacent nodes, DFS will backtrack to the previous node and meanwhile, the current node is pushed into the stack.
5. Finally, we will get the stack containing one of the topological sortings of the graph. We can use an array instead of stack as well. In the code I have used an array and then reversed it at the end.

## Code ->
```cpp
class Solution {
public:
    // Helper function for Depth-First Search (DFS)
    void DFS(int i, vector<int>& ans, vector<int>& visited, vector<int> adj[]) {
        visited[i] = 1;  // Step 3: Mark the current node as visited
        for (auto neighbor : adj[i]) {
            if (visited[neighbor] == 0) {
                // Step 3: Recursively call DFS for unvisited neighbors
                DFS(neighbor, ans, visited, adj);
            }
        }
        ans.push_back(i);  // Step 4: Push the current node into the result after visiting all neighbors
    }

    // Function to return a list containing vertices in Topological order.
    vector<int> topoSort(int V, vector<int> adj[]) {
        vector<int> ans;      // Vector to store the topological order
        vector<int> visited(V, 0);  // Step 2: Array to keep track of visited nodes

        // Step 1: Iterate through all nodes in the graph
        for (int i = 0; i < V; i++) {
            if (visited[i] == 0) {
                // Step 2: Call DFS for unvisited nodes
                DFS(i, ans, visited, adj);
            }
        }

        // Step 5: Reverse the order to get the final topological sort
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

# [Detect cycle in a directed graph (USING BFS)](https://www.geeksforgeeks.org/problems/detect-cycle-in-a-directed-graph/1)

## Approach ->
We have already solved this using DFS. Now we will solve it using BFS. This is an implementation of Kahn's algo.

Simply run the Kahn's algo and we know for a fact that Kahn's algo only runs (topo sort only runs) for DAG (Directed Asyclic Graph). So if there is a cycle then it will not run for all its nodes. 

## Code ->
```cpp
class Solution {
  public:
    // Function to detect cycle in a directed graph.
    bool isCyclic(int V, vector<int> adj[]) {
        // code here
         vector<int> indegree(V, 0);
	    
	    for(int i=0; i<V; i++){
	        int size = adj[i].size();
	        for(int j=0; j<size; j++){
	            indegree[adj[i][j]]++;  
	        } 
	    }
	    
	    queue<int> q;
	    int cnt = 0;
	    
	    for(int i=0; i<V; i++){
	        if(indegree[i]==0) q.push(i);
	    }
	    
	    while(q.size()){
	        int curr = q.front(); 
	        q.pop();
	        // increase the count if we find an element in our queue
	        cnt++;
	        
	        for(int i=0; i<adj[curr].size(); i++){
	            indegree[adj[curr][i]]--;
	            if(indegree[adj[curr][i]] == 0) q.push(adj[curr][i]);
	        }
	    }
	    
	    // if the number of elements processed are equal to no. of nodes then no loop present
	    return cnt==V ? false : true;
    }
};
```

# [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/description/) && [207. Course Schedule](https://leetcode.com/problems/course-schedule/description/)

## Approach for Course Schedule II->
The solutions will be similar for both questions as we need to check for one, and in the other, we need to print the order. The questions state that the given pairs signify the dependencies of tasks. For example, the pair {u, v} signifies that to perform task v, first we need to finish task u. Now, if we closely observe, we can think of a directed edge between u and v(u -> v) where u and v are two nodes. Now, if we can think of each task as a node and every pair as a directed edge between those two nodes, the whole problem becomes a graph problem of topological sort. We clearly have dependencies here so it's a normal topo sort problem.

## Code ->
```cpp
class Solution {
public:
    // Helper function to create an adjacency list from prerequisites
    void makeAdj(vector<vector<int>> &adj, vector<vector<int>> &prerequisites) {
        for (int i = 0; i < prerequisites.size(); i++) {
            adj[prerequisites[i][0]].push_back(prerequisites[i][1]);
        }
    }

    // Main function to find the order of courses to be taken
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> adj(numCourses);
        makeAdj(adj, prerequisites);  // Step 1: Create an adjacency list

        vector<int> indegree(numCourses, 0);  // Step 2: Calculate indegree for each course

        for (int i = 0; i < prerequisites.size(); i++) {
            indegree[prerequisites[i][1]]++;
        }

        queue<int> q;
        vector<int> ans;

        // Step 3: Push courses with indegree 0 into the queue
        for (int i = 0; i < indegree.size(); i++) {
            if (indegree[i] == 0) q.push(i);
        }

        // Step 4: Perform topological sort using BFS
        while (q.size()) {
            int curr = q.front();
            q.pop();
            ans.push_back(curr);

            // Reduce indegree of adjacent courses and push those with indegree 0
            for (int i = 0; i < adj[curr].size(); i++) {
                indegree[adj[curr][i]]--;
                if (indegree[adj[curr][i]] == 0) q.push(adj[curr][i]);
            }
        }

        // Step 5: Check if all courses can be completed, otherwise return an empty vector
        if (ans.size() != numCourses) return {};
        
        reverse(ans.begin(), ans.end());  // Reverse the order to get the final result
        return ans;
    }
};
```

# [Alien Dictionary](https://www.geeksforgeeks.org/problems/alien-dictionary/1)

## Approach ->
The problem involves finding the order of characters in an alien language.
By analyzing consecutive pairs of words, we can determine the order of characters that appear before others.
This analysis can be modeled as a directed graph where each node represents a character, and edges indicate their order.
The goal is to find a linear ordering of characters using topological sort.


1. **Problem Overview:**
   - Given a sorted dictionary of an alien language.
   - Need to find the order of characters in the language.

2. **Observation:**
   - Analyzing pairs of consecutive words helps us determine character order.

3. **Modeling as a Graph:**
   - Consider each character as a node in a directed graph.
   - Edges indicate the order of characters.

4. **Creating the Graph:**
   - Iterate through consecutive word pairs.
   - Find the first differing character and create a directed edge.
   - Form an adjacency list for the graph.

5. **Graph Analysis:**
   - The problem now resembles finding a linear ordering of nodes (characters) using topological sort.

6. **Steps for Solution:**
   - Calculate the indegree of each node (count of incoming edges).
   - Use BFS for topological sorting.
   - Result is a linear order of characters.

7. **Final Result:**
   - Convert the ordered nodes to characters to get the language order.

## Code ->

```cpp
class Solution {
public:
    vector<int> topoSort(int V, vector<int> adj[]) {
        int indegree[V] = {0};
        // Calculate indegree for each node
        for (int i = 0; i < V; i++) {
            for (auto it : adj[i]) {
                indegree[it]++;
            }
        }

        queue<int> q;
        // Enqueue nodes with indegree 0 to start BFS
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                q.push(i);
            }
        }
        vector<int> topo;
        while (!q.empty()) {
            int node = q.front();
            q.pop();
            topo.push_back(node);

            // Reduce indegree of adjacent nodes
            for (auto it : adj[node]) {
                indegree[it]--;
                if (indegree[it] == 0) q.push(it);
            }
        }

        return topo;
    }

    string findOrder(string dict[], int N, int K) {
        vector<int> adj[K];
        
        // Create adjacency list based on word comparisons
        for (int i = 0; i < N - 1; i++) {
            string s1 = dict[i];
            string s2 = dict[i + 1];
            int len = min(s1.size(), s2.size());
            
            // Find the first differing character and create a directed edge
            for (int ptr = 0; ptr < len; ptr++) {
                if (s1[ptr] != s2[ptr]) {
                    adj[s1[ptr] - 'a'].push_back(s2[ptr] - 'a'); // word on top has more priority
                    break;
                }
            }
        }

        // Perform topological sorting
        vector<int> topo = topoSort(K, adj);
        string ans = "";
        
        // Convert ordered nodes to characters
        for (auto it : topo) {
            ans = ans + char(it + 'a');
        }
        
        return ans;
    }
};
```

**Explanation:**

- The problem involves understanding the order of characters in an alien language.
- We represent characters as nodes in a graph and analyze consecutive words to create directed edges.
- Topological sorting helps find the linear order of characters, and the result is converted to the final language order.