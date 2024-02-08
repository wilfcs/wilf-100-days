
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
            return NULL;
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

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        int idx = 0; // Index to keep track of the current root value in the preorder traversal.
        
        // Call the solve function to construct the entire binary tree.
        return solve(preorder, inorder, 0, n - 1, idx);
    }
};
```

# [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)

## Approach->
You will probably need to see the vid for understanding this properly + solve it on your own. But here is the chatgpt generated explanation anyway->

The build function takes the current ranges of inorder and postorder arrays and identifies the root node based on the last element of the postorder array. It then finds the position of this root element in the inorder array, dividing the tree into left and right subtrees. Recursively, it constructs the left and right subtrees by updating the ranges of the arrays accordingly.

The intuition behind this approach is rooted in the nature of inorder and postorder traversals. In a postorder traversal, the last element is always the root of the tree. By identifying the root in the inorder array, we can determine the sizes of the left and right subtrees, allowing us to recursively build the binary tree structure. This process continues until the entire tree is constructed, and the root of the entire tree is returned.

[video link](https://www.youtube.com/watch?v=y6zSY_z7Kh4)
## Code ->
```cpp
class Solution {
public:
    // Recursive function to build the binary tree
    TreeNode* build(vector<int> &inorder, vector<int> &postorder, int inStart, int inEnd, int postStart, int postEnd) {
        // Base case: If the inorder range is empty, return NULL
        if (inStart > inEnd)
            return NULL;

        // Identify the root element from the postorder array
        int element = postorder[postEnd];
        int i = inStart;

        // Find the position of the root element in the inorder array
        for (; i <= inEnd; i++) {
            if (inorder[i] == element)
                break;
        }

        // Calculate the sizes of the left and right subtrees
        int leftSize = i - inStart;
        int rightSize = inEnd - i;

        // Create the root node
        TreeNode* root = new TreeNode(element);

        // Recursively build the left and right subtrees
        root->left = build(inorder, postorder, inStart, i - 1, postStart, postStart + leftSize - 1);
        root->right = build(inorder, postorder, i + 1, inEnd, postStart + leftSize, postEnd - 1);

        return root;
    }

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        int n = inorder.size();
        int inStart = 0, inEnd = n - 1;
        int postStart = 0, postEnd = n - 1;

        // Call the build function to initiate the construction process
        return build(inorder, postorder, inStart, inEnd, postStart, postEnd);
    }
};
```

# [Morris Inorder Traversal of a Binary Tree](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)

## [Approach](https://takeuforward.org/data-structure/morris-inorder-traversal-of-a-binary-tree/)

## Code ->
```cpp
class Solution {
public:
    vector<int> morrisInorderTraversal(TreeNode* root) {
        // Vector to store the inorder traversal result
        vector<int> inorder;
        
        // Current node pointer
        TreeNode* cur = root;

        while (cur) {
            // Case 1: No left subtree
            if (cur->left == nullptr) {
                inorder.push_back(cur->val);
                cur = cur->right;
            } else {
                // Find the rightmost node of the left subtree
                TreeNode* prev = cur->left;
                while (prev->right && prev->right != cur) {
                    prev = prev->right;
                }

                // Case 2: Rightmost child of left subtree points to null
                if (prev->right == nullptr) {
                    prev->right = cur;
                    cur = cur->left;
                } else {
                    // Case 3: Rightmost child already points to current node
                    prev->right = nullptr;
                    inorder.push_back(cur->val);
                    cur = cur->right;
                }
            }
        }

        return inorder;
    }
};
```

# [Morris Preorder Traversal of a Binary Tree](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)

## [Approach](https://takeuforward.org/data-structure/morris-preorder-traversal-of-a-binary-tree/)

## Code ->
```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> preorder;
        TreeNode* cur = root;

        while(cur){
            if(cur->left==NULL){
                preorder.push_back(cur->val);
                cur = cur->right;
            }
            else{
                TreeNode* prev = cur->left;
                while(prev->right && prev->right!=cur){
                    prev = prev->right;
                }
                if(prev->right == NULL){
                    prev->right = cur;
                    // only difference from the previous code is that we push the cur value when we reach the leaf node the first time
                    preorder.push_back(cur->val);
                    cur = cur->left;
                }
                else{
                    prev->right = NULL;
                    cur = cur->right;
                }
            }
        }

        return preorder;
    }
};
```


# [99. Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/description/)

## Approaches ->
1. Do the inorder traversal on the tree and store the elements, the elements are supposed to be in the sorted order but two elements will not be in the sorted order. Now sort the vector. Now do the inorder traversal again and and keep comparing the elements with the sorted vector, and when you find that the values are different, then just replace the value of the node while doing the inorder traversal. TC-> O(n log n) SC-> O(n)
2. Let's try to reduce the TC as well as SC. So there are two possible cases when we do the inorder traversal. Case 1 is when the two elements at the wrong position are far from each other and the case 2 is when both the elements are adjacent to each other.. Let's lool at both of them->

Case1: To be swapped nodes are not adjacent -> [3, 25, 7, 8, 10, 15, 20, 5]
Case2: To be swapped nodes are adjacent -> [3, 5, 8, 7, 10, 15, 20, 25]

To tackle both the cases we will maintian three global variables-> start, mid, and end. And to keep track of the previous element while doing the recursive calls we keep a prev variable also. When there is first violation, i.e. when the first time you encounter that the previous element is greater than the current element then you make the start as prev and mid as current node. Now, in the case 2 there will not be second violation because both faulty nodes are adjacent to each other. But in case 1 there will be a second violation. So in case of second violation make end as current node. And after the recursive calls are over, you can simply swap start and mid if there was just one violation i.e. end is NULL. And you cvan swap start and end if there were two violations i.e. end is no null to finally recover the BST.

## Code ->
```cpp
class Solution {
public:
    // Global variables to keep track of nodes that need to be swapped
    TreeNode* start = NULL;
    TreeNode* mid = NULL;
    TreeNode* end = NULL;
    
