# [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/description/)
---
Approaches ->
1. Find the size of the ll and return the size/2 node.
2. Fast-Slow pointer
---
Code ->
```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        //ListNode *temp = head;
//         int size=0;
//         while(temp){
//             temp = temp->next;
//             size++;
//         }
        
//         size = size/2 + 1;
//         cout << size;
        
//         temp = head;
//         while(--size)
//             temp = temp->next;
//         return temp;
        
        ListNode *slow=head, *fast=head;
        while(fast && fast->next ){
            slow=slow->next;
            fast=fast->next->next;
        }
        return slow;
    }      
};
```
# [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)
## Iterative Approach Code (TC->O(N) SC->O(1)) ->
```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* p = NULL, *c = head, *n = NULL;
        while(c){
            n = c->next; // don't forget to assign the value of next here instead at the end to avoid runtime error
            c->next = p;
            p=c;
            c=n;
        }
        return p;
    }
};
```
## [Recursive Approach Code (TC->O(N) SC->O(1)) ->](https://takeuforward.org/data-structure/reverse-a-linked-list/)
```cpp
Node* reverseLinkedList(Node* head) {
    // Base case:
    // If the linked list is empty or has only one node,
    // return the head as it is already reversed.
    if (head == NULL || head->next == NULL) {
        return head;
    }
    
    // Recursive step:
    // Reverse the linked list starting 
    // from the second node (head->next).
    Node* newHead = reverseLinkedList(head->next);
    
    // Save a reference to the node following
    // the current 'head' node.
    Node* front = head->next;
    
    // Make the 'front' node point to the current
    // 'head' node in the reversed order.
    front->next = head;
    
    // Break the link from the current 'head' node
    // to the 'front' node to avoid cycles.
    head->next = NULL;
    
    // Return the 'newHead,' which is the new
    // head of the reversed linked list.
    return newHead;
}
```
# [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/description/)
## Approaches ->
1. Maintain a hashmap. Traverse the ll and find the node in the map. Insert the node in the map.
2. Take a slow and fast pointer. If cycle exist, fast and slow pointer will meet each other at some point.
---
## Code ->
2. 
```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *fast = head, *slow = head;
        while(fast && fast->next)
        {
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow)
                return true;
        }
        return false;
    }
};
```

# [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/description/)

## Approaches ->
1. We can store nodes in a hash table so that, if a loop exists, the head will encounter the same node again.
2. Two pointer approach ->
- Initially take two pointers, fast and slow. The fast pointer takes two steps ahead while the slow pointer will take a single step ahead for each iteration.
- We know that if a cycle exists, fast and slow pointers will collide.
- If the cycle does not exist, the fast pointer will move to NULL
- Else, when both slow and fast pointer collides, it detects a cycle exists.
- Take another pointer, say entry. Point to the very first of the linked list.
- Move the slow and the entry pointer ahead by single steps until they collide. 
- Once they collide we get the starting node of the linked list.

Why does this work though?
 In the presence of a cycle, these pointers will eventually meet within the cycle. When they meet, the distance covered by the fast pointer is a multiple of the cycle length. To find the starting point of the cycle, another pointer is initiated at the beginning of the linked list. By moving this new pointer and the slow pointer one step at a time, they meet at the starting point of the cycle, effectively nullifying the extra distance covered by the fast pointer within the cycle. This method leverages the relative speeds of the pointers and ensures that the meeting point of the slow pointer and the entry pointer is indeed the starting point of the cycle.

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head;
        ListNode *fast = head;
        
        
        if(head==NULL || head->next==NULL) return NULL;
        while(fast!=NULL && fast->next!=NULL){
            slow = slow->next;
            fast = fast->next->next;
            
            if(slow == fast){
                break;
            }
                
        }
        
        if(slow!=fast) return NULL; // important condition just incase the loop doesn't exist
        
        fast = head;
        while(fast!=slow){
            slow=slow->next;
            fast=fast->next;
        }
        
        return fast;
    }
};
```

# [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/description/)

## Approaches ->
1. Use extra space and create an array of ll element and compare in the array.
2. Split the linkedlist into two linked lists from the middle and compare the two linkedlists. (Don't forget to reverse the second linkedlist)

## Code ->
```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        // Finding the middle of the linked list
        ListNode *slow = head, *fast = head, *temp = head;
        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
        }

        // reverse the linked list from slow pointer
        ListNode *p = NULL, *c = slow, *n = NULL;
        while(c){
            n = c->next;
            c->next = p;
            p = c;
            c = n;
        }

        // check both linked lists
        while(p && temp){
            if(p->val!=temp->val) return false;
            p = p->next;
            temp = temp->next;
        }
        return true;
    }
};
```

# [328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/description/)

## Approaches ->
1. Brute Force: Separate odd and even index nodes into an array using two iterations. Replace the data in the linked list by traversing it again and using the array.
Time complexity: O(n), Space complexity: O(n).
2. Instead of using an external array, rearrange the linked list in-place by changing the links. Traverse the linked list using two pointers, odd and even, starting at the head and head.next. Iterate through the linked list, rearranging the links to connect odd and even nodes alternately. Connect the last odd node to the head of even nodes.
Time complexity: O(n), Space complexity: O(1)

## Code ->
```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head==NULL || head->next==NULL || head->next->next==NULL) return head;
        ListNode* odd = head, *even = head->next, *toPoint = head->next;

        // connecting all odd nodes and even nodes seperately
        while(odd->next && even->next){
            odd->next = odd->next->next;
            odd = odd->next;
            even->next = even->next->next;
            even = even->next;
        }

        // connecting the end of odd linked list with start of even linked list
        odd->next = toPoint;
        return head;
    }
};
```
# [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)
## Approaches -> 
1. We can traverse through the Linked List while maintaining a count of nodes, let’s say in the variable count, and then traversing for the 2nd time for (n – count) nodes to get to the nth node of the list. TC-> O(2n)
2. Unlike the above approach, we don’t have to maintain the count value, we can find the nth node just by one traversal by using two pointer approach. So what we will do is take a fast pointer and slow pointer. Then start traversing until the fast pointer reaches the nth node. Now start traversing by one step for both the pointers fast and slow until the fast pointers reach the end. Now your slow pointer will for sure be at the nth position from the last. TC-> O(n)

## Code ->
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode * start = new ListNode();
        start -> next = head;
        ListNode* fast = start;
        ListNode* slow = start;     

        for(int i = 1; i <= n; ++i)
            fast = fast->next;
    
        while(fast->next != NULL)
        {
            fast = fast->next;
            slow = slow->next;
        }
        
        slow->next = slow->next->next;
        
        return start->next;
    }
};
```

