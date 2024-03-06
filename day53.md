
# [Shortest path in Undirected Graph having unit distance](https://www.geeksforgeeks.org/problems/shortest-path-in-undirected-graph-having-unit-distance/1)

## Approach ->
Use BFS to traverse the graph starting from the source node. Update the distance vector for each visited node, considering the unit distance between connected nodes. Read the code for further understanding.

## Code ->
```cpp
class Solution {
  public:
    vector<int> shortestPath(vector<vector<int>>& edges, int N,int M, int src){
        // code here
        vector<int> adj[N];
        
        // first we make the adjacency list
        for(auto a: edges){
            adj[a[0]].push_back(a[1]);
            adj[a[1]].push_back(a[0]);
        }
        
        // then we create a distance vector to store distance from source
        // initialize all its elements to INT MAX
        vector<int> dist(N, INT_MAX);
        vector<int> visited(N, 0);
        
        // distance of src from src would be 0 and its visited
        dist[src] = 0;
        visited[src] = 1;
        
        // create q and do normal bfs
        queue<int> q;
        q.push(src);
        
        while(q.size()){
            int curr = q.front();
            q.pop();
            
            for(int i=0; i<adj[curr].size(); i++){
                if(!visited[adj[curr][i]]){
                    visited[adj[curr][i]] = 1;
                    // make the distance of the adj child of a node as node's dist from src+1
                    dist[adj[curr][i]] = dist[curr] + 1;
                    q.push(adj[curr][i]);
                }
            }
        }
        
        // if a node is cannot be visited from src then we have to mark it as -1 atc
        // create ans vector and populate the ans vector with dist if dist isnt INT MAX
        
        vector<int> ans(N);
        for(int i=0; i<dist.size(); i++){
            if(dist[i]!=INT_MAX) ans[i]=dist[i];
            else ans[i] = -1;
        }
        
        return ans;
    }
};
```

# [Shortest path in Directed Acyclic Graph](https://www.geeksforgeeks.org/problems/shortest-path-in-undirected-graph/1)

## Approach ->
In a Directed Acyclic Graph (DAG), finding the shortest path from a source node to all other nodes is simplified due to the absence of cycles. The idea is to perform a Topological Sort, which orders the nodes based on their dependencies, ensuring that a node is processed only after all its prerequisites have been processed.

Steps:
1. **Topological Sort (DFS):** Implement a depth-first search (DFS) based topological sort to obtain the order of nodes.
2. **Initialize Distances:** Set initial distances from the source to all nodes to a large value except for the source node, which is set to 0.
3. **Relaxation:** Iterate through the topologically sorted nodes and relax their adjacent nodes by updating the distance if a shorter path is found.
4. **Unreachable Nodes:** Convert nodes with still-large distances to -1, indicating that they are unreachable from the source.
5. **Output:** The resulting vector contains the shortest distances from the source to all nodes in the DAG.

## Code ->
```cpp
#include<bits/stdc++.h>
using namespace std;

class Solution {
public:
    // Function to perform Topological Sort using DFS
    void topoSort(int node, vector<pair<int, int>>& adj[], int vis[], stack<int>& st) {
        vis[node] = 1;
        for (auto it : adj[node]) {
            int v = it.first;
            if (!vis[v]) {
                topoSort(v, adj, vis, st);
            }
        }
        st.push(node);
    }

    vector<int> shortestPath(int N, int M, vector<vector<int>>& edges) {
        // Create a graph in the form of an adjacency list
        vector<pair<int, int>> adj[N];
        for (int i = 0; i < M; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            int wt = edges[i][2];
            adj[u].push_back({v, wt});
        }

        // Initialize a visited array with all nodes marked as unvisited (0)
        int vis[N] = {0};

        // Perform topological sort using DFS and store the result in the stack 'st'
        stack<int> st;
        for (int i = 0; i < N; i++) {
            if (!vis[i]) {
                topoSort(i, adj, vis, st);
            }
        }

        // Initialize a vector 'dist' to store the shortest distances, initially set to a large value
        vector<int> dist(N, 1e9);

        // Distance from source node (0) to itself is 0
        dist[0] = 0;

        // Process nodes in topological order and relax their adjacent nodes
        while (!st.empty()) {
            int node = st.top();
            st.pop();

            for (auto it : adj[node]) {
                int v = it.first;
                int wt = it.second;

                // Relaxation step: Compare current distance with the new distance through the current node
                if (dist[node] + wt < dist[v]) {
                    dist[v] = wt + dist[node];
                }
            }
        }

        // Convert unreachable nodes (with distance still set to a large value) to -1
        for (int i = 0; i < N; i++) {
            if (dist[i] == 1e9) dist[i] = -1;
        }

        return dist;
    }
};
```
LEC-30: Thrashing 
1. Thrashing
a. If the process doesn’t have the number of frames it needs to support pages in active use, it will
quickly page-fault. At this point, it must replace some page. However, since all its pages are in active
use, it must replace a page that will be needed again right away. Consequently, it quickly faults
again, and again, and again, replacing pages that it must bring back in immediately.
b. This high paging activity is called Thrashing.
c. A system is Thrashing when it spends more time servicing the page faults than executing
processes.
d. Technique to Handle Thrashing
i. Working set model
1. This model is based on the concept of the Locality Model.
2. The basic principle states that if we allocate enough frames to a process to
accommodate its current locality, it will only fault whenever it moves to some
new locality. But if the allocated frames are lesser than the size of the current
locality, the process is bound to thrash.
ii. Page Fault frequency
1. Thrashing has a high page-fault rate.
2. We want to control the page-fault rate.
3. When it is too high, the process needs more frames. Conversely, if the page-fault
rate is too low, then the process may have too many frames.
4. We establish upper and lower bounds on the desired page fault rate.
5. If pf-rate exceeds the upper limit, allocate the process another frame, if pf-rate
fails falls below the lower limit, remove a frame from the process.
6. By controlling pf-rate, thrashing can be prevented.