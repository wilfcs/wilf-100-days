
# [Implementing Dijkstra Algorithm - Using Priority Queue](https://www.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix/1?utm_source=youtube&utm_medium=collab_striver_ytdescription&utm_campaign=implementing-dijkstra-set-1-adjacency-matrix)

## Approach ->
So whenever there is a undirected graph with non negative weighths, we use Dijkstra's algorithm. Dijkstra's doesn't work when there is a negative weight.

To use Dijkstra's algo, this time we will be using pq.
The priority queue (pq) is used in Dijkstra's algorithm to efficiently select the node with the minimum distance from the source vertex. The key intuition behind this choice is to always explore the node with the currently smallest known distance, ensuring that the algorithm prioritizes paths that are likely to be shorter.

## Code ->
```cpp
class Solution
{
public:
    // Function to find the shortest distance of all the vertices
    // from the source vertex S.
    vector<int> dijkstra(int V, vector<vector<int>> adj[], int S)
    {
        // Create min heap of pair to store min distance and node from the source
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        
        // Vector to store the distances from the source to all vertices
        vector<int> dist(V, 1e9); // Initialize with a large value as infinity
        
        // Push the source vertex into the priority queue with distance 0
        pq.push({0, S});
        dist[S] = 0; // Distance from source to itself is 0

        // Dijkstra's algorithm loop
        while (pq.size())
        {
            // Extract the node with the minimum distance from the priority queue
            int node = pq.top().second;
            int distance = pq.top().first;
            pq.pop();

            // Iterate over the adjacent nodes of the current node
            for (auto itr : adj[node])
            {
                int edgeWeight = itr[1];
                int adjNode = itr[0];

                // Relaxation step: Update the distance if a shorter path is found
                if (distance + edgeWeight < dist[adjNode])
                {
                    dist[adjNode] = distance + edgeWeight;
                    pq.push({dist[adjNode], adjNode});
                }
            }
        }

        // Return the vector containing the shortest distances from the source
        return dist;
    }
};
```

Q. Why don't we use Dijkstra if there is a negative weight?

ans -> Dijkstra's algorithm is not suitable for graphs with negative weights since it may lead to an undesirable scenario. Consider the case where two nodes, 0 and 1, are connected by an edge with a weight of -2. Initially, the priority queue (pq) is populated with the pair (0,0) representing node 0 with a distance of 0, as the starting point. Subsequently, the pair (-2,1) is added to the pq to reach node 1. However, to revisit node 0, a weight of -4 is required, which is less than the initial weight of 0, resulting in an endless loop of continually pushing the same elements into the priority queue.

Time Complexity: O(E log V) - This is due to the priority queue operations. For each edge (E), there are potentially log(V) operations, considering the insertion and extraction operations in the priority queue.

[Complexity analysis video in depth](https://www.youtube.com/watch?v=3dINsjyfooY&list=PLgUwDviBIf0oE3gA41TKO2H5bHpPd7fzn&index=34)

Space Complexity: O(|E| + |V|)


### Using set instead of a pq ->

We can use a set instead of a priority queue because both data structures essentially keep the smallest element on top. However, opting for a set provides an advantage: we can efficiently delete elements. For instance, if a node has already been visited and is present in the set, finding a better path allows us to delete the previous occurrence of that node from the set. This deletion operation helps avoid unnecessary reprocessing of the same node, making the set an effective alternative to a priority queue in this context.

## Code ->
```cpp
class Solution
{
	public:
	//Function to find the shortest distance of all the vertices
    //from the source vertex S.
    vector <int> dijkstra(int V, vector<vector<int>> adj[], int S)
    {
        // Code here
        // Create min heap of pair to store min distance and node from source
        set<pair<int, int>> st;
        vector<int> dist(V, 1e9);
        
        st.insert({0, S});
        dist[S] = 0;
        
        
        while(st.size()){
            // Note how we did these operations 
            auto it = *(st.begin());
            int node = it.second;
            int distance = it.first;
            st.erase(it);
            
            for(auto itr: adj[node]){
                int edgeWeight = itr[1];
                int adjNode = itr[0];
                
                if(distance + edgeWeight < dist[adjNode]){
                    // Here we can remove the node from set if it already was visited
                    // this is to reduce tc a little
                    if(dist[adjNode]!=1e9) st.erase({dist[adjNode], adjNode});
                    
                    dist[adjNode] = distance + edgeWeight;
                    st.insert({dist[adjNode], adjNode});
                }
            }
        }
        
        return dist;
    }
};
```

# DBMS ->
What is Data?
Data is a collection of raw, unorganized facts and details like text, observations, figures, symbols, and descriptions of things etc. In other words, data does not carry any specific purpose and has no significance by itself.

Types of Data:
a. Quantitative
- Numerical form
- Weight, volume, cost of an item.
b. Qualitative
- Descriptive, but not numerical.
- Name, gender, hair color of a person.

What is Information?
- Information is processed, organized, and structured data.
- It provides context of the data and enables decision-making.

What is a Database?
- A database is an electronic place/system where data is stored in a way that it can be easily accessed, managed, and updated. 

What is DBMS?
- A database-management system (DBMS) is a collection of interrelated data and a set of programs to access those data. The collection of data, usually referred to as the database, contains information relevant to an enterprise. The primary goal of a DBMS is to provide a way to store and retrieve database information that is both convenient and efficient.
- A DBMS is the database itself, along with all the software and functionality.

Three Schema Architecture:
-  Physical level / Internal level:
	- The lowest level of abstraction describes how the data are stored.
	- Low-level data structures used.
	- Talks about: Storage allocation (N-ary tree etc), Data compression & encryption etc.
	- Goal: We must define algorithms that allow efficient access of data.
- Logical level / Conceptual level:
	- The conceptual schema describes the design of a database at the conceptual level, describes what data are stored in DB, and what relationships exist among those data.
	- User at logical level does not need to be aware about physical-level structures.
	- Mostly a DBA (Database Administrator) works on this level.
- View level / External level:
	- The highest level of abstraction aims to simplify users’ interaction with the system by providing different view to different end-user.
	- Each view schema describes the database part that a particular user group is interested and hides the remaining database from that user group. eg. Students can only see lectures but teachers can also see students details.
	- At the external level/view level, a database contains several schemas that are sometimes called subschema. The subschema is used to describe the different view of the database.