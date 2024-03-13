# DBMS ->
Instances and Schemas:
- The collection of information stored in the DB at a particular moment is called an instance of DB.
- The overall design of the DB is called the DB schema.
- Schema is a structural description of data.

Data Models:
-  Provides a way to describe the design of a DB at a logical level.
-  E.g., ER model, Relational Model, object-oriented model, object-relational data model etc.

Database Languages:
- Data definition language (DDL): to specify the database schema.
- Data manipulation language (DML): to express database queries and updates. Performing CRUD operation in DB is an example of DML.
- Practically, both language features are present in a single DB language e.g. mySQL.
- Query language, a part of DML to specify statements requesting the retrieval of information.

How is Database accessed by application programs?
- Apps (written in host languages, C/C++, Java) interacts with DB
- In C language by ODBC
- In Java by JDBC

# [2485. Find the Pivot Integer](https://leetcode.com/problems/find-the-pivot-integer/)
## Code ->
```cpp
class Solution {
public:
    int pivotInteger(int n) {
        int totalSum=n*(n+1)/2, preSum=0;

        for(int i=1;i<=n;i++){
            if(totalSum-preSum==i){
                return i;
            }
            preSum+=i;
            totalSum-=i;
        }

        return -1;
    }
};
```

# [1171. Remove Zero Sum Consecutive Nodes from Linked List](https://leetcode.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/description/)

## Code ->
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeZeroSumSublists(ListNode* head) {
        int pre = 0;
        ListNode* h = new ListNode(0);
        h->next = head;

        unordered_map<int, ListNode*> s;

        for(auto i = h; i; i = i->next){
            s[pre += i->val] = i;
        }

        pre = 0;
        for(auto i = h; i; i = i->next) i->next = s[pre += i->val]->next;
        
        return h->next;
    }
};
```