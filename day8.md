# [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)

## Approaches ->
1. Brute-Force: Keep any one of the list to check its node present in the other list. Here, we are choosing the second list for this task.
Iterate through the other list. Here, it is the first one. 
Check if the both nodes are the same. If yes, we got our first intersection node.
If not, continue iteration.
If we did not find an intersection node and completed the entire iteration of the second list, then there is no intersection between the provided lists. Hence, return null.
2. Hashing
3. Better Approach: Find the length of both lists.
Find the positive difference between these lengths.
Move the dummy pointer of the larger list by the difference achieved. This makes our search length reduced to a smaller list length.
Move both pointers, each pointing two lists, ahead simultaneously till they collide.
4. Best Approach: The difference of length method requires various steps to work on it. Using the same concept of difference of length, a different approach can be implemented. The process is as follows:-

Take two dummy nodes for each list. Point each to the head of the lists.
Iterate over them. If anyone becomes null, point them to the head of the opposite lists and continue iterating until they collide.

## Code ->
```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *p1 = headA;
        ListNode *p2 = headB;

        while(p1 || p2){
            if(p1==NULL) p1 = headB;
            if(p2==NULL) p2 = headA;
            if(p1==p2) return p1;
            // make sure to check those conditions at first else the sol. would be wrong.
            p1 = p1->next;
            p2 = p2->next;
        }
        return p1;
    }
};
```

# [Find pairs with given sum in doubly linked list](https://www.codingninjas.com/studio/problems/find-pairs-with-given-sum-in-doubly-linked-list_1164172?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Approaches ->
1. Brute Force: O(n^2) Time complexity nested loops.
2. Two Pointer: Keep one pointer at start and one at end and move accordingly

## Code ->
```cpp
vector<pair<int, int>> findPairs(Node* head, int k)
{
    // Write your code here.
    Node *st=head;
    Node *en=head;
    while(en->next) en = en->next;
    vector<pair<int, int>> ans;

    while(st && en && st!=en){
        int res = st->data + en->data;
        if(res==k){
            ans.push_back(make_pair(st->data, en->data));
            st = st->next;
            //Check if 'st' and 'en' pointers have converged, if yes, break out of the loop.
            if(st==en) break; // important to check
            en = en->prev;
        }
        else if(res>k){
            en = en->prev;
        }
        else{
            st = st->next;
        }
    }

    return ans;
}
```
# [61. Rotate List](https://leetcode.com/problems/rotate-list/description/)
## Approach ->
- Calculate the length of the list.
- Connect the last node to the first node, converting it to a circular linked list.
- Iterate to cut the link of the last node and start a node of k%length of the list rotated list.

## Code ->
```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        // Check if the linked list is empty or has only one node, no rotation needed.
        if (head == NULL || head->next == NULL) {
            return head;
        }

        // Initialize two pointers, 'temp' and 'ptr', and a variable 'size' to 0.
        ListNode* temp = head, *ptr = head;
        int size = 0;

        // Traverse the linked list to find its size.
        while (temp->next) {
            size++;
            temp = temp->next;
        }

        // Include the last node in the size calculation.
        size++;

        // If k is a multiple of the size, no rotation is needed.
        if (k % size == 0) {
            return head;
        }

        // Calculate the number of nodes to be rotated from the end.
        int toChange = size - k % size - 1;

        // Move the 'ptr' pointer to the node before the rotation point.
        while (toChange--) {
            ptr = ptr->next;
        }

        // Connect the last node to the head to create a circular structure.
        temp->next = head;

        // Update the 'head' to the node after the rotation point.
        head = ptr->next;

        // Break the circular structure at the rotation point.
        ptr->next = NULL;

        // Return the new head of the rotated linked list.
        return head;
    }
};
```

# [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/description/)

## Approach ->
This is a fairly simple question but questions like these might seem tough to code if you are out of practice. But there is a very simple way to join the links of the nodes without creating any confusion and that is, you make a dummy node temp and keep pointing it to the node that has the smaller value. Move temp and move list1 and list2 as well. So basically temp is for the conncetion purpose and list1 list2 is for comparision purpose.

