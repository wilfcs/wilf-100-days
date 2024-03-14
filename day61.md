Database Administrator (DBA):
- A person or an entity who has central control of both the data and the programs that access those data.
- Functions of DBA: Schema Design, Storage structure, Access  Methods, Authorization control, Routine maintenance (Periodic backups, Security patches, Any upgrades)

DBMS Application Architectures:
There are Three Architectures of a DBMS Application.
- T1 Architecture
	- The client, server & DB all present on the same machine. Used while developing an app.
- T2 Architecture
	- The 2-tier architecture is similar to a basic client-server model. The application at the client end directly communicates with the database on the server side. APIs like ODBC and JDBC are used for this interaction. The server side is responsible for providing query processing and transaction management functionalities. You can relate this to writing a backend code. You don't interact with a frontend while doing so, you deal with just the backend. So basically there is no frontend.
- T3 Architecture
	- There is another layer between the client and the server. The client does not directly communicate with the db. 
	- Client machine is just a frontend and doesn’t contain any direct DB calls
	- Client machine (frontend) communicates with App server, and App server communicates with DB system to access data
	- Best for WWW Applications.

ER Model:
- It is a high-level data model based on a perception of a real-world that consists of a collection of basic objects, called entities, and of relationships among these objects. eg- ER model for a library management system -> Entities: Books, Authors, Members. Relationships: Written by (relationship between Books and authors), Borrowed by, etc.
- The graphical representation of ER Model is ER diagram, which acts as a blueprint of DB.

Entity:
- An Entity is a “thing” or “object” in the real world that is distinguishable from all other objects. This object might not be living or physical but an entity exists. eg. Each Student in a college is an entity. 
- Entity is often interchangeable with entity set. for example we can call each student in a college as an entity set and one study is an entity.
- Strong Entity: Can be uniquely identified. eg.  Course
- Weak Entity: Can’t be uniquely identified, and it depends on some other strong entity. eg. Enrollment. Its identification is dependent on the existence of two other entities, "Student" and "Course.". 
- Loan -> Strong Entity, Payment -> Weak Entity

Attributes:
- An entity is represented by a set of attributes.
- E.g., Student Entity has following attributes: Student_ID, Name, Standard, Course, etc. Basically features.
- So in simple terms an entity is a table and an attribute is a column.

Types of Attributes:
- Simple - Attributes which can’t be divided further. eg. Student’s Roll number
- Composite - Can be divided into subparts i.e. some more attributes. eg. Name of a student can be divided into first name and last name.
- Single-valued - Only one value attribute. e.g., Student ID.
- Multi-valued - Attribute having more than one value. eg. Student's phone number.
- Derived - Value of this type of attribute can be derived from the value of other related attributes. eg. Age of student can be derived from d.o.b and current date.
- Descriptive - The attribute(s) used for describing the relationship is called descriptive attributes, also referred as relationship attributes. They are actually used for storing information about the relationship.

Relationships Constraints:
- Mapping Cardinality / Cardinality ratio -> Number of entities involved in a relationship.
	- One to one - Citizen has Aadhar card.
	- One to many - Citizen has vehicle. One can have many.
	- Many to one - Course taken by prof. Many courses can be taken by one prof.
	- Many to many - Student attend course. Many can attend many.
- Minimum Cardinality / Participation constraints -> Minimum number of instances of one entity that can be associated with the instances of another entity in a relationship in an Entity-Relationship (ER) model.

# DSA ->

## [260. Single Number III](https://leetcode.com/problems/single-number-iii/description/)

## Code ->
```cpp
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        unordered_map<int, int> mp;
        vector<int> ans;

        for(int i = 0; i < nums.size(); i++) mp[nums[i]]++;
        
        for(int i = 0; i < nums.size(); i++){
            if(mp[nums[i]] == 1){
                ans.push_back(nums[i]);
            }
        }
        
        return ans;
    }
};
```