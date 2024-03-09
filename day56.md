

# [Distance from the Source (Bellman-Ford Algorithm)](https://www.geeksforgeeks.org/problems/distance-from-the-source-bellman-ford-algorithm/1)

## Approach ->
The bellman-Ford algorithm helps to find the shortest distance from the source node to all other nodes. But, we have already learned Dijkstra’s algorithm (Dijkstra’s algorithm article link) to fulfill the same purpose. Now, the question is how this algorithm is different from Dijkstra’s algorithm.

While learning Dijkstra’s algorithm, we came across the following two situations, where Dijkstra’s algorithm failed:

If the graph contains negative edges.
If the graph has a negative cycle (In this case Dijkstra’s algorithm fails to minimize the distance, keeps on running, and goes into an infinite loop. As a result it gives TLE error).
Negative Cycle: A cycle is called a negative cycle if the sum of all its weights becomes negative. 

Bellman-Ford’s algorithm successfully solves these problems. It works fine with negative edges as well as it is able to detect if the graph contains a negative cycle. But this algorithm is only applicable for directed graphs. In order to apply this algorithm to an undirected graph, we just need to convert the undirected edges into directed edges like the following:

![Diagram](images/bellmanFord.webp)

In this algorithm, the edges can be given in any order. The intuition is to relax all the edges for N-1( N = no. of nodes) times sequentially. After N-1 iterations, we should have minimized the distance to every node.

## Code ->
```cpp
class Solution {
public:
    /* Function to implement Bellman Ford
    * edges: vector of vectors which represents the graph
    * S: source vertex to start traversing the graph with
    * V: number of vertices
    */
    vector<int> bellman_ford(int V, vector<vector<int>>& edges, int S) {
        // Initialize distance array to store the shortest distances from the source
        vector<int> dist(V, 1e8); // 1e8 represents infinity, as it is greater than any possible distance
        dist[S] = 0; // Distance from the source to itself is always 0

        // Relax edges for V-1 iterations
        for (int i = 0; i < V - 1; i++) {
            // Iterate through all edges
            for (auto e : edges) {
                int node = e[0];
                int adjNode = e[1];
                int weight = e[2];

                // Relaxation step: Update the distance if a shorter path is found
                if (dist[node] != 1e8 && dist[node] + weight < dist[adjNode]) {
                    dist[adjNode] = dist[node] + weight;
                }
            }
        }

        // Nth relaxation to check for negative cycles
        for (auto e : edges) {
            int node = e[0];
            int adjNode = e[1];
            int weight = e[2];

            // If further relaxation is possible, there's a negative cycle
            if (dist[node] != 1e8 && dist[node] + weight < dist[adjNode]) {
                return {-1}; // Negative cycle detected, return a special value
            }
        }

        return dist; // Return the shortest distances array
    }
};
```

# [Floyd Warshall](https://www.geeksforgeeks.org/problems/implementing-floyd-warshall2042/1)