# [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/description/)
## Approach -> 
Keep track of the carry using a variable and simulate digits-by-digits sum starting from the head of the list, which contains the least significant digit.

---
## Code ->
```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        // Initialize variables to keep track of carry, sum, and the value to add to the new node
        int carry = 0, sum = 0, toAdd = 0;

        // Create a dummy node to serve as the head of the result linked list
        ListNode* head = new ListNode(-1);
        
        // Create a pointer to traverse the result linked list
        ListNode* mover = head;

        // Iterate through the input linked lists until both are exhausted
        while (l1 || l2) {
            // Reset sum for each iteration
            sum = 0;

            // Add values from the current nodes of l1 and l2, if available
            if (l1) sum += l1->val;
            if (l2) sum += l2->val;

            // Add the carry from the previous iteration
            sum += carry;

            // Calculate the value to be added to the new node and update the carry
            toAdd = sum % 10;
            carry = sum / 10;

            // Create a new node with the calculated value and append it to the result linked list
            ListNode* temp = new ListNode(toAdd);
            mover->next = temp;
            mover = mover->next;

            // Move to the next nodes in the input linked lists, if available
            if (l1) l1 = l1->next;
            if (l2) l2 = l2->next;
        }

        // If there's a carry remaining after the last iteration, create a new node for it
        if (carry) {
            ListNode* temp = new ListNode(carry);
            mover->next = temp;
            mover = mover->next;
        }

        // Set the next pointer of the last node to NULL to terminate the result linked list
        mover->next = nullptr;

        // Return the actual head of the result linked list (excluding the dummy node)
        return head->next;
    }
};
```
DNS:
DNS stands for "Domain Name System," and it is a fundamental component of the internet's infrastructure. DNS is essentially a distributed naming system that translates human-readable domain names (like www.example.com) into numeric IP (Internet Protocol) addresses (like 192.0.2.1) that computers use to identify each other on the internet. Without DNS, users must know the IP address of the web page that you wanted
to access.
DNS Forwarder:
A forwarder is used with a DNS server when it receives DNS queries that cannot be resolved quickly. So it forwards those requests to external DNS servers for resolution. A DNS server which is configured as a forwarder will behave differently than the DNS server which is not configured as a forwarder.

SMTP:
SMTP is the Simple Mail Transfer Protocol. SMTP sets the rule for communication between servers. This set of rules helps the software to transmit emails over the internet. It supports both End-to-End and Store-and-Forward methods. It is in always-listening mode on port 25.

How email works?
1. User Composes Email: A user creates an email message using an email client.
2. Sending Email: The email client sends the message to the user's outgoing mail server (SMTP server).
3. SMTP Server Processing: The SMTP server routes the email to the recipient's email server using DNS. Basically the email sent by sender stays in the sender's SMTP server for a while till it connects to the receiver's SMTP server, and when the connection is established the email is sent to the receiver's SMTP server. DNS is used to resolve the domain part of the email address to find the IP address of the destination (recipient's) mail server. 
4. Recipient's Email Server: The recipient's email server receives the email and stores it in the recipient's mailbox.
5. User Retrieves Email: The recipient accesses the email using an email client (POP3 or IMAP) and reads it.
Email communication involves sending and receiving messages through email servers, using protocols like SMTP, DNS, and POP3/IMAP.
- When the sender and receiver are on the same domain/server:
	- The email doesn't need to go out to the internet. Instead, it is internally routed.
	- The SMTP server of the sender realizes the destination mailbox is on the same system, so it just transfers the email internally to the appropriate mailbox without needing external DNS resolution for routing.
	- Steps like DNS lookup and communicating with an external SMTP server are skipped, and the email is directly delivered to the recipient's mailbox on the same server.

