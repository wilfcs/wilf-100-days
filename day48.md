
# [Is Binary Tree Heap](https://www.geeksforgeeks.org/problems/is-binary-tree-heap/1)

## Approach ->
In the provided code, the isHeap function checks whether a given binary tree follows the max heap property. The approach involves three helper functions: countNodes to calculate the total number of nodes, isCBT to verify if the tree is a complete binary tree, and isMaxOrder to ensure it adheres to the max heap ordering. The key idea is to validate completeness first, and then verify the max heap property recursively. If both conditions hold, the tree is deemed a max heap.

## Code ->
```cpp
class Solution {
public:
    // Helper function to count the number of nodes in the binary tree
    int countNodes(struct Node* tree) {
        if (tree == NULL) return 0;
        return 1 + countNodes(tree->left) + countNodes(tree->right);
    }

    // Helper function to check if the binary tree is a complete binary tree
    bool isCBT(struct Node* tree, int index, int cnt) {
        if (tree == NULL) return true;

        if (index >= cnt) return false;

        bool left = isCBT(tree->left, index * 2 + 1, cnt);
        bool right = isCBT(tree->right, index * 2 + 2, cnt);

        return left && right;
    }

    // Helper function to check if the binary tree follows max heap property
    bool isMaxOrder(struct Node* tree) {
        if (tree->left == NULL && tree->right == NULL) return true;

        if (tree->right == NULL) {
            return tree->data > tree->left->data;
        } else {
            bool left = isMaxOrder(tree->left);
            bool right = isMaxOrder(tree->right);

            return (left && right && tree->data > tree->left->data && tree->data > tree->right->data);
        }
    }

    // Main function to check if the binary tree is a max heap
    bool isHeap(struct Node* tree) {
        int index = 0;
        int totalCount = countNodes(tree);
        
        // Check if the binary tree is a complete binary tree and follows max heap property
        if (isCBT(tree, index, totalCount) && isMaxOrder(tree)) return true;
        else return false;
    }
};
```

# [Merge two binary Max heaps](https://www.geeksforgeeks.org/problems/merge-two-binary-max-heap0144/1)

## Approach ->
Just merge the two given arrays and then heapify them to get your answer.

## Code ->
```cpp
class Solution{
public:
    // Function to heapify a subtree rooted at node i in the given vector 'a' of size 'n'
    void heapify(vector<int> &a, int i, int n){
        int largest = i;
        int left = i * 2 + 1;
        int right = i * 2 + 2;

        // Compare with left child
        if (left < n && a[left] > a[largest])
            largest = left;

        // Compare with right child
        if (right < n && a[right] > a[largest])
            largest = right;

        // If the largest is not the current node, swap and recursively heapify
        if (i != largest){
            swap(a[i], a[largest]);
            heapify(a, largest, n);
        }
    }

    // Function to merge two binary max heaps represented by vectors 'a' and 'b' of sizes 'n' and 'm'
    vector<int> mergeHeaps(vector<int> &a, vector<int> &b, int n, int m) {
        // Merge both heaps into one vector 'a'
        for (int i = 0; i < m; i++)
            a.push_back(b[i]);

        // Build heap using the merged array
        for (int i = (n + m) / 2 + 1; i >= 0; i--){
            heapify(a, i, a.size());
        }

        return a;
    }
};
```

So during heapifying the array why do we run the loop from size/2-1? That's because when heapifying an array to maintain the heap property, we start the loop from size/2-1 because all the elements beyond this point are leaf nodes in the binary heap. Since leaf nodes don't have children, there is no need to perform heapification on them, making the process more efficient by avoiding unnecessary operations on nodes that won't affect the heap structure.

# [Minimum Cost of ropes](https://www.geeksforgeeks.org/problems/minimum-cost-of-ropes-1587115620/1)

## Code ->
```cpp
class Solution
{
    public:
    //Function to return the minimum cost of connecting the ropes.
    long long minCost(long long arr[], long long n) {
        // Your code here
        priority_queue<long long, vector<long long>, greater<long long>> min_heap(arr, arr+n);

        long long ans = 0;
        
        while(min_heap.size()>1){
            long long n1 = min_heap.top();
            min_heap.pop();
            long long n2 = min_heap.top();
            min_heap.pop();
            
            ans+=n1+n2;
            min_heap.push(n1+n2);
        }
        
        return ans;
    }
};
```

# [K-th Largest Sum Subarray](https://www.codingninjas.com/studio/problems/k-th-largest-sum-contiguous-subarray_920398?leftPanelTabValue=PROBLEM)

## Approach ->
1. Run a n^2 loop and store the subarray sum in a vector. Then sort the vector to find your answer. The complexity analysis is in the code itself.
2. Use a min heap to find the kth largest sum subarray. Find out yourself by looking at the code how we did that.

## Code ->
1. 
```cpp
#include <bits/stdc++.h>
int getKthLargest(vector<int> &arr, int k)
{
	//	Write your code here.
	vector<int> ans;


	for(int i=0; i<arr.size(); i++){
		int sum = 0;
		for(int j=i; j<arr.size(); j++){
			sum+=arr[j];
			ans.push_back(sum);
		}
	}

	sort(ans.begin(), ans.end());

	return ans[ans.size()-k];
}

// TC -> O(n^2 * log(n^2)) = O(2 * n^2 * log(n)) = O(n^2 * log(n))
// This is because sorting TC is O(n log n) and the ans array that we are sorting is already of size n^2.
//SC->O(n^2)
```
2. 
```cpp
#include <bits/stdc++.h>
int getKthLargest(vector<int> &arr, int k)
{
	//	Write your code here.
	priority_queue<int, vector<int>, greater<int>> min_heap;

	for(int i=0; i<arr.size(); i++){
		int sum = 0;
		for(int j=i; j<arr.size(); j++){
			sum+=arr[j];

			if(min_heap.size()<k) min_heap.push(sum);
			else{
				if(min_heap.top()<sum){
					min_heap.pop();
					min_heap.push(sum);
				}
			}
		}
	}
	return min_heap.top();
}

//  TC->O(n^2 * log(k))
// SC->O(n)
```



# [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/description/)

## Code ->
```cpp

class Solution {
public:
    ListNode *mergeKLists(vector<ListNode *> &lists) {
        if(lists.empty()){
            return nullptr;
        }
        while(lists.size() > 1){
            lists.push_back(mergeTwoLists(lists[0], lists[1]));
            lists.erase(lists.begin());
            lists.erase(lists.begin());
        }
        return lists.front();
    }
    ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
        if(l1 == nullptr){
            return l2;
        }
        if(l2 == nullptr){
            return l1;
        }
        if(l1->val <= l2->val){
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        }
        else{
            l2->next = mergeTwoLists(l1, l2->next);
            return l2;
        }
    }
};
```