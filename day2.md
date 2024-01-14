# DSA

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

# Implement string to integer

## Code ->
```cpp
class Solution {
public:
    int helper(string s, int n){
        if (n == 0) 
            return s[n] - '0';

        // Recursive case: combine previous value with current digit
        int preNums = helper(s, n - 1);
        int curNum = s[n] - '0';

        // Combine values by multiplying the previous value by 10 and adding the current value
        return preNums * 10 + curNum;
    }
    int myAtoi(string s) {
        return helper(s, s.size()-1);
    }
};
```

# [50. Pow(x, n)](https://leetcode.com/problems/powx-n/description/)

## Approach ->
The recursive approach leverages the observation that exponentiation problems, such as x^n, can be optimized by breaking them down into subproblems. By dividing the exponent (n) into halves, the recursive function efficiently calculates the result, taking advantage of the fact that we can write x^8 as (x^4 * x^4) and we can write x^9 as (x^4 * x^4 * x) This approach reduces the time complexity to logarithmic (log n) by repeatedly halving the exponent.

## Code ->
```cpp
#include <cmath>

class Solution {
public:
    double helper(double x, int n) {
        // Base case: if x is 0, result is 0
        if (x == 0) return 0;
        // Base case: if n is 0, result is 1
        if (n == 0) return 1;

        // Recursive calculation for half of the exponent
        double res = helper(x, n / 2);
        // Square the result obtained from the recursive call
        res = res * res;

        // Adjust result for even exponent
        if (n % 2 == 0) return res;
        // Multiply the result by x for odd exponent
        else return res * x;
    }

    // Main function to calculate power with handling for negative exponent
    double myPow(double x, int n) {
        // Call the helper function to calculate power
        double ans = helper(x, n);

        // Adjust the result for negative exponent
        if (n < 0) return 1 / ans;
        else return ans;
    }
};
```

# [1922. Count Good Numbers](https://leetcode.com/problems/count-good-numbers/description/)

## Approach -> 
It is a simple PNC question where we mod everything and make everything long long as much as possible to avoid wrong answers due to overflow. Please remember to make everything long long as much as possible during the interview. We know that the even places will have 5 possible numbers(0,2,4,6,8) and odd places will have 4 possible numbers(1,3,5,7). So we will simply return 5^even * 4^odd as our answer and find the power using recursion because we need to mod everything inside power function.

## Code -> 
```cpp
class Solution {
public:
    int MOD = 1000000007;
    int power(long long int n, long long int m){
        if(n==0) return 0;
        if(m==0) return 1;

        long long int res = power(n, m/2);
        res = (res*res)%MOD;
        if(m%2==0) return (res);
        else return (res*n)%MOD;
    }
    int countGoodNumbers(long long n) {
        long long int odd = n/2;
        long long int even = (n+1)/2;

        long long int first = power(5, even);
        long long int second = power(4, odd);

        return int((first*second)%MOD);
    }
};
```

# [39. Combination Sum](https://leetcode.com/problems/combination-sum/)

## [Approach](https://takeuforward.org/data-structure/combination-sum-1/)

## Code ->
```cpp
class Solution {
public:
    vector<vector<int>> ans;

    void helper(vector<int>& candidates, int target, vector<int> temp, int sum, int idx){
        if(sum>target) return;
        if(idx==candidates.size()){
            if(sum==target) ans.push_back(temp);

            return;
        }

        temp.push_back(candidates[idx]);
        helper(candidates, target, temp, sum+candidates[idx], idx);

        temp.pop_back();
        helper(candidates, target, temp, sum, idx+1);
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> temp;
        helper(candidates, target, temp, 0, 0);
        return ans;
    }
};
```
Time Complexity: O(2^t * k) where t is the target, k is the average length

Reason: Assume if you were not allowed to pick a single element multiple times, every element will have a couple of options: pick or not pick which is 2^n different recursion calls, also assuming that the average length of every combination generated is k. (to put length k data structure into another data structure)

Why not (2^n) but (2^t) (where n is the size of an array)?

Assume that there is 1 and the target you want to reach is 10 so 10 times you can “pick or not pick” an element.

Space Complexity: O(k*x), k is the average length and x is the no. of combinations

# [40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)

## [Approach (look at the recursive tree)](https://takeuforward.org/data-structure/combination-sum-ii-find-all-unique-combinations/)
# Intuition:
- The problem involves finding all unique combinations of candidates that sum to the target.
- The key challenge is to avoid duplicate combinations in the result.
- A recursive backtracking approach is used to explore and generate combinations.
# Approach:
- Sort the Candidates: Before starting the recursive calls, sort the candidates. Sorting is crucial to avoid duplicate combinations and to make the combinations appear in a sorted order.

