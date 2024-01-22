# [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/description/)

## [Approach](https://takeuforward.org/data-structure/reverse-linked-list-in-groups-of-size-k/)

## Code ->
```cpp
class Solution {\
public:

    // Helper function to calculate the length of the linked list
    int lengthOfLinkedList(ListNode* head) {
        int length = 0;
        while (head != NULL) {
            ++length;
            head = head->next;
        }
        return length;
    }

    ListNode* reverseKGroup(ListNode* head, int k) {
        // Check if the list is empty or has only one node
        if (head == NULL || head->next == NULL) 
            return head;
        
        // Calculate the length of the linked list
        int length = lengthOfLinkedList(head);
        
        // Create a dummy head to simplify the code
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        
        // Pointers for reversing the k-group
        ListNode* pre = dummyHead;
        ListNode* cur;
        ListNode* nex;
        
        // Reverse k nodes at a time
        while (length >= k) {
            cur = pre->next;
            nex = cur->next;
            
            // Reverse the k nodes
            for (int i = 1; i < k; i++) {
                cur->next = nex->next;
                nex->next = pre->next;
                pre->next = nex;
                nex = cur->next;
            }
            
            // Move the pointers to the next k-group
            pre = cur;
            length -= k;
        }
        
        return dummyHead->next;
    }
};
```
# [ Flatten A Linked List](https://www.codingninjas.com/studio/problems/flatten-a-linked-list_1112655?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf)

## [Approach](https://takeuforward.org/data-structure/flattening-a-linked-list/)

## Code ->
```cpp
// Definition for linked list node
class Node {
public:
    int data;
    Node* next;
    Node* bottom;
    Node() : data(0), next(nullptr), bottom(nullptr) {}
    Node(int x) : data(x), next(nullptr), bottom(nullptr) {}
};

// Helper function to merge two sorted linked lists
Node* mergeTwoLists(Node* a, Node* b) {
    Node* temp = new Node(0);
    Node* res = temp; 
    
    // Merge the two linked lists in sorted order
    while (a != NULL && b != NULL) {
        if (a->data < b->data) {
            temp->bottom = a; 
            temp = temp->bottom; 
            a = a->bottom; 
        } else {
            temp->bottom = b;
            temp = temp->bottom; 
            b = b->bottom; 
        }
    }
    
    // If one of the lists is not empty, append it to the result
    if (a) temp->bottom = a; 
    else temp->bottom = b; 
    
    return res->bottom; 
}

// Main function to flatten a linked list
Node* flatten(Node* root) {
    // Check if the root is NULL or has only one node
    if (root == NULL || root->next == NULL) 
        return root; 
  
    // Recur for the list on the right
    root->next = flatten(root->next); 
  
    // Merge the current list with the flattened list on the right
    root = mergeTwoLists(root, root->next); 
  
    // Return the root, which will be in turn merged with its left
    return root; 
}
```

# [148. Sort List](https://leetcode.com/problems/sort-list/description/)

## Approach ->
Proper merge sort approach. Read the comments to understand the code, I wrote it beautifully :)

## Code ->
```cpp
class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2){
        // Function to merge two sorted LL

        // Create a temp node that will be your dummy node and a mover that points to temp
        ListNode* temp = new ListNode(-1);
        ListNode* mover = temp;

        // Use l1 and l2 to move forward and compare them and keep adding the lower value nodes to mover's next
        while(l1 && l2){
            if(l1->val < l2->val){
                mover->next = l1;
                l1 = l1->next;
            }
            else{
                mover->next = l2;
                l2 = l2->next;
            }
            // don't forget to move l1, l2 and then mover as well
            mover = mover->next;
        }

        // if either of the remaining l1 or l2 has some nodes left then connect that to mover as well
        if(l1) mover->next = l1;
        if(l2) mover->next = l2;

        // return the sorted array i.e. temp->next
        return temp->next;
    }

    // main function
    ListNode* sortList(ListNode* head) {
        // Let's try solving it like a normal merge sort question

        // Step 0: Base Condition
        if(head==NULL || head->next==NULL) return head;

        // Step 1: Dividing the linked list into two halves.
        ListNode *slow = head, *fast = head, *prev = NULL;
        while(fast && fast->next){
            prev = slow;
            slow = slow->next;
            fast = fast->next->next;
        }

        // Step 2: To properly divide the LL into two seperate LL we have to point the last node of the first LL to null. The second part is already pointing to null.
        prev->next = NULL;

        // Step 3: Sorting the two linked lists by dividing them into further parts
        ListNode* l1 = sortList(head);
        ListNode* l2 = sortList(slow);

        // Step 4: Merge two sorted linked lists
        return merge(l1, l2);
    }

};
```

