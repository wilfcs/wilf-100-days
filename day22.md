
# [Children Sum Property](https://www.codingninjas.com/studio/problems/children-sum-property_8357239?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Code ->
```cpp
bool isParentSum(Node *root) {
    // Base case: If the root is NULL or it is a leaf node (no children),
    // then the children sum property is trivially satisfied.
    if (root == NULL || (root->left == NULL && root->right == NULL))
        return true;

    int v1 = 0, v2 = 0;

    // If the left child exists, assign its value to v1.
    if (root->left) v1 = root->left->data;

    // If the right child exists, assign its value to v2.
    if (root->right) v2 = root->right->data;

    // Check if the children sum property doesn't hold for the current value then return false
    if (v1 + v2 != root->data) return false;

    // Recursively check the children sum property for the left and right subtrees.
    // If either subtree violates the property, the entire tree violates it so we are using & operator
    return (isParentSum(root->left) && isParentSum(root->right));
}
```

# [222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/description/)

## Approaches->
1. Simply cound all nodes. TC-> O(n)
2. [Approach 2](https://takeuforward.org/binary-tree/count-number-of-nodes-in-a-binary-tree/)

## Codes ->
1. 
```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(root==NULL) return 0;

        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```
2. 
```cpp
class Solution {
public:
    // Function to calculate the height of the left subtree
    int leftHeight(TreeNode* root) {
        if (root == NULL) return 0;
        return 1 + leftHeight(root->left);
    }

    // Function to calculate the height of the right subtree
    int rightHeight(TreeNode* root) {
        if (root == NULL) return 0;
        return 1 + rightHeight(root->right);
    }

    // Function to count the number of nodes in the complete binary tree
    int countNodes(TreeNode* root) {
        // Base case: if the root is NULL, there are no nodes
        if (root == NULL) return 0;

        // Calculate the height of the left and right subtrees
        int lh = leftHeight(root);
        int rh = rightHeight(root);

        // If the left and right subtrees have the same height, the tree is complete
        if (lh == rh) {
            // Calculate the number of nodes using the formula for a complete binary tree
            return pow(2, lh) - 1;
        }

        // If the left and right subtrees have different heights, recursively count nodes
        // in the left and right subtrees and add 1 for the current node
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```

# [987. Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/description/)

## Approach ->
Idea is very simple, perform the level order traversal and instead of just storing the node in the queue like you do in a normal level order traversal, store its vertical and level as well.
Take a map and store the vertical, level and values on that level. Finally formulate your answer. You might not have understood this explanation so check the link below. Solve this question while revising Himanshu, this is a code heavy question and not a concept heavy question.
[link](https://takeuforward.org/data-structure/vertical-order-traversal-of-binary-tree/)

## Code ->
```cpp
class Solution {
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        // Using a map to store the nodes with the same vertical and level position
        map<int, map<int, multiset<int>>> mp;
        // Using a queue for BFS traversal
        queue<pair<TreeNode*, pair<int, int>>> q;

        // Pushing the root node to the queue with initial vertical and level positions (0, 0)
        q.push({root, {0,0}});

        // BFS traversal
        while(q.size()){
            // Extracting the current node and its positions from the front of the queue
            auto p = q.front();
            TreeNode* node = p.first;
            int vertical = p.second.first, level = p.second.second;
            q.pop();

            // Inserting the node's value into the map at the corresponding positions
            mp[vertical][level].insert(node->val);

            // Enqueueing the left child with adjusted positions
            if(node->left) q.push({node->left, {vertical-1, level+1}});
            // Enqueueing the right child with adjusted positions
            if(node->right) q.push({node->right, {vertical+1, level+1}});
        }

        // Constructing the final result from the map
        vector<vector<int>> ans;

        for(auto x: mp){
            vector<int> temp;
            for(auto y: x.second){
                for(auto z: y.second){
                    temp.push_back(z);
                }
            }
            ans.push_back(temp);
        }

        return ans;
    }
};
```

# [Top View of Binary Tree](https://www.geeksforgeeks.org/problems/top-view-of-binary-tree/1)

## Approach ->
Imagine if you do vertical order traversal and just include the first occurence of any vertical node then will that be your answer? yes. So in this question no need to keep track of your level, just keep track of your node and vertical and you're good.
Code this please while revising.

## Code ->
```cpp
class Solution {
public:
    // Function to return a list of nodes visible from the top view 
    // from left to right in Binary Tree.
    vector<int> topView(Node *root) {
        // Map to store the top view nodes with their vertical positions
        map<int, int> mp;
        // Queue for level order traversal
        queue<pair<Node*, int>> q;
        
        // Pushing the root node to the queue with its initial vertical position (0)
        q.push({root, 0});
        
        // Level order traversal
        while(q.size()) {
            auto it = q.front();
            q.pop();
            Node* curNode = it.first;
            int vertical = it.second;
            
            // If the vertical position is not present in the map, add the node's data to the map
            if (mp.find(vertical) == mp.end()) {
                mp[vertical] = curNode->data;
            }
            
            // Enqueue the left child with adjusted vertical position
            if (curNode->left) {
                q.push({curNode->left, vertical - 1});
            }
            
            // Enqueue the right child with adjusted vertical position
            if (curNode->right) {
                q.push({curNode->right, vertical + 1});
            }
        }
        
        // Constructing the final result from the map
        vector<int> ans;
        
        for (auto x : mp) {
            ans.push_back(x.second);
        }
        
        return ans;
    }
};
```

# [Bottom View of Binary Tree](https://www.geeksforgeeks.org/problems/bottom-view-of-binary-tree/1)

## Code ->
```cpp
class Solution {
  public:
    vector <int> bottomView(Node *root) {
        map<int, int> mp;
        queue<pair<Node*, int>> q;
        
        q.push({root, 0});
        
        // Level order traversal
        while(q.size()) {
            auto it = q.front();
            q.pop();
            Node* curNode = it.first;
            int vertical = it.second;
            
            // the last occuring vertical node would make the answer
            mp[vertical] = curNode->data;
            
            if (curNode->left) {
                q.push({curNode->left, vertical - 1});
            }
            
            if (curNode->right) {
                q.push({curNode->right, vertical + 1});
            }
        }
        
        vector<int> ans;
        
        for (auto x : mp) {
            ans.push_back(x.second);
        }
        
        return ans;
    }
};
```

# [662. Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/description/)

## Approach ->
[link](https://takeuforward.org/data-structure/maximum-width-of-a-binary-tree/)
Everything is same as of this approach in the link provided other than the "Prevention of Integer Overflow" part. I did the Prevention of Integer Overflow by taking unsigned long long integer values all around.

## Code ->
```cpp
class Solution {
public:
    // Using unsigned long long for larger range
    typedef unsigned long long ll;

    int widthOfBinaryTree(TreeNode* root) {
        // Queue to perform level order traversal
        queue<pair<TreeNode*, ll>> q;
        // Pushing the root node with its index (assuming 0-based indexing)
        q.push({root, 0});
        // Variable to store the maximum width
        ll ans = 0;

        while (q.size()) {
            // Get the number of nodes at the current level
            int size = q.size();
            // Get the index of the leftmost and rightmost nodes at the current level
            ll left =  q.front().second;
            ll right = q.back().second;

            // Update the maximum width
            ans = max(ans, right - left + 1);

            // Process nodes at the current level
            for (int i = 0; i < size; i++) {
                // Get the current node and its index
                TreeNode* node = q.front().first;
                ll idx = q.front().second;
                q.pop();

                // Enqueue the left child with adjusted index
                if (node->left) {
                    q.push({node->left, 2 * idx + 1});
                }

                // Enqueue the right child with adjusted index
                if (node->right) {
                    q.push({node->right, 2 * idx + 2});
                }
            }
        }

        // Convert the result to int before returning
        return static_cast<int>(ans);
    }
};
```