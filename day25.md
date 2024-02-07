
# [1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/description/)

## Approach ->
The code aims to construct a binary search tree (BST) from a preorder traversal. It utilizes a recursive approach, where a helper function constructs nodes by ensuring that each node's value follows the BST property. The upper bound parameter dynamically adjusts during recursion, controlling the valid range of values for each node. This ensures that the left subtree contains values strictly less than the node, and the right subtree contains values strictly greater. The process continues until the entire BST is constructed from the given preorder traversal.

## Code ->
```cpp
class Solution {
public:
    // Helper function to construct the BST from preorder traversal
    TreeNode* helper(vector<int> &preorder, int &i, int upperBound) {
        // If we have processed all elements in preorder or the current element is greater than the upper bound
        if (i == preorder.size() || preorder[i] > upperBound) {
            return NULL; // Return NULL indicating no subtree here
        }

        // Create a new node with the current element as its value
        TreeNode* node = new TreeNode(preorder[i++]);

        // Recursively construct the left subtree with values less than the current node's value
        node->left = helper(preorder, i, node->val);

        // Recursively construct the right subtree with values greater than the upper bound i.e. the range of the parent nodes
        node->right = helper(preorder, i, upperBound);

        // Return the constructed node
        return node;
    }

    // Main function to construct the BST from preorder traversal
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        int i = 0; // Start from the first element in preorder
        return helper(preorder, i, INT_MAX); // Call the helper function with initial upper bound as INT_MAX
    }
};
```

# [ Predecessor And Successor In BST](https://www.codingninjas.com/studio/problems/predecessor-and-successor-in-bst_893049?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf)

## Approaches ->
1. We know that the inorder of a bst is sorted, so store the elements in the sorted order inside a vector and return the elements just before and after key from the vector.
2. The code aims to find the predecessor and successor of a given key in a Binary Search Tree (BST). It traverses the tree, updating the predecessor when the current node's value is less than the key and updating the successor when the value is greater. The final predecessor is the maximum value in the left subtree, and the successor is the minimum value in the right subtree. This approach ensures that the predecessor is the largest node before the key, and the successor is the smallest node after the key in the BST's inorder traversal.

## Code ->
```cpp
// The function takes a binary search tree (BST) represented by the root node and a key as input.
// It returns a pair of integers representing the predecessor and successor of the given key in the BST.
pair<int, int> predecessorSuccessor(TreeNode *root, int key)
{
    // Initialize variables to store the predecessor and successor values.
    TreeNode* cur = root;  // Start from the root node.
    int pred = -1;  // Initialize predecessor to -1.
    int suc = -1;   // Initialize successor to -1.

    // Traverse the BST to find the node with the given key or determine its predecessor and successor.
    while(cur){
        if(cur->data == key) {
            break;  // If the current node's data matches the key, exit the loop.
        }
        else if(cur->data > key){
            // If the current node's data is greater than the key, update successor and move to the left subtree.
            suc = cur->data;
            cur = cur->left;
        }
        else{
            // If the current node's data is less than the key, update predecessor and move to the right subtree.
            pred = cur->data;
            cur = cur->right;
        } 
    }

    // In a BST, the inorder predecessor will be the maximum element in the left subtree from the target.
    TreeNode* leftTree = cur->left;
    while(leftTree){
        pred = leftTree->data;  // Update predecessor.
        leftTree = leftTree->right;  // Move to the rightmost node in the left subtree.
    }

    // The successor will be the minimum element in the right subtree from the target.
    TreeNode* rightTree = cur->right;
    while(rightTree){
        suc = rightTree->data;  // Update successor.
        rightTree = rightTree->left;  // Move to the leftmost node in the right subtree.
    }

    // Return the pair of predecessor and successor.
    return {pred, suc};
}
```

# [653. Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/description/)

## Approaches ->
1. We know that if we store the inorder of bst it will be in sorted order. Once that is done we can simply apply the two pointer to reach our answer in the sorted array that we obtained. TC->O(2n) where O(n) is for tree traversal and O(n) is for array traversal. SC-> O(n)
2. We can do this in O(n) TC also by simply storing the visited nodes in a hashmap and simply checking if target - node.val exists or not in the hashmap.

## Code ->
```cpp
class Solution {
public:
    bool helper(TreeNode* root, int k, unordered_set<int> &visited){
        if(root == NULL) return false;

        // If k - root->val is found in the visited set, return true
        if(visited.count(k - root->val)) return true;

        // Add the current node's value to the visited set
        visited.insert(root->val);

        // Recursively check the left and right subtrees
        return helper(root->left, k, visited) || helper(root->right, k, visited);
    }

    bool findTarget(TreeNode* root, int k) {
        unordered_set<int> visited;
        return helper(root, k, visited);
    }
};
```

# OS->
Multi-Tasking vs Multi-Threading:
- Program: A Program is an executable file that contains a certain set of instructions written to complete a specific job or operation on your computer. It’s a compiled code. Ready to be executed. It is stored in Disk.
- Process: Program under execution. Resides in the Computer’s primary memory (RAM).
- Thread:
	- Single sequence stream within a process. Basically a light-weight process.
	- An independent path of execution in a process. E.g., Multiple tabs in a text editor(When you are typing in an editor, spell-checking, formatting of text and saving the text are done concurrently by multiple threads.)
	- Used to achieve parallelism by dividing a process’s tasks which are independent path of execution.
	- Another example -> Suppose you wrote a logic to convert 100x100 png to jpg. You are given 200x100 png. You can break the png into two sets of 100x100 and assign them to two independent threads. Those threads will use the same logic written by you but work in parallelism. After the work is done you will get their combined effect and have a jpg of 200x100.
	- Above is an example of multithreading.
	- It makes the process faster.
	- It works efficiently only when the number of CPUs are greater than 1 because each thread is processed by a separate CPU.
	- There is a new concept of hyperthreading that even the interviewers don't know about. Hyperthreading is a technology developed by Intel that allows a single physical CPU core to act like two logical cores. It does this by sharing the core's computational resources between two separate sets of registers and execution units within the core. Hyperthreading allows a second thread to utilize the otherwise idle resources within the core by switching to the second thread's instructions whenever the first thread is stalled.
Multitasking vs Multithreading:
Multitasking
Multithreading
The execution of more than one task is called multitasking.
The execution of more than one thread is called multithreading.
Concept of more than 1 process being context switched.
Concept of more than 1 thread being context switched.
Number of CPU 1
Number of CPUs >= 1
Isolation and memory protection exist. OS must allocate separate memory and resources to each program that the CPU is executing.
No isolation and memory protection, resources are shared among threads of that process. OS allocates memory to a process; multiple threads of that process share the same memory and resources allocated to the process.
Thread Scheduling:
Threads are scheduled for execution based on their priority. Even though threads are executing within the runtime, all threads are assigned processor time slices by the operating system.
Difference between Thread Context Switching and Process Context Switching:
Thread Context switching
Process context switching
OS saves current state of thread & switches to another thread of same process.
OS saves current state of process & switches to another process by restoring its state.
Doesn’t includes switching of memory address space. (But Program counter, registers & stack are included.)
Includes switching of memory address space.
Fast switching.
Slow switching.
CPU’s cache state is preserved.
CPU’s cache state is flushed.