# CN ->
- Difference between HTTP and HTTPS:
	- Security:
		- HTTP: It transmits data in plain text, which means that any information sent over an HTTP connection is not encrypted and can be intercepted by attackers. This lack of encryption makes it vulnerable to eavesdropping and data tampering.
		- HTTPS: It encrypts data using secure protocols such as SSL (Secure Sockets Layer) or TLS (Transport Layer Security). This encryption ensures that data exchanged between the client (usually a web browser) and the server remains confidential and secure, making it much more difficult for third parties to intercept and decipher the information.
	- Authentication:
		- HTTP: There is no authentication of the server or website, which means it's easy for attackers to create malicious websites that appear legitimate.
		- HTTPS: It involves a process of authentication, where a trusted third party (a certificate authority) verifies the identity of the website's owner. This provides a level of trust that you're actually communicating with the intended website and not a fraudulent one.
	- SEO and Trust:
		- HTTPS is favored by search engines like Google. Websites that use HTTPS receive a slight boost in search rankings, and modern browsers may display a "Secure" or padlock icon in the address bar to indicate a secure connection, instilling more trust in users.
- What is API Gateway:
	- An API Gateway is a server that acts as an intermediary for requests from clients seeking resources from other servers. It provides a centralized entry point for managing, controlling, and routing API calls. Here are some of the primary functions and benefits of an API Gateway:
	- API Composition: Aggregates multiple services into a single API.
	- Rate Limiting: Restricts the number of API calls from a particular client within a specified time.
	- Security: Provides features like authentication, authorization, and threat detection to secure backend services.
	- Caching: Stores data from previous requests to improve speed and reduce the load on backend services.
	- Load Balancing: Distributes incoming requests to ensure no single service instance is overwhelmed.
- What is SSL/TLS:
	- SSL (Secure Sockets Layer) and TLS (Transport Layer Security) are cryptographic protocols designed to provide secure communication over a computer network. They encrypt data, authenticate communicating parties, and ensure data hasn't been tampered with. SSL is older and has known vulnerabilities, so it's been superseded by TLS. These protocols are commonly used to secure web traffic, turning HTTP into HTTPS.
    - What is a reverse proxy (and proxy):
	- A reverse proxy sits in front of web servers and forwards client requests to them. It can enhance security, performance, and reliability.
	- A forward proxy, often just called a proxy or web proxy, sits in front of a group of clients. It acts as an intermediary, making requests to websites and services on the Internet on behalf of these clients.
		- Use Cases for Forward Proxy:
			1. Bypassing state or institutional browsing restrictions.
			2. Blocking access to certain content (e.g., blocking social media sites in schools).
			3. Protecting user identity online, especially in regions with political sensitivities.
	- The distinction between the two:
		- A forward proxy stands in front of clients, ensuring no server directly communicates with a specific client.
		- A reverse proxy sits in front of servers, ensuring no client directly communicates with a specific server.
	- Benefits of using a reverse proxy:
		1. Load Balancing: Distributes traffic among multiple servers.
		2. Protection from Attacks: Hides the IP address of origin servers, making targeted attacks, like DDoS, more difficult.
		3. Global Server Load Balancing (GSLB): Directs clients to the geographically nearest server, reducing load times.
		4. Caching: Speeds up performance by storing and delivering cached content.
		5. SSL Encryption: Handles the encryption and decryption, relieving the origin server from the computational overhead.