- Define Recursive Helper Function: The helper function is a recursive function that explores combinations.
It takes parameters: the current candidates array, the remaining target, the current temporary combination (temp), the current sum, and the current index (idx).

- Base Cases: If the current sum exceeds the target, return. If the current sum equals the target, add the current combination to the result (ans) and return.

- Loop Through Candidates: Use a loop starting from the current index (idx) to the end of the candidates array. Skip consecutive duplicates to avoid duplicate combinations. If the current candidate is greater than the remaining target, break the loop (optimization). Include the current candidate in the temporary combination. Recursively call the helper function with updated parameters. Remove the last added element to backtrack and explore other combinations.

- Recursive Calls: The recursive calls explore combinations with the current element included (temp.push_back) and without it (temp.pop_back).

- Main Function: The combinationSum2 function initializes an empty vector (temp) and calls the helper function with the starting parameters. The sorted candidates array is used, and the result is returned.

Extra Note: In this question we are required to ignore duplicates and return the answer in sorted order.
So an approach might come in your head that why not keep the code same as of the last question and just increase the index even when we pick the element. Well that would have worked if there was no condition saying "find all unique combinations in candidates where the candidate numbers sum to target". But since we also have to keep the ans with unique elements only we will have to apply a different method of doing so.

Here's an example of understanding what is the wrong output and what is the right output:

Wrong output: [[1,2,5],[1,7],[1,6,1],[2,6],[2,1,5],[7,1]]
Right output: [[1,2,5],[1,7],[1,6,1],[2,6]]

---
## Code ->
```cpp
class Solution {
public:
    vector<vector<int>> ans;

    void helper(vector<int>& candidates, int target, vector<int> temp, int sum, int idx){
        if(sum>target) return;
        if(sum==target){
            ans.push_back(temp);
            return;
        }

        for(int i=idx; i<candidates.size(); i++){
            // Skip duplicates to avoid duplicate combinations
            if(i>idx && candidates[i]==candidates[i-1]) continue;
            // If the current candidate is greater than the target, break the loop. This will optimize the code more
            if(candidates[i]>target) break;

            // Include the current candidate in the temporary combination
            temp.push_back(candidates[i]);
            // Recursively call the helper function with updated parameters
            helper(candidates, target, temp, sum+candidates[i], i+1);
            // Remove the last added element to backtrack and explore other combinations
            temp.pop_back();

            // helper(candidates, target, temp, sum, i+1); -> we will not include this line like the previous solution. reason given below.
        }
        

        
    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end()); // sort the array to avoid duplicates in the future
        vector<int> temp;
        helper(candidates, target, temp, 0, 0);
        return ans;
    }
};
```
Question -> Why did we not include helper(candidates, target, temp, sum, i+1);

Reason ->  The specific line helper(candidates, target, temp, sum, i+1); (which represents the recursive call without including the current candidate) is omitted in this particular implementation because of the nature of the problem and the way duplicates are handled.

The condition if(i > idx && candidates[i] == candidates[i-1]) continue; takes care of skipping over duplicate elements at the current level of recursion. In other words, when you encounter a duplicate, you skip the recursive call for that duplicate element, ensuring that you don't generate duplicate combinations.

If you were to include the line helper(candidates, target, temp, sum, i+1);, it would mean that you are considering the case where you skip the current element and move to the next one. However, the way duplicates are handled in the current code essentially achieves the same effect without explicitly making that recursive call.

# Time Complexity:O(2^n*k)

Reason: Assume if all the elements in the array are unique then the no. of subsequence you will get will be O(2^n). we also add the ds to our ans when we reach the base case that will take “k”//average space for the ds.

# Space Complexity:O(k*x)

Reason: if we have x combinations then space will be x*k where k is the average length of the combination.

# [Subset Sum](https://www.codingninjas.com/studio/problems/subset-sum_3843086?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Approach -> 
Easy peasy using recursion

## Code ->
```cpp
void helper(vector<int> num, int sum, int idx, vector<int> &ans){
	if(idx==num.size()){
		ans.push_back(sum);
		return;
	}
	helper(num, sum+num[idx], idx+1, ans);
	helper(num, sum, idx+1, ans);
}
vector<int> subsetSum(vector<int> &num){
	// Write your code here.	
	vector<int> ans;
	helper(num, 0, 0, ans);
	sort(ans.begin(), ans.end());
	return ans;
}
```

# [90. Subsets II](https://leetcode.com/problems/subsets-ii/description/)

## Approach ->
Lets  understand  with an example where arr = [1,2,2 ].

Initially start with an empty data structure. In the first recursion, call make a subset of size one, in the next recursion call a subset of size 2, and so on. But first, in order to make a subset of size one what options do we have?

