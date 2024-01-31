
# [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/description/)

## Code ->
```cpp
class Solution {
public:
    int ans = INT_MIN;

    int helper(TreeNode* root){
        if(root==NULL) return 0;

        // Calculate the maximum path sum in the left and right subtrees. If any node is making the sum as negative then it's better to not include that negative number, so we will take 0 instead. That's why we have taken the max bw 0 and sum returned.
        int l = max(0, helper(root->left));
        int r = max(0, helper(root->right));

        // Update the global maximum path sum using the current node + left subtree sum + right subtree sum
        ans = max(ans, l+r+root->val);

        // Return the maximum path sum starting from the current node to its parent
        return root->val + max(l, r);
    }
    int maxPathSum(TreeNode* root) {
        helper(root);
        return ans;
    }
};
```

# [100. Same Tree](https://leetcode.com/problems/same-tree/description/)

## Code ->
```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(p==NULL && q==NULL) return true;
        if(p==NULL || q==NULL) return false;
        if(p->val != q->val) return false;

        bool l = isSameTree(p->left, q->left);
        bool r = isSameTree(p->right, q->right);

        return (l&&r);
    }
};
```

# [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/)

## Approach ->
Simple level order traversal but on odd levels reverse the level vector. Don't increase the TC of the code by using the reverse stl, instead insert into the level vector in reverse manner by keeping a flag variable. Look at the code properly.

## Code ->
```cpp
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        queue<TreeNode*> q;

        // Boolean flag to track whether the current level is even or odd
        bool isEven = true;

        // Check if the tree is empty to avoid runtime error 
        if(root==NULL) return ans;

        q.push(root);

        while(q.size()){
            int size = q.size();
            vector<int> level(size);

            for(int i=0; i<size; i++){
                TreeNode* node = q.front();
                q.pop();
                
                if(node->left != NULL) q.push(node->left);
                if(node->right != NULL) q.push(node->right);
                
                // Determine the index to insert the node's value based on whether the level is even or odd
                int index = isEven ? i : size-i-1;
                // Insert the node's value into the level vector at the calculated index
                level[index]=node->val;
            }
            // Toggle the isEven flag
            isEven = !isEven;
            ans.push_back(level); 
        }

        return ans;
    }
};
```

# [Boundary Traversal of Binary Tree](https://www.codingninjas.com/studio/problems/boundary-traversal-of-binary-tree_790725?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Approach ->
Traverse the left boundary of the tree and add its elements to the resultant vector except the leaf nodes, then add the leaf nodes to the resultant, then traverse the right boundary and add the reverse of it to the resultant vector other than the leaf nodes. Follow iterative approach to traverse the boundaries to avoid conflict in answer.

## Code ->
```cpp
// Check if the given node is a leaf node
bool isLeaf(TreeNode<int> *root) {
  return !root->left && !root->right;
}

// Add the left boundary nodes to the result vector
void addLeftBoundary(TreeNode<int> *root, vector<int> &res) {
  TreeNode<int> *cur = root->left;
  while (cur) {
    // If the current node is not a leaf, add it to the result
    if (!isLeaf(cur)) res.push_back(cur->data);
    // Move to the left child if it exists; otherwise, move to the right child
    if (cur->left) cur = cur->left;
    else cur = cur->right;
  }
}

// Add the right boundary nodes to the result vector
void addRightBoundary(TreeNode<int> *root, vector<int> &res) {
  TreeNode<int> *cur = root->right;
  vector<int> tmp;
  while (cur) {
    // If the current node is not a leaf, add it to a temporary vector
    if (!isLeaf(cur)) tmp.push_back(cur->data);
    // Move to the right child if it exists; otherwise, move to the left child
    if (cur->right) cur = cur->right;
    else cur = cur->left;
  }
  // Add the nodes from the temporary vector in reverse order to maintain anti-clockwise order
  for (int i = tmp.size() - 1; i >= 0; --i) {
    res.push_back(tmp[i]);
  }
}

// Recursively add leaf nodes to the result vector
void addLeaves(TreeNode<int> *root, vector<int> &res) {
  // If the current node is a leaf, add it to the result
  if (isLeaf(root)) {
    res.push_back(root->data);
    return;
  }
  // Recursively process left and right children
  if (root->left) addLeaves(root->left, res);
  if (root->right) addLeaves(root->right, res);
}

// Main function to traverse the boundary nodes in anti-clockwise order
vector<int> traverseBoundary(TreeNode<int> *root) {
  vector<int> res;
  // Return an empty vector if the tree is empty
  if (!root) return res;

  // Add the root node to the result if it is not a leaf
  if (!isLeaf(root)) res.push_back(root->data);

  // Add the left boundary nodes
  addLeftBoundary(root, res);

  // Add leaf nodes
  addLeaves(root, res);

  // Add the right boundary nodes
  addRightBoundary(root, res);

  return res;
}
```

