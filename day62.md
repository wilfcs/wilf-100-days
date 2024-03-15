Symbols used to represent ER Diagrams 
- Entity - Rectangle
- Weak entity - Double Rectangle
- Attribute - Ellipse
- Multi-valued attribute - Double ellipse
- Primary key - Underline inside ellipse's text
- Weak key Attribute - Dotted Underline inside ellipse's text
- Derived attribute - Dotted ellipse 
- Relationship - kite shape
- Weak relationship - Double kite shape

Extended ER Features:
Used when complexity of the DB increases.
So for example we have an entity called customers and there are multiple attributes of the person like profile pic, custID, address, etc. This will make the ER diagram more complex. That's why we came up with extended ER Features. Instead of making the person entity, we make two entities called customer and employee that will be a subclass of Person. The common attributes like name and address will be in Person and the unique attributes like CustomerID and EmployeeID will be in Customer and Employee attributes.
We will see the extended ER Features now..
- Specialisation:
	- Specialisation is splitting up the entity set into further sub entity sets on the basis of their functionalities, specialities and features.
	- It is a Top-Down approach.
	- e.g., Person entity set can be divided into customer, student, employee. Person is superclass and other specialised entity sets are subclasses.
	- We have “is-a” relationship between superclass and subclass. This is depicted by triangle component.	  
	  

- Generalisation:
	- It is just a reverse of Specialisation. It is a Bottom-up approach.
	- e.g., Car, Jeep and Bus all have some common attributes, to avoid data repetition for the common attributes. DB designer may consider generalizing to a new entity set called "vehicles".
	- Makes DB more refined and simpler.
	- Both Specialisation and Generalisation, has attribute inheritance.
	- In simple terms, Himanshu, Generalisation is just the reverse of Specialisation. The diagram of both remains the same. It's just a way of thinking things in the bottom-up way. You may think about customer and employee first and then think about the Person entity.
- Aggregation:
	- Aggregation is something else. It shows relationships among relationships. 
	- Aggregation is the process of combining things. That is, putting those things together so that we can refer to them collectively
	- Example -> There is a Job entity and it has two children called Employee and Branch and they are two separate entities. This is how its ER Diagram looks like -> 
	  
	  
	- now if there is a Manager that manages all these entities, how to show that on an ER Diagram? If we connect the entity Manager with all the other entities using 3 separate lines then that will make the diagram redundant. 


# [1005. Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/description/)

## Code ->
```cpp
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(),nums.end());
        int i = 0;
        while(i<nums.size() && nums[i]<0 && k>0)
        {
            nums[i] *= -1;
            i++;
            k--;
        }

        if(k%2 == 1)
        {
            sort(nums.begin(),nums.end());
            nums[0] *= -1;
        }
        int sum = 0;
        for(int n : nums) sum += n;
        return sum;
    }
};
```

# [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/description/)

## Code ->
```cpp
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> left_products(n);
        vector<int> right_products(n);
        vector<int> answer(n);

        left_products[0] = 1;
        for (int i = 1; i < n; ++i) {
            left_products[i] = left_products[i - 1] * nums[i - 1];
        }

        right_products[n - 1] = 1;
        for (int i = n - 2; i >= 0; --i) {
            right_products[i] = right_products[i + 1] * nums[i + 1];
        }

        for (int i = 0; i < n; ++i) {
            answer[i] = left_products[i] * right_products[i];
        }

        return answer;
    }
};
```

# [768. Max Chunks To Make Sorted II](https://leetcode.com/problems/max-chunks-to-make-sorted-ii/description/)
## Code ->
```cpp
class Solution {

public:
    int maxChunksToSorted(vector<int>& arr) {
        int n=arr.size();  
        vector<int> rmin(n+1);
            rmin[n]=INT_MAX;
            for(int i=n-1;i>=0;i--){
                rmin[i]=min(arr[i],rmin[i+1]);
                
            }
            int lmax=INT_MIN;
            int chunk=0;
            for(int  i=0;i<n;i++){
                lmax=max(arr[i],lmax);
                if(lmax<=rmin[i+1]){
                    chunk++;
                }
            }
            return chunk;
    }
};
```