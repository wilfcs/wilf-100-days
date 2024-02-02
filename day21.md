
# [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/description/)

## Code ->
```cpp
class Solution {
public:
    // Helper function to traverse the binary tree and construct paths
    void helper(TreeNode* root, vector<string> &ans, string s) {
        // Base case: If the current node is NULL, return
        if (root == NULL) {
            return;
        }
        
        // If the current node is a leaf node (no left or right child),
        // append its value to the current path and add the path to the answer
        if (root->left == NULL && root->right == NULL) {
            s += to_string(root->val); // Convert int to string
            ans.push_back(s); // Add the current path to the answer
            return;
        }

        // If the current node has left or right child, append its value to
        // the current path and continue traversing left and right subtrees
        s += to_string(root->val) + "->"; // Append current node value to the path

        helper(root->left, ans, s); // Recursively traverse left subtree
        helper(root->right, ans, s); // Recursively traverse right subtree

        s.pop_back(); // Backtrack: Remove the last character (->) to backtrack in the path
    }

    // Main function to get all binary tree paths
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> ans; // Vector to store the paths
        helper(root, ans, ""); // Call helper function to populate the answer vector
        return ans; // Return the vector containing all binary tree paths
    }
};
```
# [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)

## Approaches ->
1. Brute Force: 
Make two vectors and store the path of both the target nodes. Now compare these two path vectors and return the last matching node. For example in example 2: pathOfP = [3,5] and pathOfQ = [3,5,2,4], so return 5. But here the space complexity will be O(2N).

2. Visit every node, if you find NULL return NULL, if you find the target return target, if for any recursion call one side is returning target or null just return target or null and if both sides are returning target then return the root itself. You won't understand this ik for sure. So here is the explanation, watch the dry run carefully and you'd understand [link](https://www.youtube.com/watch?v=_-QHfMDde90&list=PLkjdNRgDmcc0Pom5erUBU4ZayeU9AyRRu&index=27)

## Codes->
1. 
```cpp
class Solution {
public:
    // Helper function to find the path from the root to the target node in the tree
    bool findPath(TreeNode* root, TreeNode* target, vector<TreeNode*>& path) {
        // Base case: If the current node is NULL, return false
        if (root == NULL)
            return false;

        // Add the current node to the path
        path.push_back(root);

        // If the current node is the target node, return true
        if (root == target)
            return true;

        // Recursively search for the target node in the left and right subtrees
        if (findPath(root->left, target, path) || findPath(root->right, target, path))
            return true;

        // If the target node is not found in the current subtree, backtrack
        path.pop_back();
        return false;
    }

    // Main function to find the lowest common ancestor of two nodes in the binary tree
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // Vectors to store the paths from the root to nodes p and q
        vector<TreeNode*> pathOfP;
        vector<TreeNode*> pathOfQ;

        // Find the paths from the root to nodes p and q
        findPath(root, p, pathOfP);
        findPath(root, q, pathOfQ);

        // Initialize the result node as a placeholder
        TreeNode* ans = new TreeNode(-1);

        // Compare the paths to find the lowest common ancestor
        for (int i = 0; i < pathOfP.size(); i++) {
            // If one of the paths is shorter than the other, break the loop
            if (i == pathOfQ.size())
                break;

            // If the nodes in the paths are the same, update the result node
            if (pathOfP[i] == pathOfQ[i])
                ans = pathOfP[i];
            else
                break;
        }
        // Return the lowest common ancestor
        return ans;
    }
};
```
2.
```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // If the current node is NULL or either of the nodes is found, return the current node
        if (root == NULL || root == p || root == q)
            return root;

        // Recursively search for the nodes in the left and right subtrees
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);

        // If one of the nodes is not found in the left subtree, return the right subtree result even if its null
        if (left == NULL)
            return right;
        // If one of the nodes is not found in the right subtree, return the left subtree result
        else if (right == NULL)
            return left;
        // If both nodes are found in different subtrees, return the current node as the LCA
        else
            return root;
    }
};
```

# CN ->

- Types of Email Attacks:
	- Phishing:
		- What: Fraudulent emails pretending to be from reputable sources to extract personal data.
		- Goal: Obtain sensitive data, like passwords or credit card numbers.
		- Variants: Spear phishing (highly targeted), whaling (targets high-profile individuals).
	- Vishing:
		- What: Voice-based phishing usually via phone calls.
		- Goal: Trick victims into sharing personal information.
	- Smishing:
		- What: SMS-based phishing on mobile phones.
		- Goal: Deceive recipients into clicking malicious links or sharing data.
	- Whaling:
		- What: Phishing targeting high-value targets like executives.
		- Goal: Steal sensitive company data or facilitate fraudulent money transfers.
	- Pharming:
		- What: Redirecting users to a fake website mimicking a legitimate one.
		- Goal: Capture login credentials or other sensitive information.
	- Spyware:
		- What: Malicious software monitoring user's actions.
		- Goal: Collect data on user behavior, keystrokes, or modify system settings.
	- Scareware:
		- What: Fake warnings or alerts tricking users into downloading malicious software.
		- Goal: Deceive users into installing malware based on fabricated threats.
	- Adware:
		- What: Software displaying unwanted ads.
		- Goal: Generate ad revenue and possibly monitor user behavior for targeted ads.
	- Spam:
		- What: Unsolicited emails, mostly for advertising.
		- Goal: Advertise products, spread malware, or scam recipients.
- Some more terms to know about:
	- Cryptography: The science of protecting information by transforming it (encrypting it) into an unreadable format, called cipher text. Only those who possess a secret key can decipher (or decrypt) the message into plain text.
	- Symmetric Key: A type of encryption where the same key is used to both encrypt and decrypt the data. Examples include AES and DES.
	- Asymmetric Key: Uses a pair of keys: a public key, which can be shared openly, and a private key, which is kept secret. Data encrypted with the public key can only be decrypted by the private key and vice-versa. RSA is an example.
	- Hashing: A process that converts input data (of any size) into a fixed-length value. This value is typically used to verify data integrity. It's one-way, meaning you can't get back the original data from its hash. MD5 is an example of a hashing algorithm, though it's considered outdated and insecure.
	- Cryptanalysis: The study of methods for obtaining the meaning of encrypted information without access to the key used in the encryption.
	- AES: Advanced Encryption Standard, a symmetric encryption algorithm widely used and deemed very secure.
	- DES: Data Encryption Standard, an older symmetric encryption algorithm that was once standard but is now considered insecure due to vulnerabilities.
	- RSA: An asymmetric encryption algorithm based on the mathematical properties of large prime numbers. It's used for secure data transmission.
	- MD5 Hashing: A widely used cryptographic hash function that produces a 128-bit (16-byte) hash value, commonly represented as a 32-digit hexadecimal number. It's vulnerable to hash collisions and is not recommended for further use.