Pending -> vertical order traversal, top view, bottom view of bt

# [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/description/)

## Approaches ->
1. You can do the level order traversal and put the last node of each level in your answer vector.
2. Better approach would be to use recursion to do so to avoid the extra space (auxiliary space would be O(log n) or O(Height) in avg case due to recursive stack and O(n) in worst case if the tree is skewed). In this approach simply maintain a variable to determine the level of the BT. Maintian a DS to store the visited levels. Call the recursion for the right side of the tree and then the left side. When a level is visited for the first time, it signifies the discovery of a node from the right side at that level, so push that node's value in our answer.  Also push that level in the DS to ensure that it has been visited the first time.

## Codes ->
1.
```cpp
class Solution {
public:
    vector <int> ans;
    void helper(TreeNode* root){
        if(root == NULL) return;
        queue <TreeNode*> q;
        q.push(root);
        while(q.size()){
            int size = q.size();
            // vector <int> temp;
            for(int i=0; i<size; i++){
                TreeNode* nnode = q.front();
                q.pop();
                if(nnode->left) q.push(nnode->left);
                if(nnode->right) q.push(nnode->right);

                // if it is the last node of the current level then push it in ans
                if(i==size-1) ans.push_back(nnode->val);
            }
            // ans.push_back(temp[temp.size()-1]);
        }  
    }
    vector<int> rightSideView(TreeNode* root) {
        helper(root);
        return ans;
    }
};
```

2.
```cpp
class Solution {
public:
    void helper(TreeNode* root, vector<int> &ans, vector<int> &visitedLevels, int curLevel){
        if(root==NULL) return;

        // if the level is visited for the first time, enter it into the visitedLevels and push the node's val in ans.
        if(visitedLevels.size()==curLevel){
            ans.push_back(root->val);
            visitedLevels.push_back(curLevel);
        }

        // call recursion for the right side and then the left side while increasing the size of the curLevel by 1
        helper(root->right, ans, visitedLevels, curLevel+1);
        helper(root->left, ans, visitedLevels, curLevel+1);
    }
    vector<int> rightSideView(TreeNode* root) {
        vector<int> ans;
        vector<int> visitedLevels;

        helper(root, ans, visitedLevels, 0);
        return ans;
    }
};
```
# [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/description/)

## Code ->
```cpp
class Solution {
public:
    // Recursive helper function to check if two subtrees are symmetric
    bool helper(TreeNode* t1, TreeNode* t2) {
        // Base case: Both nodes are NULL, indicating symmetry
        if (t1 == NULL && t2 == NULL) {
            return true;
        }
        // Base case: One of the nodes is NULL, indicating asymmetry
        if (t1 == NULL || t2 == NULL || t1->val != t2->val) {
            return false;
        }

        // Recursive call for comparing left subtree of t1 with right subtree of t2
        // and right subtree of t1 with left subtree of t2
        return (helper(t1->left, t2->right) && helper(t1->right, t2->left));
    }

    // Main function to check if a binary tree is symmetric
    bool isSymmetric(TreeNode* root) {
        // Call the helper function to compare the left subtree with the right subtree
        return helper(root->left, root->right);
    }
};
```

