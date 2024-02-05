
# [863. All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)

## Approach->
While revising just code this out man, otherwise you won't be able to grasp it properly and might forget the approach during the interview.

Intuition:

Storing Parent Nodes:

The initial step involves creating a map (parent) to store the parent of each node in the binary tree using DFS.
This is done to facilitate easy traversal from child nodes to their parent nodes during the search for nodes at distance K.
Breadth-First Search (BFS) from Target Node:

The primary goal is to find nodes at a distance K from a given target node.
Utilize BFS to explore nodes level by level, starting from the target node.
A queue (q) is employed to keep track of nodes to be processed.
Exploring Neighbors:

Within each level of BFS:
Check and enqueue the left child if it exists and has not been visited.
Check and enqueue the right child if it exists and has not been visited.
Check and enqueue the parent node if it exists and has not been visited.
This ensures that the traversal explores the immediate neighbors of the current node.
Tracking Visited Nodes:

Maintain a set (visited) to keep track of nodes that have been visited to avoid redundant processing.
Distance K Adjustment:

After processing each level, decrement the distance k.
If k reaches 0, break out of the loop, as further exploration beyond distance K is unnecessary.
Collecting Result:

During BFS, collect the values of nodes at distance K in a vector (ans).
The final result vector contains the values of nodes at the desired distance.
Overall Strategy:

Utilizing DFS to store parent nodes aids in efficient traversal during BFS.
BFS explores the tree level by level, keeping track of visited nodes.
The algorithm stops when the desired distance K is reached or when all reachable nodes have been explored.

[Link to explanation vid](https://www.youtube.com/watch?v=1PMjfQl_UVQ)

## Code ->
```cpp
class Solution {
public:
    // Function to store the parent of each node in the binary tree using DFS.
    void storeParent(TreeNode* root, unordered_map<TreeNode*, TreeNode*>& parent) {
        if (root == NULL) return;

        // Store the parent of the left child.
        if (root->left) parent[root->left] = root;
        // Recursively call the function for the left subtree.
        storeParent(root->left, parent);

        // Store the parent of the right child.
        if (root->right) parent[root->right] = root;
        // Recursively call the function for the right subtree.
        storeParent(root->right, parent);
    }

    // Main function to find nodes at distance K from the target node in the binary tree.
    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        // Map to store the parent of each node in the binary tree.
        unordered_map<TreeNode*, TreeNode*> parent;
        // Populate the parent map using DFS.
        storeParent(root, parent);

        // Set to keep track of visited nodes.
        unordered_set<int> visited;
        // Queue for BFS traversal.
        queue<TreeNode*> q;
        // Start BFS from the target node.
        q.push(target);
        // Mark the target node as visited.
        visited.insert(target->val);

        // BFS traversal to find nodes at distance K.
        while (q.size()) {
            // Number of nodes at the current level.
            int size = q.size();

            // Process nodes at the current level.
            while (size--) {
                // Get the front node from the queue.
                TreeNode* curNode = q.front();
                q.pop();

                // Check left child and enqueue if not visited.
                if (curNode->left && !visited.count(curNode->left->val)) {
                    q.push(curNode->left);
                    visited.insert(curNode->left->val);
                }

                // Check right child and enqueue if not visited.
                if (curNode->right && !visited.count(curNode->right->val)) {
                    q.push(curNode->right);
                    visited.insert(curNode->right->val);
                }

                // Check parent and enqueue if not visited.
                if (parent.count(curNode) && !visited.count(parent[curNode]->val)) {
                    q.push(parent[curNode]);
                    visited.insert(parent[curNode]->val);
                }
            }

            // Decrement the distance after processing nodes at the current level.
            k--;
            // If distance becomes 0, break out of the loop.
            if (k == 0) break;
        }

        // Collect the values of nodes at distance K in a vector.
        vector<int> ans;
        while (q.size()) {
            ans.push_back(q.front()->val);
            q.pop();
        }

        // Return the result vector.
        return ans;
    }
};
```

# [Burning Tree](https://www.geeksforgeeks.org/problems/burning-tree/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=bottom_sticky_on_article)

## Approach ->
Very similar to the last q. Read the code and you'd understand. ( tip: don't miss your & operator if you pass your nodes by reference, that wasted half an hour of my time in debugging).

