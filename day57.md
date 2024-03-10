
# Disjoint Set | Union by Rank | Union by Size | Path Compression

Read the extensive article [here](https://takeuforward.org/data-structure/disjoint-set-union-by-rank-union-by-size-path-compression-g-46/) if you don't understand the explanation below.

**Question Summary:**

The article discusses the Disjoint Set data structure, a crucial topic in graph theory. The focus is on dynamic graphs, where the structure changes over time. The question involves determining if two nodes, 1 and 5, are in the same component of an undirected graph.

Question: Given two components of an undirected graph

![Diagram](images/disjointSet.webp)

The question is whether node 1 and node 5 are in the same component or not.

**Intuition:**

Traditional approaches like DFS or BFS may take O(N+E) time, but Disjoint Set can provide a constant-time solution. The Disjoint Set data structure is particularly useful for dynamic graphs, allowing efficient operations after each graph modification.

**Approach:**

1. **Functionalities of Disjoint Set:**
   - Finding the ultimate parent for a node (`findUltimateParent()`).
   - Union operation, which involves connecting two nodes.

2. **Union by Rank:**
   - **Rank:** Distance from the furthest leaf node. The ultimate parent will have the highest rank.
   - **Ultimate Parent:** The topmost/root node.
   - Union by rank optimizes the union operation by considering ranks. Smaller rank trees connect to larger rank trees. Equal ranks involve increasing one rank.

3. **`findUltimateParent()` Function:**
   - Finds the ultimate parent for a given node.
   - Employs path compression to optimize by connecting each node in a path to its ultimate parent.

4. **Path Compression:**
   - Connects each node in a path to its ultimate parent.
   - Reduces time complexity by eliminating the need to traverse the entire path.

5. **Code Implementation:**
   - `DisjointSet` class with `rank` and `parent` vectors.
   - `findUltimateParent()` uses path compression.
   - `unionByRank()` implements union by rank, considering ranks and ultimate parents.

Look at the code to understand everything->

## Code ->

```cpp
#include <bits/stdc++.h>
using namespace std;

// Implementation of the Disjoint Set data structure
class DisjointSet {
    // Vector to store the rank of each node
    vector<int> rank;
    // Vector to store the parent of each node
    vector<int> parent;

public:
    // Constructor to initialize the Disjoint Set with 'n' nodes
    DisjointSet(int n) {
        // Initialize rank vector with zeros
        rank.resize(n + 1, 0);
        parent.resize(n+1);
        // Initialize parent vector with node indices
        for (int i = 0; i <= n; i++) 
            parent[i] = i;
    }

    // Function to find the ultimate parent of a node with path compression
    int findUltimateParent(int node) {
        // If the current node is its own parent, return the node
        if (parent[node] == node) 
            return node;
        // Use path compression by updating the parent to the ultimate parent
        return parent[node] = findUltimateParent(parent[node]);
    }

    // Function to perform union by rank operation
    void unionByRank(int u, int v) {
        // Find the ultimate parents of nodes 'u' and 'v'
        int up_u = findUltimateParent(u);
        int up_v = findUltimateParent(v);

        // If they already belong to the same component, no need to union
        if (up_u == up_v) 
            return;

        // Connect the smaller rank tree to the larger rank tree
        if (rank[up_u] > rank[up_v]) 
            parent[up_v] = up_u;
        else if (rank[up_u] < rank[up_v]) 
            parent[up_u] = up_v;
        // If ranks are equal, connect any parent and increase the rank
        else {
            parent[up_u] = up_v;
            rank[up_v]++;
        }
    }
};

// Main function for testing the Disjoint Set implementation
int main() {
    // Create a Disjoint Set with 7 nodes
    DisjointSet ds(7);

    // Perform union by rank operations on sample edges
    ds.unionByRank(1, 2);
    ds.unionByRank(2, 3);
    ds.unionByRank(4, 5);
    ds.unionByRank(6, 7);
    ds.unionByRank(5, 6);

    // Check if nodes 3 and 7 belong to the same component
    if (ds.findUltimateParent(3) == ds.findUltimateParent(7)) 
        cout << "Same" << endl;
    else 
        cout << "Not Same" << endl;

    // Perform another union and check again
    ds.unionByRank(3, 7);

    if (ds.findUltimateParent(3) == ds.findUltimateParent(7)) 
        cout << "Same" << endl;
    else 
        cout << "Not Same" << endl;

    return 0;
}
```

Can we not use rank and use size to solve the same proble? Yes. Here is the code snippet of the unionBySize function. Rest everything else will be the same.

## Code ->
```cpp
void unionBySize(int u, int v) {
    // Find the ultimate parents of nodes 'u' and 'v'
    int up_u = findUltimateParent(u);
    int up_v = findUltimateParent(v);

    // If nodes 'u' and 'v' already belong to the same component, no union is needed
    if (up_u == up_v)
        return;

    // Connect the smaller size tree to the larger size tree
    if (size[up_u] < size[up_v]) {
        // Update the parent of 'u' to 'v', as 'v' has a larger size
        parent[up_u] = up_v;
        // Update the size of 'v' by adding the size of 'u'
        size[up_v] += size[up_u];
    } else {
        // Update the parent of 'v' to 'u', as 'u' has a larger size or sizes are equal
        parent[up_v] = up_u;
        // Update the size of 'u' by adding the size of 'v'
        size[up_u] += size[up_v];
    }
    // The union operation is now complete
}
```

Time Complexity:  The time complexity is O(4 * alpha) which is very small and close to 1. So, we can consider 4 as a constant.

# [Kruskal’s Algorithm – Minimum Spanning Tree](https://www.geeksforgeeks.org/problems/minimum-spanning-tree/1)

## Approach ->

We will be using Kruskal's algo to solve the MST problem.

In Kruskal's algorithm, we'll use the Disjoint Set data structure, which has two important functions: `findUltimateParent()` (helps find the ultimate parent of a node) and `unionByRank()` or `unionBySize()` (used to connect two nodes in the graph).

Here's a simpler breakdown of the steps:

1. First, we need to collect the edge information from the given adjacency list. This information includes the weight of the edge (`wt`), the current node (`u`), and its adjacent node (`v`). We store these tuples in an array.

2. Sort the array in ascending order based on edge weights. This sorting is crucial as it helps us process edges from the smallest to the largest weight.

3. Iterate through the sorted edges. For each edge:
   - Extract the nodes `u` and `v`.
   - Check if the ultimate parents of both nodes are the same using `findUltimateParent()`.
   - If the ultimate parents are the same, skip this edge as there's already a connection.
   - If the ultimate parents are different, add the edge weight to the final result (`mstWt`) and perform the union operation (`unionByRank()` or `unionBySize()`) on nodes `u` and `v`.

4. After processing all edges, the final result (`mstWt`) gives us the sum of weights in the Minimum Spanning Tree (MST).

In simpler terms, Kruskal's algorithm builds the MST by considering edges in ascending order of weights. For each edge, it checks if connecting its nodes would create a cycle. If not, it adds the edge to the MST and updates the data structure accordingly. This process continues until all nodes are connected with the minimum possible total weight.

## Code ->
```cpp
class DisjointSet {
    // Vector to store the rank of each node
    vector<int> rank;
    // Vector to store the parent of each node
    vector<int> parent;

public:
    // Constructor to initialize the Disjoint Set with 'n' nodes
    DisjointSet(int n) {
        // Initialize rank vector with zeros
        rank.resize(n + 1, 0);
        // Initialize parent vector with node indices
        parent.resize(n + 1);
        for (int i = 0; i <= n; i++) 
            parent[i] = i;
    }

    // Function to find the ultimate parent of a node with path compression
    int findUltimateParent(int node) {
        // If the current node is its own parent, return the node
        if (parent[node] == node) 
            return node;
        // Use path compression by updating the parent to the ultimate parent
        return parent[node] = findUltimateParent(parent[node]);
    }

    // Function to perform union by rank operation
    void unionByRank(int u, int v) {
        // Find the ultimate parents of nodes 'u' and 'v'
        int up_u = findUltimateParent(u);
        int up_v = findUltimateParent(v);

        // If they already belong to the same component, no need to union
        if (up_u == up_v) 
            return;

        // Connect the smaller rank tree to the larger rank tree
        if (rank[up_u] > rank[up_v]) 
            parent[up_v] = up_u;
        else if (rank[up_u] < rank[up_v]) 
            parent[up_u] = up_v;
        // If ranks are equal, connect any parent and increase the rank
        else {
            parent[up_u] = up_v;
            rank[up_v]++;
        }
    }
};

class Solution {
public:
    // Function to find the sum of weights of edges of the Minimum Spanning Tree.
    int spanningTree(int V, vector<vector<int>> adj[]) {
        // Vector to store edges with weights and corresponding nodes
        vector<pair<int, pair<int, int>>> edges;
        
        // Populate the edges vector with weights and node pairs
        for(int i = 0; i < V; i++) {
            for(int j = 0; j < adj[i].size(); j++) {
                int adjNode = adj[i][j][0];
                int wt = adj[i][j][1];
                int node = i;
                
                edges.push_back({wt, {node, adjNode}});
            }
        }
        
        // Initialize Disjoint Set with 'V' nodes
        DisjointSet ds(V);
        
        // Sort edges by weight in ascending order
        sort(edges.begin(), edges.end());
        
        int mstW = 0; // Variable to store the total weight of the Minimum Spanning Tree
        
        // Iterate through sorted edges and build the Minimum Spanning Tree
        for(int i = 0; i < edges.size(); i++) {
            int wt = edges[i].first;
            int u = edges[i].second.first;
            int v = edges[i].second.second;
            
            // Check if including the edge forms a cycle in the MST
            if(ds.findUltimateParent(u) != ds.findUltimateParent(v)) {
                // Include the edge and update the Disjoint Set
                mstW += wt;
                ds.unionByRank(u, v);
            }
        }
        
        return mstW; // Return the total weight of the Minimum Spanning Tree
    }
};
```

# [547. Number of Provinces - Using Disjoint Set](https://leetcode.com/problems/number-of-provinces/description/)

## Approach ->
Very easy, use the same disjoint set class to find the ultimate parents. After you find them, just iterate in the parents vector and see if parent[i]==i itself, reflecting that i is the ultimate parent. The number of UP is the ans basically. 
Note: Don't forget to bring parent under public in our DisjointSet class.

## Code ->
```cpp
class DisjointSet {
public:
     // Vector to store the rank of each node
    vector<int> rank;
    // Vector to store the parent of each node
    vector<int> parent;

    // Constructor to initialize the Disjoint Set with 'n' nodes
    DisjointSet(int n) {
        // Initialize rank vector with zeros
        rank.resize(n + 1, 0);
        // Initialize parent vector with node indices
        parent.resize(n + 1);
        for (int i = 0; i <= n; i++) 
            parent[i] = i;
    }

    // Function to find the ultimate parent of a node with path compression
    int findUltimateParent(int node) {
        // If the current node is its own parent, return the node
        if (parent[node] == node) 
            return node;
        // Use path compression by updating the parent to the ultimate parent
        return parent[node] = findUltimateParent(parent[node]);
    }

    // Function to perform union by rank operation
    void unionByRank(int u, int v) {
        // Find the ultimate parents of nodes 'u' and 'v'
        int up_u = findUltimateParent(u);
        int up_v = findUltimateParent(v);

        // If they already belong to the same component, no need to union
        if (up_u == up_v) 
            return;

        // Connect the smaller rank tree to the larger rank tree
        if (rank[up_u] > rank[up_v]) 
            parent[up_v] = up_u;
        else if (rank[up_u] < rank[up_v]) 
            parent[up_u] = up_v;
        // If ranks are equal, connect any parent and increase the rank
        else {
            parent[up_u] = up_v;
            rank[up_v]++;
        }
    }
};

// Main function
class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();
        DisjointSet ds(n);

        // Iterate through the adjacency matrix and perform union for connected nodes
        for(int i=0; i<n; i++){
            for(int j=0; j<n; j++){
                if(isConnected[i][j]) ds.unionByRank(i, j);
            }
        }

        int ans = 0;

        // Count the number of unique parent nodes, each representing a connected component
        for(int i=0; i<n; i++){
            if(ds.parent[i]==i) ans++;
        }

        return ans;
    }
};
```

# [1319. Number of Operations to Make Network Connected](https://leetcode.com/problems/number-of-operations-to-make-network-connected/description/)

## Approach ->
Intuition:
- The goal is to make the given graph connected by removing a minimal number of edges.
- We cannot add random edges from outside, only remove edges and place them elsewhere in the graph.
- To connect different components, we need to connect any node from one component to any node from another.
- We aim to find the minimum number of edges to remove and place strategically to make the graph connected.

Observations:
1. Connecting Components: To connect two components, we connect any node from the first to any node from the second component.
2. Minimum Edges: We need a minimum of (n - 1) edges to connect n components. eg-> We will return an ans of 3 if there are 4 components to connect.
3. Extra Edges: Edges that can be assumed as removable, which, if used, could make the graph connected.
4. Components and Extra Edges: If a graph has (n - 1) extra edges, we can make it connected and return n-1 as our answer.

Approach:
1. Extract all edge information in the form of pairs (u, v) and store it in an array.
2. Iterate through the array, check ultimate parent equality using findUltimateParent() from Disjoint Set:
   - If ultimate parents of u and v are the same, and u is obviously connected to v atq, then that means that we have an extra edge in the graph that we don't need. So increase the count of extra edges by 1.
   - If different, apply union (unionBySize() or unionByRank()) on u and v to connect and shrink them.
3. Count the number of components:
   - Iterate over nodes, and for each node:
     - If the node is its ultimate parent, increase the count of components by 1.
4. Compare counts of extra edges and components:
   - If extra edges count is greater or equal to the number of components - 1, return number of components - 1.
   - Otherwise, return -1.

## Code ->
```cpp
// DisjointSet class for implementing Union-Find operations
class DisjointSet {
public:
    // Vector to store the rank of each node
    vector<int> rank;
    // Vector to store the parent of each node
    vector<int> parent;

    // Constructor to initialize the Disjoint Set with 'n' nodes
    DisjointSet(int n) {
        // Initialize rank vector with zeros
        rank.resize(n + 1, 0);
        // Initialize parent vector with node indices
        parent.resize(n + 1);
        for (int i = 0; i <= n; i++) 
            parent[i] = i;
    }

    // Function to find the ultimate parent of a node with path compression
    int findUltimateParent(int node) {
        // If the current node is its own parent, return the node
        if (parent[node] == node) 
            return node;
        // Use path compression by updating the parent to the ultimate parent
        return parent[node] = findUltimateParent(parent[node]);
    }

    // Function to perform union by rank operation
    void unionByRank(int u, int v) {
        // Find the ultimate parents of nodes 'u' and 'v'
        int up_u = findUltimateParent(u);
        int up_v = findUltimateParent(v);

        // If they already belong to the same component, no need to union
        if (up_u == up_v) 
            return;

        // Connect the smaller rank tree to the larger rank tree
        if (rank[up_u] > rank[up_v]) 
            parent[up_v] = up_u;
        else if (rank[up_u] < rank[up_v]) 
            parent[up_u] = up_v;
        // If ranks are equal, connect any parent and increase the rank
        else {
            parent[up_u] = up_v;
            rank[up_v]++;
        }
    }
};

// Solution class for finding the minimum number of edges to make the graph connected
class Solution {
public:
    int makeConnected(int n, vector<vector<int>>& connections) {
        // Create an instance of the DisjointSet class
        DisjointSet ds(n);

        int cntExtras = 0;

        // Iterate through the given connections
        for(int i=0; i<connections.size(); i++){
            int u = connections[i][0];
            int v = connections[i][1];

            // Check if the ultimate parents of 'u' and 'v' are the same
            if(ds.findUltimateParent(u) == ds.findUltimateParent(v)) 
                cntExtras++;  // Increment the count of extra-edges
            else{
                ds.unionByRank(u, v);  // Apply union operation to connect nodes 'u' and 'v'
            }
        }

        int numOfComponents = 0;

        // Count the number of components in the graph
        for(int i=0; i<n; i++){
            if(ds.parent[i]==i) numOfComponents++;
        }

        int ans = numOfComponents - 1;  // Calculate the minimum edges required to make the graph connected

        // Check if the number of extra-edges is greater or equal to the required minimum edges
        if(ans <= cntExtras) 
            return ans;
        else 
            return -1;  // If not, return -1 as it is not possible to make the graph connected
    }
};
```
# [721. Accounts Merge](https://leetcode.com/problems/accounts-merge/description/)

## Approach ->
By far the toughest graph question so far...

**Question Explanation:**
- We are given a list of accounts where each account is represented by a list of strings.
- The first element in each account is the name, and the remaining elements are emails associated with that account.
- The goal is to merge accounts that belong to the same person. Two accounts belong to the same person if they share at least one common email.
- After merging, return the accounts in the format where the first element is the name, and the rest are emails sorted in order.

**Intuition:**
- To merge accounts, we need to establish connections between them based on common emails.
- We can use the Disjoint Set data structure (Union-Find) to efficiently manage these connections and find the ultimate parent of each account.

**Approach:**
1. **Data Structure Initialization:**
   - Use Disjoint Set to keep track of connected components.
   - Each account is a node in the Disjoint Set.

2. **Union Operation:**
   - Iterate through each account's emails.
   - If an email is already present in the map, perform the union operation between the current account and the account to which the email belongs.
   - This step establishes connections between accounts that share common emails.

3. **Merging Accounts:**
   - Iterate through the map of emails and find the ultimate parent for each account using Disjoint Set's find operation.
   - Group emails based on their ultimate parent.

4. **Sorting Emails and Creating Result:**
   - Sort emails within each group.
   - Create the final result in the required format.

**Code Explanation:**
- **DisjointSet Class:**
  - Maintains the rank and parent vectors.
  - Implements the findUltimateParent and unionByRank operations.

- **Solution Class:**
  - Initializes DisjointSet and a map to store email-index associations.
  - Performs union operation and establishes connections between accounts.
  - Creates groups of emails based on ultimate parents.
  - Sorts emails within each group.
  - Constructs the final result in the required format.

## Code ->
```cpp
class DisjointSet {
public:
    // Vector to store the rank of each node
    vector<int> rank;
    // Vector to store the parent of each node
    vector<int> parent;

    // Constructor to initialize the Disjoint Set with 'n' nodes
    DisjointSet(int n) {
        // Initialize rank vector with zeros
        rank.resize(n + 1, 0);
        // Initialize parent vector with node indices
        parent.resize(n + 1);
        for (int i = 0; i <= n; i++) 
            parent[i] = i;
    }

    // Function to find the ultimate parent of a node with path compression
    int findUltimateParent(int node) {
        // If the current node is its own parent, return the node
        if (parent[node] == node) 
            return node;
        // Use path compression by updating the parent to the ultimate parent
        return parent[node] = findUltimateParent(parent[node]);
    }

    // Function to perform union by rank operation
    void unionByRank(int u, int v) {
        // Find the ultimate parents of nodes 'u' and 'v'
        int up_u = findUltimateParent(u);
        int up_v = findUltimateParent(v);

        // If they already belong to the same component, no need to union
        if (up_u == up_v) 
            return;

        // Connect the smaller rank tree to the larger rank tree
        if (rank[up_u] > rank[up_v]) 
            parent[up_v] = up_u;
        else if (rank[up_u] < rank[up_v]) 
            parent[up_u] = up_v;
        // If ranks are equal, connect any parent and increase the rank
        else {
            parent[up_u] = up_v;
            rank[up_v]++;
        }
    }
};

class Solution {
public:
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        // Get the number of accounts
        int n = accounts.size();
        // Initialize DisjointSet for managing connections
        DisjointSet ds(n);
        // Map to store email-index associations for union operations
        unordered_map<string, int> mp;

        // Iterate through each account
        for(int i=0; i<accounts.size(); i++){
            // Iterate through each email in the account
            for(int j=1; j<accounts[i].size(); j++){
                // Extract the email
                string mail = accounts[i][j];
                // Check if the email is already mapped to an index
                if(mp.count(mail)) 
                    // If yes, perform the union operation between the current account and the mapped account
                    ds.unionByRank(i, mp[mail]);
                else 
                    // If no, map the email to the current account's index
                    mp[mail] = i;
            }
        }

        // Vector to store grouped emails based on ultimate parents
        vector<vector<string>> mergedMail(n);

        // Iterate through the map of emails
        for(auto it: mp){
            // Extract the email
            string mail = it.first;
            // Find the ultimate parent for the mapped account's index
            int node = ds.findUltimateParent(it.second);
            // Group the email with others under the ultimate parent
            mergedMail[node].push_back(mail);
        }

        // Vector to store the final result
        vector<vector<string>> ans;

        // Iterate through grouped emails
        for(int i=0; i<mergedMail.size(); i++){
            // Skip empty groups
            if(mergedMail[i].size()==0) continue;

            // Sort emails within each group
            sort(mergedMail[i].begin(), mergedMail[i].end());

            // Vector to store the account information in the required format
            vector<string> temp;
            // Add the account name as the first element
            temp.push_back(accounts[i][0]);
            // Add sorted emails to the temp vector
            for(auto a: mergedMail[i]) temp.push_back(a);

            // Add the temp vector to the final result
            ans.push_back(temp);
        }

        // Return the final result
        return ans;
    }
};
```
Time Complexity: O(N+E) + O(E*4ɑ) + O(N*(ElogE + E)) where N = no. of indices or nodes and E = no. of emails. The first term is for visiting all the emails. The second term is for merging the accounts. And the third term is for sorting the emails and storing them in the answer array.

Space Complexity: O(N)+ O(N) +O(2N) ~ O(N) where N = no. of nodes/indices. The first and second space is for the ‘mergedMail’ and the ‘ans’ array. The last term is for the parent and size array used inside the Disjoint set data structure.





Whenever there is a dynamic graph that is changing and nodes are connecting to each other, disjoint set should come in your mind    