# CN->
- Denial of Service attack and its prevention:
	- A Denial of Service (DoS) attack is an attempt to make a machine, network resource, or service unavailable to its intended users by temporarily or indefinitely disrupting services of a host connected to the Internet. This is achieved by overwhelming the target or its surrounding infrastructure with a flood of internet traffic.
	- When it's executed from multiple devices, often compromised in a botnet, it's called a Distributed Denial of Service (DDoS) attack.
	- Prevention:
		- Cloud Mitigation Provider – Cloud mitigation providers are experts at providing DDoS mitigation from the cloud. This means they have built out massive amounts of network bandwidth and DDoS mitigation capacity at multiple sites around the Internet that can take in any type of network traffic. They can scrub the traffic for you and only send “clean” traffic to your data center.
		- Firewall – This is the simplest and least effective method. Python scripts are often written to filter out malicious traffic, or existing firewalls can be utilized by enterprises to block such traffic.
		- Internet Service Provider (ISP) – Some enterprises use their ISP to provide DDoS mitigation. These ISPs have more bandwidth than an enterprise would, which can help with large volumetric attacks.
		- Implement Content Delivery Network (CDN): A CDN can help to distribute traffic and reduce the impact of a DoS attack by distributing the load across multiple servers.
		- Network Segmentation: Segmenting the network can help prevent a DoS attack from spreading throughout the entire network. This limits the impact of an attack and helps to isolate the affected systems.
		- Use Intrusion Detection and Prevention Systems
		- Use Anti-Malware Software
		- Perform Regular Network Scans
- Digital Signatures and Certificates:
  
	- Digital Signatures:
	  A digital signature is a cryptographic technique used to verify the authenticity and integrity of a message, software, or digital document. It's the digital counterpart to a handwritten signature or stamped seal, but much more secure. It provides proof of the origin, identity, and status of an electronic document, transaction, or message and confirms the signer's consent.
	  How Digital Signatures Work:
		- The signing software/algorithm creates a hash of the document/data.
		- This hash is encrypted with the signer's private key to create the digital signature.
		- The signed document and the digital signature are sent to the recipient.
		- The recipient decrypts the signature using the signer's public key to obtain the hash.
		- The recipient generates a new hash of the received document.
		- If the new hash matches the decrypted hash, the signature is valid; if not, it's either invalid or the document has been altered.
	- Digital Certificates:
	  A digital certificate, often known as a public key certificate or identity certificate, is a digital document that uses a digital signature to bind a public key with an identity — this identity could be an individual, organization, server, or application. It provides a means of proving your identity in electronic transactions, much like a driver's license or a passport does in offline transactions.
	  Components of a Digital Certificate include:
		1. Subject's public key.
		2. Subject's name or alias.
		3. Issuer (the Certificate Authority that issued the digital certificate).
		4. Serial number.
		5. Expiration date.
		6. Name of the algorithm used for creating the digital signature.
		7. Digital signature of the issuer.
	- Certificate Authorities (CAs): CAs are trusted entities that issue digital certificates. The role of the CA in this process is to guarantee that the individual granted the unique certificate is, in fact, who he or she claims to be. Popular CAs include VeriSign, DigiCert, and Let's Encrypt.
	- Purpose and Use:
		- Authentication: The primary purpose of a digital certificate is to ensure that the public key contained in the certificate belongs to the entity it claims to belong to.
		- Secure Communications: In the context of website security, certificates are used in conjunction with the SSL/TLS protocol to establish a secure channel.
		- Digital Signatures: Digital certificates facilitate the use of digital signatures, providing assurance that the signed data has not been altered and ensuring the signer's identity.
- Active and Passive attacks:
	- Active Attacks: These are attacks where the attacker intervenes, intercepts, or alters the data being transmitted. It's about doing something actively to interfere with information. Examples include:
		- Masquerading: Pretending to be an authorized user to gain certain privileges.
		- Modification: Changing the content of the information being exchanged.
		- Replay: Intercepting and retransmitting a message.
		- Denial of Service (DoS): Preventing genuine users from accessing a service.
	- Passive Attacks: These are more about listening and gathering information. Attackers eavesdrop but do not modify the actual data. The primary purpose is to gain unauthorized knowledge. Examples include:
		- Eavesdropping: Listening to communications, often over unsecured channels.
		- Traffic Analysis: Analyzing the traffic to gather patterns or meta-information without necessarily understanding the content of the data.