# [525. Contiguous Array](https://leetcode.com/problems/contiguous-array/description/)

## Code ->
```cpp
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        unordered_map<int,int> mp; mp[0]=-1;
        int sum = 0,longest_subarray = 0;
        for(int i = 0; i < nums.size(); i++)
        {
            sum += nums[i] == 0 ? - 1 : 1;    
            if(mp.find(sum) != mp.end())
            {
                if(longest_subarray < i - mp[sum])
                {
                    longest_subarray = i - mp[sum];
                }
            }
            else
            {
                mp[sum] = i;
            }
        }
        return longest_subarray;
    }
};
```

# [319. Bulb Switcher](https://leetcode.com/problems/bulb-switcher/description/)

## Code ->
```cpp
class Solution {
public:
    int bulbSwitch(int n) {
        int counts = 0;
        
        for (int i=1; i*i<=n; ++i) counts++;    
        
        return counts;
    }
};
```

# [3079. Find the Sum of Encrypted Integers](https://leetcode.com/contest/biweekly-contest-126/problems/find-the-sum-of-encrypted-integers/)

## Code ->
```cpp
class Solution {
public:
    int makeNum(int &number){
        int largestDigit = 0;
        int temp = number;

        while (temp != 0) {
            int digit = temp % 10;
            largestDigit = std::max(largestDigit, digit);
            temp /= 10;
        }

        int result = 0;
        int multiplier = 1;
        while (number != 0) {
            result += largestDigit * multiplier;
            multiplier *= 10;
            number /= 10;
        }

        return result;
    }
    int sumOfEncryptedInt(vector<int>& nums) {
       int ans = 0;
        for(int i=0; i<nums.size(); i++){
            int num = nums[i];
            
            num = makeNum(num); 
            
            // cout << num << " ";

            // while(num){
            //     ans += num % 10;
            //     num = num/10;
            // }
            // cout << ans << " ";
            ans+=num;
        }
        
        return ans;
    }
};
```

# [3080. Mark Elements on Array by Performing Queries](https://leetcode.com/contest/biweekly-contest-126/problems/mark-elements-on-array-by-performing-queries/)

## Code ->
```cpp
class Solution {
public:
    vector<long long> unmarkedSumArray(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        int m = queries.size();
        
        vector<long long> ans;

        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

        set<int> mk;

        long long unmarkedSum = 0;

        for (int i = 0; i < n; i++) {
            pq.push({nums[i], i});
            unmarkedSum += nums[i];
        }

        
        for (int i = 0; i < m; i++) {
            int index = queries[i][0];
            int k = queries[i][1];

            if (mk.find(index) == mk.end()) {
                mk.insert(index);
                unmarkedSum -= nums[index];
            }

            while (k > 0 && !pq.empty()) {
                pair<int, int> top = pq.top();
                pq.pop();
                // cout << pq.top()

                if (mk.find(top.second) == mk.end())  {
                    mk.insert(top.second);
                    unmarkedSum -= top.first;
                    k--;
                }
            }

            ans.push_back(unmarkedSum);
            
        }

        return ans;
    }
};
```

How to make an ER Diagram of any given dataset?
- Identify entity sets.
- Identify attributes and its types.
- Identify relations and constraints.
- Know about the requirements of Db users.
- Now see the video and practice making the ER Diagram on pen and paper. It is not possible to make it here on this screen sorry. Also watch ER diagram of Facebook by codehelp.

Relational Model
- Relational Model (RM) organises the data in the form of relations (tables). So in simple terms, Himanshu, after we create ER Diagrams we have to make tables out of it. Those tables are called nothing but Relational Models. 
- Tuple: A single row of the table representing a single data point / a unique record.
- Relation Schema: defines the design and structure of the relation, contains the name of the relation and all the columns/attributes.
- Common RM based DBMS systems, aka RDBMS: Oracle, IBM, MySQL, MS Access.
- Degree of table: number of attributes/columns in a given table/relation.
- Cardinality: Total no. of tuple in a given table. 
- Relational Model Keys:
	- Super Key - Any permutation and combination of attributes present in a table which can uniquely identify each tuple. eg. {name, phone, aadhar number} We can identify any person using this super key aka subset of its  attributes. There can be one or more than one super key in a table. It can be just {phone} or {name, phone, aadhar}.
	- Candidate key - minimum subset of super keys required, which can uniquely identify each tuple. eg. {phone, aadhar number} Here we can remove name because 2 persons names can be the same but not phone and aadhar no. A candidate key should not be NULL. Super key can be null. Here possible candidate keys are->  {phone, aadhar number},  {aadhar number},  {phone}
	- Primary key - A primary key is a unique identifier for each record in a table. It uniquely identifies each row and ensures that no two rows in the table have the same combination of values. There is just one PK.
	- Alternate Key - Candidate keys that are left unimplemented or unused after implementing the primary key are called as alternate keys
	- Foreign key - By using foreign keys, you can establish relationships between tables, such as one-to-one, one-to-many, or many-to-many relationships.
	- Composite key - PK formed using at least 2 attributes.
	- Compound key - PK which is formed using 2 FK.
	- Surrogate key - Generated automatically by DB, usually an integer value. It may be used as a PK. eg- a value from 1 to n generated automatically to identify each customer in a db.