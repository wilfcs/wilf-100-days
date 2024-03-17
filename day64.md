- Integrity Constraints: They ensure the correctness of data in the database
	- We have some constraints in the db that ensure that there is correctness of data. for example there is an attribute called roll number in student table. we want to insert 10 in it, it will be alright to do so. but if we try to insert "ABC" in it, it should throw an error. This is done to maintain the integrity of data.
	- Here are four types of integrity constraints:- 
		- Domain Constraint: Restricts the value in the attribute. It also restricts the data type of an attribute. eg. candidate birth year < 2002. candidate birth year should be integer only. etc.
		- Entity Constraints: Every relation should have Primary Key. PrimaryKey != NULL.
		- Referential Constraints: Specified between two relations of tables & helps maintain consistency among tuples of two relations. It Makes sure that If FK in referencing table refers to PK of referenced table then every value of the FK must be either NULL or available in referenced table. FK must have the matching PK for its each value in the parent table or it must be NULL. If a tuple is deleted in the referenced table then FK in the referencing table should become NULL or we should delete the entry in the referencing table itself to maintain the integrity. To make the FK null we use "on delete null" and to delete the entire row we use "delete cascade".
		- Key Constraints:
			- When defining a table we keep some constraints on the attributes of the table. These are called key constraints. These are its types:
			- NOT NULL: This constraint will restrict the user from not having a NULL value in the attribute. eg- create Table Cust(ID int NOT NULL);
			- UNIQUE: It helps us to ensure that all the values consisting in a column are different from each other.
			- DEFAULT: it is used to set the default value to the column. The default value is added to the columns if no value is specified for them.
			- CHECK: We define domain range. check age >= 19.
			- PRIMARY KEY
			- FOREIGN KEY
- Interview question -> when can FK be null? when there is a data in the table that is being referenced to and when we delete that data then we must delete the FK corresponding to it in the table it is being referenced by, we must make the FK null to maintain integrity.

Look at ER model to relational model video now because that is the most important thing along with making ER model as well and I cannot explain it here. Skip everything else but don't skip this please. please.  Also I will write the notes the moment I re read it. right now. 


Normalisation:
- Normalisation is a step towards DB optimisation. This is used to deal with redundant data.
- To avoid redundancy in the DB and not to store redundant data aka duplicate data we use normalization.
- Redundancy means having multiple copies of the same data in the database.
- Normalisation divides the larger table into smaller and links them using relationships. It is done to eliminate undesirable anomalies like insertion, updation and deletion anamolies. 
- eg. If we have a table called students and inside students suppose we are also having two rows called faculty name and faculty id. This table is very redundant because all the students data is unique but they have the same set of faculty. To solve this problem we can normalize the table and have two entities - students and faculty and map them using foreign keys.