## Code ->
```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        // Check if either of the lists is empty.
        if(list1==NULL) return list2;
        if(list2==NULL) return list1;

        // Create a dummy node to simplify handling the merged list.
        ListNode * head = new ListNode(-1);
        ListNode *temp = head;

        // Traverse both lists while they are not empty.
        while(list1 && list2){
            // Compare values of nodes in the two lists.
            if(list1->val < list2->val){
                // Connect the link of smaller value to temp.
                temp->next = list1;
                temp = temp->next;
                list1 = list1->next; 
            }
            else{
                temp->next = list2;
                temp = temp->next;
                list2 = list2->next; 
            }
        }

        // If list1 is not empty, append the remaining nodes.
        while(list1){
            temp->next = list1;
            temp = temp->next;
            list1 = list1->next;
        }
        // If list2 is not empty, append the remaining nodes.
        while(list2){
            temp->next = list2;
            temp = temp->next;
            list2 = list2->next; 
        }
        // Skip the dummy node and return the merged list.
        return head->next;
    }
};
```

# CN ->
Some common terms:
- Client: A software program or hardware device that requests services or resources from a server. In a typical scenario, your web browser acts as a client that requests web pages from a web server.
- Server: A computer, device, or software that provides services, data, or resources to clients. For example, a web server delivers web pages to users' browsers.
- Peer: In peer-to-peer (P2P) networks, a peer is a computer that acts as both a client and a server for other computers in the network, allowing shared access to files and peripherals.
- Host: A computer or other device on a network that offers services or resources. Every host has a unique IP address.
- Bandwidth: Refers to the maximum rate of data transfer across a network. It's often used to gauge the speed of internet connections.
- Jitter: Represents the variability in delay/latency of received packets in a flow. It's an important factor in VoIP and online gaming where a steady stream of data packets is needed.
- Bit rate: The number of bits processed per unit of time, typically measured in bits per second (bps). It often indicates the quality of audio, video, or data transfer.
- Noise: Unwanted electrical or electromagnetic energy that degrades the quality of signals and data. Noise can be introduced by various sources and can interfere with the signal transmission.
- Attenuation: The loss of signal strength as it travels through a medium (like a cable). In networking, attenuation can be caused by the length of cables or interference.
- Distortion: A change, twist, or exaggeration that makes something appear different from the way it really is. In signals, distortion can result from various factors, including electronic interference and physical changes in the transmission medium.


Some common interview questions:
- Type of transmission Media: 
	- Transmission media can be broadly classified into two categories:
		- Guided Media (Wired): This includes twisted-pair cable, coaxial cable, and optical fiber. They guide the signal through a physical medium.
		- Unguided Media (Wireless): This includes radio waves, microwaves, and infrared signals. They transmit data through the air or vacuum.
- Unicast, Broadcast, and Multicast:
	- Unicast: Transmission of data to a single destination.
	- Broadcast: Transmission of data to all possible destinations in the network.
	- Multicast: Transmission of data to a group of destinations.
- Common networking commands:
	- ping: This command is used to test the reachability of a host on an Internet Protocol (IP) network and to measure the round-trip time for packets sent from the local host to a destination computer.
	- netstat: This command displays various network-related information such as active connections, listening ports, routing tables, etc.
	- tracert (or traceroute on some systems): It's used to display the route and measure transit delays of packets across an IP network. It shows the path that the packet took to reach the destination.
	- ipconfig: On Windows systems, this command is used to display all current network configuration values and refresh Dynamic Host Configuration Protocol (DHCP) and Domain Name System (DNS) settings.
	- nslookup: This command is used to query the Domain Name System (DNS) to obtain domain name or IP address mapping or other DNS records.
	- route: This command is used to view and manipulate the IP routing table in the operating system.
	- pathping: Combines features of both ping and tracert. It sends packets to each router on the way to a final destination over a period of time, and then computes results based on the packets returned from each hop.
	- netDiag: This is not a standard command in most operating systems, but in some contexts, it might refer to a network diagnostic tool.
	- hostname: It displays the hostname of the machine the command is executed on.
	- arp: Stands for Address Resolution Protocol. This command displays and modifies entries in the ARP cache, which maps IP addresses to MAC addresses.