We can pick up elements from either the first index or the second index or the third index. However, if we have already picked up two from the second index, picking up two from the third index will make another duplicate subset of size one. Since we are trying to avoid duplicate subsets we can avoid picking up from the third index. This should give us an intuition that whenever there are duplicate elements in the array we pick up only the first occurrence.

The next recursion calls will continue from the point the previous one ended.

Let’s summarize:-

Sort the input array.Make a recursive function that takes the input array ,the current subset,the current index and  a list of list/ vector of vectors to contain the answer.
Try to make a subset of size n during the nth recursion call and consider elements from every index while generating the combinations. Only pick up elements that are appearing for the first time during a recursion call to avoid duplicates.
Once an element is picked up, move to the next index.The recursion will terminate when the end of array is reached.While returning backtrack by removing the last element that was inserted.

## Code -> 
```cpp
class Solution {
public:
    void helper(vector<int> nums, int idx, vector<int>temp, vector<vector<int>>& ans){
        // No explicit base condition because we want to generate subsets for all indices.
        ans.push_back(temp);

        for(int i=idx; i<nums.size(); i++){
            if(i>idx && nums[i]==nums[i-1]) continue;
            temp.push_back(nums[i]);
            helper(nums, i+1, temp, ans);
            temp.pop_back();
        }
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end()); // very important to sort since we are ignoring duplicate at a given position
        vector<int> temp;
        vector<vector<int>> ans;
        helper(nums, 0, temp, ans);
        return ans;
    }
};
```
Time Complexity: O(2^n) for generating every subset and O(k)  to insert every subset in another data structure if the average length of every subset is k. Overall O(k * 2^n).

Space Complexity: O(2^n * k) to store every subset of average length k. Auxiliary space is O(n)  if n is the depth of the recursion tree.

# Computer Networks ->

## OSI Model (Open System Interconnections) (more formal way)
It is a network architecture model based on the ISO standards (International Organization for Standardization). It is called the OSI model as it deals with connecting the systems that are open for communication with other systems. The OSI model has seven layers->

1. Application Layer:
- Application layer enables the user to access the network.
- It is used by network applications i.e. computer applications that use internet. eg. Google chrome, Zoom , Call of Duty, etc. These all are dependent on application layer protocols to function.
- There are multiple application layer protocols that enable various functions at this layer. eg. HTTP, HTTPS, FTP, FMTP, DHCP, TELNET, etc. All these protocols collectively form application layer.
- For example file transfer is done using FTP protocol. Web surfing using HTTP/S. Email using SMTP. Virtual terminals using TELNET.

2. Presentation Layer:
- Presentation layer is responsible for three tasks: translation, data compression, and data encryption/decryption.
	- Application layer sends data to presentation layer. The data is in human-readable format. The presentation layer converts this into machine-readable format. eg. It converts ASCII to EBCDIC. This is known as translation.
	- Presentation layer also reduces the number of bits that are used to represent original data. This is called data compression. Data compression can be lossy or lossless.
		- Lossy: To reduce the size of a file as much as possible.
		- Lossless: To reduce the size of a file without any loss of data or quality.
	- Presentation layer also encrypts and decrypts the data that is sent or received. It uses SSL (Secure Socket Layer) to do so.
- At the sender side, this layer translates the data format used by the application layer to the common format and at the receiver side, this layer translates the common format into a format used by the application layer.

3. Session Layer:
- The session layer helps in setting up and managing connections, enabling the sending and receiving of data followed by the termination of sessions. 
- The session layer has helpers called APIs (Application Programing Interfaces). APIs help in the communication process. 
- Before a connection or session is established with a server, the server performs two functions called authentication and authorization.
	- Authentication: Process of verifying who you are? Uses username and password.
	- Authorization: Process used by server to identify if you have permission to access a file. 
- Both the functions authentication and authorization are performed by session layer. 
- Session layer also keeps a track of the file sent and received.
- So basically session layer helps in session management, authentication and authorization. 
- Your web browser performs all the functions of application layer, presentation layer and session layer. 

4. Transport Layer:
- It delivers the message through the network and provides us with reliability of transportation using segmentation, flow control and error control.
	-  Segmentation: Data received from the session layer is divided into small units called segments. Each segment contains a source and destination port number, a sequence number, and a checksum. Port number helps to direct each segment to the correct application. Sequence number helps to assemble the segments in the correct order to deliver the correct message in the correct order later. 
	- Flow control: In flow control, transport layer controls the flow of the data being transmitted. Suppose a server can transmit data at 100mbps and a mobile can receive data at 10mbps, so mobile phone with the help of transport layer can tell the server to slow down data transmission rate to 10mbps so that no data is lost.
	- Error control: If some data does not arrive the destination, the transport layer uses an automatic repeat request scheme to retransmit the lost or corrupted data. A group of bits called checksum is added to each data segment by the transport layer to find out the received corrupted segment. 
