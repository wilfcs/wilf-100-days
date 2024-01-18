# [241. Different Ways to Add Parentheses](https://leetcode.com/problems/different-ways-to-add-parentheses/description/)

## Code ->
```cpp
class Solution {
public:
    vector<int> diffWaysToCompute(string expression) {
        vector<int> ans;
        int n = expression.size();

        // Iterate through each character in the expression
        for(int i = 0; i < n; i++) {
            char ch = expression[i];

            // If the current character is an operator
            if(ch == '+' || ch == '-' || ch == '*') {
                // Recursively compute the left and right parts of the expression
                vector<int> left = diffWaysToCompute(expression.substr(0, i));
                vector<int> right = diffWaysToCompute(expression.substr(i + 1, n));

                // Combine the results of left and right using the current operator
                for(int x : left) {
                    for(int y : right) {
                        if(ch == '-') ans.push_back(x - y);
                        else if(ch == '+') ans.push_back(x + y);
                        else ans.push_back(x * y);
                    }
                }
            }
        }

        // If the expression contains only a number (no operator)
        if(ans.empty()) ans.push_back(stoi(expression));

        return ans;
    }
};
```
# [486. Predict the Winner](https://leetcode.com/problems/predict-the-winner/description/)
## Approach->
The basic idea and the crux of this game is this->

1. When it is your turn, do your best
2. When it is your opponent's turn, expect the worst from result because your opponent is also playing optimally.

So for example you have {1,5,2} and if you pick 1 then your opponent will have {5,2} as option so they will pick 5 for sure and you'll be left with minimum (hence the expect the worst from result is true). And if you picked 2 from {1,5,2} then also your opponent would have chose 5.

So in this approach we are just going to find the most optimal total price of player 1 using recursion. Then we will subtract that total optimal price of player 1 with the total price available in the nums array to find the total price of player 2. And then we can return true if player 1 has more or equal than player 2.

So it is very clear that we will only find the optimal price of player 1. So player 1 has two options-> to pick the first element i.e. from st or to pick from the end i.e. en. If player 1 picks from st, player 2 can pick from either st or en. So player 1 in the next turn will have to pick from st+2,en or st+1,en-1. If player 1 picks from en, in the next turn player 1 will have to pick from st+1,en-1 or st+2,en.