## Approach ->
[Read Article](https://takeuforward.org/data-structure/floyd-warshall-algorithm-g-42/)

Floyd Warshall will take the Tc of O(N^3). But it will solve the problem even if there is a negative cyclic (it will be able to detect cyclic loop if to go from point A to point A itself the distance is in negative). But if there is no negative cycle then you can apply dijkstra's and this problem will be solved in Tc of O(N * E * log N).

## Code ->
```cpp
class Solution {
  public:
	void shortest_distance(vector<vector<int>>&matrix){
	    // Code here
	    int n = matrix.size();
	    
	    // ATQ if we can't reach a node from another node then the weight is given as -1
	    // Let's convert that to INT_MAX for ease of calculations.
	    for(int i=0; i<n; i++)
	        for(int j=0; j<n; j++)
	            if(matrix[i][j]==-1) matrix[i][j] = 1e8;
	            
	   
	   // Floyd Warshall
	   for(int k=0; k<n; k++)
	       for(int i=0; i<n; i++)
	           for(int j=0; j<n; j++)
	               // Update the distance matrix with the shortest path from i to j through k
	               matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j]);
    
	   
	   // Convert back the INT_MAX that we did in the start to -1 again
	   for(int i=0; i<matrix.size(); i++)
	        for(int j=0; j<matrix[0].size(); j++)
	            if(matrix[i][j]==1e8) matrix[i][j] = -1;
	}
};
```

# [1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/description/)

## Approach ->
The question might sound confusing but lool at the first example given in the q to understand it. Basically we have to find all the nodes we can go to from all the nodes that is present and if let's say we can go from node a to nodes b,c and d within the threshold value given then we know we can go to 3 nodes from a. We have to return the greatest node value that has the least number of nodes we can visit. Look at the example to understand the problem.

In order to solve this problem, we will use the Floyd Warshall algorithm.

We know Floyd Warshall algorithm helps us to generate a 2D matrix, that stores the shortest distances from each node to every other node. In the generated 2D matrix, each cell matrix[i][j] represents the shortest distance from node i to node j.

After generating the 2D matrix(that contains the shortest paths) using the Floyd Warshall algorithm, for each node, we will count the number of nodes with a distance lesser or equal to the distanceThreshold by iterating each row of that matrix. Finally, we will choose the node with the minimum number of adjacent cities(whose distance is at the most distanceThreshold) and with the largest value.

Note: This 2D matrix can also be generated using Dijkstra’s algorithm. As Dijkstra’s algorithm is a single-source shortest-path algorithm, we need to calculate the shortest distances for one single node at a time. So, to create the 2D matrix we need to apply Dijkstra’s algorithm to each of the V nodes separately.

## Code ->
```cpp
class Solution {
public:
    int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        vector<vector<int>> dist(n, vector<int>(n, INT_MAX));

        for(int i=0; i<edges.size(); i++){
            int from = edges[i][0];
            int to = edges[i][1];
            int weight = edges[i][2];

            // since undirected graph so we from can go to to and to can go to from
            // update them with weight in the adj list dist
            dist[from][to] = weight;
            dist[to][from] = weight;
        }

        // Distance from a city to itself is 0
        for(int i=0; i<n; i++) dist[i][i] = 0;

        // Floyd Warshall algorithm to find shortest distances between all pairs of cities
        for(int k=0; k<n; k++){
            for(int i=0; i<n; i++){
                for(int j=0; j<n; j++){
                    // Avoid integer overflow by checking for INT_MAX
                    if (dist[i][k] == INT_MAX || dist[k][j] == INT_MAX)
						continue;
                    dist[i][j] = min(dist[i][j], dist[i][k]+dist[k][j]);
                }
            }
        }

        int ans = -1;
        int mini = n;

        // Count the number of reachable cities for each city
        for(int i=0; i<n; i++){
            int numOfNodes = 0;
            for(int j=0; j<n; j++){
                if(dist[i][j]<=distanceThreshold) numOfNodes++;
            }
            // Update answer if the current city has fewer or equal reachable cities
            if(numOfNodes<=mini){
                mini = numOfNodes;
                ans = i;
            }
        }

        return ans;
    }
};
```

# Minimum Spanning Tree (MST)

A spanning tree is a tree in which we have N nodes(i.e. All the nodes present in the original graph) and N-1 edges and all nodes are reachable from each other.

Among all possible spanning trees of a graph, the minimum spanning tree is the one for which the sum of all the edge weights is the minimum.

For example:

This is a spanning tree ->

![ST](images/MST1.webp)

This is the MST for the above spanning tree ->

![MST](images/MST2.webp)

Sum of edge weights = 17

Note: There may exist multiple minimum spanning trees for a graph like a graph may have multiple spanning trees.


If we were required to find the MST Path then we would have taken three things into our pq but since we are required to only find the sum of the MST so we will take the weight and the node in the pq.

The intuition behind this is simple greedy. We are just taking the minimal path and storing it in our pq and marking the node as visited and that helps us only processing the minimal paths.


# [Minimum Spanning Tree -> USING Prim's Algo](https://www.geeksforgeeks.org/problems/minimum-spanning-tree/1)

## Approach ->
In order to implement Prim’s algorithm, we will be requiring an array(visited array) and a priority queue that will essentially represent a min-heap. We need another array(MST) as well if we wish to store the edge information of the minimum spanning tree.

The algorithm steps are as follows:

Priority Queue(Min Heap): The priority queue will be storing the pairs (edge weight, node). We can start from any given node. Here we are going to start from node 0 and so we will initialize the priority queue with (0, 0). If we wish to store the mst of the graph, the priority queue should instead store the triplets (edge weight, adjacent node, parent node) and in that case, we will initialize with (0, 0, -1).

Visited array: All the nodes will be initially marked as unvisited.

sum variable: It will be initialized with 0 and we wish that it will store the sum of the edge weights finally.

MST array(optional): If we wish to store the minimum spanning tree(MST) of the graph, we need this array. This will store the edge information as a pair of starting and ending nodes of a particular edge.

1. We will first push edge weight 0, node value 0, and parent -1 as a triplet into the priority queue to start the algorithm.
Note: We can start from any node of our choice. Here we have chosen node 0.
2. Then the top-most element (element with minimum edge weight as it is the min-heap we are using) of the priority queue is popped out.
3. After that, we will check whether the popped-out node is visited or not.
If the node is visited: We will continue to the next element of the priority queue.
If the node is not visited: We will mark the node visited in the visited array and add the edge weight to the sum variable. If we wish to store the mst, we should insert the parent node and the current node into the mst array as a pair in this step.
4. Now, we will iterate on all the unvisited adjacent nodes of the current node and will store each of their information in the specified triplet format i.e. (edge weight, node value, and parent node) in the priority queue.
5. We will repeat steps 2, 3, and 4 using a loop until the priority queue becomes empty.
6. Finally, the sum variable should store the sum of all the edge weights of the minimum spanning tree.

## Code ->

```cpp
class Solution
{
public:
    // Function to find sum of weights of edges of the Minimum Spanning Tree.
    int spanningTree(int V, vector<vector<int>> adj[])
    {
        // Variable to store the sum of weights of the Minimum Spanning Tree.
        int sum = 0;

        // Priority Queue to store edges with their weights in ascending order.
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

        // Vector to keep track of visited nodes during the traversal.
        vector<int> visited(V, 0);

        // Initializing the algorithm by adding the starting node (0) with weight 0 to the priority queue.
        pq.push({0, 0});

        // Loop until the priority queue is not empty.
        while (pq.size())
        {
            // Extracting the edge with the minimum weight from the priority queue.
            int dist = pq.top().first; // Weight of the current edge.
            int node = pq.top().second; // End vertex of the current edge.
            pq.pop();

            // Check if the current node is already visited, skip if it is.
            if (visited[node])
                continue;

            // Mark the current node as visited.
            visited[node] = 1;

            // Add the weight of the current edge to the overall sum.
            sum += dist;

            // Iterate through the adjacency list of the current node.
            for (int i = 0; i < adj[node].size(); i++)
            {
                // Extract the end vertex and weight of the adjacent edge.
                int adjDist = adj[node][i][1]; // Weight of the adjacent edge.
                int adjNode = adj[node][i][0]; // End vertex of the adjacent edge.

                // Add the adjacent edge to the priority queue for further exploration.
                pq.push({adjDist, adjNode});
            }
        }

        // Return the sum of weights of the Minimum Spanning Tree.
        return sum;
    }
};
```

# [Find Center of Star Graph](https://leetcode.com/problems/find-center-of-star-graph/description/)

## Code ->
```cpp
class Solution {
public:
    int findCenter(vector<vector<int>>& arr) {
        // The intuition behind this problem is that in a star graph, 
        // the center node is the only node that is connected to all other nodes. 
        // Therefore, if we look at the first two edges in the given array, 
        // the center node must be one of the nodes common to both edges.
        
        int a = arr[0][0];
        int b = arr[0][1];
        int c = arr[1][0];
        int d = arr[1][1];

        if(a==c || a==d) return a;
        if(b==c || b==d) return b;

        return -1;
    }
};
```