- Protocols of transport layer are TCP(Transmission Control Protocol) and UDP(User Datagram Protocol).
- Transport layer provides with two additional types of services - connection-oriented transmission, and connectionless transmission.
	- Connection-oriented transmission: In this transmission, the receiver sends the acknowledgment to the sender after the packet has been received. This is done by TCP. TCP provides feedback. Therefore lost data can be retransmitted in TCP. It is full duplex. Used in WWW, Email, FTP, etc. where full data delivery is important. 
	- Connectionless transmission: In this transmission, the receiver does not send the acknowledgment to the sender.This is done by UDP. UDP is faster than TCP because it does not provide us with feedback on whether the data was really delivered. Used in online streaming, games, TFTP, DNS, etc. where speed and real-time data sync are more important.
- TCP (Transmission Control Protocol):
	- TCP is a connection-oriented protocol that guarantees the delivery of data packets in the order they were sent. It achieves this through sequence numbers, acknowledgments, retransmission of lost packets, and flow control. It's widely used for applications where data integrity and order are crucial, like file transfers and web browsing.
- UDP (User Datagram Protocol):
	- UDP is a connectionless protocol. It sends datagrams without establishing a formal connection and without guarantees regarding order or delivery. It’s faster and has less overhead than TCP because it lacks many of TCP’s reliability features. UDP is suited for applications where speed or low latency is a priority, like streaming audio/video or online gaming.
- Congestion Control:
	- Congestion control mechanisms manage the sending rate of data packets in networks to prevent network congestion and collapse. When too much data is sent too quickly, network resources (like bandwidth and buffer space in routers) can be overwhelmed, leading to dropped packets and reduced overall network throughput. TCP, for example, uses congestion control algorithms like Slow Start, Congestion Avoidance, and Fast Recovery to modulate its sending rate based on perceived network conditions.
- Checksum:
	- The checksum is a value calculated from a data set and is used for error detection. It's often a simple mathematical calculation where you sum up byte or word values in the data and sometimes apply modulo arithmetic. When data is transmitted, the checksum value is sent along with it. The receiving end recalculates the checksum from the received data and compares it to the received checksum. If they match, it's likely (but not guaranteed) that the data was received correctly.
- Retransmission Timers:
	- Retransmission timers are used in reliable transport protocols like TCP. When a segment is sent, a timer is started. If the acknowledgment (ACK) for that segment isn’t received before the timer expires, the segment is assumed to be lost in transit and is retransmitted. The duration of this timer might be adaptive, adjusting based on network conditions.
- 3-way Handshake:
	- This is a process used by TCP to establish a connection between a client and a server. The steps are:
		1. SYN: The client sends a segment with the SYN (synchronize) flag set, indicating an initial sequence number.
		2. SYN-ACK: The server responds with a segment with both the SYN and ACK flags set, acknowledging the client's SYN and providing its own initial sequence number.
		3. ACK: The client sends an acknowledgment segment back to the server to tell the server that it received the SYN and ACK from the server. After these steps, the TCP connection is established, and data transfer can begin.
	- The three-way handshake in TCP occurs only at the beginning of a TCP session to establish the connection between the client and the server. It doesn't happen every time a data packet is sent.
	- After the connection is established through the three-way handshake, data segments can be sent bidirectionally between the client and server. Each segment of data that's sent will typically be acknowledged by the receiving party, but this acknowledgment is not another three-way handshake; it's simply an ACK segment.
	- A separate set of steps (often referred to as the "four-way handshake") is used to gracefully close a TCP connection. It involves FIN (finish) and ACK flags.
- Data Transfer and Acknowledgments:
	- After the connection is established, the client and server can send data segments to each other.
	- When a data segment is received, the recipient (either client or server) sends an acknowledgment (ACK) for that specific segment. This ACK is just a simple acknowledgment and is not part of the three-way handshake.
	- The acknowledgment is used to inform the sender that the segment was received successfully. If the sender doesn't get this ACK within a certain time frame (due to the ACK being lost, the data segment being lost, or other network issues), it will retransmit the segment.
- Two army problem / Acknowledgment Problems: 
	- It happens at the time of data transfer. If the data is received by the receiver, but the acknowledgment (ACK) sent by the receiver doesn't reach the sender, the sender assumes the data was lost and retransmits it.
	- If the receiver gets the same data again, it can detect that this data is a duplicate, thanks to the sequence numbers used by reliable protocols like TCP which helps in avoiding this problem. If the receiver gets this retransmitted segment but has already received the original segment (i.e., it's a duplicate), the receiver can recognize this because the sequence number of the retransmitted segment won't match the expected next sequence number.