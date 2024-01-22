# [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/description/)

## Approaches ->
1. Hashmap with extra space
2. Optimal: The optimisation will be in removing the extra spaces, i.e, the hashmap used in brute force. We are going to create dummy nodes and place the dummy nodes in between of two actual nodes. Here the dummy node placed after the actual node represents the copy of that actual node. This way, the new nodes carry both the value and connections. To understand this imagine placing dummy node in between of actual nodes like this..

A -> D -> A -> D -> NULL

Using a smart pointer (let's call it 'temp'), we link the random pointers in the dummy nodes based on the original node.

To separate the original and copied lists, we use two pointers: one for the dummy node, and one for the original list

By iterating through this process, we efficiently create a deep copy of the linked list without the need for extra storage. The result is the copied list, and we return it. Read the code to understand properly


## Codes ->
1. 
```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        // Create a hash map to store the mapping between original nodes and their corresponding new nodes
        unordered_map<Node*, Node*> mp;

        // First iteration: Create new nodes with the same values as the original nodes and store them in the hash map
        Node* temp = head;
        while (temp) {
            // Key: Original node, Value: New node with the same value
            mp[temp] = new Node(temp->val);
            temp = temp->next;
        }

        // Reset temp to the head of the original list for the second iteration
        temp = head;

        // Second iteration: Link next and random pointers of new nodes based on the mapping in the hash map
        while (temp) {
            // Link the next and random pointers of the new nodes using the hash map
            mp[temp]->next = mp[temp->next];
            mp[temp]->random = mp[temp->random];
            temp = temp->next;
        }

        // Return the head of the new linked list (value node of the original head node)
        return mp[head];
    }
};
```

2. 
```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        // Check if the original list is empty
        if (head == NULL) return NULL;

        // Step 1: Create deep nodes next to the original nodes
        Node* temp = head;
        while (temp) {
            // Create a new node with the same value as the current node
            Node* dummy = new Node(temp->val);

            // Insert the new node next to the original node
            dummy->next = temp->next;
            temp->next = dummy;

            // Move to the next pair of original nodes
            temp = temp->next->next;
        }

        // Initialize pointers for the second pass
        temp = head->next; // Points to the first deep copy node
        Node* prev = head;  // Points to the original list
        Node* toReturn = head->next; // Points to the head of the copied list

        // Step 2: Set random pointers for the deep copy nodes
        while (temp) {
            // Set random pointers based on the original list
            if (prev->random == NULL) temp->random = NULL;
            else temp->random = prev->random->next;

            // Break if there are no more nodes to process
            if (temp->next == NULL) break;

            // Move to the next pair of nodes
            temp = temp->next->next;
            prev = prev->next->next;
        }

        // Reset pointers for the third pass
        temp = head->next; // Reset to the first deep copy node
        prev = head; // Reset to the original list

        // Step 3: Separate the original and copied lists
        while (temp->next) {
            // Adjust next pointers to separate the lists
            prev->next = prev->next->next;
            temp->next = temp->next->next;

            // Move to the next pair of nodes
            prev = prev->next;
            temp = temp->next;
        }

        // Handle the last node in the original and copied list here to avoid runtime error
        prev->next = prev->next->next;
        temp->next = NULL;

        // Return the head of the copied list
        return toReturn;
    }
};
```

# [1472. Design Browser History](https://leetcode.com/problems/design-browser-history/)

## Code ->
```cpp
// Define a structure for nodes to represent pages in the browser history. You have to do this yourself in the question, there is no template already made for it. 
struct Node {
    string val;   // URL value
    Node* back = NULL;  // Pointer to the previous page in history
    Node* next = NULL;  // Pointer to the next page in history
};

// Implement the BrowserHistory class
class BrowserHistory {
public:
    Node * currentPage;  // Pointer to the current page in the browser history

    // Constructor to initialize the object with the homepage of the browser
    BrowserHistory(string homepage) {
        // Create a new node for the homepage and set it as the current page
        currentPage = new Node();
        currentPage->val = homepage;
    }
    
    // Function to visit a new URL, creating a new page and updating the history
    void visit(string url) {
        // Create a new node for the visited URL
        Node *newPage = new Node();
        newPage->val = url;

        // Update pointers to link the new page to the current page
        currentPage->next = newPage;
        newPage->back = currentPage;

        // Update the current page to the newly visited page
        currentPage = newPage;
    }
    
    // Function to move back in history by a given number of steps
    string back(int steps) {
        // Move back in history until the specified number of steps or until the beginning of history
        while (steps-- && currentPage->back) {
            currentPage = currentPage->back;
        }

        // Return the URL of the current page after moving back
        return currentPage->val;
    }
    
    // Function to move forward in history by a given number of steps
    string forward(int steps) {
        // Move forward in history until the specified number of steps or until the end of history
        while (steps-- && currentPage->next) {
            currentPage = currentPage->next;
        }

        // Return the URL of the current page after moving forward
        return currentPage->val;
    }
};
```

# Selection Sort:
## Approach->
Selection sort is a simple and efficient sorting algorithm that works by repeatedly selecting the smallest (or largest) element from the unsorted portion of the list and moving it to the sorted portion of the list. 
Lets consider the following array as an example: arr[] = {64, 25, 12, 22, 11}

First pass:

For the first position in the sorted array, the whole array is traversed from index 0 to 4 sequentially. The first position where 64 is stored presently, after traversing whole array it is clear that 11 is the lowest value.
Thus, replace 64 with 11. After one iteration 11, which happens to be the least value in the array, tends to appear in the first position of the sorted list.

New array after first pass: arr[] = {11,     25, 12, 22, 64}

Second Pass:

For the second position, where 25 is present, again traverse the rest of the array in a sequential manner.
After traversing, we found that 12 is the lowest value in the array and it should appear at the second place in the array where i is, thus swap these values.

Continue this till the array is sorted

## Code ->
```cpp
for (int i = 0; i < n - 1; i++) {
    int mini = i;
    for (int j = i + 1; j < n; j++) {
      if (arr[j] < arr[mini]) {
        mini = j;
      }
    }
    swap(arr[i], arr[mini]);
}
```

# Bubble Sort:
## Approach->
In Bubble Sort algorithm, 
traverse from left and compare adjacent elements and the higher one is placed at right side. 
In this way, the largest element is moved to the rightmost end at first. 
This process is then continued to find the second largest and place it and so on until the data is sorted.

## Code->
```cpp
void bubbleSort(int arr[], int n)
{
    int i, j;
    bool swapped;
    for (i = 0; i < n - 1; i++) {
        swapped = false;
        for (j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
 
        // If no two elements were swapped by inner loop, that means the array is already completely sorted
        if (swapped == false)
            break;
    }
}
```
# Insertion sort
## Approach ->
The idea is to insert the selected element at its right position. 
- Select an element in each iteration from the unsorted array(using a loop).
- Place it in its corresponding position in the sorted part and shift the remaining elements accordingly (using an inner loop and swapping).
- The “inner while loop” basically shifts the elements using swapping.

## Code->
```cpp
for (int i = 0; i <= n - 1; i++) {
    int j = i;
    while (j > 0 && arr[j - 1] > arr[j]) {
        swap(arr[j-1], arr[j]);
        j--;
    }
}
```

# Merge Sort

## Approach ->
- Merge Sort is a divide and conquers algorithm, it divides the given array into equal parts and then merges the 2 sorted parts. 
- There are 2 main functions :
-  merge(): This function is used to merge the 2 halves of the array. It assumes that both parts of the array are sorted and merges both of them.
-  mergeSort(): This function divides the array into 2 parts. low to mid and mid+1 to high where,

```
low = leftmost index of the array

high = rightmost index of the array

mid = Middle index of the array 
```

We recursively split the array, and go from top-down until all sub-arrays size becomes 1.

---
## Code ->
```cpp
#include <bits/stdc++.h>
using namespace std;

void merge(vector<int> &arr, int low, int mid, int high) {
    vector<int> temp; // temporary array
    int left = low;      // starting index of left half of arr
    int right = mid + 1;   // starting index of right half of arr

    //storing elements in the temporary array in a sorted manner//

    while (left <= mid && right <= high) {
        if (arr[left] <= arr[right]) {
            temp.push_back(arr[left]);
            left++;
        }
        else {
            temp.push_back(arr[right]);
            right++;
        }
    }

    // if elements on the left half are still left //

    while (left <= mid) {
        temp.push_back(arr[left]);
        left++;
    }

    //  if elements on the right half are still left //
    while (right <= high) {
        temp.push_back(arr[right]);
        right++;
    }

    // transfering all elements from temporary to arr //
    for (int i = low; i <= high; i++) {
        arr[i] = temp[i - low];
    }
    // instead of using i-low we could have created another variable x=0 and could have done temp[x++]
}

void mergeSort(vector<int> &arr, int low, int high) {
    if (low >= high) return;
    int mid = (low + high) / 2 ;
    mergeSort(arr, low, mid);  // left half
    mergeSort(arr, mid + 1, high); // right half
    merge(arr, low, mid, high);  // merging sorted halves
}

int main() {

    vector<int> arr = {9, 4, 7, 6, 3, 1, 5}  ;
    int n = 7;

    cout << "Before Sorting Array: " << endl;
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " "  ;
    }
    cout << endl;
    mergeSort(arr, 0, n - 1);
    cout << "After Sorting Array: " << endl;
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " "  ;
    }
    cout << endl;
    return 0 ;
}
```
Time complexity: O(nlogn) 

Reason: At each step, we divide the whole array into two halves, for that logn and we assume n steps are taken to get sorted array, so overall time complexity will be nlogn

Space complexity: O(n)  

Reason: We are using a temporary array to store elements in sorted order.

# Quick Sort

## Approach/Intuition ->
Quick Sort is a divide-and-conquer algorithm like the Merge Sort. But unlike Merge sort, this algorithm does not use any extra array for sorting(though it uses an auxiliary stack space). So, from that perspective, Quick sort is slightly better than Merge sort.

This algorithm is basically a repetition of two simple steps that are the following:

Pick a pivot and place it in its correct place in the sorted array.
Shift smaller elements(i.e. Smaller than the pivot) on the left of the pivot and larger ones to the right.
Now, let’s discuss the steps in detail considering the array {4,6,2,5,7,9,1,3}:

Step 1: The first thing is to choose the pivot. A pivot is basically a chosen element of the given array. The element or the pivot can be chosen by our choice. So, in an array a pivot can be any of the following:

The first element of the array
The last element of the array
Median of array
Any Random element of the array
After choosing the pivot(i.e. the element), we should place it in its correct position(i.e. The place it should be after the array gets sorted) in the array. For example, if the given array is {4,6,2,5,7,9,1,3}, the correct position of 4 will be the 4th position.

Note: Here in this tutorial, we have chosen the first element as our pivot. You can choose any element as per your choice.

Step 2: In step 2, we will shift the smaller elements(i.e. Smaller than the pivot) to the left of the pivot and the larger ones to the right of the pivot. In the example, if the chosen pivot is 4, after performing step 2 the array will look like: {3, 2, 1, 4, 6, 5, 7, 9}. 

From the explanation, we can see that after completing the steps, pivot 4 is in its correct position with the left and right subarray unsorted. Now we will apply these two steps on the left subarray and the right subarray recursively. And we will continue this process until the size of the unsorted part becomes 1(as an array with a single element is always sorted).

So, from the above intuition, we can get a clear idea that we are going to use recursion in this algorithm.

To summarize, the main intention of this process is to place the pivot, after each recursion call, at its final position, where the pivot should be in the final sorted array.

## Code ->
```cpp
#include <bits/stdc++.h>
using namespace std;

// Function to partition the array and return the pivot index
int partition(vector<int> &arr, int low, int high) {
    // Choose the first element as the pivot
    int pivot = arr[low];
    int i = low;
    int j = high;

    // Move elements smaller than the pivot to the left and larger to the right
    while (i < j) {
        while (arr[i] <= pivot && i <= high - 1) {
            i++;
        }

        while (arr[j] > pivot && j >= low + 1) {
            j--;
        }

        // Swap arr[i] and arr[j] if they are out of order
        if (i < j) swap(arr[i], arr[j]);
    }

    // Swap the pivot element to its correct position
    swap(arr[low], arr[j]);

    // Return the index of the pivot element
    return j;
}

// Quicksort recursive function
void qs(vector<int> &arr, int low, int high) {
    // Base case: if the partition size is greater than 1
    if (low < high) {
        // Find the pivot index such that elements on the left are smaller and on the right are larger
        int pIndex = partition(arr, low, high);

        // Recursively sort the subarrays on the left and right of the pivot
        qs(arr, low, pIndex - 1);
        qs(arr, pIndex + 1, high);
    }
}

// Wrapper function for quicksort
vector<int> quickSort(vector<int> arr) {
    // Call the quicksort function with the entire array
    qs(arr, 0, arr.size() - 1);

    // Return the sorted array
    return arr;
}

int main() {
    // Sample array for testing
    vector<int> arr = {4, 6, 2, 5, 7, 9, 1, 3};
    int n = arr.size();

    // Display the array before using quicksort
    cout << "Before Using Quick Sort: " << endl;
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    // Call the quicksort function and display the sorted array
    arr = quickSort(arr);
    cout << "After Using Quick Sort: " << "\n";
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << "\n";

    return 0;
}
```
- Time Complexity: O(N*logN), where N = size of the array.
- Space Complexity: O(1) + O(N) auxiliary stack space.

# [189. Rotate Array](https://leetcode.com/problems/rotate-array/description/)

## Approaches->
1. Make a temp array, place items in it accordingly, copy the temp array's elements to original array. SC-> O(N).
2. Reverse the array from start to end. Now reverse from start to k-1th index. Now reverse from kth index to end. Dry run.

## Code ->
```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        if(nums.size()<k) k = k%nums.size();

        // reverse the entire array
        reverse(nums.begin(), nums.end()); 

        // now reverse from start to index k-1
        // note: we used nums.begin()+k and not nums.begin()+k-1 because for reverse, the ending index is exclusive
        reverse(nums.begin(), nums.begin()+k);
        
        // reverse from kth index to the end
        // note: we still used nums.begin()+k because the starting index is inclusive
        reverse(nums.begin()+k, nums.end());
    }
};
```

# [Superior Elements](https://www.codingninjas.com/studio/problems/superior-elements_6783446?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Approaches->
1. Brute force
2. Make a postfix array with maximum number from right and compare in the original array from the left
3. You don't actually need to make the postfix array, instead you can maintain just a maxium element integer, traverse the given array from the back and compare the elements with the maximum element integer. 

## Code ->
```cpp
vector<int> superiorElements(vector<int>&a) {
    // Write your code here.
    int maxi = INT_MIN;
    vector<int> ans;
    for(int i=a.size()-1; i>=0; i--){
        if(a[i]>maxi){
            ans.push_back(a[i]);
            maxi = a[i];
        }
    }
    return ans;
}
```

# [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/description/)

## Approaches->
1. Brute Force
2. Prefix-Suffix method: Just maintain two integers prefix and suffix of multiplication and keep a maxi variable to store maximum. TC-> O(n) SC-> O(1)

## Code ->
```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int maxi = INT_MIN, prefix = 1, suffix = 1, n=nums.size();

        for(int i=0; i<nums.size(); i++){
            // make prefix and suffix 1 if it came out to be 0 beacuse if it remains 0 it will give 0 for anything afterwards
            if(prefix==0) prefix=1;
            if(suffix==0) suffix=1;

            // find prefix and suffix
            prefix = nums[i]*prefix;
            suffix = nums[n-i-1]*suffix;

            // update maxi
            maxi = max(maxi, max(prefix, suffix));
        }
        return maxi;
    }
};
```

# CN->
- What is load balancer:
	- A load balancer is a device or software application that distributes incoming network traffic across multiple servers. By doing so, it ensures that no single server is overwhelmed with too much demand, which can help optimize resource use, maximize throughput, reduce latency, and ensure fault-tolerant system configurations.
	- Benefits of a Load Balancer: 
		- Redundancy and Reliability: If one server fails, the load balancer redirects traffic to the remaining operational servers, ensuring services remain uninterrupted.
		- Scalability: As traffic grows, more servers can be added to the load-balancing routine. This allows for horizontal scalability, where additional machines are added to distribute the load, rather than increasing the capacity of a single machine.
		- Efficient Resource Utilization: Preventing any single server from being a bottleneck.
		- Reduced Latency: By directing users to the least busy or closest server
		- Flexibility: For example, read and write requests can be directed to different servers if needed.
		- No down time while maintenance
- Types of IP Addresses:
	- There are two types of IP addresses:
		1. Public IP Address: This is the IP address assigned to your home or business network by your Internet Service Provider (ISP). When devices on your network communicate with the outside world (like visiting a website), they all share this single public IP. This is often what people refer to when they say a "network has an IP address."
		2. Private IP Address: Within your home or business network, each device is assigned its own unique IP address so that they can communicate with each other. This assignment is typically done by a device called a router using a service called DHCP (Dynamic Host Configuration Protocol). These IP addresses are only valid within the local network and can't be used directly on the wider internet.
- ARP (Address Resource Protocol):
	- ARP, or the Address Resolution Protocol, is a fundamental protocol used in local area networks (LAN) to convert an IP address into a physical address, typically a MAC address.
	- ARP's main function is to map 32-bit IP addresses to MAC addresses within a local network. This mapping ensures that networked devices can communicate with each other effectively within a local network segment.
	- How ARP works:
		- Your computer (Device A) wants to communicate with another computer (Device B) on the same local network, so it first checks its ARP cache (a table stored in memory) to see if it already knows the MAC address corresponding to that IP address.
		- Device A knows the IP address (private IP Address) of Device B but not its MAC address.
		- Device A asks everyone on the network, "Who has this IP address?"[
		- Device B responds, "That's me, and here's my MAC address!". This is called an ARP reply.
		- Once the Device A receives the MAC address of Device B, it updates its ARP cache with the IP-to-MAC address mapping so that it doesn't have to make the same request in the near future and the communication between the two devices can then proceed at the data link layer using the MAC address.
- Horizontal vs Vertical Scaling:
	- Vertical Scaling (Scaling Up):
	  
		- Definition: Vertical scaling involves adding more resources to an individual server, such as increasing the amount of RAM, CPU, or storage.
		- Advantages: 
			- Simplicity: Doesn't require changes in the application's code.
			- Immediate Performance Boost: System immediately benefits from increased power.
		- Disadvantages:
			- Downtime: Server downtime.
			- Limits of Hardware: There's an upper limit to how much you can scale vertically.
			- Cost: High-end hardware can be expensive.
			- Single Point of Failure: Even if you scale a server vertically, it remains a single machine. If it fails, your service goes down.
	- Horizontal Scaling (Scaling Out):
	  
		- Definition: Horizontal scaling involves adding more machines to the system, distributing the load across multiple servers.
		- Advantages:
			- Flexibility: You can add as many machines as needed.
			- High Availability: Even if one machine fails, the system can often continue to operate by relying on the remaining machines.
			- Easily Managed Growth: Easier to add more machines than continuously upgrading a single machine.
			- Cost-Effective: Often, it's cheaper to add more standard machines than to invest in high-end, super-powerful single machines.
		- Disadvantages:
			- Complexity: Load balancers may be needed to distribute traffic, and the application might need to be architected to support distributed computing.
			- Maintenance Overhead: Multiple machines mean multiple points of potential failure, so more active monitoring is required.
			- Data Consistency: With data spread out, ensuring data integrity and consistency can become challenging, especially in real-time processing systems.