## Code ->
```cpp
class Solution {
public:
    // Helper function to map parent nodes using Depth-First Search (DFS).
    void mapParent(Node* root, unordered_map<Node*, Node*>& parent) {
        if (root == NULL) return;

        // Store the parent of the left child.
        if (root->left) parent[root->left] = root;
        mapParent(root->left, parent);

        // Store the parent of the right child.
        if (root->right) parent[root->right] = root;
        mapParent(root->right, parent);
    }

    // Helper function to find the target node in the tree.
    void findTargetNode(Node* root, int start, Node*& target) {
        if (root == NULL) return;

        // If the current node's data matches the target, set target to the current node.
        if (root->data == start) {
            target = root;
            return;
        }

        // Recursively search for the target in the left and right subtrees.
        findTargetNode(root->left, start, target);
        findTargetNode(root->right, start, target);
    }

    // Main function to calculate the minimum time to burn the entire tree.
    int minTime(Node* root, int start) {
        // Map to store the parent of each node.
        unordered_map<Node*, Node*> parent;
        mapParent(root, parent);

        // Variable to store the target node.
        Node* target = NULL;
        // Find the target node in the tree.
        findTargetNode(root, start, target);

        // If the target node is not found, return an appropriate value (here, -1).
        if (target == NULL) {
            return -1;
        }

        // Set to keep track of visited nodes.
        unordered_set<int> visited;
        // Queue for Breadth-First Search (BFS).
        queue<Node*> q;
        // Start BFS from the target node.
        q.push(target);
        // Mark the target node as visited.
        visited.insert(target->data);

        // Variable to store the minimum time required.
        int ans = 0;

        // BFS traversal to calculate the minimum time.
        while (!q.empty()) {
            int size = q.size();

            // Process nodes at the current level.
            while (size--) {
                // Get the front node from the queue.
                Node* cur = q.front();
                q.pop();

                // Check left child and enqueue if not visited.
                if (cur->left && !visited.count(cur->left->data)) {
                    visited.insert(cur->left->data);
                    q.push(cur->left);
                }
                // Check right child and enqueue if not visited.
                if (cur->right && !visited.count(cur->right->data)) {
                    visited.insert(cur->right->data);
                    q.push(cur->right);
                }
                // Check parent and enqueue if not visited.
                if (parent.count(cur) && !visited.count(parent[cur]->data)) {
                    visited.insert(parent[cur]->data);
                    q.push(parent[cur]);
                }
            }
            // Increment the time.
            ans++;
        }

        // Return the minimum time (decremented by 1 to exclude the initial level).
        return --ans;
    }
};
```

# [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

## Approach ->
The algorithm leverages the properties of preorder and inorder traversals to reconstruct a binary tree.

In the preorder traversal, the first element is always the root of the current subtree. The algorithm starts by picking the root from the preorder list and incrementing an index to move to the next element.

Then, it searches for the position of the root value in the inorder traversal. The elements to the left of this position correspond to the left subtree, and the elements to the right correspond to the right subtree.

Recursively, the algorithm constructs the left subtree with the elements from the left side of the inorder position and the right subtree with the elements from the right side. This process is repeated for each subtree until the entire binary tree is reconstructed.

The key idea is that in each recursive call, the algorithm focuses on a specific root, finds its position in the inorder traversal, and uses this information to divide and conquer, constructing the left and right subtrees of the current root.