So everytime we will choose both st and en prices and call recursion for the minimum value of recursion because the opponent is not letting us choose the maximum value (opponent's optimal moves that's why we are expecting the least (crux 2)). And at the end we will return the maximum value among the two (our optimal move because it's our chance to pick (crux 1))

## Code ->
```cpp
class Solution {
public:
    // Helper function to find the optimal total price for Player 1
    int helper(vector<int>& nums, int st, int en){
        // If st is greater than en, there are no elements left
        if(st > en) return 0;
        // If st is equal to en, there is only one element left
        if(st == en) return nums[st];

        // Option 1 for Player 1: Pick from the start and consider opponent's optimal moves i.e. anticipate minimum result from the future
        int op1ForPlayer = nums[st] + min(helper(nums, st + 2, en), helper(nums, st + 1, en - 1));
        // Option 2 for Player 1: Pick from the end and consider opponent's optimal moves i.e. anticipate minimum result from the future
        int op2ForPlayer = nums[en] + min(helper(nums, st, en - 2), helper(nums, st + 1, en - 1));

        // Return the maximum value among the two options for Player 1 because it's your turn and you will obviously do your best
        return max(op1ForPlayer, op2ForPlayer);
    }

    // Main function
    bool predictTheWinner(vector<int>& nums) {
        // Calculate the total sum of all elements in the nums array
        int total = accumulate(nums.begin(), nums.end(), 0);

        // Find the optimal total price for Player 1 using the helper function
        int p1 = helper(nums, 0, nums.size() - 1);

        // Calculate the total price for Player 2
        int p2 = total - p1;

        // Return true if Player 1's total price is greater than or equal to Player 2's total price
        return (p1 >= p2);
    }
};
```

# [Count Inversion](https://www.codingninjas.com/studio/problems/count-inversions_615?leftPanelTabValue=PROBLEM)

## [Approaches](https://takeuforward.org/data-structure/count-inversions-in-an-array/)

## Code->
```cpp
// The code from top to bottom is a merge sort code, just added one additional line to count answer.
#include <bits/stdc++.h> 
void merge(vector<long long> &ar, long long low, long long mid, long long high, long long &ans){
    long long left = low;
    long long right = mid+1;

    vector<int> vec;

    while(left <= mid && right<= high){
        if(ar[left]>ar[right]){
            ans += (mid - left + 1); // only additional line to count answer
            vec.push_back(ar[right++]);
        } 
        else vec.push_back(ar[left++]);
    }
    while(left<= mid) vec.push_back(ar[left++]);
    while(right<=high) vec.push_back(ar[right++]);

    int x = 0;
    for(int i=low; i<=high; i++) ar[i] = vec[x++];
}
void mergeSort(vector<long long> &ar, long long low, long long high, long long &ans){
    if(low >= high){
        return;
    }

    long long mid = (high + low) / 2;

    mergeSort(ar, low, mid, ans);
    mergeSort(ar, mid+1, high, ans);
    merge(ar, low, mid, high, ans);
}

long long getInversions(long long *arr, int n){
    vector<long long> ar;
    for(int i=0; i<n; i++) ar.push_back(arr[i]);
    long long ans = 0;
    mergeSort(ar, 0, n-1, ans);
    return ans;
}
```

TC ->  O(N*logN), SC-> O(N), as in the merge sort We use a temporary array to store elements in sorted order.

# [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/description/)

## Approach -> Exactly same as the approach in the question above. Just make another function to calculate the answer and call the function before merging the two arrays(sorted). [link](https://takeuforward.org/data-structure/count-reverse-pairs/)
```cpp
class Solution {
public:
    void merge(vector<long long int> &ar, int low, int mid, int high, long long int &ans){
        int left = low;
        int right = mid+1;

        vector<int> vec;

        while(left <= mid && right<= high){
            if(ar[left]>ar[right]){
                vec.push_back(ar[right++]);
            } 
            else vec.push_back(ar[left++]);
        }
        while(left<= mid) vec.push_back(ar[left++]);
        while(right<=high) vec.push_back(ar[right++]);

        int x = 0;
        for(int i=low; i<=high; i++) ar[i] = vec[x++];
    }

    // Function to calculate the answer
    void calculate(vector<long long int> &ar, int low, int mid, int high, long long int &ans){
        int left = low, right = mid+1;
        while(left<=mid && right<=high){
            if( ar[left] <= (ar[right])*2 ) left++;
            else{
                ans+=(mid-left+1);
                right++;
            }
        }
    }

    void mergeSort(vector<long long int> &ar, int low, int high, long long int &ans){
        if(low >= high){
            return;
        }

        int mid = (high + low) / 2;

        mergeSort(ar, low, mid, ans);
        mergeSort(ar, mid+1, high, ans);
        calculate(ar, low, mid, high, ans); // calculate function call
        merge(ar, low, mid, high, ans);
    }

    int reversePairs(vector<int>& nums) {
        long long int ans = 0;
        vector<long long int> ar;
        for(int i=0; i<nums.size(); i++) ar.push_back(nums[i]);
        mergeSort(ar, 0, nums.size()-1, ans);
        return ans;
    }
};
```
TC-> O(2N*logN). SC-> O(N)

# CN 
Two types of architectures:
Client-Server Architecture:
-  In a client-server architecture, computers in a network are divided into two categories: clients and servers. Clients request services or resources, while servers provide those services or resources.
Peer-to-Peer (P2P) Architecture:
- In a P2P architecture, all computers (nodes) on the network have equal status and can act as both clients and servers. They share resources and services directly with each other without relying on centralized servers.
- It is decentralized in nature.

Some networking devices:
- Repeater - It operates at the physical layer. Repeater is a powerful network hardware device that regenerates an incoming signal from the sender before retransmitting it to the receiver. It is also known as a signal booster, and it helps in extending the coverage area of networks. The Incoming data can be in optical, wireless or electrical signals.
- Hub - It operates at the physical layer. Its primary function is to connect multiple devices in a network, such as computers, printers, or other networked devices, and facilitate data transmission among them. However, hubs are relatively simple and have limited intelligence compared to more advanced network devices like switches and routers.
- Bridge - It operates at the data link layer. A network bridge is a computer networking device that creates a single, aggregate network from multiple communication networks or network segments. This function is called network bridging. eg - Imagine you have a large office building with two separate floors. Each floor has its own Ethernet LAN with multiple devices, including computers, printers, and servers. To improve network performance and reduce collision domains, you decide to connect all the segmented networks using a bridge.
- Switch - It operates at the data link layer. It connects multiple devices within a local network and uses MAC addresses to efficiently forward data frames only to the device that needs them. eg. Ethernet switches. 
- Router - It operates at the network layer. It connects different networks, such as your home network to the internet, and uses IP addresses to route data packets between them. Routers make forwarding decisions based on network addresses, enabling data to traverse multiple networks.
- Gateway - A gateway is a network device or software application that connects two different networks using different protocols.
- Brouter - A brouter is a network device that combines the features of both a bridge and a router.

HTTP and HTTPS:
- HTTP is the HyperText Transfer Protocol which defines the set of rules and standards on how the information can be transmitted on the World Wide Web (WWW). It helps the web browsers and web servers for communication. It is a ‘stateless protocol’ where each command is independent with respect to the previous command. HTTP is an application layer protocol built upon the TCP. It uses port 80 by default.
- HTTPS is the HyperText Transfer Protocol Secure or Secure HTTP. It is an advanced and secure version of HTTP. On top of HTTP, SSL/TLS protocol is used to provide security. It enables secure transactions by encrypting the communication and also helps identify network servers securely. It uses port 443 by default.