    // Variable to store the previous node during the inorder traversal
    TreeNode* prev;

    // Inorder traversal to identify nodes violating the BST property
    void inorder(TreeNode* root){
        // Base case: If the current node is NULL, return
        if(root == NULL) return;

        // Traverse left subtree
        inorder(root->left);

        // Check if there is a violation in the BST property
        if(prev->val > root->val){
            // First violation: Set start and mid with prev and root respectively
            if(start == NULL){
                start = prev;
                mid = root;
            }
            // Second violation: Set end as root
            else
                end = root;
        }

        // Update prev to the current node
        prev = root;

        // Traverse right subtree
        inorder(root->right);
    }

    // Main function to recover the BST
    void recoverTree(TreeNode* root) {
        // Initialize prev with a dummy node having a very small value
        prev = new TreeNode(INT_MIN);

        // Perform inorder traversal to identify faulty nodes
        inorder(root);

        // Swap the values of start and end if two violations occurred, else swap start and mid
        if(end != NULL)
            swap(start->val, end->val);
        else
            swap(start->val, mid->val);
    }
};

```

# [Size of Largest BST in Binary Tree](https://www.codingninjas.com/studio/problems/size-of-largest-bst-in-binary-tree_893103?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Code ->
```cpp
// Class to store information about a subtree
class info {
public:
    int maxi;   // Maximum value in the subtree
    int mini;   // Minimum value in the subtree
    bool isBST; // Whether the subtree is a BST or not
    int size;   // Size of the subtree
};

// Recursive function to calculate information about the subtree rooted at 'root'
info solve(TreeNode* root, int& ans) {
    // Base case: If the current node is NULL, return default info
    if (root == NULL) {
        return {INT_MIN, INT_MAX, true, 0};
    }

    // Recursively calculate info for left and right subtrees
    info left = solve(root->left, ans);
    info right = solve(root->right, ans);

    // Information for the current node
    info currNode;

    // Size of the current subtree
    currNode.size = left.size + right.size + 1;

    // Maximum value in the subtree
    currNode.maxi = max(root->data, max(left.maxi, right.maxi));

    // Minimum value in the subtree
    currNode.mini = min(root->data, min(left.mini, right.mini));

    // Check if the current subtree is a BST
    if (left.isBST && right.isBST && root->data > left.maxi && root->data < right.mini) {
        currNode.isBST = true;
    } else {
        currNode.isBST = false;
    }

    // Update the global maximum size if the current subtree is the largest BST
    if (currNode.isBST) {
        ans = max(ans, currNode.size);
    }

    return currNode;
}

// Main function to find the size of the largest BST in a binary tree
int largestBST(TreeNode* root) {
    // Initialize the global variable to store the result
    int ans = 0;

    // Call the solve function to calculate information about the subtree and update the result
    info temp = solve(root, ans);

    // Return the size of the largest BST
    return ans;
}
```
# [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)

## Approach ->
Pick middle element, make a new node with that middle element. Now assign left of that node and right of that node with recursive calls from start to mid and mid+1 to end.

## Code ->
```cpp
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums, int start, int end){
        if(end<=start) return NULL; 
        int midIdx=(end+start)/2;
        TreeNode* root=new TreeNode(nums[midIdx]);
        root->left=sortedArrayToBST(nums, start, midIdx);
        root->right=sortedArrayToBST(nums, midIdx+1,end);
        return root;
    }
    
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return sortedArrayToBST(nums, 0,nums.size());
    }
};
```

# [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)

## Approaches->
1. O(N^2) solution by traversing and storing in an array and finally comparing each element with each other.
2. Take advantage of BST. Inorder traversal of BST give elements in sorted order. Store sorted elements and then compare them. O(N).

## Code ->
```cpp
class Solution {
public:
    vector<int> vec;
    void inorder(TreeNode* root){
        if(!root) return;
        
        inorder(root->left);
        vec.push_back(root->val);
        inorder(root->right);
    }
    int getMinimumDifference(TreeNode* root) {
        int ans = INT_MAX;
        inorder(root);
        for(int i=0; i<vec.size()-1; i++)    
            ans = min(ans, vec[i+1]-vec[i]);
        return ans;
    }
};
```