[Video explanation](https://www.youtube.com/watch?v=G5c1wM3Kpuw)

## Code ->
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

class Solution {
public:
    // Helper function to construct the binary tree recursively.
    TreeNode* solve(vector<int>& preorder, vector<int>& inorder, int start, int end, int &idx) {
        // Base case: If the start index exceeds the end index, return NULL.
        // note: start will always be greater than end if there is no element left in inorder vector to add in the root. dry run
        if (start > end) {
            return nullptr;
        }

        // Extract the current root value from the preorder traversal.
        int rootVal = preorder[idx++];
        
        // Find the index of the root value in the inorder traversal.
        int i = start;
        for (; i <= end; i++) {
            if (inorder[i] == rootVal) {
                break;
            }
        }

        // Create a new TreeNode with the current root value.
        TreeNode* root = new TreeNode(rootVal);

        // Recursively construct the left subtree with the range (start to i-1) in the inorder traversal.
        root->left = solve(preorder, inorder, start, i - 1, idx);
        // Recursively construct the right subtree with the range (i+1 to end) in the inorder traversal.
        root->right = solve(preorder, inorder, i + 1, end, idx);

        // Return the constructed root node.
        return root;
    }

    // Main function to build the binary tree using preorder and inorder traversals.
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        int idx = 0; // Index to keep track of the current root value in the preorder traversal.
        
        // Call the solve function to construct the entire binary tree.
        return solve(preorder, inorder, 0, n - 1, idx);
    }
};
```
# OS->
Why OS?
	1. What if there is no OS?
		a. Bulky and complex app. (Hardware interaction code must be in app’s code base)
		b. Resource exploitation by 1 App.
		c. No memory protection. Any app can hack the entire memory and make your hardware useless.
		d. Violation of DRY principle because every app will have a bulky code to interact with the hardware.
	2. What is an OS made up of?
	- Collection of system software.
		- System software operates and controls the computer system and provides a platform to run application software.
		
An operating system's function -
- Access to the computer hardware.
- interface between the user and the computer hardware
- Resource management (Aka, Arbitration) (memory, device, file, security, process etc)
- Hides the underlying complexity of the hardware. (Aka, Abstraction)
- Facilitates execution of application programs by providing isolation and protection.
	- Isolation means that an application software has no idea about the memory of another application software. Their memory is being managed in isolation. This in turn helps in the protection of any mishaps with the running application like another application killing its process.

Operating System Definition:
- An operating system is a piece of software that manages all the resources of a computer system, both hardware and software. It provides an environment in which the user can execute his/her programs conveniently and efficiently by hiding the underlying complexity of the hardware and acting as a resource manager.

Goals of OS:
1. Maximum CPU utilization: The CPU should never sit idle.
2. Less process starvation: A process should not starve because of other processes. Suppose we write while(1); then the OS should not work on that process till eternity.
3. Higher priority job execution: Suppose if a job like antivirus scan comes, then every other job should wait and this job shall be given the first priority.
Types of Operating System:
1. Single process Operating system: 
	- Only 1 process executes at a time from the ready queue, i.e. the oldest process.
	- It violates all three goals of OS.
	- eg - MS DOS.
2. Batch-Processing Operating system:
	- Firstly, user prepares his job using punch cards.
	- Then, he submits the job to the computer operator.
	- Operator collects the jobs from different users and sort the jobs into batches with similar needs.
	- Then, operator submits the batches to the processor one by one.
	- All the jobs of one batch are executed together.
	- It is similar to the single process os but it just works in batches. Batches consist of multiple jobs. It violates all the OS goals as well.
	- 
3. Multiprogramming Operating system:
	- Has a single CPU.
	- In multiprogramming operating system, we have a memory(PCB) that stores the jobs (code and data). Suppose CPU is executing Job A. Now Job A got busy with I/O operations, the CPU will become idle if it sits and waits for Job A to return. But the CPU will save the state of Job A in PCB(Process control block) and take out the state of Job B and start executing it instead of waiting for Job A.
	- The above is an example of context switching.
	- CPU is never idle in the case of multiprogramming OS.
	- Also gives priority to high priority tasks.
	- All three OS goals are achieved but not in the most optimal manner.
4. Multitasking Operating system:
	- Single CPU
	- Context switching
	- Time sharing: A process only runs till a certain time limit.
	- Because of time sharing and context switching we are able to run more than one task simultaneously.
	- High priority tasks.
	- All three OS goals are achieved more optimally than multiprogramming OS.
5. Multiprocessing Operating System:
	- More than one CPU.
	- Reliability: If one CPU fails, the others can work.
	- Less process starvation.
	- Is perfect for all three OS goals.
6. Distributed Operating System:
	- A distributed operating system is a software system that manages a collection of independent computers and makes them appear to the user as a single coherent system. It provides a consistent and transparent interface for users to access shared resources and perform tasks across multiple machines.
	- Loosely connected autonomous, interconnected computer nodes.
		- Loosely connected: The connection might be through anything, LAN, MAN, WAN, etc.
		- Autonomous: Each computer node operates independently of the others.
		- Interconnected computer nodes: Computer devices connected to each other.
7. RTOS (Real Time Operating System):
	- Real-time error-free, computations within tight-time boundaries.
	- Used in air Traffic control systems, ROBOTS, Nuclear power plants, army etc.